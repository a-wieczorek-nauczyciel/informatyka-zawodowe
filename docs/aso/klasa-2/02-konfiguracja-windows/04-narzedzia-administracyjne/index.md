# Narzędzia administracyjne

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć otworzyć i wykorzystać najważniejsze narzędzia administracyjne Windows: Computer Management, Device Manager, Event Viewer, Services, Task Manager i System Information — oraz wiedzieć, które z nich wybrać przy konkretnym problemie.

## Mapa narzędzi

| Narzędzie | Skrót | Do czego |
|---|---|---|
| **Computer Management (Zarządzanie komputerem)** | `compmgmt.msc` | jedno okno, w którym są m.in. Event Viewer, użytkownicy i grupy lokalne, Device Manager, Disk Management, Services |
| **Device Manager (Menedżer urządzeń)** | `devmgmt.msc` | sprzęt i sterowniki |
| **Event Viewer (Podgląd zdarzeń)** | `eventvwr.msc` | dzienniki systemu: błędy, ostrzeżenia, logowania |
| **Services (Usługi)** | `services.msc` | usługi działające w tle |
| **Disk Management (Zarządzanie dyskami)** | `diskmgmt.msc` | partycje, woluminy, litery dysków |
| **Task Manager (Menedżer zadań)** | ++ctrl+shift+esc++ | procesy, wydajność, programy uruchamiane przy starcie |
| **System Information** | `msinfo32` | pełny opis sprzętu i systemu |
| **Performance Monitor** | `perfmon` | wykresy wydajności, raporty |
| **Windows Tools / Administrative Tools** | `control admintools` | folder ze wszystkimi narzędziami |

Wszystkie otworzysz przez ++win+r++, wpisując skrót z tabeli. Większość wymaga uprawnień administratora.

## Device Manager — sprzęt i sterowniki

Każde urządzenie ma **sterownik** — program, który pozwala systemowi z nim rozmawiać.

| Ikona przy urządzeniu | Znaczenie |
|---|---|
| żółty trójkąt z wykrzyknikiem | problem z urządzeniem, najczęściej brak sterownika |
| strzałka w dół | urządzenie wyłączone |
| *Unknown device* w *Other devices* | system nie rozpoznał urządzenia |

Prawy przycisk na urządzeniu:

- **Update driver** — wyszukaj nowszy sterownik (automatycznie albo z folderu),
- **Properties → Driver → Roll Back Driver** — wróć do poprzedniego sterownika, gdy nowy sprawia problemy,
- **Disable device** / **Uninstall device**.

!!! tip "Sterowniki w maszynie wirtualnej"
    W VirtualBox brakujące sterowniki karty graficznej i integracji z hostem instalują **Dodatki Gościa (Guest Additions)**. Po ich instalacji znika większość żółtych trójkątów.

## Event Viewer — co się działo w systemie

Dzienniki w **Windows Logs**:

| Dziennik | Co zawiera |
|---|---|
| **Application** | zdarzenia programów |
| **Security** | logowania, zmiany uprawnień (audyt) |
| **Setup** | instalacje aktualizacji i funkcji |
| **System** | sterowniki, usługi, uruchamianie i zamykanie systemu |

Poziomy zdarzeń: **Critical**, **Error**, **Warning**, **Information**. Przy problemie zaczynasz od **System** i **Application**, a w panelu po prawej używasz **Filter Current Log** — np. tylko błędy z ostatniej doby.

To samo w PowerShell:

```powershell
Get-EventLog -LogName System -Newest 10
Get-EventLog -LogName System -EntryType Error -Newest 10
Get-WinEvent -LogName Application -MaxEvents 5
```

## Services — usługi

**Usługa** to program działający w tle, niezależnie od zalogowanego użytkownika. W oknie `services.msc` każda usługa ma:

- **Status** — czy działa teraz (*Running*),
- **Startup Type** — co się stanie po uruchomieniu systemu: *Automatic*, *Automatic (Delayed Start)*, *Manual*, *Disabled*.

Dwuklik na usłudze pozwala ją zatrzymać, uruchomić i zmienić typ uruchamiania. Na karcie **Dependencies** widać, od jakich usług zależy.

## Task Manager

- **Processes** — co działa i ile zużywa (CPU, pamięć, dysk, sieć); **End task** kończy zawieszony program.
- **Performance** — wykresy wykorzystania sprzętu.
- **Startup apps** — programy uruchamiane przy logowaniu; wyłączenie zbędnych przyspiesza start.
- **Users** — kto jest zalogowany.

## Który do czego

| Problem | Narzędzie |
|---|---|
| komputer nagle się zrestartował | Event Viewer → System |
| nie działa karta dźwiękowa | Device Manager |
| system długo się uruchamia | Task Manager → Startup apps |
| drukowanie przestało działać | Services → Print Spooler |
| trzeba sprawdzić model procesora i ilość RAM | System Information (`msinfo32`) |
| trzeba założyć partycję na nowym dysku | Disk Management |

## Zadania

??? question "Zadanie 1: Inwentaryzacja sprzętu"
    Za pomocą `msinfo32` i Device Manager zapisz: model procesora, ilość pamięci RAM, nazwę karty sieciowej i wersję jej sterownika. Sprawdź, czy w Device Manager jest urządzenie z żółtym trójkątem.

??? question "Zadanie 2: Sterownik"
    Wybierz kartę sieciową w Device Manager. Wyłącz ją i sprawdź skutek (`ipconfig`). Włącz ją ponownie. Sprawdź, czy przycisk **Roll Back Driver** jest aktywny, i wyjaśnij, od czego to zależy.

??? question "Zadanie 3: Dziennik zdarzeń"
    W Event Viewer odfiltruj z dziennika **System** wyłącznie błędy z ostatnich 7 dni. Wybierz jeden i zapisz: datę, źródło (*Source*), identyfikator (*Event ID*) i opis. To samo znajdź poleceniem PowerShell.

??? question "Zadanie 4: Usługa"
    W `services.msc` znajdź usługę **Print Spooler**. Zatrzymaj ją, ustaw typ uruchamiania na *Manual*, uruchom ponownie system i sprawdź jej stan. Przywróć ustawienia. Odszukaj w Event Viewer wpis o zatrzymaniu tej usługi.

??? question "Zadanie 5: Computer Management"
    Otwórz `compmgmt.msc` i wypisz wszystkie narzędzia dostępne w lewym panelu. Przy każdym zapisz jednym zdaniem, do czego służy.
