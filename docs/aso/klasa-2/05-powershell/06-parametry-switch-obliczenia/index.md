# Lekcja 6 — Parametry, switch i obliczenia w skryptach

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć odróżnić zmienne systemowe od własnych, przekazywać do skryptu PowerShell parametry wywołania (także obowiązkowe, z typem i wartością domyślną), wykonywać obliczenia i konwersje liczb, stosować instrukcję wyboru `switch` oraz pętle `do-while`, `do-until` i `for`.

## Zmienne systemowe i własne

| Rodzaj | Przykłady | Skąd się biorą |
|---|---|---|
| **zmienne środowiskowe** (systemowe) | `$env:USERNAME`, `$env:COMPUTERNAME`, `$env:USERPROFILE`, `$env:PATH`, `$env:windir` | ustawia system Windows; te same, które w `cmd` zapisuje się `%USERNAME%` |
| **zmienne automatyczne** PowerShella | `$PSVersionTable`, `$HOME`, `$PWD`, `$true`, `$false`, `$null`, `$_`, `$args`, `$LASTEXITCODE` | tworzy i aktualizuje sam PowerShell |
| **zmienne własne** | `$imie`, `$suma`, `$uslugi` | tworzysz je w skrypcie |

```powershell
Write-Output "Użytkownik: $env:USERNAME na komputerze $env:COMPUTERNAME"
Get-ChildItem env:                       # wszystkie zmienne środowiskowe
$env:MOJA_ZMIENNA = "test"               # zmienna środowiskowa tylko dla tej sesji
Get-Variable                             # wszystkie zmienne PowerShella w sesji
```

## Parametry wywołania skryptu

### Najprostszy sposób: $args

```powershell
# powitaj.ps1
Write-Output "Liczba argumentów: $($args.Count)"
Write-Output "Pierwszy: $($args[0])"
```

```text
PS C:\PS_Lab> .\powitaj.ps1 Ada 2B
Liczba argumentów: 2
Pierwszy: Ada
```

`$args` to tablica wszystkich argumentów. Działa, ale nie mówi, co oznacza który argument.

### Blok param

Profesjonalne skrypty deklarują parametry na **samym początku** pliku:

```powershell
# powitaj.ps1
param(
    [string]$Imie = "nieznajomy",
    [int]$Wiek
)

Write-Output "Cześć, $Imie!"
if ($Wiek -gt 0) {
    Write-Output "Za rok będziesz mieć $($Wiek + 1) lat."
}
```

```text
PS C:\PS_Lab> .\powitaj.ps1 -Imie Ada -Wiek 16
PS C:\PS_Lab> .\powitaj.ps1 Ada 16          # pozycyjnie — w kolejności z bloku param
PS C:\PS_Lab> .\powitaj.ps1                  # Imie = "nieznajomy", Wiek = 0
```

Parametry skryptu działają tak samo jak parametry cmdletów z lekcji 2: nazwane, pozycyjne, ze skróconą nazwą (`-I Ada`), z podpowiedzią po ++tab++.

### Parametr obowiązkowy, przełącznik, walidacja

```powershell
# kopia.ps1
param(
    [Parameter(Mandatory)]
    [string]$Zrodlo,

    [string]$Cel = "C:\PS_Lab\Kopie",

    [ValidateRange(1, 30)]
    [int]$Dni = 7,

    [switch]$Podglad
)

$granica = (Get-Date).AddDays(-$Dni)
$pliki = Get-ChildItem $Zrodlo -File | Where-Object LastWriteTime -ge $granica

if ($Podglad) {
    $pliki | Select-Object Name, LastWriteTime
}
else {
    New-Item -Path $Cel -ItemType Directory -Force | Out-Null
    $pliki | Copy-Item -Destination $Cel
    Write-Output "Skopiowano plików: $($pliki.Count)"
}
```

| Element | Działanie |
|---|---|
| `[Parameter(Mandatory)]` | parametr obowiązkowy — jeśli go brak, PowerShell sam o niego zapyta |
| `[string]`, `[int]` | typ — PowerShell zamieni argument na ten typ albo zgłosi błąd |
| `= "C:\PS_Lab\Kopie"` | wartość domyślna |
| `[ValidateRange(1, 30)]` | dozwolony zakres — `-Dni 50` zakończy się błędem przed wykonaniem skryptu |
| `[switch]` | przełącznik bez wartości: `-Podglad` ustawia go na `$true` |

```text
PS C:\PS_Lab> .\kopia.ps1 -Zrodlo C:\PS_Lab\Dane -Dni 3 -Podglad
```

## Obliczenia

```powershell
$a = 7
$b = 2
$a + $b          # 9
$a - $b          # 5
$a * $b          # 14
$a / $b          # 3,5 — dzielenie liczb całkowitych daje wynik z ułamkiem!
$a % $b          # 1 — reszta z dzielenia
[math]::Floor($a / $b)    # 3 — część całkowita
[math]::Pow(2, 10)        # 1024
[math]::Sqrt(81)          # 9
[math]::Round(2.567, 2)   # 2,57
[math]::Abs(-5)           # 5
```

!!! warning "Dzielenie w PowerShell, C# i cmd"
    W PowerShellu `7 / 2` daje `3,5`. W C# `7 / 2` daje `3` (dzielenie całkowite), a w `cmd` `set /a x=7/2` też daje `3`. Do części całkowitej w PowerShellu użyj `[math]::Floor()` (zaokrągla w dół) albo `[math]::Truncate()` (odcina ułamek).

### Zwiększanie i skrócone przypisania

```powershell
$licznik = 0
$licznik++              # 1
$licznik += 10          # 11
$licznik *= 2           # 22
```

### Liczba czy tekst

`Read-Host` zawsze zwraca **tekst**. Przed obliczeniami trzeba go zamienić na liczbę:

```powershell
$tekst = Read-Host "Podaj liczbę"
$liczba = [int]$tekst            # błąd, jeśli tekst nie jest liczbą całkowitą
$wynik = $liczba * 2

# bezpieczna wersja
$liczba = 0
if ([int]::TryParse($tekst, [ref]$liczba)) {
    Write-Output "Podwojona: $($liczba * 2)"
}
else {
    Write-Output "To nie jest liczba całkowita"
}
```

Kolejność ma znaczenie, bo PowerShell dopasowuje działanie do typu **lewej** strony:

```powershell
"5" + 3      # "53"  — tekst + liczba = sklejanie
5 + "3"      # 8     — liczba + tekst = dodawanie
"ab" * 3     # "ababab"
```

!!! warning "Rzutowanie [int] zaokrągla"
    `[int]2.5` daje `2`, a `[int]3.5` daje `4` — PowerShell zaokrągla do najbliższej **parzystej** liczby przy wartości dokładnie w połowie (tzw. zaokrąglanie bankierskie). Gdy potrzebujesz zawsze w dół — `[math]::Floor()`.

## Instrukcja switch

`switch` porównuje wartość z listą przypadków i wykonuje blok przy pasującym.

```powershell
$dzien = Read-Host "Numer dnia (1-7)"

switch ($dzien) {
    1 { Write-Output "poniedziałek" }
    2 { Write-Output "wtorek" }
    3 { Write-Output "środa" }
    4 { Write-Output "czwartek" }
    5 { Write-Output "piątek" }
    { $_ -in 6, 7 } { Write-Output "weekend" }
    default { Write-Output "błędny numer" }
}
```

- każdy przypadek to wartość i blok `{ }`,
- `default` wykonuje się, gdy nic nie pasuje,
- przypadek może być **warunkiem** w nawiasach klamrowych — `$_` to sprawdzana wartość,
- porównanie tekstu **nie rozróżnia wielkości liter** (chyba że dodasz `-CaseSensitive`),
- `Read-Host` zwraca tekst, ale `switch` sam porówna `"3"` z `3`.

### Różnica względem C#: sprawdzane są wszystkie przypadki

W PowerShellu `switch` **nie kończy się** na pierwszym dopasowaniu — sprawdza wszystkie przypadki i wykonuje każdy pasujący. Żeby zatrzymać się na pierwszym, użyj `break`:

```powershell
$punkty = 82
switch ($punkty) {
    { $_ -ge 90 } { "celujący"; break }
    { $_ -ge 75 } { "bardzo dobry"; break }
    { $_ -ge 60 } { "dobry"; break }
    { $_ -ge 50 } { "dostateczny"; break }
    default       { "niedostateczny" }
}
```

Bez `break` dla 82 punktów wypisałyby się trzy oceny: „bardzo dobry”, „dobry” i „dostateczny”.

### Wzorce: -Wildcard i -Regex

```powershell
$plik = "raport_2026.xlsx"
switch -Wildcard ($plik) {
    "*.txt"  { "plik tekstowy" }
    "*.xlsx" { "arkusz" }
    "*.ps1"  { "skrypt PowerShell" }
    default  { "inny plik" }
}
```

`-Wildcard` pozwala używać `*` i `?`, a `-Regex` — wyrażeń regularnych.

### switch dla wielu wartości

Gdy sprawdzana wartość jest tablicą, `switch` działa jak pętla — dla każdego elementu po kolei:

```powershell
$stany = (Get-Service).Status
$dziala = 0
$stoi = 0
switch ($stany) {
    "Running" { $dziala++ }
    "Stopped" { $stoi++ }
}
Write-Output "Działa: $dziala, zatrzymane: $stoi"
```

## Pozostałe pętle

```powershell
# for — licznik
for ($i = 1; $i -le 5; $i++) {
    Write-Output "Obrót $i"
}

# do-while — wykonuj, dopóki warunek jest prawdziwy (sprawdzany na końcu)
do {
    $odp = Read-Host "Podaj liczbę od 1 do 10"
} while (-not ($odp -match '^\d+$' -and [int]$odp -ge 1 -and [int]$odp -le 10))

# do-until — wykonuj, aż warunek stanie się prawdziwy
do {
    $haslo = Read-Host "Hasło"
} until ($haslo -eq "tajne")
```

| Pętla | Kiedy kończy | Ile razy co najmniej |
|---|---|---|
| `while (warunek)` | gdy warunek fałszywy | 0 |
| `do { } while (warunek)` | gdy warunek fałszywy | 1 |
| `do { } until (warunek)` | gdy warunek **prawdziwy** | 1 |
| `for (start; warunek; krok)` | gdy warunek fałszywy | 0 |
| `foreach ($x in kolekcja)` | po ostatnim elemencie | 0 |

`break` kończy pętlę, `continue` przechodzi do następnego obrotu.

## Przykład: kalkulator z parametrami

```powershell
# kalk.ps1
param(
    [Parameter(Mandatory)][double]$A,
    [Parameter(Mandatory)][double]$B,
    [ValidateSet("dodaj", "odejmij", "pomnoz", "podziel", "potega")]
    [string]$Dzialanie = "dodaj"
)

switch ($Dzialanie) {
    "dodaj"   { $wynik = $A + $B }
    "odejmij" { $wynik = $A - $B }
    "pomnoz"  { $wynik = $A * $B }
    "podziel" {
        if ($B -eq 0) {
            Write-Warning "Nie można dzielić przez zero"
            exit 1
        }
        $wynik = $A / $B
    }
    "potega"  { $wynik = [math]::Pow($A, $B) }
}

Write-Output "$A $Dzialanie $B = $([math]::Round($wynik, 2))"
```

```text
PS C:\PS_Lab> .\kalk.ps1 -A 7 -B 2 -Dzialanie podziel
7 podziel 2 = 3.5
PS C:\PS_Lab> .\kalk.ps1 7 2 potega
7 potega 2 = 49
```

Liczby wstawione do tekstu w cudzysłowie (`"$wynik"`) PowerShell zawsze zapisuje z kropką, niezależnie od ustawień regionalnych — dlatego wynik to `3.5`, a nie `3,5`.

`[ValidateSet(...)]` dopuszcza tylko podane wartości, a ++tab++ podpowiada je przy wpisywaniu. `exit 1` kończy skrypt z kodem błędu (odczytasz go w `$LASTEXITCODE`).

## Przykład: menu administracyjne

```powershell
# menu.ps1
do {
    Write-Output ""
    Write-Output "1 - informacje o systemie"
    Write-Output "2 - pięć procesów z największą pamięcią"
    Write-Output "3 - zatrzymane usługi automatyczne"
    Write-Output "0 - koniec"
    $wybor = Read-Host "Wybierz"

    switch ($wybor) {
        "1" {
            Write-Output "Komputer: $env:COMPUTERNAME, użytkownik: $env:USERNAME"
            Write-Output "PowerShell: $($PSVersionTable.PSVersion)"
        }
        "2" {
            Get-Process | Sort-Object WorkingSet -Descending |
                Select-Object -First 5 Name, @{ Name = 'MB'; Expression = { [math]::Round($_.WorkingSet / 1MB, 1) } } |
                Format-Table -AutoSize
        }
        "3" {
            Get-Service | Where-Object { $_.StartType -eq "Automatic" -and $_.Status -eq "Stopped" } |
                Select-Object Name, DisplayName
        }
        "0" { Write-Output "Do widzenia" }
        default { Write-Warning "Nieznana opcja: $wybor" }
    }
} while ($wybor -ne "0")
```

## Zadania

??? question "Zadanie 1: Zmienne systemowe"
    Napisz skrypt, który wypisuje nazwę użytkownika, nazwę komputera, folder profilu i folder Windows, korzystając wyłącznie ze zmiennych środowiskowych. Porównaj z plikiem `info.bat` z poprzedniej lekcji.

??? question "Zadanie 2: Prostokąt z parametrami"
    Skrypt `prostokat.ps1` przyjmuje obowiązkowe parametry `-A` i `-B` (typ `[double]`, zakres 0,1–1000) i przełącznik `-Szczegoly`. Wypisuje pole, a z przełącznikiem także obwód i długość przekątnej (zaokrągloną do 2 miejsc).

??? question "Zadanie 3: Ocena przez switch"
    Skrypt przyjmuje parametr `-Punkty` (0–100) i wypisuje ocenę, korzystając ze `switch` z warunkami. Sprawdź, co wypisze dla 95 punktów **bez** `break` i z `break`, i wyjaśnij różnicę.

??? question "Zadanie 4: Sortowanie plików"
    Skrypt przechodzi po plikach w podanym folderze i za pomocą `switch -Wildcard` przenosi je do podfolderów `Dokumenty` (`*.docx`, `*.pdf`, `*.txt`), `Obrazy` (`*.png`, `*.jpg`), `Skrypty` (`*.ps1`, `*.bat`) i `Inne`. Najpierw uruchom go z przełącznikiem `-Podglad`, który tylko wypisuje, co zostałoby przeniesione.

??? question "Zadanie 5: Zgadywanka"
    Skrypt losuje liczbę od 1 do 100 (`Get-Random -Minimum 1 -Maximum 101`) i pozwala ją zgadywać w pętli `do-until`. Niepoprawne wpisy (nie liczby) są pomijane z komunikatem. Na końcu wypisuje liczbę prób.

??? question "Zadanie 6: Kalkulator z menu"
    Połącz kalkulator i menu: skrypt w pętli pyta o działanie (`switch`), dwie liczby (z bezpieczną konwersją) i wypisuje wynik, aż użytkownik wybierze koniec.
