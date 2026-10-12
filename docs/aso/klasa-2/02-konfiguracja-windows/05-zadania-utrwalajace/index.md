# Zadania utrwalające

Zadania podsumowujące dział „Konfiguracja systemu Windows”. Wykonujesz je na maszynach wirtualnych i dokumentujesz zrzutami ekranu w kluczowych momentach.

## Zadanie 1: Stanowisko według specyfikacji

| Parametr | Wartość |
|---|---|
| nazwa komputera | `PC-BIURO-01` |
| adres IPv4 | statyczny, zgodny z siecią NAT VirtualBox (np. `10.0.2.40/24`) |
| brama, DNS | `10.0.2.2`, `10.0.2.3` |
| strefa czasowa, format | Polska, `pl-PL` |
| wygląd | tryb ciemny, pasek zadań do lewej, przypięty PowerShell |

- [ ] Zrzut ekranu: `ipconfig /all` z widocznym adresem statycznym
- [ ] Zrzut ekranu: udany `ping` do bramy i `ping wp.pl`
- [ ] Zrzut ekranu: nazwa komputera i strefa czasowa
- [ ] Zrzut ekranu: pulpit z ustawionym wyglądem

## Zadanie 2: Udostępnianie internetu

- [ ] Maszyna A z dwiema kartami (NAT + Internal Network), maszyna B tylko z Internal Network
- [ ] ICS włączone na maszynie A
- [ ] Zrzut ekranu: `ipconfig` na maszynie B z adresem z sieci `192.168.137.0/24`
- [ ] Zrzut ekranu: `tracert 8.8.8.8` z maszyny B

## Zadanie 3: Rozwiąż usterkę

Kolega z ławki wprowadza na Twojej maszynie jedną usterkę sieci (adres, maska, brama, DNS albo karta). Znajdź ją.

- [ ] Opis objawu
- [ ] Kolejne kroki diagnostyczne z wynikami — od dołu do góry
- [ ] Wskazana przyczyna i naprawa
- [ ] Zrzut ekranu poprawnego działania

## Zadanie 4: Raport z narzędzi administracyjnych

Przygotuj plik `raport.txt` zawierający:

- [ ] model procesora i ilość RAM (z `msinfo32`)
- [ ] listę urządzeń z problemami w Device Manager (albo informację, że ich nie ma)
- [ ] trzy ostatnie błędy z dziennika System: data, źródło, Event ID
- [ ] trzy usługi o typie *Automatic*, które są zatrzymane
- [ ] trzy programy uruchamiane przy logowaniu (Task Manager)

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. Cztery parametry"
    Jakie cztery parametry musi mieć komputer, żeby korzystać z internetu? Do czego służy każdy z nich?

??? question "2. 169.254"
    Co oznacza adres zaczynający się od `169.254`?

??? question "3. Ping po adresie i po nazwie"
    `ping 8.8.8.8` działa, a `ping wp.pl` nie. Gdzie leży problem?

??? question "4. ICS"
    Jaki adres dostaje karta lokalna komputera udostępniającego internet przez ICS?

??? question "5. Gdzie szukać"
    W którym narzędziu sprawdzisz, dlaczego komputer w nocy się zrestartował, a w którym — dlaczego nie działa mikrofon?
