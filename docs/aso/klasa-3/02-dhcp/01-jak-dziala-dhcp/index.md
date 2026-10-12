# Jak działa DHCP

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, jaki problem rozwiązuje DHCP, opisać cztery etapy procesu DORA, wyjaśnić pojęcie dzierżawy i rozpoznać adres APIPA jako objaw braku serwera DHCP.

## Problem: konfiguracja sieci na stu komputerach

Każde urządzenie w sieci IPv4 potrzebuje co najmniej czterech informacji: **adresu IP**, **maski podsieci**, **bramy domyślnej** i **adresu serwera DNS**. Wpisanie ich ręcznie na stu komputerach oznacza:

- godziny pracy przy każdej zmianie (np. nowy serwer DNS = sto komputerów do przekonfigurowania),
- ryzyko **konfliktu adresów** — dwa komputery z tym samym IP,
- brak kontroli nad tym, które adresy są zajęte,
- kłopot z laptopami i telefonami, które co chwilę pojawiają się w sieci i z niej znikają.

**DHCP (Dynamic Host Configuration Protocol)** rozwiązuje to centralnie: serwer DHCP przechowuje pulę adresów i ustawień, a klienci pobierają z niej konfigurację automatycznie, przy każdym podłączeniu do sieci.

## Kluczowe pojęcia

**Serwer DHCP** — urządzenie lub usługa rozdająca konfigurację. W domu jest nim zwykle router, w firmie — serwer (u nas: rola DHCP na Windows Server 2022).

**Klient DHCP** — urządzenie, które ma ustawione „Uzyskaj adres IP automatycznie”.

**Zakres (scope)** — ciągła pula adresów, z której serwer rozdaje adresy w danej podsieci, np. `192.168.10.100 – 192.168.10.200`.

**Dzierżawa (lease)** — adres nie jest przydzielany na zawsze, tylko **na określony czas** (w Windows Server domyślnie 8 dni). Po jego upływie, jeśli klient się nie odezwie, adres wraca do puli.

**Opcje DHCP** — dodatkowe ustawienia przekazywane razem z adresem: brama (opcja 003), serwery DNS (006), nazwa domeny DNS (015).

## Proces DORA

Klient po podłączeniu do sieci **nie ma jeszcze adresu IP** i **nie wie, gdzie jest serwer**. Dlatego pierwsze komunikaty wysyła jako **rozgłoszenie** (broadcast) — do wszystkich w sieci lokalnej.

| Etap | Komunikat | Kto → do kogo | Treść |
|---|---|---|---|
| **D** | DHCP**D**ISCOVER | klient → wszyscy (broadcast) | „Czy jest tu jakiś serwer DHCP?” |
| **O** | DHCP**O**FFER | serwer → klient | „Mogę ci dać adres 192.168.10.105, maska, brama, DNS…” |
| **R** | DHCP**R**EQUEST | klient → wszyscy (broadcast) | „Biorę ofertę od serwera 192.168.10.10” |
| **A** | DHCP**A**CK | serwer → klient | „Potwierdzam, adres jest twój na 8 dni” |

Komunikat REQUEST jest również rozgłoszeniem — dzięki temu, jeśli w sieci działają dwa serwery DHCP, ten, którego oferty klient nie wybrał, dowiaduje się o tym i zwalnia zaproponowany adres.

DHCP działa na protokole **UDP**: serwer nasłuchuje na porcie **67**, klient na porcie **68**.

## Odnawianie dzierżawy

Klient nie czeka do końca dzierżawy:

- po **50%** czasu dzierżawy próbuje ją odnowić bezpośrednio u swojego serwera (REQUEST → ACK),
- jeśli serwer nie odpowiada, po **87,5%** czasu próbuje u dowolnego serwera (broadcast),
- jeśli dzierżawa wygaśnie — klient traci adres i zaczyna od DISCOVER.

## Gdy serwera nie ma: APIPA

Jeśli klient nie otrzyma odpowiedzi od żadnego serwera DHCP, Windows przydziela sobie sam adres z zakresu **169.254.0.0/16** (APIPA — *Automatic Private IP Addressing*). Taki adres pozwala komunikować się tylko z innymi komputerami w tej samej sytuacji — nie ma bramy, nie ma internetu.

!!! tip "Objaw do zapamiętania"
    Adres zaczynający się od `169.254` w wyniku `ipconfig` niemal zawsze oznacza jedno: **klient nie dostał odpowiedzi od serwera DHCP**. Sprawdzasz wtedy kabel / kartę sieciową maszyny wirtualnej, działanie usługi DHCP na serwerze i to, czy zakres jest aktywny.

## Polecenia po stronie klienta

```powershell
ipconfig /all        # pełna konfiguracja: czy DHCP włączony, adres serwera, czas dzierżawy
ipconfig /release    # zwolnij adres
ipconfig /renew      # poproś o adres (pełne DORA)
```

## Zadania

??? question "Zadanie 1: Policz koszt ręcznej konfiguracji"
    Firma ma 120 komputerów skonfigurowanych ręcznie. Zmienia się adres serwera DNS. Oszacuj, ile czasu zajmie zmiana, jeśli jeden komputer to 3 minuty pracy. Opisz, jak ta sama zmiana wyglądałaby w sieci z serwerem DHCP.

??? question "Zadanie 2: Ułóż DORA"
    Opisz własnymi słowami cztery etapy procesu DORA. Przy każdym zaznacz, czy komunikat jest wysyłany jako rozgłoszenie, czy bezpośrednio do odbiorcy, i wyjaśnij dlaczego.

??? question "Zadanie 3: Dlaczego REQUEST to broadcast"
    Klient otrzymał już ofertę od serwera i zna jego adres. Dlaczego mimo to wysyła REQUEST jako rozgłoszenie, a nie bezpośrednio do tego serwera?

??? question "Zadanie 4: Oś czasu dzierżawy"
    Dzierżawa trwa 8 dni i zaczęła się w poniedziałek o 8:00. Wyznacz moment pierwszej próby odnowienia u tego samego serwera, moment próby u dowolnego serwera i moment wygaśnięcia dzierżawy.

??? question "Zadanie 5: Diagnoza po ipconfig"
    Uczeń zgłasza brak internetu. Wynik `ipconfig` pokazuje adres `169.254.37.12` i brak bramy. Wypisz w kolejności trzy rzeczy, które sprawdzisz, i uzasadnij tę kolejność.

??? question "Zadanie 6: Twoja sieć domowa"
    Sprawdź na swoim komputerze w domu (`ipconfig /all`): adres serwera DHCP, czas uzyskania i wygaśnięcia dzierżawy. Jakie urządzenie pełni u Ciebie rolę serwera DHCP i jaki czas dzierżawy ustawił?
