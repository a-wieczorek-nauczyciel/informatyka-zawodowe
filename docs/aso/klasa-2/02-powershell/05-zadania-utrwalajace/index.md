# Zadania utrwalające

Zadania podsumowujące dział „PowerShell”. Wykonujesz je na maszynie wirtualnej z Windows, **wyłącznie w PowerShell** — bez okien konfiguracyjnych systemu. Dokumentujesz je zrzutami ekranu z widocznym poleceniem i jego wynikiem.

!!! tip "Snapshot przed startem"
    Przed rozpoczęciem zrób snapshot maszyny, np. `przed-powershell`. Jeśli coś pójdzie nie tak, wrócisz do czystego stanu bez reinstalacji systemu.

## Zadanie 1: Przygotowanie stanowiska dla działu

Firma zatrudnia trzy osoby w dziale handlowym. Przygotuj dla nich komputer.

- [ ] Utworzona grupa lokalna `Handel` z opisem „Dział handlowy”
- [ ] Utworzone konta `mlis`, `kwrona`, `tsowa` z pełnymi imionami i nazwiskami (wymyśl je)
- [ ] Wszystkie trzy konta należą do grup `Handel` i `Użytkownicy` (lub `Users`)
- [ ] Konto `tsowa` jest wyłączone (osoba zaczyna pracę za miesiąc)
- [ ] Utworzony folder `C:\Handel\Dokumenty`
- [ ] Zrzut ekranu: wynik `Get-LocalGroupMember Handel`
- [ ] Zrzut ekranu: wynik `Get-LocalUser` z kolumnami `Name`, `Enabled`, `Description`

## Zadanie 2: Raport stanu systemu

Przygotuj plik `C:\Raporty\raport.txt` zawierający (każda sekcja z nagłówkiem):

- [ ] nazwę komputera i wersję systemu
- [ ] adres IPv4, bramę domyślną i serwer DNS
- [ ] pięć procesów zużywających najwięcej pamięci
- [ ] liczbę usług uruchomionych i liczbę zatrzymanych
- [ ] listę kont lokalnych z informacją, czy są włączone
- [ ] Zrzut ekranu: zawartość pliku wyświetlona poleceniem `Get-Content`

## Zadanie 3: Skrypt zakładający konta

Napisz skrypt `nowi-pracownicy.ps1`, który:

- [ ] wczytuje plik `pracownicy.csv` (kolumny: `Login`, `Imie`, `Nazwisko`, `Dzial`)
- [ ] dla każdego działu tworzy grupę lokalną, jeśli jeszcze nie istnieje
- [ ] tworzy konto, jeśli jeszcze nie istnieje, a jeśli istnieje — wypisuje komunikat
- [ ] dodaje konto do grupy swojego działu
- [ ] na końcu wypisuje podsumowanie: ile kont utworzono, ile pominięto
- [ ] Skrypt zawiera komentarz nagłówkowy z opisem działania
- [ ] Zrzut ekranu: dwa kolejne uruchomienia skryptu i różnica w wyniku

## Zadanie 4: Porządki w usługach

- [ ] Wyeksportuj do CSV listę wszystkich usług z kolumnami `Name`, `DisplayName`, `Status`, `StartType`
- [ ] Wyświetl usługi z typem uruchamiania `Automatic`, które są zatrzymane
- [ ] Zatrzymaj usługę `Spooler` i ustaw jej typ uruchamiania na `Disabled`
- [ ] Spróbuj uruchomić `Spooler` poleceniem `Start-Service` — zapisz komunikat i wyjaśnij go
- [ ] Przywróć pierwotne ustawienia usługi
- [ ] Zrzuty ekranu każdego kroku

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. Obiekty"
    Czym różni się wynik polecenia w PowerShell od wyniku polecenia w `cmd`? Podaj jedną praktyczną konsekwencję tej różnicy.

??? question "2. Kolejność w potoku"
    Dlaczego `Format-Table` powinien być ostatnim poleceniem w potoku?

??? question "3. $_"
    Co oznacza zmienna `$_` w bloku `Where-Object { ... }`?

??? question "4. -WhatIf"
    Do czego służy parametr `-WhatIf` i przy jakich poleceniach warto go używać?

??? question "5. ExecutionPolicy"
    Twój skrypt nie uruchamia się z komunikatem o wyłączonym wykonywaniu skryptów. Jakie polecenie wykonasz i dlaczego wybierzesz właśnie taki zakres (`-Scope`)?

??? question "6. Stan a typ uruchamiania"
    Usługa ma stan `Stopped` i typ uruchamiania `Automatic`. Co się z nią stanie po restarcie komputera?
