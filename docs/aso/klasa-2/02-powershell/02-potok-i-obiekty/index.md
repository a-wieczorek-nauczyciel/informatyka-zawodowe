# Potok i obiekty

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć przekazywać wynik jednego polecenia do drugiego, wybierać z wyniku potrzebne właściwości, filtrować i sortować dane oraz zapisywać wynik do pliku.

## Wynik to obiekt, nie tekst

Uruchom `Get-Process`. Na ekranie widać tabelę — ale to tylko **sposób wyświetlenia**. Pod spodem każdy wiersz to obiekt: proces z kilkudziesięcioma właściwościami (nazwa, identyfikator, zużycie pamięci, czas startu, ścieżka do pliku…). Domyślnie widać tylko kilka z nich.

Jakie właściwości ma obiekt? Sprawdzisz to poleceniem `Get-Member`:

```powershell
Get-Process | Get-Member -MemberType Property
```

## Potok `|`

Znak `|` (pipe) przekazuje obiekty z lewej strony do polecenia po prawej. Łańcuch poleceń połączonych potokiem nazywamy **potokiem** (*pipeline*):

```powershell
Get-Service | Where-Object Status -eq 'Running' | Sort-Object DisplayName
```

Czytamy od lewej: *pobierz usługi → zostaw tylko uruchomione → posortuj po nazwie wyświetlanej*.

## Cztery cmdlety, których użyjesz najczęściej

### Where-Object — filtrowanie wierszy

```powershell
Get-Process | Where-Object CPU -gt 10
Get-ChildItem C:\Windows | Where-Object Length -gt 1MB
Get-LocalUser | Where-Object Enabled -eq $false
```

Operatory porównania w PowerShell to słowa, nie symbole:

| Operator | Znaczenie |
|---|---|
| `-eq` / `-ne` | równe / różne |
| `-gt` / `-ge` | większe / większe lub równe |
| `-lt` / `-le` | mniejsze / mniejsze lub równe |
| `-like` | pasuje do wzorca z `*` (np. `-like 'win*'`) |
| `-match` | pasuje do wyrażenia regularnego |

Gdy warunków jest więcej, używa się pełnej składni z blokiem `{ }`, w którym `$_` oznacza „bieżący obiekt”:

```powershell
Get-Process | Where-Object { $_.CPU -gt 10 -and $_.Name -like 'm*' }
```

### Select-Object — wybór kolumn i liczby wyników

```powershell
Get-Process | Select-Object Name, Id, WorkingSet
Get-Process | Select-Object -First 5
```

### Sort-Object — sortowanie

```powershell
Get-Process | Sort-Object WorkingSet -Descending
```

### Measure-Object — liczenie i sumowanie

```powershell
Get-Service | Measure-Object                          # ile jest usług
Get-ChildItem C:\Windows -File | Measure-Object Length -Sum   # łączny rozmiar
```

## Formatowanie i zapis wyników

```powershell
Get-Service | Format-Table Name, Status -AutoSize   # tabela
Get-Service | Format-List *                         # wszystkie właściwości, pionowo
Get-Service | Out-GridView                          # okno z filtrowaniem (tylko GUI)
```

!!! warning "Format-* zawsze na końcu potoku"
    `Format-Table` i `Format-List` zamieniają obiekty w gotowy do wyświetlenia tekst. Po nich nie da się już sortować ani filtrować. Formatowanie to ostatni krok.

Zapis do pliku:

```powershell
Get-Process | Out-File C:\raporty\procesy.txt                     # jak na ekranie
Get-Process | Select-Object Name, Id, WorkingSet |
    Export-Csv C:\raporty\procesy.csv -NoTypeInformation -Encoding UTF8   # do Excela
Import-Csv C:\raporty\procesy.csv                                  # wczytanie z powrotem
```

Plik CSV zachowuje strukturę danych — po `Import-Csv` znów masz obiekty, które możesz filtrować.

## Zadania

??? question "Zadanie 1: Co kryje obiekt"
    Wywołaj `Get-Service | Get-Member`. Znajdź trzy właściwości, których domyślnie nie widać w wyniku `Get-Service`, i wyświetl je dla wszystkich usług przy pomocy `Select-Object`.

??? question "Zadanie 2: Pięciu największych"
    Wyświetl pięć procesów zużywających najwięcej pamięci (`WorkingSet`), pokazując tylko ich nazwę i zużycie pamięci.

??? question "Zadanie 3: Usługi zatrzymane, ale automatyczne"
    Wyświetl usługi, które mają typ uruchamiania `Automatic`, a mimo to są zatrzymane. Wskazówka: właściwość typu uruchamiania znajdziesz przez `Get-Member`.

??? question "Zadanie 4: Duże pliki"
    Znajdź w `C:\Windows\System32` pliki większe niż 10 MB. Wyświetl ich nazwy i rozmiary, posortowane malejąco.

??? question "Zadanie 5: Raport do CSV"
    Wyeksportuj listę uruchomionych usług (nazwa, nazwa wyświetlana, typ uruchamiania) do pliku `uslugi.csv` na pulpicie. Otwórz plik w Notatniku i sprawdź, czy polskie znaki w nazwach wyświetlanych są poprawne.

??? question "Zadanie 6: Dlaczego to nie działa?"
    Uruchom polecenie:

    ```powershell
    Get-Process | Format-Table Name, CPU | Sort-Object CPU
    ```

    Opisz, co się stało, wyjaśnij przyczynę i popraw polecenie.
