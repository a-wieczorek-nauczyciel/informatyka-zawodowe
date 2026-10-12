# Lekcja 5 — Pliki wsadowe (.bat)

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć napisać i uruchomić plik wsadowy `.bat`, korzystać w nim ze zmiennych własnych i systemowych, odczytywać parametry wywołania, wykonywać obliczenia, podejmować decyzje instrukcją `if`, wybierać jedną z wielu ścieżek (odpowiednik instrukcji `switch` / *case*), powtarzać polecenia pętlą `for` oraz zbudować proste menu.

## Czym jest plik wsadowy

**Plik wsadowy** (*batch file*) to plik tekstowy z rozszerzeniem `.bat` (lub `.cmd`), zawierający polecenia wiersza poleceń **cmd.exe**, wykonywane po kolei od góry do dołu. To najstarszy sposób automatyzacji w Windows — działa na każdym komputerze z Windows, bez żadnej konfiguracji i bez Execution Policy.

| | Plik wsadowy `.bat` | Skrypt PowerShell `.ps1` |
|---|---|---|
| interpreter | `cmd.exe` | `powershell.exe` / `pwsh.exe` |
| dane | tekst | obiekty |
| zmienne | `%nazwa%`, tylko tekst i liczby całkowite | `$nazwa`, dowolne typy |
| obliczenia | `set /a`, tylko liczby całkowite | pełna arytmetyka, `[math]` |
| uruchomienie | dwuklik albo nazwa w konsoli | `.\skrypt.ps1`, wymaga zezwolenia Execution Policy |
| zastosowanie dziś | proste zadania, stare systemy, skrypty logowania | administracja Windows i Windows Server |

Na egzaminie INF.02 i w starszych firmach wciąż spotkasz pliki `.bat`, dlatego trzeba umieć je czytać i pisać.

## Pierwszy plik wsadowy

1. Otwórz Notatnik i wpisz:

    ```bat
    @echo off
    rem Mój pierwszy plik wsadowy
    echo Witaj, %USERNAME%!
    echo Dzisiaj jest %DATE%, godzina %TIME%.
    pause
    ```

2. **Plik → Zapisz jako** → *Zapisz jako typ:* **Wszystkie pliki** → nazwa `witaj.bat` → zapisz w `C:\PS_Lab`.
3. Uruchom dwuklikiem albo w konsoli:

    ```text
    C:\PS_Lab> witaj.bat
    ```

    W PowerShellu: `.\witaj.bat` — PowerShell przekaże plik do `cmd.exe`.

| Polecenie | Działanie |
|---|---|
| `@echo off` | nie wyświetlaj poleceń przed ich wykonaniem (`@` ukrywa także samo `echo off`) |
| `echo tekst` | wypisz tekst |
| `echo.` | wypisz pustą linię |
| `rem komentarz` lub `:: komentarz` | komentarz — linia nie jest wykonywana |
| `pause` | czekaj na naciśnięcie klawisza („Naciśnij dowolny klawisz…”) |
| `cls` | wyczyść ekran |
| `title Tekst` | ustaw tytuł okna |

!!! warning "Polskie znaki"
    Konsola `cmd` domyślnie używa starej strony kodowej, więc polskie litery zapisane w Notatniku jako UTF-8 mogą wyświetlić się błędnie. Na początku pliku dodaj `chcp 65001 > nul` albo pisz komunikaty bez polskich znaków.

## Zmienne

### Zmienne własne

```bat
@echo off
set imie=Ada
set klasa=2B
echo Uczen: %imie%, klasa %klasa%
```

- `set nazwa=wartość` — **bez spacji** wokół `=`. Zapis `set imie = Ada` tworzy zmienną o nazwie `imie ` (ze spacją) i wartości ` Ada`.
- odczyt: `%nazwa%`
- wielkość liter w nazwie nie ma znaczenia,
- `set` bez parametrów wypisuje wszystkie zmienne, `set u` — zmienne zaczynające się od „u”.

### Dane od użytkownika

```bat
set /p imie=Podaj imie: 
echo Czesc, %imie%!
```

`set /p` wyświetla komunikat i zapisuje do zmiennej to, co wpisze użytkownik.

### Zmienne systemowe

| Zmienna | Zawartość |
|---|---|
| `%USERNAME%` | nazwa zalogowanego użytkownika |
| `%COMPUTERNAME%` | nazwa komputera |
| `%USERPROFILE%` | folder profilu, np. `C:\Users\Ada` |
| `%SystemRoot%` | folder Windows, zwykle `C:\Windows` |
| `%CD%` | bieżący folder |
| `%DATE%`, `%TIME%` | bieżąca data i godzina (format zależy od ustawień regionalnych) |
| `%RANDOM%` | liczba losowa od 0 do 32767 |
| `%ERRORLEVEL%` | kod zakończenia ostatniego polecenia (0 = sukces) |
| `%PATH%` | foldery, w których system szuka programów |
| `%OS%` | system operacyjny (`Windows_NT`) |

## Obliczenia: set /a

```bat
@echo off
set /a suma=7+5
set /a iloczyn=7*5
set /a iloraz=7/2
set /a reszta=7%%2
set /a licznik=0
set /a licznik+=1
echo suma=%suma% iloczyn=%iloczyn% iloraz=%iloraz% reszta=%reszta% licznik=%licznik%
```

- `set /a` liczy wyłącznie na **liczbach całkowitych** — `7/2` daje `3`,
- operatory: `+ - * / %` oraz `+= -= *= /=` i nawiasy,
- w pliku `.bat` reszta z dzielenia to `%%` (pojedynczy `%` ma znaczenie specjalne); w konsoli wpisuje się jeden `%`,
- wewnątrz `set /a` nazwy zmiennych można pisać bez `%`: `set /a wynik=a*b`.

## Parametry wywołania

Plik wsadowy może dostać argumenty z wiersza poleceń:

```text
C:\PS_Lab> powitanie.bat Ada 2B
```

| Zapis | Znaczenie |
|---|---|
| `%0` | nazwa uruchomionego pliku |
| `%1` … `%9` | kolejne argumenty |
| `%*` | wszystkie argumenty |
| `shift` | przesuwa argumenty: `%2` staje się `%1` itd. |
| `%~1` | argument bez otaczających cudzysłowów |

```bat
@echo off
rem powitanie.bat — uzycie: powitanie.bat imie klasa
if "%~1"=="" (
    echo Uzycie: %0 imie [klasa]
    exit /b 1
)
echo Czesc, %~1!
if not "%~2"=="" echo Twoja klasa: %~2
```

Cudzysłowy w porównaniu `"%~1"==""` chronią przed błędem, gdy argumentu brak albo zawiera spacje.

## Instrukcja warunkowa if

```bat
if "%odp%"=="tak" echo Potwierdzono
if /i "%odp%"=="TAK" echo Potwierdzono bez wzgledu na wielkosc liter
if not "%odp%"=="tak" echo Nie potwierdzono

if exist C:\PS_Lab\raport.txt echo Raport istnieje
if not exist C:\PS_Lab\Kopie mkdir C:\PS_Lab\Kopie
if defined imie echo Zmienna imie jest ustawiona
```

### Porównywanie liczb

| Operator | Znaczenie |
|---|---|
| `EQU` | równe |
| `NEQ` | różne |
| `LSS` | mniejsze |
| `LEQ` | mniejsze lub równe |
| `GTR` | większe |
| `GEQ` | większe lub równe |

```bat
set /p wiek=Podaj wiek: 
if %wiek% GEQ 18 (
    echo Pelnoletni
) else (
    echo Niepelnoletni
)
```

!!! warning "Składnia else"
    `) else (` musi stać w **jednej linii**, a nawias otwierający blok `if` — w tej samej linii co `if`. Inaczej `cmd` zgłosi błąd składni.

### Kod zakończenia polecenia

```bat
ping -n 1 192.168.1.1 > nul
if %ERRORLEVEL% EQU 0 (
    echo Brama odpowiada
) else (
    echo Brak odpowiedzi bramy
)
```

`> nul` ukrywa wynik polecenia. `if errorlevel 1` (bez `%`) oznacza „kod zakończenia **co najmniej** 1”.

## Wybór jednej z wielu ścieżek (odpowiednik switch)

W `cmd` nie ma instrukcji `switch`. Ten sam efekt uzyskuje się na dwa sposoby.

### goto i etykiety

```bat
@echo off
set /p dzien=Numer dnia (1-7): 
for %%d in (1 2 3 4 5 6 7) do if "%dzien%"=="%%d" goto dzien_%%d
goto inny

:dzien_1
echo poniedzialek
goto koniec

:dzien_2
echo wtorek
goto koniec

:dzien_3
echo sroda
goto koniec

:dzien_4
echo czwartek
goto koniec

:dzien_5
echo piatek
goto koniec

:dzien_6
:dzien_7
echo weekend
goto koniec

:inny
echo bledny numer dnia

:koniec
pause
```

- `:nazwa` to **etykieta**, `goto nazwa` przenosi wykonanie do etykiety,
- pętla `for` sprawdza dozwolone wartości; dla pasującej przechodzi do etykiety zbudowanej z tej wartości (`dzien_1` … `dzien_7`) — każda wartość ma własną gałąź, jak `case` w `switch`,
- jeśli żadna wartość nie pasuje, wykonuje się `goto inny` — gałąź domyślna, jak `default`,
- nie używaj samego `goto dzien_%dzien%` bez sprawdzenia: skok do nieistniejącej etykiety kończy działanie całego pliku z błędem,
- **każda gałąź kończy się `goto koniec`** — inaczej wykonanie „przeleci” do kolejnych etykiet (w `switch` w C# zabezpiecza przed tym `break`),
- kilka etykiet jedna pod drugą (`:dzien_6`, `:dzien_7`) to kilka wartości z tą samą akcją.

### Seria instrukcji if

```bat
if "%wybor%"=="1" goto dodaj
if "%wybor%"=="2" goto usun
if "%wybor%"=="0" goto koniec
echo Nieznana opcja
```

### choice — wybór klawiszem

```bat
choice /c TNA /m "Kontynuowac? T-tak, N-nie, A-anuluj"
if errorlevel 3 goto anuluj
if errorlevel 2 goto nie
if errorlevel 1 goto tak
```

`choice` ustawia `ERRORLEVEL` na numer wybranego klawisza z listy `/c`. Sprawdzamy **od największego**, bo `if errorlevel 2` jest prawdziwe także dla 3.

## Pętle: for

W pliku `.bat` zmienną pętli zapisujemy z **dwoma** znakami `%%` (w konsoli — z jednym).

```bat
rem kolejne liczby: start, krok, koniec
for /l %%i in (1,1,5) do echo Obrot %%i

rem kazdy plik pasujacy do wzorca
for %%f in (C:\PS_Lab\*.txt) do echo Plik: %%f

rem kazdy podfolder
for /d %%d in (C:\PS_Lab\*) do echo Folder: %%d

rem kazda linia pliku tekstowego
for /f "usebackq tokens=*" %%l in ("C:\PS_Lab\uczniowie.txt") do echo Uczen: %%l
```

`for /f` z `tokens` i `delims` dzieli linię na kawałki, np. dla pliku z liniami `jkowalski;Jan;Kowalski`:

```bat
for /f "usebackq tokens=1,2,3 delims=;" %%a in ("C:\PS_Lab\konta.txt") do (
    echo Login: %%a, imie: %%b, nazwisko: %%c
)
```

Kolejne kawałki trafiają do kolejnych liter: `%%a`, `%%b`, `%%c`.

### Zmienne zmieniane w pętli

```bat
@echo off
setlocal enabledelayedexpansion
set /a suma=0
for /l %%i in (1,1,10) do (
    set /a suma+=%%i
    echo Po dodaniu %%i suma wynosi !suma!
)
echo Wynik: %suma%
endlocal
```

`cmd` podstawia wartości `%zmienna%` **raz, przed wykonaniem całego bloku** w nawiasach. Wewnątrz pętli `%suma%` miałoby więc ciągle wartość początkową. Rozwiązanie: `setlocal enabledelayedexpansion` i zapis `!suma!` — wartość odczytywana w chwili wykonania.

`setlocal` sprawia też, że zmienne utworzone w skrypcie znikają po jego zakończeniu (po `endlocal`).

## Polecenia do zarządzania systemem, plikami i folderami

| Polecenie | Działanie |
|---|---|
| `mkdir`, `rmdir /s /q` | tworzenie i usuwanie folderu (z zawartością) |
| `copy`, `xcopy /e /i`, `robocopy` | kopiowanie plików / folderów |
| `move`, `ren`, `del` | przenoszenie, zmiana nazwy, usuwanie |
| `dir`, `type`, `find`, `findstr` | lista plików, zawartość, wyszukiwanie tekstu |
| `>`, `>>` | zapis wyniku do pliku / dopisanie |
| `net user`, `net localgroup` | konta i grupy lokalne |
| `ipconfig`, `ping`, `netsh` | sieć |
| `tasklist`, `taskkill /im nazwa.exe` | procesy |
| `sc query`, `net start`, `net stop` | usługi |
| `shutdown /s /t 60`, `shutdown /a` | wyłączenie za 60 s, anulowanie |

## Podprogramy: call

```bat
@echo off
call :wypisz_ramke "Raport dzienny"
call :wypisz_ramke "Koniec raportu"
exit /b 0

:wypisz_ramke
echo ==========================
echo  %~1
echo ==========================
exit /b 0
```

`call :etykieta argumenty` uruchamia fragment pliku jak funkcję, a `exit /b` wraca z niego. Kod zakończenia `exit /b 1` można odczytać w `%ERRORLEVEL%`.

## Przykład: kopia zapasowa z menu

```bat
@echo off
setlocal
set zrodlo=C:\PS_Lab\Dane
set cel=C:\PS_Lab\Kopie

:menu
cls
set wybor=
echo 1 - wykonaj kopie
echo 2 - pokaz kopie
echo 0 - koniec
set /p wybor=Wybierz: 

if "%wybor%"=="1" goto kopia
if "%wybor%"=="2" goto lista
if "%wybor%"=="0" goto koniec
echo Nieznana opcja
pause
goto menu

:kopia
if not exist "%zrodlo%" (
    echo Brak folderu %zrodlo%
    pause
    goto menu
)
set znacznik=%RANDOM%
xcopy "%zrodlo%" "%cel%\kopia_%znacznik%" /e /i /q
echo Kopia zapisana w %cel%\kopia_%znacznik%
pause
goto menu

:lista
dir /b "%cel%"
pause
goto menu

:koniec
endlocal
```

## Zadania

??? question "Zadanie 1: Informacje o systemie"
    Napisz plik `info.bat`, który wypisuje: nazwę użytkownika, nazwę komputera, folder profilu, bieżący folder, datę i godzinę, a następnie zapisuje te same informacje do pliku `C:\PS_Lab\info.txt` (użyj `>` i `>>`).

??? question "Zadanie 2: Kalkulator z parametrami"
    Napisz `kalk.bat`, uruchamiany np. jako `kalk.bat 12 5`, który wypisuje sumę, różnicę, iloczyn, iloraz całkowity i resztę z dzielenia. Gdy nie podano dwóch argumentów, wypisuje instrukcję użycia. Gdy drugi argument to 0, nie dzieli.

??? question "Zadanie 3: Ocena"
    Plik pyta o liczbę punktów (0–100) i wypisuje ocenę według progów 90 / 75 / 60 / 50. Użyj `if` z operatorami `GEQ` i `LSS`. Dla wartości spoza zakresu wypisz komunikat o błędzie.

??? question "Zadanie 4: Dzień tygodnia"
    Plik przyjmuje numer dnia jako parametr (`dzien.bat 3`) i wypisuje jego nazwę. Zrealizuj wybór przez `goto` z etykietami tworzonymi ze zmiennej. Obsłuż brak parametru i numer spoza zakresu.

??? question "Zadanie 5: Tabliczka mnożenia"
    Plik wypisuje tabliczkę mnożenia dla liczby podanej przez użytkownika (od 1 do 10), korzystając z `for /l`, `set /a` i opóźnionego rozwijania zmiennych.

??? question "Zadanie 6: Konta z pliku"
    Przygotuj plik `konta.txt` z liniami w formacie `login;imie;nazwisko`. Napisz `konta.bat`, który dla każdej linii zakłada konto (`net user login Haslo123! /add /fullname:"imie nazwisko"`) i dodaje je do grupy `Uczniowie`. Uruchom jako administrator na maszynie wirtualnej. Sprawdź wynik poleceniem `net localgroup Uczniowie`.

??? question "Zadanie 7: Ten sam skrypt w PowerShell"
    Przepisz zadanie 2 lub 4 na skrypt PowerShell (zobacz następną lekcję). Porównaj długość i czytelność obu wersji.
