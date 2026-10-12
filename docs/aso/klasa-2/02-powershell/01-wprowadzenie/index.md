# Wprowadzenie do PowerShell

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć uruchomić PowerShell (także jako administrator), rozpoznać budowę cmdletu, samodzielnie wyszukać polecenie do danego zadania i odczytać jego pomoc.

## Dlaczego wiersz poleceń, skoro jest GUI

Utworzenie jednego konta użytkownika w oknie „Zarządzanie komputerem” zajmuje minutę. Utworzenie trzydziestu kont dla nowej klasy — pół godziny klikania i ryzyko literówki w co którymś. W PowerShell to samo zadanie to kilka linijek, które można zapisać, sprawdzić i uruchomić ponownie na innym komputerze.

Administrator serwerów pracuje w terminalu, bo:

- polecenia da się **powtórzyć** i **zautomatyzować**,
- da się je wykonać **zdalnie**, na wielu maszynach naraz,
- Windows Server bywa instalowany **bez GUI** (wersja Server Core) — tam terminal jest jedynym narzędziem.

## PowerShell a cmd

| | `cmd.exe` | PowerShell |
|---|---|---|
| Wynik polecenia | tekst | **obiekty** (z właściwościami i metodami) |
| Nazwy poleceń | krótkie, niespójne (`dir`, `xcopy`, `net user`) | spójny schemat `Czasownik-Rzeczownik` |
| Wbudowana pomoc | `polecenie /?` | `Get-Help` z przykładami |
| Skrypty | `.bat` | `.ps1` |

W systemie znajdziesz dwie wersje: **Windows PowerShell 5.1** (wbudowany, niebieskie okno) i **PowerShell 7** (instalowany osobno, polecenie `pwsh`). Na zajęciach używamy wbudowanego 5.1 — jest na każdym Windows i Windows Server.

!!! warning "Uruchom jako administrator"
    Wiele poleceń administracyjnych (konta, usługi, role serwera) wymaga uprawnień administratora. Uruchamiaj PowerShell przez prawy przycisk → **Uruchom jako administrator**. W pasku tytułu okna pojawi się wtedy słowo *Administrator*.

## Budowa cmdletu

Polecenia PowerShell nazywają się **cmdletami** (czyt. *komandlet*). Każdy ma nazwę w schemacie:

```text
Czasownik-Rzeczownik   →   Get-Process, Stop-Service, New-LocalUser
```

Najczęstsze czasowniki:

| Czasownik | Znaczenie |
|---|---|
| `Get` | pobierz, wyświetl |
| `Set` | zmień ustawienie |
| `New` | utwórz nowe |
| `Remove` | usuń |
| `Start` / `Stop` / `Restart` | uruchom / zatrzymaj / uruchom ponownie |
| `Add` | dodaj do czegoś istniejącego |

Cmdlety przyjmują **parametry** poprzedzone myślnikiem:

```powershell
Get-ChildItem -Path C:\Windows -Filter *.exe
```

!!! tip "Tabulator to Twój najlepszy przyjaciel"
    Wpisz `Get-Chi` i naciśnij ++tab++ — PowerShell uzupełni nazwę. Działa to też dla parametrów (`-Pa` + ++tab++) i ścieżek. Mniej pisania, zero literówek.

## Jak znaleźć polecenie: Get-Command

Nie musisz znać wszystkich cmdletów na pamięć. Wystarczy wiedzieć, jak je znaleźć:

```powershell
Get-Command -Noun Service          # wszystko, co działa na usługach
Get-Command -Verb Get -Noun *User* # wszystkie "Get" dotyczące użytkowników
Get-Command *ip*                   # wszystko, co ma "ip" w nazwie
```

## Jak użyć polecenia: Get-Help

```powershell
Get-Help Get-Service               # skrócony opis i składnia
Get-Help Get-Service -Examples     # gotowe przykłady użycia
Get-Help Get-Service -Online       # pełna dokumentacja w przeglądarce
```

Przy pierwszym użyciu pomoc może być niepełna — pełne pliki pomocy pobiera polecenie `Update-Help` (uruchomione jako administrator, wymaga internetu).

## Aliasy

Część cmdletów ma krótkie nazwy zastępcze, często znane z `cmd` lub Linuksa:

| Alias | Cmdlet |
|---|---|
| `dir`, `ls`, `gci` | `Get-ChildItem` |
| `cd` | `Set-Location` |
| `cls`, `clear` | `Clear-Host` |
| `copy`, `cp` | `Copy-Item` |
| `del`, `rm` | `Remove-Item` |
| `cat`, `type` | `Get-Content` |

```powershell
Get-Alias ls        # czym naprawdę jest "ls"?
Get-Alias -Definition Get-ChildItem   # jakie aliasy ma ten cmdlet?
```

!!! warning "Aliasy w terminalu — tak, w skryptach — nie"
    Aliasy przyspieszają pisanie na bieżąco, ale w skryptach używaj pełnych nazw cmdletów. Skrypt ma być czytelny dla kogoś, kto otworzy go za rok.

## Zadania

??? question "Zadanie 1: Rozpoznaj czasowniki"
    Bez uruchamiania komputera określ, co robi każde z poleceń: `Get-LocalUser`, `New-Item`, `Restart-Computer`, `Remove-LocalGroupMember`, `Set-Date`. Następnie sprawdź swoje odpowiedzi poleceniem `Get-Help`.

??? question "Zadanie 2: Znajdź polecenie"
    Za pomocą `Get-Command` znajdź cmdlet, który:

    1. wyświetla adresy IP komputera,
    2. zmienia nazwę komputera,
    3. wyświetla zainstalowane aktualizacje (*hotfix*).

    Zapisz polecenie, którym szukałeś, i znaleziony cmdlet.

??? question "Zadanie 3: Pomoc z przykładami"
    Wyświetl przykłady użycia `Get-ChildItem`. Na ich podstawie napisz polecenie, które wyświetli **tylko pliki** (bez folderów) z katalogu `C:\Windows`, a następnie polecenie, które przeszuka ten katalog **rekurencyjnie** w poszukiwaniu plików `*.log`.

??? question "Zadanie 4: Alias czy cmdlet"
    Sprawdź, czym są aliasy: `gsv`, `ps`, `h`, `md`. Dla każdego zapisz pełną nazwę cmdletu i krótko opisz, co robi.

??? question "Zadanie 5: cmd kontra PowerShell"
    Uruchom `ipconfig` w `cmd`, a potem `Get-NetIPAddress` w PowerShell. Opisz, czym różni się sposób prezentacji wyniku. Który z nich łatwiej przefiltrować, żeby zostawić tylko adresy IPv4? Uzasadnij.
