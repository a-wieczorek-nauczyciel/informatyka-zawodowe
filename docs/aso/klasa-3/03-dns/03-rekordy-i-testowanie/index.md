# Rekordy, przekazywanie, testowanie

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć utworzyć rekordy A, PTR, CNAME i MX, skonfigurować przekazywanie zapytań do zewnętrznego serwera DNS, testować serwer poleceniami `nslookup` i `Resolve-DnsName` oraz zdiagnozować typowe błędy rozwiązywania nazw.

## Rekordy w praktyce

Załóżmy, że w sieci firmy działają:

| Usługa | Serwer | Adres |
|---|---|---|
| DHCP, DNS | `srv01` | `192.168.10.10` |
| strona intranetu | `srv02` | `192.168.10.11` |
| poczta | `mail` | `192.168.10.12` |

### Rekord A (i PTR)

```powershell
Add-DnsServerResourceRecordA -ZoneName firma.local -Name srv02 -IPv4Address 192.168.10.11 -CreatePtr
Add-DnsServerResourceRecordA -ZoneName firma.local -Name mail  -IPv4Address 192.168.10.12 -CreatePtr
```

GUI: prawy przycisk na strefie → **New Host (A or AAAA)…**

### Rekord CNAME — alias

Intranet ma być dostępny pod przyjazną nazwą `intranet.firma.local` i `www.firma.local`. Zamiast dodawać dwa kolejne rekordy A z tym samym adresem, tworzymy **aliasy** wskazujące na `srv02`:

```powershell
Add-DnsServerResourceRecordCName -ZoneName firma.local -Name intranet -HostNameAlias srv02.firma.local
Add-DnsServerResourceRecordCName -ZoneName firma.local -Name www      -HostNameAlias srv02.firma.local
```

GUI: **New Alias (CNAME)…** → *Alias name* `intranet`, *FQDN for target host* `srv02.firma.local`.

!!! tip "Po co alias zamiast drugiego rekordu A"
    Gdy intranet przeniesie się na inny serwer z innym adresem, zmieniasz **jeden** rekord A (`srv02`). Wszystkie aliasy wskazujące na niego zaczną działać z nowym adresem automatycznie.

### Rekord MX — serwer poczty

```powershell
Add-DnsServerResourceRecordMX -ZoneName firma.local -Name "." `
    -MailExchange mail.firma.local -Preference 10
```

GUI: **New Mail Exchanger (MX)…** → *Host or child domain* puste, *FQDN of mail server* `mail.firma.local`, *Mail server priority* `10`.

Rekordów MX może być kilka — **niższa liczba = wyższy priorytet**. Serwer zapasowy dostaje np. priorytet `20`.

!!! warning "MX wskazuje na nazwę, nie na adres"
    Rekord MX i rekord CNAME zawsze wskazują na **nazwę** hosta, który ma własny rekord A. Wpisanie tam adresu IP to błąd.

### Przegląd i usuwanie rekordów

```powershell
Get-DnsServerResourceRecord -ZoneName firma.local
Get-DnsServerResourceRecord -ZoneName firma.local -RRType CNAME
Remove-DnsServerResourceRecord -ZoneName firma.local -RRType A -Name srv02 -Force
```

## Przekazywanie zapytań (forwarders)

Nasz serwer jest autorytatywny dla `firma.local`. Co z zapytaniem o `www.google.com`? Domyślnie serwer sam przejdzie drogę od serwerów głównych (*Root Hints*). W firmach częściej stosuje się **przekazywanie**: serwer, który nie zna odpowiedzi, przekazuje zapytanie do wskazanego serwera — np. DNS dostawcy internetu albo publicznego DNS.

**GUI:** konsola DNS → prawy przycisk na `SRV01` → **Properties → Forwarders → Edit…** → wpisz adres.

**PowerShell:**

```powershell
Add-DnsServerForwarder -IPAddress 1.1.1.1, 8.8.8.8
Get-DnsServerForwarder
```

**Conditional forwarder** (przekazywanie warunkowe) przekazuje zapytania tylko o konkretną domenę — np. wszystkie zapytania o `partner.local` do serwera DNS firmy partnerskiej:

```powershell
Add-DnsServerConditionalForwarderZone -Name partner.local -MasterServers 10.50.0.5
```

!!! info "W odizolowanym labie"
    Sieć `lab` nie ma dostępu do internetu, więc zapytania o domeny zewnętrzne nie dostaną odpowiedzi także po ustawieniu forwarderów. Konfigurację sprawdzasz w konsoli i poleceniem `Get-DnsServerForwarder`.

## Testowanie

### nslookup

```powershell
nslookup www.firma.local                 # zapytanie do domyślnego serwera DNS
nslookup www.firma.local 192.168.10.10   # zapytanie do wskazanego serwera
nslookup -type=mx firma.local            # konkretny typ rekordu
nslookup 192.168.10.11                   # zapytanie wsteczne (PTR)
```

Tryb interaktywny — wygodny przy wielu zapytaniach:

```text
nslookup
> server 192.168.10.10
> set type=cname
> intranet.firma.local
> exit
```

### Resolve-DnsName

Odpowiednik `nslookup` w PowerShell — zwraca obiekty, więc wynik można filtrować:

```powershell
Resolve-DnsName www.firma.local
Resolve-DnsName firma.local -Type MX
Resolve-DnsName 192.168.10.11
Resolve-DnsName www.firma.local -Server 192.168.10.10 -DnsOnly   # pomija cache i plik hosts
```

### Cache

```powershell
ipconfig /displaydns          # cache klienta
ipconfig /flushdns            # wyczyść cache klienta
Clear-DnsServerCache          # wyczyść cache serwera (na SRV01)
```

## Typowe problemy

| Objaw | Najczęstsza przyczyna |
|---|---|
| `Non-existent domain` | brak rekordu lub literówka w nazwie strefy / rekordu |
| `ping srv02` nie działa, a `ping srv02.firma.local` działa | klient nie ma sufiksu DNS — brak opcji DHCP 015 |
| `DNS request timed out` | klient wskazuje zły serwer DNS, usługa DNS zatrzymana, zapora |
| po zmianie rekordu klient wciąż dostaje stary adres | stara odpowiedź w cache klienta lub serwera (TTL) |
| `nslookup` pokazuje `Server: UnKnown` | brak rekordu PTR dla adresu serwera DNS — kosmetyczne |

## Zadania

??? question "Zadanie 1: Rekordy dla firmy"
    Dodaj do strefy `firma.local` rekordy A (z PTR) dla `srv02` i `mail` oraz aliasy `intranet` i `www` dla `srv02`. Sprawdź każdy rekord z klienta poleceniem `nslookup` i wykonaj zrzuty ekranu.

??? question "Zadanie 2: Przeprowadzka intranetu"
    Intranet przenosi się na nowy serwer `srv03` (`192.168.10.13`). Wprowadź zmianę tak, aby aliasy `intranet` i `www` wskazywały na nowy serwer, zmieniając jak najmniej rekordów. Opisz, które rekordy zmieniłeś i dlaczego. Sprawdź wynik z klienta — jeśli klient pokazuje stary adres, wyjaśnij przyczynę i usuń ją.

??? question "Zadanie 3: Poczta z zapasem"
    Dodaj rekord MX dla `mail.firma.local` z priorytetem 10 oraz rekord A i MX dla zapasowego serwera `mail2` (`192.168.10.14`) z priorytetem 20. Sprawdź oba rekordy poleceniem `Resolve-DnsName -Type MX` i wyjaśnij, do którego serwera trafi poczta w normalnej sytuacji.

??? question "Zadanie 4: Forwardery"
    Skonfiguruj forwardery `1.1.1.1` i `8.8.8.8` przez konsolę DNS. Następnie dodaj przekazywanie warunkowe dla domeny `partner.local` do serwera `10.50.0.5` za pomocą PowerShell. Pokaż obie konfiguracje w konsoli.

??? question "Zadanie 5: nslookup kontra Resolve-DnsName"
    Wykonaj to samo zapytanie o rekord MX narzędziem `nslookup` i `Resolve-DnsName`. Napisz polecenie PowerShell, które z wyniku `Resolve-DnsName` wyświetli tylko nazwę serwera poczty i priorytet. Dlaczego z `nslookup` byłoby to trudniejsze?

??? question "Zadanie 6: Usterka sufiksu"
    Usuń w DHCP opcję 015 zakresu. Na kliencie odnów dzierżawę, wyczyść cache DNS i sprawdź `ping srv01` oraz `ping srv01.firma.local`. Opisz wynik i wyjaśnij go. Przywróć opcję.
