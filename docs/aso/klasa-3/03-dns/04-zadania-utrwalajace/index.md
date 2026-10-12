# Zadania utrwalające

Zadania podsumowujące dział „Serwer DNS” i łączące go z serwerem DHCP. Wykonujesz je w środowisku laboratoryjnym i dokumentujesz zrzutami ekranu — tak jak na egzaminie INF.02, gdzie DHCP i DNS często pojawiają się w jednym zadaniu.

!!! tip "Zacznij od czystego stanu"
    Przed zadaniem 1 przywróć `SRV01` do snapshotu sprzed instalacji ról DHCP i DNS. Całą konfigurację wykonujesz od zera.

## Zadanie 1: Serwer infrastruktury dla biura

Skonfiguruj `SRV01` jako serwer DHCP i DNS dla sieci `172.16.5.0/24`:

| Parametr | Wartość |
|---|---|
| Adres serwera | `172.16.5.2/24`, brama `172.16.5.1`, DNS `172.16.5.2` |
| Nazwa serwera | `SRV-BIURO` |
| Strefa do przodu | `biuro.local`, bez aktualizacji dynamicznych |
| Strefa wsteczna | dla sieci `172.16.5.0/24` |
| Zakres DHCP | `172.16.5.50 – 172.16.5.150`, dzierżawa 1 dzień |
| Opcje DHCP | brama, DNS `172.16.5.2`, domena `biuro.local` |

- [ ] Zrzut ekranu: konfiguracja IPv4 i nazwa serwera
- [ ] Zrzut ekranu: obie role widoczne w Server Manager
- [ ] Zrzut ekranu: konsola DNS z rozwiniętymi strefami do przodu i wstecz
- [ ] Zrzut ekranu: konsola DHCP z zakresem i opcjami
- [ ] Zrzut ekranu: `ipconfig /all` na kliencie — adres z puli, poprawny DNS i sufiks

## Zadanie 2: Rekordy usług

W strefie `biuro.local` utwórz:

| Rekord | Typ | Wartość |
|---|---|---|
| `srv-biuro` | A + PTR | `172.16.5.2` |
| `files` | A + PTR | `172.16.5.10` |
| `mail` | A + PTR | `172.16.5.11` |
| `dokumenty` | CNAME | `files.biuro.local` |
| `biuro.local` | MX, priorytet 10 | `mail.biuro.local` |

- [ ] Zrzut ekranu: rekordy w strefie do przodu
- [ ] Zrzut ekranu: rekordy PTR w strefie wstecznej
- [ ] Zrzut ekranu: z klienta `nslookup` dla `dokumenty.biuro.local`, `172.16.5.11` i rekordu MX

## Zadanie 3: Rezerwacja z nazwą w DNS

Klient ma być stacją kadrową z adresem stałym i własną nazwą w DNS.

- [ ] Rezerwacja DHCP adresu `172.16.5.60` dla adresu MAC klienta
- [ ] Rekordy A i PTR `kadry` → `172.16.5.60`
- [ ] Zrzut ekranu: klient ma adres `.60`, a z serwera działa `ping kadry.biuro.local`
- [ ] Krótki komentarz: dlaczego dla tej stacji rezerwacja + rekord DNS są lepsze niż adres statyczny

## Zadanie 4: Całość w skrypcie

Napisz skrypt `infra-biuro.ps1`, który na świeżo zainstalowanym serwerze (z już ustawionym adresem statycznym) wykonuje zadania 1–3 bez użycia GUI.

- [ ] Skrypt instaluje obie role
- [ ] Skrypt tworzy strefy i wszystkie rekordy z zadania 2
- [ ] Skrypt tworzy zakres, opcje i rezerwację
- [ ] Skrypt na końcu wyświetla podsumowanie: strefy, rekordy, zakres, rezerwacje
- [ ] Zrzut ekranu: uruchomienie skryptu na przywróconym snapshocie i test z klienta

## Zadanie 5: Rozwiąż usterkę

Nauczyciel (lub kolega z ławki) wprowadzi na Twoim serwerze jedną usterkę w DNS lub DHCP. Znajdź ją i usuń.

- [ ] Opis objawu widocznego na kliencie
- [ ] Lista kroków diagnostycznych (polecenia i wnioski)
- [ ] Wskazana przyczyna i sposób naprawy
- [ ] Zrzut ekranu poprawnego działania po naprawie

## Zadanie 6: Krótki quiz sprawdzający (bez VM)

??? question "1. Port i protokół"
    Na jakim porcie działa DNS? Kiedy używany jest TCP zamiast UDP?

??? question "2. Rekursja"
    Czym różni się zapytanie rekursywne od iteracyjnego? Kto wysyła które z nich?

??? question "3. Strefa wsteczna"
    Podaj nazwę strefy wyszukiwania wstecznego dla sieci `192.168.77.0/24`.

??? question "4. CNAME"
    Dlaczego rekord CNAME nie powinien wskazywać na adres IP?

??? question "5. Priorytet MX"
    Domena ma dwa rekordy MX: priorytet 10 i priorytet 30. Który serwer poczty jest podstawowy?

??? question "6. DHCP i DNS razem"
    Które opcje DHCP odpowiadają za to, że klient wie, jakiego serwera DNS używać i jaki sufiks dopisywać do krótkich nazw?

??? question "7. TTL"
    Zmieniłeś adres w rekordzie A, ale część klientów nadal łączy się ze starym adresem. Co jest najbardziej prawdopodobną przyczyną i jak ją usunąć?
