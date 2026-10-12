# Instalacja roli i zakres

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć przygotować serwer do pracy jako serwer DHCP, zainstalować rolę DHCP Server na Windows Server 2022, utworzyć i aktywować zakres oraz sprawdzić, że klient otrzymał z niego adres.

## Krok 0: serwer musi mieć adres statyczny

Serwer DHCP **nie może sam pobierać adresu z DHCP** — klienci muszą wiedzieć, gdzie go szukać, a jego adres nie może się zmieniać. Zanim zainstalujesz rolę, ustaw na `SRV01` adres statyczny.

**GUI:** `Server Manager → Local Server → Ethernet` → właściwości karty → *Internet Protocol Version 4 (TCP/IPv4)* → *Use the following IP address*.

**PowerShell:**

```powershell
Get-NetAdapter                                     # sprawdź nazwę karty, np. "Ethernet"
New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.10.10 `
    -PrefixLength 24 -DefaultGateway 192.168.10.1
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.10.10
Rename-Computer -NewName SRV01 -Restart
```

Jako DNS serwer wskazuje sam siebie — w następnym dziale zainstalujemy na nim rolę DNS.

!!! warning "Sprawdź kartę sieciową w VirtualBox"
    Obie maszyny (`SRV01` i `CLIENT01`) muszą mieć kartę w trybie **Internal Network** o **tej samej nazwie** (`lab`). Literówka w nazwie sieci (`lab` vs `Lab`) = dwie osobne sieci i klient nigdy nie zobaczy serwera.

## Krok 1: instalacja roli DHCP Server

**GUI:**

1. `Server Manager → Manage → Add Roles and Features`
2. *Installation Type:* **Role-based or feature-based installation**
3. *Server Selection:* `SRV01`
4. *Server Roles:* zaznacz **DHCP Server** → *Add Features* (narzędzia administracyjne)
5. *Confirmation* → **Install**
6. Po instalacji w Server Manager pojawi się żółty trójkąt z flagą → **Complete DHCP configuration** → *Commit*

Ostatni krok tworzy lokalne grupy zabezpieczeń `DHCP Administrators` i `DHCP Users`. W środowisku z domeną Active Directory w tym miejscu następuje też **autoryzacja** serwera w domenie — w naszym labie (grupa robocza) ten krok nie występuje.

**PowerShell:**

```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
netsh dhcp add securitygroups
Restart-Service DHCPServer
```

## Krok 2: projekt zakresu

Zanim cokolwiek klikniesz, zaplanuj sieć. Dla `192.168.10.0/24`:

| Adresy | Przeznaczenie |
|---|---|
| `192.168.10.1` | brama (router) |
| `192.168.10.2 – .49` | serwery i urządzenia sieciowe — adresy statyczne |
| `192.168.10.50 – .99` | drukarki i urządzenia z rezerwacją |
| `192.168.10.100 – .200` | **pula DHCP dla komputerów** |
| `192.168.10.201 – .254` | zapas |

!!! tip "Zakres nie musi obejmować całej podsieci"
    Adresy statyczne serwerów i bramy po prostu zostawiasz **poza zakresem**. Wtedy serwer DHCP nigdy ich nie rozda.

## Krok 3: utworzenie zakresu

**GUI:** `Server Manager → Tools → DHCP` → rozwiń `srv01` → prawy przycisk na **IPv4** → **New Scope…**

| Ekran kreatora | Wartość |
|---|---|
| Scope Name | `LAN-Klasa` (opis dowolny) |
| IP Address Range | Start `192.168.10.100`, End `192.168.10.200`, Length `24`, Subnet mask `255.255.255.0` |
| Add Exclusions and Delay | na razie pomiń |
| Lease Duration | `8 dni` (zostaw domyślne) |
| Configure DHCP Options | **Yes, I want to configure these options now** |
| Router (Default Gateway) | `192.168.10.1` → *Add* |
| Domain Name and DNS Servers | Parent domain `firma.local`, IP `192.168.10.10` → *Add* |
| WINS Servers | pomiń |
| Activate Scope | **Yes, I want to activate this scope now** |

Ostrzeżenie przy dodawaniu serwera DNS („nie jest prawidłowym serwerem DNS”) pojawi się, bo rola DNS nie jest jeszcze zainstalowana. Możesz je zignorować — zajmiemy się tym w dziale o DNS.

**PowerShell:**

```powershell
Add-DhcpServerv4Scope -Name "LAN-Klasa" -StartRange 192.168.10.100 `
    -EndRange 192.168.10.200 -SubnetMask 255.255.255.0 -LeaseDuration 8.00:00:00 -State Active

Set-DhcpServerv4OptionValue -ScopeId 192.168.10.0 -Router 192.168.10.1
Set-DhcpServerv4OptionValue -ScopeId 192.168.10.0 -DnsServer 192.168.10.10 `
    -DnsDomain firma.local -Force

Get-DhcpServerv4Scope                                # czy zakres istnieje i jest Active
```

Zakres jest identyfikowany przez **adres sieci** (`ScopeId 192.168.10.0`), a nie przez nazwę.

## Krok 4: test na kliencie

Na `CLIENT01` upewnij się, że karta ma ustawione *Obtain an IP address automatically*, a następnie:

```powershell
ipconfig /release
ipconfig /renew
ipconfig /all
```

W wyniku sprawdź: adres z puli `.100–.200`, bramę `192.168.10.1`, **DHCP Server: 192.168.10.10** i czas dzierżawy.

Na serwerze zobaczysz dzierżawę w konsoli DHCP: `IPv4 → Scope → Address Leases`, lub:

```powershell
Get-DhcpServerv4Lease -ScopeId 192.168.10.0
```

## Zadania

??? question "Zadanie 1: Adres statyczny serwera"
    Skonfiguruj na `SRV01` adres `192.168.10.10/24`, bramę `192.168.10.1` i DNS `192.168.10.10`, a nazwę komputera zmień na `SRV01`. Pokaż wynik `Get-NetIPConfiguration` i `hostname`.

??? question "Zadanie 2: Instalacja roli przez GUI"
    Zainstaluj rolę DHCP Server za pomocą kreatora Add Roles and Features i dokończ konfigurację po instalacji. Udokumentuj zrzutami ekranu: wybór roli, ekran potwierdzenia, komunikat „Complete DHCP configuration”.

??? question "Zadanie 3: Projekt adresacji"
    Zaprojektuj adresację dla sieci `10.20.0.0/24` w małym biurze: brama, 2 serwery, 3 drukarki, około 60 komputerów i zapas na rozwój. Przedstaw projekt w tabeli takiej jak w lekcji.

??? question "Zadanie 4: Zakres z kreatora"
    Utwórz zakres `LAN-Klasa` zgodnie z tabelą z lekcji. Pokaż zrzut ekranu konsoli DHCP z rozwiniętym zakresem (*Address Pool*, *Scope Options*).

??? question "Zadanie 5: Klient dostaje adres"
    Na `CLIENT01` zwolnij i odnów adres. Pokaż wynik `ipconfig /all` oraz dzierżawę widoczną na serwerze (w konsoli i przez `Get-DhcpServerv4Lease`). Porównaj adres MAC klienta w obu miejscach.

??? question "Zadanie 6: Zakres nieaktywny"
    Dezaktywuj zakres (prawy przycisk → *Deactivate*), a na kliencie wykonaj `ipconfig /release` i `ipconfig /renew`. Opisz, co się stało i jaki adres otrzymał klient. Aktywuj zakres ponownie.
