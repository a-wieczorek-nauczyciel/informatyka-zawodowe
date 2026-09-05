# Czym jest wirtualizacja

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, czym jest wirtualizacja i maszyna wirtualna, wymienić jej zastosowania, oraz zainstalować i skonfigurować podstawowe ustawienia VirtualBox.

## Problem, który rozwiązuje wirtualizacja

Wyobraź sobie, że chcesz nauczyć się administrować Windows Server, ale masz tylko jeden komputer z Windows 11 zainstalowanym jako główny system. Instalowanie drugiego systemu operacyjnego bezpośrednio "obok" (tzw. dual-boot) jest ryzykowne, czasochłonne i wymaga restartu komputera za każdym razem, gdy chcesz się przełączyć.

**Wirtualizacja** rozwiązuje ten problem: pozwala uruchomić **cały, w pełni działający system operacyjny wewnątrz innego systemu**, jako program. Możesz mieć uruchomiony Windows Server i Ubuntu Server jednocześnie, w oknach, na tym samym fizycznym komputerze, bez restartu i bez ryzyka dla systemu głównego.

## Kluczowe pojęcia

**Host** — fizyczny komputer i system operacyjny, na którym uruchamiasz wirtualizację (np. Twój laptop z Windows 11 albo macOS).

**Hypervisor** — oprogramowanie zarządzające maszynami wirtualnymi, pośredniczące między sprzętem fizycznym a systemami wirtualnymi. VirtualBox, którego używamy na zajęciach, jest właśnie hypervisorem.

**Maszyna wirtualna (VM — Virtual Machine)** — "udawany komputer" stworzony przez hypervisor, który z perspektywy zainstalowanego w nim systemu operacyjnego wygląda i zachowuje się jak prawdziwy, fizyczny sprzęt (ma "swój" dysk, "swoją" pamięć RAM, "swoją" kartę sieciową) — mimo że w rzeczywistości wszystko to jest symulowane przez oprogramowanie na hoście.

**Gość (Guest)** — system operacyjny zainstalowany wewnątrz maszyny wirtualnej (np. Windows Server jako gość na Twoim hoście z macOS).

!!! tip "Analogia"
    Host to budynek, hypervisor to zarządca budynku, a maszyny wirtualne to osobne, w pełni wyposażone mieszkania wewnątrz tego budynku — każde mieszkanie funkcjonuje niezależnie, mimo że fizycznie dzielą tę samą konstrukcję.

## Dwa typy hypervisorów

- **Typ 1 (bare-metal)** — hypervisor instaluje się bezpośrednio na sprzęcie, bez systemu operacyjnego "pod spodem" (np. VMware ESXi, Microsoft Hyper-V w wersji serwerowej). Używany głównie w profesjonalnych serwerowniach.
- **Typ 2 (hosted)** — hypervisor działa jako zwykła aplikacja wewnątrz istniejącego systemu operacyjnego (np. VirtualBox, VMware Workstation). To ten typ będziemy wykorzystywać na zajęciach — instalujesz VirtualBox tak jak każdy inny program.

## Do czego wirtualizacja się przydaje w praktyce

- **Nauka i testowanie** — możesz eksperymentować z konfiguracją serwera, celowo coś zepsuć, i bez konsekwencji zacząć od nowa (to jest dokładnie nasz przypadek na tym przedmiocie)
- **Odizolowanie środowisk** — testowanie oprogramowania bez ryzyka dla głównego systemu
- **Oszczędność sprzętu** — jeden fizyczny serwer może hostować kilkanaście wirtualnych serwerów jednocześnie, zamiast kupować osobny komputer do każdego zadania
- **Łatwe kopie zapasowe i przywracanie** — maszynę wirtualną można "zamrozić" w danym stanie (snapshot) i wrócić do niego w każdej chwili

??? question "Zadanie 1: Zidentyfikuj pojęcia"
    Na podstawie tej lekcji uzupełnij tabelę — dla Twojego komputera, na którym pracujesz na zajęciach, określ: co jest hostem, jaki hypervisor będziemy używać, i podaj przykład planowanego gościa.

??? question "Zadanie 2: Typ 1 czy typ 2?"
    VirtualBox instaluje się jak zwykły program na istniejącym systemie Windows albo macOS. Czy to hypervisor typu 1, czy typu 2? Uzasadnij, odwołując się do definicji z tej lekcji.

??? question "Zadanie 3: Znajdź własny przykład zastosowania"
    Podaj własny przykład sytuacji (inny niż wymienione w lekcji), w której wirtualizacja byłaby przydatna — może to być sytuacja związana z Twoimi własnymi zainteresowaniami czy planami zawodowymi.

## Instalacja VirtualBox

!!! warning "Sprawdź uprawnienia przed instalacją"
    Instalacja VirtualBox wymaga uprawnień administratora na komputerze. Jeśli pracujesz na komputerze szkolnym z ograniczonym kontem, VirtualBox może już być zainstalowany przez administratora pracowni — sprawdź to przed próbą własnej instalacji.

??? question "Zadanie 4: Zainstaluj VirtualBox (lub zweryfikuj instalację)"
    Sprawdź, czy VirtualBox jest już zainstalowany na Twoim stanowisku. Jeśli tak — uruchom go i zrób zrzut ekranu głównego okna programu. Jeśli nie, a masz uprawnienia — zainstaluj najnowszą wersję ze strony virtualbox.org.

## Podstawowa konfiguracja nowej maszyny wirtualnej

Zanim zainstalujesz jakikolwiek system, musisz stworzyć dla niego "pusty komputer" — maszynę wirtualną z określonymi parametrami:

- **Nazwa** — identyfikująca maszynę (np. `Windows-Server-2022`)
- **Ilość pamięci RAM** przydzielonej maszynie (pamiętaj: to RAM zabrany z Twojego hosta, nie dodatkowy)
- **Rozmiar wirtualnego dysku twardego** (plik na dysku hosta, symulujący dysk gościa)
- **Typ i wersja systemu operacyjnego**, który zamierzasz zainstalować (VirtualBox dostosowuje domyślne ustawienia na tej podstawie)

!!! warning "Częsty błąd początkujących"
    Przydzielenie zbyt dużej ilości RAM maszynie wirtualnej (np. 16 GB na komputerze mającym łącznie 16 GB) doprowadzi do tego, że host stanie się bardzo wolny albo w ogóle się zawiesi. Zostaw hostowi zawsze wystarczająco dużo pamięci na własne potrzeby.

??? question "Zadanie 5: Stwórz pustą maszynę wirtualną"
    W VirtualBox stwórz nową maszynę wirtualną o nazwie `Test-VM`, z 2 GB RAM i 20 GB wirtualnego dysku. Nie instaluj jeszcze żadnego systemu — na razie tylko skonfiguruj samą maszynę. Zrób zrzut ekranu ustawień.

## Zadania podsumowujące

??? question "Zadanie 6: Wyjaśnij różnicę"
    Wyjaśnij własnymi słowami różnicę między hostem a gościem, posługując się przykładem z Twojego stanowiska pracy.

??? question "Zadanie 7: Zaplanuj zasoby"
    Sprawdź, ile RAM ma fizycznie Twój komputer (host). Zaplanuj, ile RAM możesz bezpiecznie przydzielić dwóm maszynom wirtualnym jednocześnie (Windows Server + Ubuntu Server), zakładając że hostowi zostawiasz minimum 4 GB. Zapisz swoje wyliczenie i uzasadnienie.

??? question "Zadanie 8: Snapshot — po co to"
    Poszukaj w interfejsie VirtualBox opcji "Snapshot" (migawka). Przeczytaj krótki opis tej funkcji w dokumentacji VirtualBox i wyjaśnij własnymi słowami, w jakiej sytuacji podczas nauki administracji systemami przydałoby Ci się jej użycie.
