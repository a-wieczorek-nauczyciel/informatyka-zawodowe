# Mini-zadania 1–9

Zadania wykonywane na zajęciach po każdym bloku teorii. Wszystkie wykonujesz w PowerShellu uruchomionym jako administrator, w folderze `C:\PS_Lab`. Polecenia układasz sam — na tej stronie nie ma gotowego kodu.

!!! tip "Gdy utkniesz"
    Wróć do metody z lekcji 2: **cel → `Get-Command` → `Get-Help -Examples` → wykonanie**. Czytaj komunikaty błędów — mówią, co i gdzie się nie udało.

## Lekcja 2

### Mini-zadanie 1: Pomoc w PowerShellu (4 min)

1. Zaktualizuj system pomocy.
2. Wyświetl pomoc dla polecenia `Get-Process`.
3. Wyświetl tę pomoc w osobnym okienku.
4. Wyświetl same przykłady z pomocy `Get-Process`.
5. Wyświetl polecenia dotyczące operacji na komputerze (w nazwie występuje „computer”).
6. Wyświetl dokładny opis polecenia `Checkpoint-Computer`.
7. Wyświetl polecenia do pracy na dyskach.
8. Sprawdź, jakie polecenia są w module `WindowsUpdate`.

Aktualizacja pomocy wymaga internetu. Jeśli go nie ma, zacznij od kroku 2.

### Mini-zadanie 2: Użytkownicy lokalni (3 min)

1. Poszukaj poleceń, które pozwalają pracować z użytkownikami lokalnymi.
2. Wyświetl użytkowników lokalnych.
3. Utwórz użytkownika o nazwie `ps-user`. Jeśli PowerShell zapyta o hasło, podaj je w konsoli.
4. Wyświetl listę użytkowników i sprawdź, czy konto powstało.
5. Usuń użytkownika `ps-user`.
6. Wyświetl listę ponownie i sprawdź, że konta już nie ma.

Konto zakładaj tylko na maszynie ćwiczeniowej.

### Mini-zadanie 3: Pliki, katalogi, błędy (4 min)

1. Utwórz katalog `data`.
2. Utwórz w nim plik `report.txt`.
3. Dodaj do pliku trzy linijki tekstu jedną komendą, która umieści w pliku znaki nowej linii.
4. Wyświetl zawartość pliku.
5. Usuń katalog wraz z zawartością. Pomoże parametr `-Recurse`.

### Mini-zadanie 4: Aliasy poleceń (3 min)

1. Za pomocą polecenia `cd` przejdź do katalogu głównego dysku `C:`.
2. Sprawdź, do jakich poleceń odwołują się aliasy: `ls`, `cat`, `cd`, `md`, `cp`, `mv`, `del`.
3. Utwórz alias `gd` uruchamiający cmdlet `Get-Disk`.
4. Sprawdź, czy alias działa.
5. Wyświetl informacje o aliasie `gd`.
6. Usuń alias `gd`.

### Mini-zadanie 5: Przekazywanie parametrów (6 min)

1. Wyświetl interfejsy sieciowe komputera.
2. Wyświetl tylko te, w których nazwie występuje „Ethernet”.
3. Wyświetl te, których nazwa zawiera „Ethernet” i które są fizyczne.
4. Jak bardzo można skrócić nazwę parametru `-Physical`? Sprawdź `-Phys` i krótsze formy.
5. Wyświetl właściwości folderu `C:\Windows` poleceniem `Get-ItemProperty`.
6. Wyświetl jedną komendą właściwości folderów `C:\Windows` i `C:\Users`.
7. Zapamiętaj obie ścieżki w zmiennej i wyświetl właściwości ze zmiennej.
8. Utwórz plik, którego treścią są te dwie ścieżki.
9. Wczytaj ścieżki z pliku do zmiennej i wywołaj dla nich `Get-ItemProperty`.

## Lekcja 3

### Mini-zadanie 6, część 1: -WhatIf i -Confirm (3 min)

1. Sprawdź, co by się stało po uruchomieniu `Restart-Computer`.
2. Sprawdź, co by się stało po uruchomieniu `Stop-Computer`.
3. Wywołaj polecenie wyłączające komputer tak, by zapytało o potwierdzenie. Na pytanie odpowiedz **NIE**.
4. Wylistuj zawartość katalogu `C:\Windows` do pliku `C:\PS_Lab\listing.txt`.
5. Usuń `listing.txt` tak, aby zostać zapytanym o potwierdzenie. Odpowiedz **NIE**.
6. Usuń ten plik w normalny sposób, bez dodatkowych potwierdzeń.

!!! danger "Uwaga na krok 3"
    Odpowiedź **TAK** wyłączy maszynę.

### Mini-zadanie 6, część 2: $ConfirmPreference (4 min)

1. Jaką wartość ma teraz `$ConfirmPreference`?
2. Utwórz jeszcze raz plik `listing.txt`.
3. Zmień `$ConfirmPreference` na `Low`.
4. Usuń plik bez dodatkowego parametru. Czy było pytanie? Odpowiedz NIE.
5. Zmień `$ConfirmPreference` na `Medium`.
6. Usuń plik bez dodatkowego parametru. Czy było pytanie? Odpowiedz NIE.
7. Otwórz nową kartę PowerShella i sprawdź tam wartość. Ustawienie jest globalne czy dotyczy jednej sesji?
8. Przywróć wartość domyślną.
9. Usuń plik bez dodatkowego parametru. Czy było pytanie? Jeśli tak, odpowiedz TAK.

### Mini-zadanie 7, część 1: Potoki i drukarki (4 min)

1. Odnajdź polecenia związane z drukowaniem.
2. Wyświetl zainstalowane drukarki.
3. Wyświetl informacje tylko o jednej drukarce, po nazwie (`LabPrinter`). Dalej używaj tylko jej.
4. Wydrukuj na niej kilka tekstów. Drukarka jest wirtualna i niczego nie wydrukuje.
5. Wyświetl zadania drukowania tej drukarki.
6. Zapisz te informacje w pliku `C:\PS_Lab\printjobs.txt`.

### Mini-zadanie 7, część 2: Potoki i drukarki (5 min)

1. Przejrzyj listę zadań drukowania w przeglądarce graficznej.
2. Zobacz, jakie właściwości ma zadanie drukowania.
3. Zobacz, jakie właściwości ma drukarka.
4. Wybierz jedno zadanie i wyświetl informacje tylko o nim.
5. Usuń to jedno zadanie z kolejki i sprawdź, że zniknęło.
6. Usuń wszystkie zadania i upewnij się, że kolejka jest pusta.

Jedno zadanie może przez chwilę mieć status `Deleting`.

## Lekcja 4

### Mini-zadanie 8: Sort-Object (3 min)

1. Wyświetl listę procesów.
2. Posortuj ją według czasu procesora (właściwość `CPU`).
3. Posortuj ją malejąco.
4. Czy są procesy o tej samej nazwie i różnym zużyciu procesora? Posortuj po nazwie i `CPU`.
5. Do jakiej pełnej właściwości odwołuje się `CPU`?
6. Napisz polecenie z kroku 3 z pełną nazwą tej właściwości.

### Mini-zadanie 9: Measure-Object i Select-Object (5 min)

1. Policz, ile procesów jest uruchomionych.
2. Policz sumę, średnią i maksimum pamięci (`WorkingSet`) wszystkich procesów.
3. Wyświetl tylko nazwę i Id pięciu pierwszych procesów.
4. Wyświetl nazwę i pamięć w MB (kolumna wyliczana, 1 miejsce po przecinku) dla trzech procesów z największą pamięcią.
5. Policz usługi według stanu (`Running`, `Stopped`).
6. Wypisz same nazwy usług ze stanem `Running`.
7. Zapisz wynik z kroku 4 do pliku `C:\PS_Lab\top3.txt`.
