# Zadania utrwalające

Zadania podsumowujące dział „Podstawy sieci i adres statyczny”. Część obliczeniową wykonujesz na kartce lub w pliku tekstowym, część praktyczną — w środowisku laboratoryjnym, dokumentując ją zrzutami ekranu.

## Zadanie 1: Plan adresacji (bez VM)

Firma otrzymała sieć `10.10.20.0/24`. Zaplanuj adresację:

- [ ] adres sieci, broadcast, liczba hostów
- [ ] adres bramy (pierwszy host w sieci)
- [ ] adresy statyczne dla dwóch serwerów i trzech drukarek
- [ ] przedział adresów zostawiony na przyszłą pulę DHCP dla około 100 komputerów
- [ ] plan zapisany w tabeli: adres / urządzenie / sposób przydziału

## Zadanie 2: Obliczenia (bez VM)

Dla każdego adresu wyznacz adres sieci, broadcast, pierwszy i ostatni host oraz liczbę hostów:

- [ ] `192.168.50.99/24`
- [ ] `192.168.50.99/26`
- [ ] `172.16.8.140/27`
- [ ] `10.0.0.6/30`

## Zadanie 3: Serwer według specyfikacji

Skonfiguruj serwer według specyfikacji, wybraną przez siebie metodą:

| Parametr | Wartość |
|---|---|
| Computer name | `SRV-BIURO` |
| IP address | `10.10.20.2/24` |
| Default gateway | `10.10.20.1` |
| DNS | `10.10.20.2` |

- [ ] Zrzut ekranu: `ipconfig /all` z widocznym `DHCP Enabled: No`
- [ ] Zrzut ekranu: *Local Server* w Server Manager z nazwą i adresem
- [ ] Zapisane polecenia lub kroki, którymi wykonano konfigurację

## Zadanie 4: Łączność serwer — klient

- [ ] Klient skonfigurowany statycznie w sieci `10.10.20.0/24` (adres spoza planowanej puli DHCP)
- [ ] Na obu maszynach włączona reguła zapory dla ICMPv4 — bez wyłączania zapory
- [ ] Zrzut ekranu: udany ping w obie strony
- [ ] Zrzut ekranu: tablica ARP klienta z adresem MAC serwera

## Zadanie 5: Rozwiąż usterkę

Kolega z ławki wprowadza w Twoim labie jedną usterkę sieci (adres, maska, karta VirtualBox lub zapora). Znajdź ją, korzystając z tabeli diagnostycznej z lekcji o sieci laboratoryjnej.

- [ ] Opis objawu
- [ ] Kolejne kroki diagnostyczne z wynikami
- [ ] Przyczyna i naprawa
- [ ] Zrzut ekranu działającego pingu po naprawie

## Zadanie 6: Krótki quiz sprawdzający (bez VM)

??? question "1. Prefiks"
    Zapisz maskę `255.255.255.224` jako prefiks. Ile hostów mieści taka sieć?

??? question "2. Adresy zarezerwowane"
    Dlaczego w sieci `/24` można zaadresować 254 urządzenia, a nie 256?

??? question "3. Brama"
    Kiedy komputer wysyła pakiet do bramy domyślnej, a kiedy bezpośrednio do odbiorcy?

??? question "4. Adres serwera"
    Podaj dwa powody, dla których serwer powinien mieć adres statyczny.

??? question "5. Ping i zapora"
    Konfiguracja adresów jest poprawna, a ping nie działa. Co sprawdzisz i jak to naprawisz bez wyłączania zapory?

??? question "6. Server Core"
    Którymi dwoma sposobami skonfigurujesz adres statyczny na Windows Server w wersji bez interfejsu graficznego?
