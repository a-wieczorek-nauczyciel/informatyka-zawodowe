# Pierwsze skrypty

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zapisać ciąg poleceń w pliku `.ps1`, uruchomić go, a w skrypcie użyć zmiennych, instrukcji warunkowej i pętli do zautomatyzowania powtarzalnej czynności administracyjnej.

## Skrypt to zapisany ciąg poleceń

Skrypt PowerShell to zwykły plik tekstowy z rozszerzeniem `.ps1`. Do pisania używaj **Windows PowerShell ISE** (wbudowany edytor z podświetlaniem składni) lub **Visual Studio Code** z rozszerzeniem PowerShell.

Uruchomienie skryptu z bieżącego katalogu:

```powershell
.\raport.ps1
```

Prefiks `.\` jest obowiązkowy — PowerShell celowo nie uruchamia skryptów z bieżącego folderu po samej nazwie, żeby nie dało się podsunąć złośliwego pliku o nazwie znanego polecenia.

## Polityka wykonywania skryptów

Przy pierwszej próbie uruchomienia skryptu zobaczysz najpewniej błąd: *running scripts is disabled on this system*. To **ExecutionPolicy** — ustawienie, które określa, czy i jakie skrypty wolno uruchamiać.

| Polityka | Znaczenie |
|---|---|
| `Restricted` | żadnych skryptów (domyślna na Windows 10/11) |
| `RemoteSigned` | lokalne skrypty tak, pobrane z internetu tylko podpisane (domyślna na Windows Server) |
| `AllSigned` | tylko skrypty podpisane cyfrowo |
| `Unrestricted` / `Bypass` | wszystko — niezalecane |

```powershell
Get-ExecutionPolicy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

!!! warning "ExecutionPolicy nie jest zabezpieczeniem przed włamaniem"
    Ta polityka chroni przed **przypadkowym** uruchomieniem skryptu, a nie przed świadomym atakującym. Nie ustawiaj `Unrestricted` „żeby działało” — `RemoteSigned` w zakresie bieżącego użytkownika w zupełności wystarczy.

## Zmienne

Zmienne zaczynają się od `$`. Nie trzeba deklarować typu.

```powershell
$folder = "C:\Raporty"
$liczba = 5
$uslugi = Get-Service              # zmienna może przechowywać wynik polecenia
$uslugi.Count                      # ile elementów
"Zapisuję raport do $folder"       # w cudzysłowach " " zmienne są podstawiane
'Zapisuję raport do $folder'       # w apostrofach ' ' — nie
```

Pobranie danych od użytkownika:

```powershell
$imie = Read-Host "Podaj imię"
```

## Instrukcja warunkowa

```powershell
if (Test-Path "C:\Raporty") {
    Write-Host "Folder istnieje"
} else {
    New-Item -Path "C:\Raporty" -ItemType Directory | Out-Null
    Write-Host "Utworzono folder"
}
```

## Pętla foreach

```powershell
$nazwy = "Kadry", "Ksiegowosc", "IT"

foreach ($n in $nazwy) {
    New-Item -Path "C:\Firma\$n" -ItemType Directory
}
```

Pętla po wierszach pliku CSV — najczęstszy schemat w administracji:

```powershell
# plik uczniowie.csv:
# Login,Imie,Nazwisko
# akowal,Anna,Kowalska
# bnowak,Bartek,Nowak

$haslo = Read-Host -AsSecureString "Hasło startowe"

foreach ($u in Import-Csv .\uczniowie.csv) {
    New-LocalUser -Name $u.Login -FullName "$($u.Imie) $($u.Nazwisko)" -Password $haslo
    Write-Host "Utworzono konto $($u.Login)"
}
```

Zapis `$($u.Imie)` jest potrzebny, gdy wewnątrz tekstu odwołujemy się do **właściwości** obiektu.

## Komentarze

```powershell
# komentarz jednoliniowy

<#
  Komentarz wieloliniowy — dobry na opis
  skryptu: co robi, kto napisał, kiedy.
#>
```

## Zadania

??? question "Zadanie 1: Pierwszy skrypt"
    Napisz skrypt `info.ps1`, który wyświetla nazwę komputera, nazwę zalogowanego użytkownika, bieżącą datę i adres IPv4. Każda informacja w osobnej linii, z opisem (np. `Komputer: PC-LAB-01`). Uruchom go i udokumentuj, jaką politykę wykonywania skryptów musiałeś ustawić.

??? question "Zadanie 2: Folder raportów"
    Napisz skrypt, który sprawdza, czy istnieje folder `C:\Raporty`. Jeśli nie — tworzy go. Następnie zapisuje do niego plik CSV z listą uruchomionych usług, w którego nazwie jest bieżąca data (np. `uslugi-2026-10-12.csv`). Wskazówka: `Get-Date -Format yyyy-MM-dd`.

??? question "Zadanie 3: Konta z pliku CSV"
    Przygotuj plik CSV z pięcioma kontami (login, imię, nazwisko). Napisz skrypt, który tworzy te konta i dodaje je do grupy `Uczniowie` (skrypt sam tworzy grupę, jeśli jej nie ma). Uruchom skrypt dwa razy i opisz, co się dzieje przy drugim uruchomieniu.

??? question "Zadanie 4: Popraw skrypt z zadania 3"
    Zmodyfikuj skrypt tak, aby przed utworzeniem konta sprawdzał, czy konto o tym loginie już istnieje, i w takim przypadku wypisywał komunikat zamiast błędu. Wskazówka: `Get-LocalUser -Name ... -ErrorAction SilentlyContinue`.

??? question "Zadanie 5: Strażnik usługi"
    Napisz skrypt, który sprawdza stan usługi podanej przez użytkownika (`Read-Host`). Jeśli usługa jest zatrzymana — uruchamia ją i wypisuje komunikat. Jeśli taka usługa nie istnieje — wypisuje czytelny komunikat o błędzie.
