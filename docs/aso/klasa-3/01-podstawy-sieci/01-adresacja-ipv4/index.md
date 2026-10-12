# Adresacja IPv4

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić rolę każdego z czterech parametrów konfiguracji IPv4, zapisać maskę w postaci dziesiętnej i jako prefiks, rozpoznać adresy prywatne i specjalne oraz odczytać konfigurację sieci z wyniku `ipconfig /all`.

## Cztery parametry, bez których serwer nie działa w sieci

Każda karta sieciowa w sieci IPv4 potrzebuje:

| Parametr | Pytanie, na które odpowiada | Przykład |
|---|---|---|
| **Adres IP** | kim jestem? | `192.168.10.10` |
| **Maska podsieci** | kto jest w mojej sieci lokalnej? | `255.255.255.0` |
| **Brama domyślna** | przez kogo wysyłam pakiety poza moją sieć? | `192.168.10.1` |
| **Serwer DNS** | kogo pytam o adres dla nazwy? | `192.168.10.10` |

Brak lub błąd w którymkolwiek z nich daje inny objaw — dlatego warto rozumieć każdy osobno.

## Adres IP

Adres IPv4 ma **32 bity**, zapisywane jako cztery liczby dziesiętne (oktety) od 0 do 255, oddzielone kropkami:

```text
192      .168      .10       .10
11000000 .10101000 .00001010 .00001010
```

Adres składa się z dwóch części: **części sieciowej** (wspólnej dla wszystkich urządzeń w tej samej sieci) i **części hosta** (unikalnej dla urządzenia). Gdzie leży granica — mówi maska.

## Maska podsieci i prefiks

Maska to również 32 bity: **jedynki** oznaczają część sieciową, **zera** — część hosta. Jedynki zawsze są na początku, bez przerw.

| Maska dziesiętnie | Binarnie | Prefiks |
|---|---|---|
| `255.0.0.0` | `11111111.00000000.00000000.00000000` | `/8` |
| `255.255.0.0` | `11111111.11111111.00000000.00000000` | `/16` |
| `255.255.255.0` | `11111111.11111111.11111111.00000000` | `/24` |
| `255.255.255.128` | `11111111.11111111.11111111.10000000` | `/25` |
| `255.255.255.192` | `11111111.11111111.11111111.11000000` | `/26` |
| `255.255.255.224` | `11111111.11111111.11111111.11100000` | `/27` |

**Prefiks** (zapis CIDR) to po prostu liczba jedynek w masce: `192.168.10.10/24` znaczy to samo co adres `192.168.10.10` z maską `255.255.255.0`.

!!! tip "Wartości, które warto znać na pamięć"
    Możliwe wartości oktetu maski: `0, 128, 192, 224, 240, 248, 252, 254, 255`. Każda kolejna to jedna jedynka więcej. Windows Server w polu *Subnet mask* przyjmuje tylko takie wartości.

## Brama domyślna

Komputer porównuje adres docelowy pakietu ze swoją siecią (na podstawie maski):

- cel **w tej samej sieci** → pakiet idzie bezpośrednio do odbiorcy,
- cel **w innej sieci** → pakiet idzie do **bramy domyślnej** (routera), która przekazuje go dalej.

Brama musi leżeć **w tej samej sieci** co komputer. Brak bramy oznacza, że komputer rozmawia tylko z sąsiadami w swojej sieci lokalnej.

## Serwer DNS

Adres serwera, który tłumaczy nazwy (np. `srv01.firma.local`) na adresy IP. Bez DNS komputer połączy się z `192.168.10.10`, ale nie z `srv01` — szczegóły w dziale o serwerze DNS.

## Adresy prywatne i specjalne

Adresy **prywatne** (RFC 1918) można swobodnie używać w sieciach lokalnych — nie są routowane w internecie:

| Zakres | Prefiks |
|---|---|
| `10.0.0.0 – 10.255.255.255` | `10.0.0.0/8` |
| `172.16.0.0 – 172.31.255.255` | `172.16.0.0/12` |
| `192.168.0.0 – 192.168.255.255` | `192.168.0.0/16` |

Adresy specjalne:

| Adres | Znaczenie |
|---|---|
| `127.0.0.1` (cała sieć `127.0.0.0/8`) | pętla zwrotna (*loopback*) — „ja sam” |
| `169.254.0.0/16` | APIPA — adres przydzielony samodzielnie, gdy brak odpowiedzi DHCP |
| `0.0.0.0` | „brak adresu” / „każdy adres” |
| `255.255.255.255` | rozgłoszenie w sieci lokalnej |

## Odczyt konfiguracji

```powershell
ipconfig            # skrót: adres, maska, brama
ipconfig /all       # pełna konfiguracja: MAC, DHCP włączony/wyłączony, DNS
Get-NetIPConfiguration
```

W wyniku `ipconfig /all` zwróć uwagę na linię **DHCP Enabled** — `No` oznacza adres statyczny, `Yes` — adres z serwera DHCP.

## Zadania

??? question "Zadanie 1: Maska w trzech postaciach"
    Uzupełnij tabelę: dla prefiksów `/16`, `/24`, `/25`, `/27`, `/30` zapisz maskę dziesiętnie i binarnie.

??? question "Zadanie 2: Prefiks z maski"
    Zamień na prefiks: `255.255.255.240`, `255.255.248.0`, `255.192.0.0`. Wskaż, która z masek `255.255.255.0`, `255.255.0.255`, `255.255.254.0` jest nieprawidłowa, i uzasadnij.

??? question "Zadanie 3: Prywatny czy publiczny"
    Które adresy są prywatne: `10.200.1.1`, `172.15.0.1`, `172.20.5.5`, `192.169.1.1`, `192.168.255.254`, `100.64.0.1`? Uzasadnij każdą odpowiedź zakresem z tabeli.

??? question "Zadanie 4: Odczytaj swoją konfigurację"
    Na swoim komputerze (lub maszynie wirtualnej) wykonaj `ipconfig /all`. Wypisz adres IP, maskę z prefiksem, bramę, serwer DNS, adres MAC i informację, czy adres jest statyczny, czy z DHCP.

??? question "Zadanie 5: Co się zepsuje?"
    Opisz objaw, jaki zobaczy użytkownik, gdy w konfiguracji komputera: (a) brakuje bramy, (b) brama jest z innej sieci, (c) adres DNS jest błędny, (d) dwa komputery mają ten sam adres IP.
