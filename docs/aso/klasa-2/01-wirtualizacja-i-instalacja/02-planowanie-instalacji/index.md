# Planowanie instalacji

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić różnicę między typami partycji, rozróżnić podstawowe systemy plików używane przez Windows i Linux, oraz świadomie zaplanować podział dysku przed rozpoczęciem instalacji systemu.

## Dlaczego planowanie, a nie od razu instalacja

Łatwo jest kliknąć "Dalej, Dalej, Dalej" w instalatorze i zaakceptować wszystkie ustawienia domyślne. Problem w tym, że **niektórych decyzji nie da się łatwo cofnąć po instalacji** — zmiana schematu partycji na działającym systemie bywa ryzykowna lub wymaga ponownej instalacji od zera. Dlatego administrator planuje podział dysku **przed** uruchomieniem instalatora, a nie w trakcie.

## Partycje — co to takiego

**Partycja** to logicznie wydzielony fragment fizycznego (albo w naszym przypadku — wirtualnego) dysku, traktowany przez system operacyjny jako osobny dysk. Jeden dysk fizyczny może być podzielony na kilka partycji, z których każda może mieć inny system plików i inne przeznaczenie.

```text
Dysk fizyczny (np. 100 GB)
├── Partycja 1 (np. 100 MB) — partycja rozruchowa (boot)
├── Partycja 2 (np. 80 GB) — system operacyjny i programy
└── Partycja 3 (np. 20 GB) — dane użytkownika
```


## Style partycjonowania: MBR i GPT

- **MBR (Master Boot Record)** — starszy standard, obsługuje maksymalnie **4 partycje podstawowe** i dyski do 2 TB. Nadal spotykany, głównie ze względów kompatybilności ze starszym sprzętem.
- **GPT (GUID Partition Table)** — nowszy standard, obsługuje **znacznie więcej partycji** (praktycznie do 128) i dyski o dużo większej pojemności niż 2 TB. Wymagany przy instalacji w trybie UEFI (nowoczesny odpowiednik starszego BIOS-u).

!!! tip "Co wybrać na maszynie wirtualnej"
    Na nowych instalacjach (i tak będzie w naszych ćwiczeniach) domyślnym, zalecanym wyborem jest **GPT** — to standard używany we współczesnych wdrożeniach zarówno Windows, jak i Linux.

??? question "Zadanie 1: MBR czy GPT?"
    Firma modernizuje serwer, na którym ma być zainstalowany dysk o pojemności 4 TB. Który styl partycjonowania musi zostać użyty i dlaczego drugi z nich się nie nadaje?

??? question "Zadanie 2: Policz partycje"
    Ile maksymalnie partycji podstawowych obsługuje MBR? Jaka jest praktyczna konsekwencja tego ograniczenia dla administratora planującego podział dysku?

## Systemy plików — różne dla różnych systemów operacyjnych

System plików określa, w jaki sposób dane są organizowane i zapisywane na partycji. Windows i Linux domyślnie korzystają z innych systemów plików.

### Systemy plików Windows

- **NTFS** — domyślny, standardowy system plików współczesnego Windows. Obsługuje duże pliki, uprawnienia dostępu na poziomie plików i folderów, kompresję, szyfrowanie.
- **FAT32** — starszy, prostszy system plików, ograniczony do plików o maksymalnym rozmiarze 4 GB. Nadal używany na przenośnych nośnikach (pendrive'y) ze względu na szeroką kompatybilność.
- **exFAT** — nowszy następca FAT32, bez ograniczenia rozmiaru pliku, również popularny na nośnikach przenośnych.

### Systemy plików Linux

- **ext4** — najczęściej używany, domyślny system plików większości dystrybucji Linuksa (w tym Ubuntu, którego będziemy używać). Stabilny, sprawdzony, dobrze udokumentowany.
- **XFS** — spotykany częściej w środowiskach serwerowych wymagających obsługi bardzo dużych plików i wysokiej wydajności.
- **Btrfs** — nowszy system plików z zaawansowanymi funkcjami (snapshoty na poziomie systemu plików, kompresja), zyskujący popularność, ale wciąż rzadziej domyślny niż ext4.

!!! warning "Częsty błąd — mylenie systemów plików między systemami"
    Windows domyślnie **nie potrafi** zapisywać na partycjach ext4 bez dodatkowego oprogramowania, a Linux **nie potrafi w pełni** odczytywać niektórych funkcji NTFS (choć podstawowy odczyt/zapis zwykle działa). To ważne przy planowaniu dysków współdzielonych między systemami.

??? question "Zadanie 3: Dopasuj system plików"
    Dla każdej sytuacji wskaż odpowiedni system plików i uzasadnij:
    a) Partycja systemowa dla świeżo instalowanego Windows 11
    b) Partycja systemowa dla Ubuntu Server
    c) Pendrive, który ma być odczytywany zarówno na Windows, jak i na macOS

??? question "Zadanie 4: Zbadaj własny system plików"
    Na komputerze, na którym pracujesz, sprawdź (za pomocą narzędzi systemowych — np. "Zarządzanie dyskami" w Windows albo `df -T` w Linuksie), jaki system plików jest używany na partycji systemowej. Zrób zrzut ekranu wyniku.

## Planowanie podziału dysku — praktyczne podejście

Przy planowaniu instalacji administrator zadaje sobie kilka pytań:

1. **Ile partycji potrzebuję?** — minimalnie jedna (system + dane razem), ale często warto rozdzielić system od danych użytkownika (łatwiejsza reinstalacja systemu bez utraty danych)
2. **Jak duże mają być poszczególne partycje?** — zależnie od przeznaczenia maszyny (serwer plików potrzebuje dużo miejsca na dane, serwer aplikacji może wymagać więcej miejsca na sam system i logi)
3. **Czy zostawić miejsce na przyszłą rozbudowę?** — dobra praktyka to nie wykorzystywać 100% dostępnego miejsca od razu

??? question "Zadanie 5: Zaplanuj podział dysku — serwer plików"
    Masz zaplanować podział 100 GB dysku dla serwera, którego głównym zadaniem będzie przechowywanie i udostępnianie plików pracownikom firmy. Zaproponuj podział na partycje (ile, jakiej wielkości, jakie przeznaczenie) i uzasadnij swoje decyzje.

??? question "Zadanie 6: Zaplanuj podział dysku — stacja robocza ucznia"
    To samo zadanie, ale dla 60 GB dysku maszyny wirtualnej, która będzie stacją roboczą do nauki i testowania oprogramowania na zajęciach. Czy podział będzie taki sam jak w zadaniu 5? Uzasadnij różnice lub ich brak.

## Zadania podsumowujące

??? question "Zadanie 7: Przygotuj plan przed instalacją"
    Zanim przejdziesz do kolejnych lekcji (instalacja Windows i Linux), zaplanuj na piśmie: styl partycjonowania (MBR/GPT), system plików i podział dysku, który zastosujesz przy instalacji obu systemów na swoich maszynach wirtualnych z poprzedniej lekcji. Zachowaj te notatki — będziesz z nich korzystać w praktyce.

??? question "Zadanie 8: Pytanie problemowe"
    Kolega instaluje Windows na partycji sformatowanej jako ext4 (bo tak przypadkiem zostało po poprzedniej instalacji Linuksa) i instalator zgłasza błąd. Wyjaśnij, dlaczego tak się dzieje, powołując się na to, czego nauczyłeś się w tej lekcji.
