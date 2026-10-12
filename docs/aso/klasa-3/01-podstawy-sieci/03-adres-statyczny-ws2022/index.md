# Adres statyczny na Windows Server 2022

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć ustawić na Windows Server 2022 z angielskim interfejsem adres statyczny, maskę, bramę i serwer DNS na trzy sposoby — w oknach systemu, w narzędziu `sconfig` i w PowerShell — zmienić nazwę serwera i sprawdzić poprawność konfiguracji.

## Dlaczego serwer ma adres statyczny

Klienci łączą się z serwerem pod konkretnym adresem: wpisanym w konfiguracji DHCP jako serwer DNS, zapisanym w rekordzie DNS, wpisanym w skrypcie czy w ustawieniach drukarki. Gdyby serwer co jakiś czas dostawał nowy adres z DHCP, wszystkie te miejsca przestałyby działać. Dlatego serwery, routery i urządzenia sieciowe konfiguruje się **ręcznie**, adresem spoza puli DHCP.

W naszym labie konfigurujemy `SRV01`:

| Parametr | Wartość |
|---|---|
| IP address | `192.168.10.10` |
| Subnet mask | `255.255.255.0` (`/24`) |
| Default gateway | `192.168.10.1` |
| Preferred DNS server | `192.168.10.10` (serwer sam dla siebie — w dziale o DNS zainstalujemy na nim tę rolę) |
| Computer name | `SRV01` |

## Sposób 1: interfejs graficzny

1. Otwórz **Server Manager** → w lewym panelu **Local Server**.
2. W sekcji *Properties* kliknij niebieski link obok **Ethernet** (np. *IPv4 address assigned by DHCP, IPv6 enabled*). Otworzy się okno **Network Connections**.
3. Prawy przycisk na karcie **Ethernet** → **Properties**.
4. Zaznacz **Internet Protocol Version 4 (TCP/IPv4)** → **Properties**.
5. Wybierz **Use the following IP address** i wpisz *IP address*, *Subnet mask*, *Default gateway*.
6. Wybierz **Use the following DNS server addresses** i wpisz *Preferred DNS server*.
7. Zaznacz **Validate settings upon exit** → **OK** → **Close**.

!!! tip "Szybszy dostęp"
    Okno Network Connections otworzysz też skrótem ++win+r++ → `ncpa.cpl` → ++enter++. Działa na każdej wersji Windows i Windows Server.

**Zmiana nazwy serwera:** *Local Server* → link obok **Computer name** → **Change…** → wpisz `SRV01` → **OK** → restart (*Restart Now*).

!!! warning "Nazwa domyślna"
    Po instalacji serwer ma losową nazwę w stylu `WIN-3K8F2LQ9ABC`. Zmień ją od razu, przed instalacją ról — część ról (np. DNS) zapisuje nazwę serwera w swojej konfiguracji.

## Sposób 2: sconfig

`sconfig` to tekstowe menu konfiguracji serwera — jedyne „okienkowe” narzędzie na Windows Server w wersji **Server Core** (bez GUI). W Windows Server 2022 uruchamia się automatycznie po zalogowaniu na Server Core, a na wersji z GUI wpiszesz je w PowerShell:

```powershell
sconfig
```

Ustawienie adresu:

1. Opcja **8) Network settings**.
2. Wybierz numer (*Index*) karty sieciowej.
3. **1) Set network adapter address** → **S** (*Static*) → wpisz adres, maskę, bramę.
4. **2) Set DNS servers** → wpisz preferowany serwer DNS.
5. Powrót do menu głównego.

Zmiana nazwy: opcja **2) Computer name** → wpisz nazwę → potwierdź restart.

## Sposób 3: PowerShell

```powershell
Get-NetAdapter                                         # nazwa karty, np. "Ethernet"

New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.10.10 `
    -PrefixLength 24 -DefaultGateway 192.168.10.1

Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.10.10

Rename-Computer -NewName SRV01 -Restart
```

Uwagi:

- Maskę podajesz jako **prefiks** (`-PrefixLength 24`), nie w postaci `255.255.255.0`.
- `New-NetIPAddress` na karcie korzystającej wcześniej z DHCP sam wyłącza DHCP dla IPv4.
- Gdy karta **ma już adres statyczny** i chcesz go zmienić, najpierw usuń stary adres i starą bramę — inaczej karta będzie miała dwa adresy, a polecenie zgłosi błąd, że brama już istnieje:

```powershell
Remove-NetIPAddress -InterfaceAlias "Ethernet" -AddressFamily IPv4 -Confirm:$false
Remove-NetRoute -InterfaceAlias "Ethernet" -DestinationPrefix 0.0.0.0/0 -Confirm:$false
```

Powrót do DHCP:

```powershell
Set-NetIPInterface -InterfaceAlias "Ethernet" -Dhcp Enabled
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ResetServerAddresses
```

## Sprawdzenie konfiguracji

```powershell
ipconfig /all                  # DHCP Enabled: No, adres, maska, brama, DNS
Get-NetIPConfiguration
hostname
```

W Server Manager → *Local Server* przy *Ethernet* powinien być widoczny adres `192.168.10.10, IPv6 enabled`, a przy *Computer name* — `SRV01`.

## Zadania

??? question "Zadanie 1: Konfiguracja w GUI"
    Ustaw na `SRV01` adres statyczny zgodnie z tabelą z lekcji, korzystając wyłącznie z okien systemu. Zmień nazwę serwera na `SRV01`. Udokumentuj zrzutami ekranu: okno *Internet Protocol Version 4 (TCP/IPv4) Properties*, okno zmiany nazwy i sekcję *Properties* w *Local Server* po restarcie.

??? question "Zadanie 2: sconfig"
    Przywróć kartę do DHCP, a następnie ustaw ten sam adres statyczny, korzystając wyłącznie z `sconfig`. Zrób zrzuty ekranu kolejnych ekranów menu.

??? question "Zadanie 3: PowerShell"
    Ponownie przywróć DHCP i skonfiguruj adres, bramę, DNS i nazwę serwera tylko poleceniami PowerShell. Pokaż wynik `Get-NetIPConfiguration`.

??? question "Zadanie 4: Zmiana adresu"
    Zmień adres serwera z `192.168.10.10` na `192.168.10.11` w PowerShell, nie zostawiając starego adresu ani podwójnej bramy. Pokaż wynik `Get-NetIPAddress -AddressFamily IPv4` przed i po zmianie. Przywróć adres `.10`.

??? question "Zadanie 5: Porównanie metod"
    Porównaj trzy sposoby konfiguracji adresu: który jest najszybszy, który zadziała na Server Core, który da się powtórzyć na 20 serwerach? Uzasadnij odpowiedzi.
