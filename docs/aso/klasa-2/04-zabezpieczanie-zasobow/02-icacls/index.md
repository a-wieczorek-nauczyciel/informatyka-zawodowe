# icacls — uprawnienia z wiersza poleceń

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć odczytać uprawnienia NTFS poleceniem `icacls`, nadać, odebrać i odmówić uprawnienia, sterować dziedziczeniem oraz zapisać i odtworzyć uprawnienia folderu.

## Po co icacls, skoro jest GUI

Nadanie uprawnień jednemu folderowi w GUI to kilka kliknięć. Nadanie ich trzydziestu folderom uczniów — pół godziny. `icacls` zrobi to jedną linijką w skrypcie, a przy okazji pozwala zrobić kopię uprawnień przed ryzykowną zmianą.

`icacls` działa w `cmd` i w PowerShellu. Zmiany wymagają okna administratora.

## Odczyt uprawnień

```text
C:\> icacls C:\Firma\Kadry
C:\Firma\Kadry BUILTIN\Administrators:(I)(OI)(CI)(F)
               NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
               PC01\Kadry:(OI)(CI)(M)
               BUILTIN\Users:(I)(OI)(CI)(RX)
```

Każda linia to wpis: **kto** i w nawiasach **co**.

| Skrót | Znaczenie |
|---|---|
| `F` | Full control |
| `M` | Modify |
| `RX` | Read & execute |
| `R` | Read |
| `W` | Write |
| `D` | Delete |
| `(I)` | uprawnienie **odziedziczone** |
| `(OI)` | *object inherit* — dziedziczą je **pliki** w folderze |
| `(CI)` | *container inherit* — dziedziczą je **podfoldery** |

`(OI)(CI)` razem oznacza: „to uprawnienie obowiązuje w całym folderze, łącznie z podfolderami i plikami”.

## Nadawanie i odbieranie

```powershell
icacls C:\Firma\Kadry /grant "Kadry:(OI)(CI)M"        # nadaj grupie Kadry Modify dla całego folderu
icacls C:\Firma\Kadry /grant "Ksiegowosc:(OI)(CI)RX"  # odczyt dla księgowości
icacls C:\Firma\Kadry /deny "anowak:(OI)(CI)W"        # odmów zapisu kontu anowak
icacls C:\Firma\Kadry /remove anowak                  # usuń wszystkie wpisy dla anowak
icacls C:\Firma\Kadry /remove:d anowak                # usuń tylko wpisy Deny
icacls C:\Firma\Kadry /grant:r "Ksiegowosc:(OI)(CI)R" # zastąp dotychczasowe uprawnienia (r = replace)
```

!!! warning "Nawiasy w PowerShellu"
    W PowerShellu nawiasy mają specjalne znaczenie. Wpis z nawiasami ujmij w cudzysłów:

    ```powershell
    icacls C:\Firma\Kadry /grant "Kadry:(OI)(CI)M"
    ```

## Dziedziczenie

```powershell
icacls C:\Firma\Kadry\Archiwum /inheritance:d   # wyłącz dziedziczenie, skopiuj dotychczasowe wpisy
icacls C:\Firma\Kadry\Archiwum /inheritance:r   # wyłącz dziedziczenie i usuń odziedziczone wpisy
icacls C:\Firma\Kadry\Archiwum /inheritance:e   # włącz dziedziczenie z powrotem
icacls C:\Firma\Kadry /reset /T                 # przywróć domyślne, odziedziczone uprawnienia (cały folder)
```

Parametr `/T` wykonuje operację **rekurencyjnie** — na wszystkich plikach i podfolderach.

## Kopia uprawnień

Przed dużą zmianą zapisz uprawnienia do pliku, żeby móc je odtworzyć:

```text
icacls C:\Firma\* /save C:\Kopie\uprawnienia-firma.txt /T
icacls C:\Firma\ /restore C:\Kopie\uprawnienia-firma.txt
```

Przy `/restore` podajesz folder **nadrzędny** względem folderów zapisanych w kopii.

## Własność

```powershell
takeown /F C:\Firma\Stary /R /D Y          # przejmij własność folderu i całej zawartości
icacls C:\Firma\Stary /setowner Administrators /T
```

## Ten sam przykład w GUI i w icacls

| Cel | GUI | icacls |
|---|---|---|
| grupa `Kadry` może zmieniać pliki w folderze | Security → Edit → Add `Kadry` → Modify | `/grant Kadry:(OI)(CI)M` |
| `Archiwum` nie dziedziczy | Advanced → Disable inheritance → Convert | `/inheritance:d` |
| `anowak` nie może zapisywać | Security → Edit → `anowak` → Deny Write | `/deny anowak:(OI)(CI)W` |

## Zadania

??? question "Zadanie 1: Odczytaj i rozszyfruj"
    Wyświetl uprawnienia do `C:\Users` poleceniem `icacls`. Dla każdego wpisu zapisz słownie: kto, jakie uprawnienie, czy odziedziczone, czy dziedziczą je pliki i podfoldery.

??? question "Zadanie 2: Folder działu z wiersza poleceń"
    Odtwórz zadanie 2 z lekcji o uprawnieniach NTFS (folder `C:\Firma\Kadry`), tym razem wyłącznie poleceniami `icacls`. Porównaj wynik z GUI.

??? question "Zadanie 3: Foldery uczniów"
    Utwórz w PowerShellu foldery `C:\Prace\uczen1` … `C:\Prace\uczen5` i konta `uczen1` … `uczen5`. Napisz pętlę, która każdemu uczniowi nada *Modify* tylko do jego własnego folderu. Sprawdź, czy `uczen1` nie może otworzyć folderu `uczen2`. Wskazówka: najpierw wyłącz dziedziczenie z usunięciem wpisów grupy `Users`.

??? question "Zadanie 4: Kopia i odtworzenie"
    Zapisz uprawnienia `C:\Firma` do pliku. Celowo zepsuj uprawnienia (np. usuń wpis grupy `Kadry`). Odtwórz je z kopii i pokaż, że wszystko wróciło.

??? question "Zadanie 5: Odziedziczone czy jawne"
    Po wykonaniu zadania 2 wyświetl uprawnienia do pliku utworzonego w `C:\Firma\Kadry`. Które wpisy mają znacznik `(I)` i dlaczego?
