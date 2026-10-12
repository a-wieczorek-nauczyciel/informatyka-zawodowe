# Zadania utrwalające

Zadania podsumowujące dział „Serwer DHCP”. Wykonujesz je w środowisku laboratoryjnym (`SRV01` + `CLIENT01`, sieć wewnętrzna `lab`) i dokumentujesz zrzutami ekranu — tak jak na egzaminie INF.02.

!!! tip "Zacznij od czystego stanu"
    Przed zadaniem 1 przywróć `SRV01` do snapshotu sprzed instalacji roli DHCP (lub usuń rolę). Całą konfigurację wykonujesz od zera.

## Zadanie 1: Serwer DHCP od zera (GUI)

Skonfiguruj serwer DHCP dla sieci `172.16.5.0/24` według specyfikacji:

| Parametr | Wartość |
|---|---|
| Adres serwera | `172.16.5.2/24` |
| Brama | `172.16.5.1` |
| Nazwa zakresu | `Biuro` |
| Pula | `172.16.5.20 – 172.16.5.120` |
| Wykluczenie | `172.16.5.20 – 172.16.5.29` |
| Czas dzierżawy | 2 dni |
| Serwer DNS (opcja 006) | `172.16.5.2` |
| Domena (opcja 015) | `biuro.local` |

- [ ] Zrzut ekranu: konfiguracja IPv4 serwera
- [ ] Zrzut ekranu: zainstalowana rola DHCP Server w Server Manager
- [ ] Zrzut ekranu: zakres z rozwiniętymi gałęziami *Address Pool* i *Scope Options*
- [ ] Zrzut ekranu: `ipconfig /all` na kliencie z adresem z puli i poprawnymi opcjami
- [ ] Zrzut ekranu: dzierżawa klienta w konsoli DHCP

## Zadanie 2: Rezerwacja dla drukarki

Klient `CLIENT01` udaje drukarkę sieciową w sekretariacie.

- [ ] Rezerwacja adresu `172.16.5.25` dla adresu MAC klienta, nazwa `DRUKARKA-SEKRETARIAT`
- [ ] Na kliencie po odnowieniu dzierżawy widoczny adres `172.16.5.25`
- [ ] Zrzut ekranu: rezerwacja w konsoli i wynik `ipconfig /all` na kliencie

## Zadanie 3: Ta sama konfiguracja w PowerShell

Usuń zakres `Biuro` i odtwórz całą konfigurację z zadań 1 i 2 **jednym skryptem** `dhcp-biuro.ps1`.

- [ ] Skrypt tworzy zakres z poprawną pulą, maską i czasem dzierżawy
- [ ] Skrypt dodaje wykluczenie, opcje 003, 006, 015 i rezerwację
- [ ] Skrypt na końcu wyświetla zakres, opcje i rezerwacje
- [ ] Zrzut ekranu: uruchomienie skryptu i jego wynik
- [ ] Zrzut ekranu: klient ponownie otrzymuje adres zarezerwowany

## Zadanie 4: Rozwiąż usterkę

Nauczyciel wprowadzi na Twoim serwerze jedną usterkę (albo zrobi to kolega z ławki). Twoim zadaniem jest ją znaleźć i usunąć.

- [ ] Opis objawu widocznego na kliencie
- [ ] Lista wykonanych kroków diagnostycznych (polecenia i wnioski)
- [ ] Wskazana przyczyna i sposób naprawy
- [ ] Zrzut ekranu: klient z poprawną konfiguracją po naprawie

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. Porty"
    Na jakim protokole warstwy transportowej i na jakich portach działa DHCP? Który port należy do serwera?

??? question "2. DORA"
    Który komunikat procesu DORA jest odpowiedzią klienta na ofertę serwera i dlaczego jest wysyłany jako rozgłoszenie?

??? question "3. APIPA"
    Klient ma adres `169.254.12.7`. Co to oznacza?

??? question "4. Wykluczenie a rezerwacja"
    Czym różni się wykluczenie od rezerwacji? Podaj po jednym przykładzie zastosowania.

??? question "5. Poziomy opcji"
    Opcja 006 jest ustawiona na poziomie serwera i na poziomie zakresu z różnymi wartościami. Którą wartość otrzyma klient?

??? question "6. Adres serwera"
    Dlaczego serwer DHCP musi mieć adres statyczny?
