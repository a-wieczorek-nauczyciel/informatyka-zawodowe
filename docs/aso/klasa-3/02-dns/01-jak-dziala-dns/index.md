# Jak działa DNS

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć opisać hierarchiczną budowę przestrzeni nazw DNS, prześledzić drogę zapytania od klienta do serwera autorytatywnego, rozróżnić zapytanie rekursywne i iteracyjne oraz dobrać typ rekordu do zadania.

## Problem: ludzie pamiętają nazwy, komputery — adresy

Pakiety w sieci trafiają do celu na podstawie adresu IP. Ludzie nie zapamiętają jednak adresów setek serwisów — a adresy serwerów się zmieniają. **DNS (Domain Name System)** to rozproszona baza danych, która tłumaczy nazwy (np. `www.firma.local`) na adresy IP i odwrotnie.

Przed DNS każdy komputer miał lokalny plik z listą nazw i adresów. Ten mechanizm nadal istnieje — to plik `C:\Windows\System32\drivers\etc\hosts` — i jest sprawdzany **przed** zapytaniem do serwera DNS.

## Hierarchia nazw

Przestrzeń nazw DNS ma budowę drzewa, czytanego **od prawej do lewej**:

```text
www.wsb.krakow.pl.
│   │   │      │  └── korzeń (root) — kropka, zwykle niewidoczna
│   │   │      └───── domena najwyższego poziomu (TLD): pl
│   │   └──────────── domena drugiego poziomu: krakow
│   └──────────────── poddomena: wsb
└──────────────────── nazwa hosta: www
```

Pełną nazwę zakończoną kropką nazywamy **FQDN** (*Fully Qualified Domain Name*).

Każdy poziom drzewa jest zarządzany przez inne serwery. Serwery główne (*root*) wiedzą tylko, kto obsługuje `pl`, serwery `pl` — kto obsługuje `krakow.pl` itd. Serwer, który przechowuje **oryginalne** dane danej domeny, to serwer **autorytatywny** dla tej domeny.

## Droga zapytania

Klient wpisuje w przeglądarce `www.przyklad.pl`:

1. Klient sprawdza własną pamięć podręczną (cache) i plik `hosts`.
2. Klient pyta **swój** serwer DNS (ten z konfiguracji karty — u nas otrzymany z DHCP). To **zapytanie rekursywne**: „daj mi ostateczną odpowiedź”.
3. Serwer DNS sprawdza swój cache. Jeśli nie zna odpowiedzi, sam zaczyna pytać — tym razem **iteracyjnie**:
    - serwer główny → „nie wiem, ale `pl` obsługują serwery X”
    - serwer `pl` → „nie wiem, ale `przyklad.pl` obsługuje serwer Y”
    - serwer Y (autorytatywny) → „`www.przyklad.pl` to `203.0.113.10`”
4. Serwer DNS zapamiętuje odpowiedź w cache (na czas **TTL**) i przekazuje ją klientowi.

| | Zapytanie rekursywne | Zapytanie iteracyjne |
|---|---|---|
| Kto pyta | klient → swój serwer DNS | serwer DNS → inne serwery DNS |
| Oczekiwana odpowiedź | ostateczna (adres albo błąd) | najlepsza, jaką pytany zna — często odesłanie dalej |

**TTL (Time To Live)** — czas w sekundach, przez jaki odpowiedź może być przechowywana w cache. Krótki TTL = szybsze rozpowszechnienie zmian, ale więcej zapytań.

DNS działa na porcie **53**, głównie po **UDP**; po TCP — przy dużych odpowiedziach i przy transferze stref między serwerami.

## Strefy

**Strefa (zone)** to fragment przestrzeni nazw, za który odpowiada dany serwer, zapisany jako zbiór rekordów.

- **Strefa wyszukiwania do przodu (Forward Lookup Zone)** — nazwa → adres IP, np. strefa `firma.local`.
- **Strefa wyszukiwania wstecznego (Reverse Lookup Zone)** — adres IP → nazwa. Nazwa strefy powstaje z odwróconego adresu sieci i domeny `in-addr.arpa`, np. dla sieci `192.168.10.0/24` jest to `10.168.192.in-addr.arpa`.

Rodzaje stref w Windows Server:

| Rodzaj | Opis |
|---|---|
| **Primary** | oryginalna, edytowalna kopia strefy |
| **Secondary** | kopia tylko do odczytu, pobierana z serwera Primary (transfer strefy) — zapas i rozłożenie ruchu |
| **Stub** | zawiera tylko informację, które serwery są autorytatywne dla strefy |
| **Active Directory-integrated** | strefa przechowywana w AD i replikowana między kontrolerami domeny |

## Najważniejsze typy rekordów

| Typ | Znaczenie | Przykład |
|---|---|---|
| **A** | nazwa → adres IPv4 | `srv01` → `192.168.10.10` |
| **AAAA** | nazwa → adres IPv6 | `srv01` → `fd00::10` |
| **PTR** | adres → nazwa (strefa wsteczna) | `10` → `srv01.firma.local` |
| **CNAME** | alias — nazwa wskazująca na inną nazwę | `www` → `srv01.firma.local` |
| **MX** | serwer poczty dla domeny (z priorytetem) | `firma.local` → `mail.firma.local`, priorytet 10 |
| **NS** | serwer autorytatywny dla strefy | `firma.local` → `srv01.firma.local` |
| **SOA** | początek strefy: serwer główny, e-mail administratora, numer seryjny, czasy | jeden w każdej strefie |

## Zadania

??? question "Zadanie 1: Rozbierz nazwę"
    Dla nazwy `poczta.wydzial.uczelnia.edu.pl` wskaż: domenę najwyższego poziomu, domenę drugiego poziomu, wszystkie poddomeny i nazwę hosta. Zapisz ją jako FQDN.

??? question "Zadanie 2: Plik hosts"
    Na maszynie wirtualnej klienta otwórz jako administrator plik `hosts` i dodaj wpis kierujący nazwę `test.lab` na adres `127.0.0.1`. Sprawdź działanie poleceniem `ping test.lab`. Wyjaśnij, dlaczego plik `hosts` bywa wykorzystywany przez złośliwe oprogramowanie. Usuń wpis.

??? question "Zadanie 3: Prześledź zapytanie"
    Narysuj (na kartce lub w dowolnym narzędziu) drogę zapytania o `www.przyklad.pl` od klienta do serwera autorytatywnego. Oznacz, które strzałki to zapytania rekursywne, a które iteracyjne, i gdzie odpowiedź trafia do cache.

??? question "Zadanie 4: Nazwa strefy wstecznej"
    Podaj nazwę strefy wyszukiwania wstecznego dla sieci `10.20.30.0/24` oraz dla `172.16.5.0/24`.

??? question "Zadanie 5: Dobierz rekord"
    Jaki typ rekordu utworzysz, gdy: (a) serwer `srv02` ma adres `192.168.10.11`, (b) strona intranetu ma być dostępna jako `intranet.firma.local`, choć działa na `srv02`, (c) poczta dla `firma.local` ma trafiać na `mail.firma.local`, (d) polecenie `nslookup 192.168.10.11` ma zwracać nazwę serwera?

??? question "Zadanie 6: Cache w praktyce"
    Na swoim komputerze wykonaj `ipconfig /displaydns`, a potem `ipconfig /flushdns` i ponownie `ipconfig /displaydns`. Opisz różnicę i podaj sytuację, w której administrator czyści cache DNS klienta.
