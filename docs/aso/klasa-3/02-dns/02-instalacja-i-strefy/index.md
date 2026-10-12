# Instalacja roli i strefy

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zainstalować rolę DNS Server na Windows Server 2022, utworzyć podstawową strefę wyszukiwania do przodu i wstecz oraz skierować klientów na nowy serwer DNS przez DHCP.

## Przygotowanie

Pracujemy na tym samym serwerze co w dziale DHCP:

- `SRV01`, adres statyczny `192.168.10.10/24`,
- w ustawieniach karty serwera jako DNS wskazany **on sam** (`192.168.10.10`),
- serwer DHCP rozdaje klientom opcje 006 = `192.168.10.10` i 015 = `firma.local`.

## Krok 1: instalacja roli DNS Server

**GUI:**

1. `Server Manager → Manage → Add Roles and Features`
2. *Role-based or feature-based installation* → `SRV01`
3. *Server Roles:* zaznacz **DNS Server** → *Add Features*
4. Ostrzeżenie o braku statycznego adresu nie powinno się pojawić — jeśli się pojawi, wróć do konfiguracji karty
5. **Install**

Po instalacji konsola jest dostępna w `Server Manager → Tools → DNS` (lub `dnsmgmt.msc`).

**PowerShell:**

```powershell
Install-WindowsFeature DNS -IncludeManagementTools
Get-Service DNS
```

!!! tip "Serwer działa od razu jako serwer buforujący"
    Świeżo zainstalowany serwer DNS bez żadnych stref już potrafi odpowiadać na zapytania o domeny internetowe — korzysta z listy serwerów głównych (*Root Hints*) i zapamiętuje odpowiedzi. Taki serwer nazywamy **buforującym** (*caching-only*). W naszej odizolowanej sieci nie ma jednak dostępu do internetu, więc te zapytania się nie powiodą.

## Krok 2: strefa wyszukiwania do przodu

**GUI:** konsola DNS → `SRV01` → prawy przycisk na **Forward Lookup Zones** → **New Zone…**

| Ekran kreatora | Wartość |
|---|---|
| Zone Type | **Primary zone** (opcja *Store the zone in Active Directory* jest niedostępna — nie mamy domeny AD) |
| Zone Name | `firma.local` |
| Zone File | *Create a new file* → `firma.local.dns` (domyślnie) |
| Dynamic Update | **Do not allow dynamic updates** |

**PowerShell:**

```powershell
Add-DnsServerPrimaryZone -Name "firma.local" -ZoneFile "firma.local.dns" -DynamicUpdate None
Get-DnsServerZone
```

Nowa strefa od razu zawiera dwa rekordy: **SOA** i **NS** wskazujący na `SRV01`.

!!! warning "Dlaczego .local?"
    Domena `.local` nie istnieje w internecie, więc nadaje się do labu. W prawdziwych firmach Microsoft zaleca jednak poddomenę domeny, którą firma posiada (np. `ad.firma.pl`), bo `.local` bywa używane też przez protokół mDNS (np. urządzenia Apple).

Plik strefy znajdziesz w `C:\Windows\System32\dns\`. Możesz go otworzyć w Notatniku i porównać z tym, co widać w konsoli.

## Krok 3: strefa wyszukiwania wstecznego

**GUI:** prawy przycisk na **Reverse Lookup Zones** → **New Zone…**

| Ekran kreatora | Wartość |
|---|---|
| Zone Type | **Primary zone** |
| Reverse Lookup Zone Name | **IPv4 Reverse Lookup Zone** |
| Network ID | `192.168.10` (kreator sam zbuduje nazwę `10.168.192.in-addr.arpa`) |
| Zone File | domyślny |
| Dynamic Update | **Do not allow dynamic updates** |

**PowerShell:**

```powershell
Add-DnsServerPrimaryZone -NetworkId "192.168.10.0/24" -ZoneFile "10.168.192.in-addr.arpa.dns"
```

## Krok 4: rekord A serwera

Dodaj rekord dla samego serwera — razem z rekordem PTR w strefie wstecznej:

**GUI:** prawy przycisk na strefie `firma.local` → **New Host (A or AAAA)…** → Name `srv01`, IP `192.168.10.10`, zaznacz **Create associated pointer (PTR) record**.

**PowerShell:**

```powershell
Add-DnsServerResourceRecordA -ZoneName "firma.local" -Name "srv01" `
    -IPv4Address 192.168.10.10 -CreatePtr
```

## Krok 5: test z klienta

Na `CLIENT01` odnów dzierżawę DHCP, żeby otrzymać aktualne opcje, i sprawdź:

```powershell
ipconfig /renew
ipconfig /all                    # DNS Servers: 192.168.10.10, Connection-specific DNS Suffix: firma.local
nslookup srv01.firma.local       # zapytanie do przodu
nslookup 192.168.10.10           # zapytanie wsteczne
ping srv01                       # sufiks firma.local dopisany automatycznie
```

Pierwsze linie wyniku `nslookup` pokazują, **który serwer DNS** odpowiedział. Jeśli jest tam `UnKnown`, oznacza to tylko, że brakuje rekordu PTR dla adresu serwera DNS — samo rozwiązywanie nazw może działać poprawnie.

## Zadania

??? question "Zadanie 1: Instalacja roli"
    Zainstaluj rolę DNS Server przez Server Manager. Udokumentuj zrzutami ekranu: wybór roli, potwierdzenie instalacji i otwartą konsolę DNS.

??? question "Zadanie 2: Strefa do przodu"
    Utwórz podstawową strefę `firma.local` bez aktualizacji dynamicznych. Pokaż rekordy SOA i NS. Otwórz właściwości rekordu SOA i zapisz: numer seryjny, serwer główny, odpowiedzialną osobę i domyślny TTL.

??? question "Zadanie 3: Strefa wsteczna"
    Utwórz strefę wyszukiwania wstecznego dla sieci laboratoryjnej. Pokaż jej pełną nazwę w konsoli.

??? question "Zadanie 4: Pierwsze rekordy"
    Dodaj rekord A dla `srv01` z rekordem PTR. Sprawdź oba rekordy z klienta poleceniem `nslookup`. Zrób zrzut ekranu wyniku.

??? question "Zadanie 5: Plik strefy"
    Otwórz w Notatniku plik strefy `firma.local.dns`. Odszukaj w nim rekordy widoczne w konsoli. Dodaj w konsoli nowy rekord A, a następnie sprawdź, kiedy pojawił się w pliku (wskazówka: prawy przycisk na strefie → *Update Server Data File*).

??? question "Zadanie 6: Tylko PowerShell"
    Utwórz drugą strefę `szkola.local` i rekord A `www` → `192.168.10.20` wyłącznie poleceniami PowerShell. Wyświetl rekordy strefy poleceniem `Get-DnsServerResourceRecord`.
