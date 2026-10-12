# Wykluczenia, rezerwacje, opcje

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyłączyć wybrane adresy z puli, zarezerwować stały adres dla konkretnego urządzenia, rozróżnić opcje serwera i opcje zakresu oraz zdiagnozować typowe błędy konfiguracji DHCP.

## Wykluczenia (exclusions)

Czasem w środku puli znajdzie się urządzenie z adresem ustawionym ręcznie — np. stary przełącznik zarządzalny z adresem `192.168.10.150`, którego nikt nie chce przekonfigurowywać. Gdyby serwer DHCP rozdał ten adres komputerowi, powstałby **konflikt adresów**.

**Wykluczenie** to adres lub zakres adresów wewnątrz puli, których serwer nigdy nie rozda.

**GUI:** konsola DHCP → `IPv4 → Scope → Address Pool` → prawy przycisk → **New Exclusion Range…**

**PowerShell:**

```powershell
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.10.0 `
    -StartRange 192.168.10.150 -EndRange 192.168.10.150
Get-DhcpServerv4ExclusionRange -ScopeId 192.168.10.0
```

## Rezerwacje (reservations)

Drukarka sieciowa powinna mieć **zawsze ten sam adres** — inaczej komputery przestaną ją znajdować. Można ustawić jej adres ręcznie, ale wtedy znów tracimy centralne zarządzanie.

**Rezerwacja** wiąże adres IP z **adresem MAC** urządzenia: urządzenie nadal pobiera konfigurację z DHCP, ale zawsze dostaje ten sam adres (a przy okazji aktualną bramę i DNS).

| | Adres statyczny | Rezerwacja DHCP |
|---|---|---|
| Gdzie konfigurujesz | na urządzeniu | na serwerze |
| Zmiana DNS w sieci | ręcznie na każdym urządzeniu | automatycznie |
| Typowe zastosowanie | sam serwer DHCP, router, serwery | drukarki, kamery, punkty dostępowe |

Adres MAC klienta odczytasz poleceniem `ipconfig /all` (*Physical Address*) lub z listy dzierżaw na serwerze.

**GUI:** `IPv4 → Scope → Reservations` → prawy przycisk → **New Reservation…** — nazwa, adres IP, adres MAC (bez separatorów albo z myślnikami), typ *Both*.

Wygodny skrót: w `Address Leases` prawy przycisk na istniejącej dzierżawie → **Add to Reservation**.

**PowerShell:**

```powershell
Add-DhcpServerv4Reservation -ScopeId 192.168.10.0 -IPAddress 192.168.10.50 `
    -ClientId "08-00-27-AB-CD-EF" -Name "CLIENT01" -Description "Stanowisko nauczyciela"
Get-DhcpServerv4Reservation -ScopeId 192.168.10.0
```

!!! warning "Adres rezerwacji musi należeć do zakresu"
    Windows Server nie pozwoli utworzyć rezerwacji dla adresu spoza granic zakresu — przy zakresie `.100–.200` adres `.50` zostanie odrzucony.

    Typowe rozwiązanie: rozszerz zakres do `.50–.200` i dodaj **wykluczenie** `.50–.99`. Adresy wykluczone nie są rozdawane dynamicznie, ale rezerwacje w tym przedziale działają. Urządzenia z rezerwacją masz wtedy w osobnym, łatwym do zapamiętania przedziale.

## Opcje DHCP: serwera, zakresu, rezerwacji

Opcje można ustawić na trzech poziomach. Im węższy poziom, tym wyższy priorytet:

```text
Server Options        → dotyczą wszystkich zakresów na serwerze
  └── Scope Options   → dotyczą jednego zakresu (nadpisują opcje serwera)
        └── Reservation Options → dotyczą jednego urządzenia (nadpisują opcje zakresu)
```

Najczęściej używane opcje:

| Numer | Nazwa | Przykład |
|---|---|---|
| 003 | Router | `192.168.10.1` |
| 006 | DNS Servers | `192.168.10.10` |
| 015 | DNS Domain Name | `firma.local` |
| 042 | NTP Servers | serwer czasu |

```powershell
Set-DhcpServerv4OptionValue -DnsServer 192.168.10.10 -Force                       # serwer
Set-DhcpServerv4OptionValue -ScopeId 192.168.10.0 -Router 192.168.10.1           # zakres
Set-DhcpServerv4OptionValue -ReservedIP 192.168.10.50 -Router 192.168.10.254     # rezerwacja
Get-DhcpServerv4OptionValue -ScopeId 192.168.10.0
```

Klient otrzyma nowe opcje dopiero przy odnowieniu dzierżawy — w labie wymuszasz to poleceniem `ipconfig /renew`.

## Typowe problemy

| Objaw na kliencie | Najczęstsza przyczyna |
|---|---|
| adres `169.254.x.x` | brak łączności z serwerem (sieć VirtualBox), zatrzymana usługa DHCP Server, zakres nieaktywny |
| adres jest, ale brak internetu / nazw | brak lub błędna opcja 003 (brama) lub 006 (DNS) |
| komunikat o konflikcie adresów | adres statyczny urządzenia leży w puli bez wykluczenia |
| klient nie dostaje adresu, pula pełna | wszystkie adresy wydzierżawione — za mała pula lub za długa dzierżawa |
| rezerwacja nie działa | błędny adres MAC (literówka, MAC innej karty) |

Na serwerze sprawdzisz statystykę zakresu:

```powershell
Get-DhcpServerv4ScopeStatistics       # ile adresów wolnych, zajętych, procent użycia
Get-Service DHCPServer                 # czy usługa działa
```

## Zadania

??? question "Zadanie 1: Wykluczenie"
    W zakresie `LAN-Klasa` wyklucz adresy `192.168.10.180 – 192.168.10.190`. Pokaż wynik w konsoli DHCP i przez `Get-DhcpServerv4ExclusionRange`.

??? question "Zadanie 2: Rezerwacja dla klienta"
    Odczytaj adres MAC karty `CLIENT01`. Utwórz rezerwację adresu `192.168.10.50` dla tego klienta, dostosowując zakres tak, aby rezerwacja zadziałała. Na kliencie odnów adres i pokaż, że otrzymał `.50`. Opisz, co musiałeś zmienić w zakresie.

??? question "Zadanie 3: Priorytet opcji"
    Ustaw opcję 006 (DNS) na poziomie serwera na `8.8.8.8`, a na poziomie zakresu na `192.168.10.10`. Sprawdź na kliencie, jaki serwer DNS otrzymał. Następnie dodaj opcję 006 dla rezerwacji z adresem `1.1.1.1` i sprawdź ponownie. Udokumentuj wyniki i wyjaśnij je.

??? question "Zadanie 4: Statyczny czy rezerwacja"
    Dla każdego urządzenia zdecyduj: adres statyczny, rezerwacja czy zwykła dzierżawa. Uzasadnij każdą decyzję: serwer DHCP, drukarka w sekretariacie, laptop gościa, kamera IP przy wejściu, router, komputer w pracowni.

??? question "Zadanie 5: Symulacja usterki"
    Zatrzymaj usługę `DHCPServer` na serwerze. Na kliencie zwolnij i odnów adres. Zapisz objaw, a potem przeprowadź diagnostykę tak, jakbyś nie wiedział, co się stało: zapisz kolejne polecenia i wnioski, aż do przywrócenia działania.

??? question "Zadanie 6: Pełna pula"
    Zmień zakres na `192.168.10.100 – 192.168.10.100` (jeden adres). Co stanie się, gdy drugi klient poprosi o adres? Sprawdź statystykę zakresu poleceniem `Get-DhcpServerv4ScopeStatistics` i opisz wynik. Przywróć poprzedni zakres.
