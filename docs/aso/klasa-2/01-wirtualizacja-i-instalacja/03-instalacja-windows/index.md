# Instalacja Windows

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć samodzielnie zainstalować system Windows jako maszynę wirtualną w VirtualBox, od utworzenia maszyny po pierwsze uruchomienie gotowego systemu.

!!! tip "Wykorzystaj plan z poprzedniej lekcji"
    W lekcji "Planowanie instalacji" przygotowałeś notatki dotyczące stylu partycjonowania, systemu plików i podziału dysku. Skorzystaj z nich teraz — to jest moment, w którym zamieniasz plan w rzeczywistą konfigurację.

## Czego potrzebujesz przed rozpoczęciem

- Zainstalowany VirtualBox (z poprzedniej lekcji)
- Plik instalacyjny systemu Windows w formacie ISO (dostarczony przez nauczyciela — sprawdź materiał udostępniony na zajęciach)
- Zaplanowane parametry maszyny wirtualnej: ilość RAM, rozmiar dysku, styl partycjonowania

## Krok 1 — utworzenie maszyny wirtualnej

1. W VirtualBox kliknij **"Nowa"** (New)
2. Nadaj nazwę (np. `Windows-Server-2022`) — VirtualBox rozpozna typ systemu na podstawie nazwy i dobierze sensowne ustawienia domyślne
3. Ustaw ilość pamięci RAM zgodnie z Twoim planem z poprzedniej lekcji (dla Windows Server zalecane minimum to 2 GB, w praktyce lepiej 4 GB, jeśli host na to pozwala)
4. Utwórz nowy wirtualny dysk twardy — format **VDI**, alokacja **dynamiczna** (dysk rośnie w miarę zapełniania, zamiast od razu zajmować pełny zadeklarowany rozmiar)

!!! tip "Alokacja dynamiczna vs. stała"
    Dysk dynamiczny zajmuje na hoście tylko tyle miejsca, ile faktycznie wykorzystuje gość — wygodne przy nauce, gdzie nie wiesz z góry, ile miejsca faktycznie zajmie system. Dysk o stałym rozmiarze działa nieco szybciej, ale od razu rezerwuje całą zadeklarowaną przestrzeń.

## Krok 2 — podpięcie obrazu ISO

1. Zaznacz nowo utworzoną maszynę, wejdź w **Ustawienia → Nośniki (Storage)**
2. Kliknij ikonę pustej płyty przy kontrolerze, wybierz **"Wybierz plik dysku..."**, wskaż pobrany plik ISO z systemem Windows

!!! warning "Częsty błąd"
    Zapomnienie podpięcia ISO to najczęstsza przyczyna, dla której maszyna wirtualna "nie chce się uruchomić" — bez podpiętego nośnika instalacyjnego VirtualBox nie ma z czego zainstalować systemu.

## Krok 3 — uruchomienie instalatora

1. Uruchom maszynę (przycisk **"Start"**)
2. Instalator Windows uruchomi się automatycznie z podpiętego ISO — wybierz język, format czasu i klawiatury
3. Kliknij **"Zainstaluj teraz"**

## Krok 4 — partycjonowanie zgodnie z planem

To moment, w którym wykorzystujesz notatki z poprzedniej lekcji:

1. Instalator pokaże listę dostępnych dysków (w tym przypadku — Twojego wirtualnego dysku)
2. Jeśli planujesz **więcej niż jedną partycję**, użyj opcji "Nowa" i podaj rozmiar w MB dla każdej partycji zgodnie z planem
3. Wybierz partycję docelową dla systemu i kliknij "Dalej"

!!! info "Windows sam tworzy dodatkowe partycje systemowe"
    Nie zdziw się, jeśli po zatwierdzeniu instalator pokaże więcej partycji, niż zaplanowałeś — Windows automatycznie dokłada niewielkie partycje systemowe (np. partycję odzyskiwania, partycję EFI) potrzebne do prawidłowego działania i rozruchu systemu.

## Krok 5 — dokończenie instalacji i pierwsze uruchomienie

1. Instalator skopiuje pliki i kilkukrotnie automatycznie zrestartuje maszynę — to normalne, nie przerywaj tego procesu
2. Po zakończeniu kopiowania system poprosi o ustawienia regionalne, utworzenie konta administratora i hasło
3. Poczekaj na pierwsze pełne uruchomienie pulpitu

??? question "Zadanie 1: Zainstaluj Windows zgodnie z planem"
    Wykonaj instalację Windows w VirtualBox, wykorzystując parametry zaplanowane w poprzedniej lekcji. Dokumentuj proces zrzutami ekranu na kluczowych etapach: tworzenie maszyny, ekran partycjonowania, pierwsze uruchomienie pulpitu.

??? question "Zadanie 2: Zweryfikuj partycje po instalacji"
    Po zakończonej instalacji, w zainstalowanym systemie otwórz narzędzie "Zarządzanie dyskami" (Disk Management). Porównaj rzeczywisty układ partycji z tym, co zaplanowałeś — zapisz, czy się zgadza, i jeśli nie, co się różni i dlaczego (przypomnij sobie informację o automatycznie tworzonych partycjach systemowych).

## Dodatki Gościa VirtualBox (Guest Additions)

Po zainstalowaniu systemu warto zainstalować **Dodatki Gościa** — specjalny pakiet sterowników i narzędzi poprawiających integrację maszyny wirtualnej z hostem (m.in. lepsza rozdzielczość ekranu, współdzielony schowek, płynniejsza praca myszką).

1. W uruchomionej maszynie, z menu VirtualBox wybierz **Urządzenia → Wstaw obraz CD Dodatków Gościa**
2. Uruchom instalator, który pojawi się w systemie gościa, i przejdź przez kreator
3. Zrestartuj maszynę wirtualną po zakończeniu

??? question "Zadanie 3: Zainstaluj Dodatki Gościa"
    Zainstaluj Dodatki Gościa w swojej maszynie z Windows. Sprawdź, czy po restarcie rozdzielczość ekranu automatycznie dopasowuje się do rozmiaru okna VirtualBox — to najprostszy sposób sprawdzenia, że instalacja się powiodła.

## Zadania podsumowujące

??? question "Zadanie 4: Sprawdź podstawowe informacje o systemie"
    W zainstalowanym Windows, za pomocą narzędzia systemowego (np. `Ustawienia → System → O systemie` albo polecenie `systeminfo` w wierszu poleceń), znajdź i zapisz: wersję systemu, ilość dostępnej pamięci RAM, typ systemu (32/64-bit).

??? question "Zadanie 5: Opisz etapy uruchamiania systemu"
    Na podstawie tego, co zaobserwowałeś podczas instalacji i pierwszych uruchomień, opisz własnymi słowami, jakie etapy przechodzi Windows od włączenia maszyny wirtualnej do pojawienia się pulpitu.

??? question "Zadanie 6: Refleksja nad planowaniem"
    Czy podczas instalacji natrafiłeś na coś, czego nie przewidziałeś w planie z poprzedniej lekcji? Opisz sytuację i to, jak sobie z nią poradziłeś.
