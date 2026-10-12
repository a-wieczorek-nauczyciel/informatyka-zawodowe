# Adres sieci i hosty

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć na podstawie adresu i maski wyznaczyć adres sieci, adres rozgłoszeniowy, pierwszy i ostatni adres hosta oraz liczbę hostów, a także sprawdzić, czy dwa urządzenia są w tej samej sieci.

## Po co to liczyć

Zanim nadasz serwerowi adres statyczny albo utworzysz zakres DHCP, musisz wiedzieć, **które adresy w ogóle wolno nadać**. Dwa adresy w każdej sieci są zarezerwowane:

- **adres sieci** — wszystkie bity hosta równe `0`; identyfikuje sieć, nie można go nadać urządzeniu,
- **adres rozgłoszeniowy (broadcast)** — wszystkie bity hosta równe `1`; wiadomość wysłana na ten adres trafia do wszystkich w sieci.

Wszystkie adresy pomiędzy nimi to **adresy hostów**.

## Liczba hostów

Jeśli część hosta ma `h` bitów (`h = 32 − prefiks`), to:

```text
liczba hostów = 2^h − 2
```

| Prefiks | Bity hosta | Adresów w sieci | Hostów |
|---|---|---|---|
| `/24` | 8 | 256 | 254 |
| `/25` | 7 | 128 | 126 |
| `/26` | 6 | 64 | 62 |
| `/27` | 5 | 32 | 30 |
| `/28` | 4 | 16 | 14 |
| `/30` | 2 | 4 | 2 |

## Metoda dla masek /24 i większych

Dla prefiksów od `/24` w górę zmienia się tylko ostatni oktet. Wystarczy znać **rozmiar bloku** = `256 − ostatni oktet maski`.

**Przykład:** `192.168.10.75/26`

1. Maska `/26` = `255.255.255.192` → rozmiar bloku `256 − 192 = 64`.
2. Sieci zaczynają się co 64: `0, 64, 128, 192`.
3. `75` leży w bloku `64 – 127`.

| Wynik | Adres |
|---|---|
| Adres sieci | `192.168.10.64` |
| Pierwszy host | `192.168.10.65` |
| Ostatni host | `192.168.10.126` |
| Broadcast | `192.168.10.127` |
| Liczba hostów | 62 |

## Metoda binarna (zawsze działa)

**Adres sieci** = adres IP **AND** maska (bit po bicie: `1` tylko tam, gdzie oba bity to `1`).

```text
192.168.10.75   → ostatni oktet: 01001011
255.255.255.192 → ostatni oktet: 11000000
AND             →                01000000 = 64   → adres sieci 192.168.10.64
```

**Broadcast** = adres sieci z wszystkimi bitami hosta ustawionymi na `1`:

```text
01|000000  →  01|111111 = 127   → broadcast 192.168.10.127
```

## Czy dwa adresy są w tej samej sieci?

Wyznacz adres sieci dla obu (z **tą samą maską**). Jeśli wyniki są równe — urządzenia rozmawiają bezpośrednio. Jeśli nie — potrzebny jest router.

**Przykład:** `192.168.10.60/26` i `192.168.10.70/26` → sieci `192.168.10.0` i `192.168.10.64` → **różne sieci**, mimo że adresy wyglądają podobnie.

!!! warning "Najczęstszy błąd w labie"
    Serwer ma `192.168.10.10/24`, a klient `192.168.10.20/8` albo `192.168.1.20/24`. Ping nie działa, a uczeń szuka winy w zaporze. Zawsze najpierw sprawdź, czy oba urządzenia są w tej samej sieci z tą samą maską.

## Sprawdzenie w PowerShell

Wynik obliczeń możesz potwierdzić na serwerze — PowerShell pokazuje prefiks przy adresie:

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

## Zadania

??? question "Zadanie 1: Parametry sieci /24"
    Dla adresu `172.16.5.130/24` wyznacz: adres sieci, broadcast, pierwszy i ostatni host, liczbę hostów.

??? question "Zadanie 2: Mniejsze sieci"
    Wyznacz te same parametry dla: `192.168.10.75/27`, `192.168.10.200/26`, `10.0.0.13/30`.

??? question "Zadanie 3: Ta sama sieć?"
    Określ, czy pary urządzeń mogą komunikować się bez routera: (a) `192.168.10.10/24` i `192.168.10.250/24`, (b) `192.168.10.30/27` i `192.168.10.33/27`, (c) `10.1.1.1/16` i `10.1.200.1/16`. Pokaż obliczenia.

??? question "Zadanie 4: Dobierz maskę"
    Dział ma 25 komputerów, 2 drukarki i router. Jaka jest najmniejsza maska, przy której wszystkie urządzenia zmieszczą się w jednej sieci? A jeśli dział ma urosnąć do 40 urządzeń?

??? question "Zadanie 5: Znajdź błąd"
    Administrator nadał serwerowi adres `192.168.10.0/24`, a drukarce `192.168.10.255/24`. Co jest nie tak z każdym z tych adresów?
