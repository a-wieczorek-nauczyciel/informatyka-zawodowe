# Kopie bezpieczeństwa

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć rozróżnić rodzaje kopii bezpieczeństwa, zaplanować politykę kopii według reguły 3-2-1, utworzyć punkt przywracania, skonfigurować historię plików, wykonać obraz systemu i kopię katalogów poleceniem `robocopy` oraz przywrócić z nich dane.

## Po co kopie

Awaria dysku, ransomware, przypadkowe usunięcie folderu, zalanie laptopa — w każdym z tych przypadków odzyskasz dane tylko wtedy, gdy masz **kopię zrobioną wcześniej** i trzymaną **w innym miejscu**.

!!! warning "Synchronizacja to nie kopia"
    Usługa typu OneDrive czy Dropbox synchronizuje zmiany: usunięty lub zaszyfrowany plik zostanie usunięty lub zaszyfrowany także w chmurze. Kopia zapasowa przechowuje **stan z przeszłości**.

## Rodzaje kopii

![Kopia pełna, przyrostowa i różnicowa w kolejnych dniach](../../../../assets/img/aso/windows/win-kopie.svg)

| Rodzaj | Co kopiuje | Zaleta | Wada |
|---|---|---|---|
| **pełna (full)** | wszystkie wybrane dane | przywracanie z jednej kopii | najwięcej miejsca i czasu |
| **przyrostowa (incremental)** | zmiany od **ostatniej kopii dowolnego rodzaju** | najszybsza, najmniej miejsca | do przywrócenia potrzebna pełna + **wszystkie** kolejne przyrostowe |
| **różnicowa (differential)** | zmiany od **ostatniej kopii pełnej** | do przywrócenia pełna + **jedna** ostatnia różnicowa | rośnie z każdym dniem |

Typowa praktyka: kopia pełna raz w tygodniu, przyrostowa lub różnicowa codziennie.

## Reguła 3-2-1

![Reguła 3-2-1](../../../../assets/img/aso/windows/win-3-2-1.svg)

- **3** kopie danych (oryginał i dwie kopie),
- na **2** różnych nośnikach,
- z czego **1** poza miejscem pracy.

Do tego: **sprawdzaj, czy z kopii da się przywrócić dane**. Kopia, której nikt nie przetestował, jest tylko nadzieją.

## Co i czym kopiować w Windows

| Co chronisz | Narzędzie |
|---|---|
| ustawienia i pliki systemowe (cofnięcie nieudanej instalacji) | **punkt przywracania** (System Restore) |
| pliki użytkownika w kolejnych wersjach | **historia plików** (File History) |
| cały system — do odtworzenia po awarii dysku | **obraz systemu** (System Image) |
| wybrane foldery, np. na dysk sieciowy | **robocopy** albo skrypt |

### Punkt przywracania

Zapisuje stan plików systemowych, sterowników i rejestru — **nie** chroni dokumentów użytkownika.

1. ++win+r++ → `sysdm.cpl` → karta **System Protection (Ochrona systemu)**.
2. Wybierz dysk `C:` → **Configure** → **Turn on system protection** → ustaw miejsce na dysku.
3. **Create…** → opis punktu → **Create**.
4. Przywracanie: **System Restore…** (albo `rstrui`) → wybierz punkt → **Finish**.

```powershell
Enable-ComputerRestore -Drive "C:\"
Checkpoint-Computer -Description "Przed instalacja sterownika"
Get-ComputerRestorePoint
```

### Historia plików (File History)

Co godzinę (domyślnie) zapisuje nowe wersje plików z bibliotek, pulpitu i ulubionych na **innym dysku**.

**Control Panel → System and Security → File History** → wybierz dysk (**Select drive**) → **Turn on**. **Advanced settings** — jak często i jak długo trzymać wersje.

Przywracanie: **Restore personal files** albo prawy przycisk na pliku → **Properties → Previous Versions**.

### Obraz systemu

Kopia **całych woluminów** (systemowego i rozruchowego) — pozwala odtworzyć system na nowym dysku.

**Control Panel → Backup and Restore (Windows 7) → Create a system image** → miejsce docelowe (inny dysk, DVD lub udział sieciowy) → **Start backup**.

```powershell
wbadmin start backup -backupTarget:E: -include:C: -allCritical -quiet
wbadmin get versions
```

Odtworzenie wykonuje się ze środowiska odzyskiwania: **Settings → System → Recovery → Advanced startup → Troubleshoot → Advanced options → System Image Recovery**.

### Kopia katalogów: robocopy

```powershell
robocopy C:\Firma E:\Kopia\Firma /E                 # kopiuj z podfolderami
robocopy C:\Firma E:\Kopia\Firma /MIR /R:2 /W:5     # lustro: kopia identyczna z źródłem
robocopy C:\Firma E:\Kopia\Firma /E /COPY:DATS      # z uprawnieniami NTFS (S)
robocopy C:\Firma E:\Kopia\Firma /E /LOG:E:\kopia.log
```

| Parametr | Znaczenie |
|---|---|
| `/E` | kopiuj podfoldery, także puste |
| `/MIR` | lustro: kopiuj i **usuń w celu** pliki, których nie ma w źródle |
| `/COPY:DATS` | dane, atrybuty, znaczniki czasu i uprawnienia NTFS |
| `/R:2 /W:5` | ponów 2 razy co 5 sekund przy błędzie |
| `/LOG:plik` | zapisz raport do pliku |

!!! warning "Uważaj na /MIR"
    `/MIR` usuwa w folderze docelowym wszystko, czego nie ma w źródle. Pomyłka w kolejności ścieżek (źródło ↔ cel) wyczyści dane. Najpierw uruchom z parametrem `/L` — pokaże, co by zrobił, bez wykonywania.

Prosty skrypt kopii z datą w nazwie:

```powershell
$data = Get-Date -Format "yyyy-MM-dd"
Compress-Archive -Path C:\Firma -DestinationPath "E:\Kopia\Firma-$data.zip"
```

## Zadania

??? question "Zadanie 1: Punkt przywracania w praktyce"
    Włącz ochronę systemu na `C:` i utwórz punkt przywracania „przed zmianami”. Zainstaluj dowolny mały program, a potem przywróć system do utworzonego punktu. Sprawdź, czy program zniknął, a pliki na pulpicie zostały. Wyjaśnij wynik.

??? question "Zadanie 2: Historia plików"
    Dodaj maszynie drugi dysk i włącz na nim historię plików. Utwórz dokument, zapisz trzy różne wersje w odstępach (wymuś kopię przyciskiem **Run now**). Przywróć pierwszą wersję.

??? question "Zadanie 3: robocopy"
    Wykonaj kopię `C:\Firma` na dysk `E:` z uprawnieniami NTFS i logiem. Usuń jeden plik w źródle i uruchom `robocopy /MIR` najpierw z `/L`, potem naprawdę. Co stało się w kopii?

??? question "Zadanie 4: Pełna, przyrostowa czy różnicowa"
    Kopia pełna jest w niedzielę, przyrostowe codziennie. W czwartek rano dysk ulega awarii. Które kopie są potrzebne do odtworzenia danych? Odpowiedz też dla wariantu z kopiami różnicowymi.

??? question "Zadanie 5: Polityka kopii dla firmy"
    Mała firma ma jeden komputer z dokumentami księgowymi, zewnętrzny dysk USB i dostęp do chmury. Zaprojektuj politykę kopii zgodną z regułą 3-2-1: co, czym, jak często, gdzie i jak sprawdzisz, że kopia działa.
