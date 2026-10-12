# Pliki, użytkownicy, usługi

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć z poziomu PowerShell tworzyć i porządkować pliki i foldery, zarządzać kontami i grupami lokalnymi, kontrolować procesy i usługi oraz sprawdzić konfigurację sieci.

!!! warning "Pracuj na maszynie wirtualnej"
    Ćwiczenia z tej lekcji wykonuj na maszynie wirtualnej z Windows, nie na komputerze hosta. Przed rozpoczęciem zrób **snapshot** — usunięcie złego folderu czy wyłączenie złej usługi da się wtedy cofnąć jednym kliknięciem.

## Pliki i foldery

```powershell
Set-Location C:\                                   # przejdź do katalogu (cd)
New-Item -Path C:\Lab -ItemType Directory          # utwórz folder
New-Item -Path C:\Lab\notatka.txt -ItemType File   # utwórz pusty plik
Set-Content C:\Lab\notatka.txt "Pierwsza linia"    # zapisz (nadpisuje)
Add-Content C:\Lab\notatka.txt "Druga linia"       # dopisz na końcu
Get-Content C:\Lab\notatka.txt                     # wyświetl zawartość
Copy-Item C:\Lab\notatka.txt C:\Lab\kopia.txt      # kopiuj
New-Item -Path C:\Lab\Archiwum -ItemType Directory
Move-Item C:\Lab\kopia.txt C:\Lab\Archiwum\        # przenieś
Rename-Item C:\Lab\notatka.txt readme.txt          # zmień nazwę
Remove-Item C:\Lab\Archiwum -Recurse               # usuń folder z zawartością
Test-Path C:\Lab\readme.txt                        # czy istnieje? (True/False)
```

!!! tip "-WhatIf przed każdym ryzykownym poleceniem"
    Większość cmdletów, które coś zmieniają, przyjmuje parametr `-WhatIf`. Polecenie wypisze wtedy, **co by zrobiło**, ale niczego nie zmieni:

    ```powershell
    Remove-Item C:\Lab\*.txt -WhatIf
    ```

## Konta i grupy lokalne

```powershell
Get-LocalUser                                       # lista kont
Get-LocalGroup                                      # lista grup

$haslo = Read-Host -AsSecureString "Hasło"          # bezpieczne pobranie hasła
New-LocalUser -Name "jkowalski" -FullName "Jan Kowalski" -Password $haslo

Add-LocalGroupMember -Group "Użytkownicy" -Member "jkowalski"
Get-LocalGroupMember -Group "Administratorzy"       # kto jest administratorem

Disable-LocalUser -Name "jkowalski"                 # zablokuj konto
Enable-LocalUser  -Name "jkowalski"
Set-LocalUser -Name "jkowalski" -Description "Klasa 2B"
Remove-LocalUser -Name "jkowalski"
```

!!! warning "Nazwy grup zależą od języka systemu"
    W polskim Windows grupa nazywa się `Administratorzy`, w angielskim `Administrators`. Na Windows Server z angielskim interfejsem używaj nazw angielskich (`Users`, `Administrators`, `Remote Desktop Users`).

Hasło przekazujemy jako `SecureString` — PowerShell nie przyjmie hasła wpisanego jako zwykły tekst w parametrze `-Password`. Dzięki temu hasło nie zostaje w historii poleceń.

## Procesy

```powershell
Get-Process                          # wszystkie procesy
Get-Process -Name notepad            # konkretny proces
Start-Process notepad                # uruchom program
Stop-Process -Name notepad           # zakończ wszystkie procesy o tej nazwie
Stop-Process -Id 4512                # zakończ jeden proces po identyfikatorze
```

## Usługi

**Usługa** to program działający w tle, niezależnie od zalogowanego użytkownika (np. bufor wydruku, klient DHCP, Windows Update).

```powershell
Get-Service                                        # wszystkie usługi
Get-Service -Name Spooler                          # bufor wydruku
Stop-Service -Name Spooler
Start-Service -Name Spooler
Restart-Service -Name Spooler
Set-Service -Name Spooler -StartupType Disabled    # Automatic / Manual / Disabled
Get-Service -Name Spooler | Select-Object Name, Status, StartType
```

Usługa ma dwie niezależne cechy: **stan** (`Running` / `Stopped` — teraz) i **typ uruchamiania** (`Automatic` / `Manual` / `Disabled` — co się stanie po restarcie systemu).

## Sieć i system

```powershell
Get-NetAdapter                          # karty sieciowe
Get-NetIPAddress -AddressFamily IPv4    # adresy IPv4
Get-NetIPConfiguration                  # adres, brama, DNS — jak ipconfig
Test-Connection 8.8.8.8 -Count 2        # odpowiednik ping
Test-NetConnection google.com -Port 443 # czy port jest osiągalny
Get-ComputerInfo                        # pełna informacja o systemie
Rename-Computer -NewName "PC-LAB-01" -Restart
```

## Zadania

??? question "Zadanie 1: Struktura katalogów"
    Utwórz za pomocą PowerShell strukturę:

    ```text
    C:\Firma
    ├── Kadry
    ├── Ksiegowosc
    │   └── 2026
    └── IT
        └── skrypty
    ```

    W folderze `IT` utwórz plik `readme.txt` zawierający dwie linie tekstu. Wyświetl strukturę poleceniem `Get-ChildItem -Recurse`.

??? question "Zadanie 2: Porządki z -WhatIf"
    W folderze `C:\Firma\Kadry` utwórz pięć plików `.txt` i trzy pliki `.tmp`. Napisz polecenie usuwające tylko pliki `.tmp`, uruchom je najpierw z `-WhatIf`, a dopiero potem naprawdę. Zrób zrzut ekranu obu wywołań.

??? question "Zadanie 3: Konta dla działu"
    Utwórz konta `anowak` i `pzielinski` z pełnymi imionami i nazwiskami oraz opisem „Dział kadr”. Utwórz grupę lokalną `Kadry` (`New-LocalGroup`) i dodaj do niej oba konta. Pokaż członków grupy.

??? question "Zadanie 4: Kto jest administratorem"
    Wyświetl członków grupy administratorów na swojej maszynie wirtualnej. Wyjaśnij, dlaczego regularne sprawdzanie tej listy jest jednym z podstawowych obowiązków administratora.

??? question "Zadanie 5: Usługa bufora wydruku"
    Sprawdź stan i typ uruchamiania usługi `Spooler`. Zatrzymaj ją, ustaw typ uruchamiania na `Manual`, uruchom ponownie maszynę i sprawdź jej stan. Przywróć ustawienia początkowe. Udokumentuj każdy krok zrzutem ekranu.

??? question "Zadanie 6: Diagnostyka sieci"
    Ustal za pomocą PowerShell: adres IPv4 maszyny, maskę (długość prefiksu), bramę domyślną i serwer DNS. Sprawdź łączność z bramą i z adresem `8.8.8.8`. Zapisz użyte polecenia i wyniki.
