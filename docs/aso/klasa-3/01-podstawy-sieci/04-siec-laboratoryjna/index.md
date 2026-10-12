# Sieć laboratoryjna

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć połączyć serwer i klienta w odizolowaną sieć w VirtualBox, nadać klientowi adres testowy, zezwolić na ping w zaporze Windows i krok po kroku zdiagnozować brak łączności.

## Tryby karty sieciowej w VirtualBox

| Tryb | Co widzi maszyna | Kiedy używać |
|---|---|---|
| **NAT** | internet przez hosta; inne VM jej nie widzą | instalacja aktualizacji, pobieranie plików |
| **Bridged Adapter** | całą sieć szkolną, jak osobny komputer | **nie** przy serwerach DHCP — zakłócisz sieć szkoły |
| **Internal Network** | tylko inne VM podłączone do sieci o tej samej nazwie | **nasz lab: serwer + klient** |
| **Host-only Adapter** | tylko host i inne VM w tej sieci | dostęp do VM z hosta |

## Budowa labu

1. Wyłącz obie maszyny (zmiany karty sieciowej wprowadza się przy wyłączonej VM).
2. `SRV01` → **Settings → Network → Adapter 1** → *Attached to:* **Internal Network**, *Name:* `lab`.
3. To samo na `CLIENT01` — **dokładnie ta sama nazwa** `lab`.
4. Uruchom obie maszyny.

!!! warning "Przed instalacją serwera DHCP klient nie dostanie adresu"
    W sieci wewnętrznej VirtualBox nie ma żadnego serwera DHCP. Do czasu, aż sami go zainstalujemy, klient po starcie dostanie adres APIPA `169.254.x.x`. Na potrzeby tej lekcji nadaj mu adres statyczny testowy — w dziale o DHCP przełączysz go z powrotem na automatyczny.

Konfiguracja klienta (Windows 10/11, ustawienie tymczasowe):

| Parametr | Wartość |
|---|---|
| IP address | `192.168.10.20` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `192.168.10.1` |
| Preferred DNS | `192.168.10.10` |

## Zapora blokuje ping

Pierwsza próba `ping 192.168.10.10` z klienta najczęściej kończy się komunikatem *Request timed out* — mimo poprawnej konfiguracji. Zapora Windows Defender domyślnie **blokuje przychodzące żądania echa ICMP**, czyli odpowiedzi na ping.

**GUI na serwerze:** ++win+r++ → `wf.msc` → **Inbound Rules** → odszukaj **File and Printer Sharing (Echo Request - ICMPv4-In)** → prawy przycisk → **Enable Rule** (dla wszystkich profili).

**PowerShell:**

```powershell
Enable-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)"
Get-NetFirewallRule -DisplayName "*Echo Request - ICMPv4*" |
    Select-Object DisplayName, Profile, Enabled
```

To samo zrób na kliencie, jeśli chcesz pingować go z serwera. Na polskim Windows reguła nazywa się *Udostępnianie plików i drukarek (żądanie echa — ICMPv4 — ruch przychodzący)*.

!!! tip "Nie wyłączaj całej zapory"
    Wyłączenie zapory „żeby działało” to nawyk, który na egzaminie i w pracy kosztuje punkty i bezpieczeństwo. Włączasz **jedną konkretną regułę** potrzebną do zadania.

## Diagnostyka krok po kroku

Gdy klient nie widzi serwera, sprawdzaj od najniższej warstwy w górę:

| Krok | Polecenie / miejsce | Czego szukasz |
|---|---|---|
| 1. Fizyka (wirtualna) | VirtualBox → Settings → Network | ten sam tryb i nazwa sieci, *Cable Connected* zaznaczone |
| 2. Karta w systemie | `Get-NetAdapter` | `Status: Up` |
| 3. Adres | `ipconfig /all` | adres nie zaczyna się od `169.254`, ta sama sieć i maska co serwer |
| 4. Pętla zwrotna | `ping 127.0.0.1` | stos TCP/IP działa |
| 5. Własny adres | `ping 192.168.10.20` | karta odpowiada |
| 6. Serwer | `ping 192.168.10.10` | brak odpowiedzi przy poprawnych krokach 1–5 → zapora |

```powershell
Test-Connection 192.168.10.10 -Count 2
Test-NetConnection 192.168.10.10          # ping + szczegóły trasy
Get-NetNeighbor -AddressFamily IPv4       # tablica ARP: czy znamy MAC serwera
```

## Zadania

??? question "Zadanie 1: Zbuduj lab"
    Skonfiguruj karty sieciowe `SRV01` i `CLIENT01` w trybie Internal Network `lab`. Nadaj klientowi adres testowy z tabeli. Zrób zrzuty ekranu ustawień sieci obu maszyn w VirtualBox.

??? question "Zadanie 2: Pierwszy ping"
    Wykonaj ping z klienta do serwera przed zmianą reguł zapory. Zapisz wynik. Włącz regułę ICMPv4 na serwerze w `wf.msc` i powtórz ping. Udokumentuj oba wyniki i włączoną regułę.

??? question "Zadanie 3: Ping w obie strony"
    Włącz regułę na kliencie za pomocą PowerShell i wykonaj ping z serwera do klienta. Pokaż wynik `Get-NetFirewallRule` z włączonymi regułami.

??? question "Zadanie 4: Tablica ARP"
    Po udanym pingu wyświetl na kliencie tablicę ARP (`arp -a` lub `Get-NetNeighbor`). Znajdź adres MAC serwera i porównaj go z adresem MAC widocznym na serwerze w `ipconfig /all`.

??? question "Zadanie 5: Trzy usterki"
    Wprowadź po kolei (i za każdym razem cofnij) trzy usterki: inna nazwa sieci wewnętrznej na kliencie, maska `/16` na kliencie przy adresie `192.168.11.20`, odłączony kabel (*Cable Connected*). Dla każdej zapisz objaw i krok z tabeli diagnostycznej, na którym ją wykryłeś.
