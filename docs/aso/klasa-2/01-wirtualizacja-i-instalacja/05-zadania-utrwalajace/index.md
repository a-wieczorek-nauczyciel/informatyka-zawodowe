# Zadania utrwalające

Zadania podsumowujące dział "Wirtualizacja i instalacja systemów operacyjnych". W przeciwieństwie do zadań teoretycznych z poprzednich lekcji, to są **zadania praktyczne do wykonania na maszynach wirtualnych** — dokumentujesz je zrzutami ekranu, tak jak wymaga tego prawdziwy egzamin zawodowy INF.02.

!!! tip "Jak dokumentować zadania praktyczne"
    Dla każdego zadania rób zrzuty ekranu w kluczowych momentach (nie tylko na końcu) i zapisuj je w uporządkowanym folderze, np. `zadanie-1/01-start.png`, `zadanie-1/02-partycje.png`. To dokładnie ten nawyk, którego oczekuje się na egzaminie zawodowym.

## Zadanie 1: Kompletna maszyna Windows od zera

Stwórz nową maszynę wirtualną i przeprowadź na niej pełną instalację Windows, dokumentując każdy etap.

- [ ] Utworzona maszyna wirtualna z parametrami: min. 2 GB RAM, dysk VDI dynamiczny min. 40 GB
- [ ] Zrzut ekranu: ustawienia maszyny przed pierwszym uruchomieniem
- [ ] Podpięty poprawny obraz ISO Windows
- [ ] Zrzut ekranu: ekran partycjonowania z Twoim planem podziału dysku
- [ ] Zainstalowany system, utworzone konto administratora
- [ ] Zrzut ekranu: pulpit po pierwszym pełnym uruchomieniu
- [ ] Zainstalowane Dodatki Gościa
- [ ] Zrzut ekranu: `Ustawienia → System → O systemie` pokazujący wersję i parametry systemu

## Zadanie 2: Kompletna maszyna Linux od zera

To samo, tym razem dla Ubuntu.

- [ ] Utworzona maszyna wirtualna z parametrami: min. 2 GB RAM, dysk VDI dynamiczny min. 25 GB
- [ ] Zrzut ekranu: ustawienia maszyny przed pierwszym uruchomieniem
- [ ] Podpięty poprawny obraz ISO Ubuntu
- [ ] Zrzut ekranu: ekran partycjonowania
- [ ] Zainstalowany system, utworzone konto użytkownika
- [ ] Zainstalowane Dodatki Gościa (z użyciem poleceń z lekcji)
- [ ] Zrzut ekranu terminala z wynikiem poleceń: `whoami`, `hostname`, `ip a`, `df -h`

## Zadanie 3: Diagnostyka porównawcza — Windows vs. Linux

Na obu zainstalowanych systemach sprawdź i zapisz w jednym pliku tekstowym (lub tabeli) poniższe informacje:

| Informacja | Windows | Linux |
|---|---|---|
| Wersja systemu | | |
| Ilość RAM widoczna w systemie | | |
| Nazwa komputera (hostname) | | |
| Adres IP | | |
| Wykorzystanie dysku (% zajętości) | | |

- [ ] Wypełniona tabela dla obu systemów
- [ ] Zrzuty ekranu narzędzi/poleceń użytych do uzyskania każdej informacji

## Zadanie 4: Symulacja sytuacji problemowej

Wyłącz (lub usuń) maszynę wirtualną z Zadania 1 albo 2 i **odtwórz ją od zera z pamięci**, bez podglądania notatek z poprzednich lekcji — jedynie na podstawie tego, co zapamiętałeś.

- [ ] Maszyna odtworzona samodzielnie, bez notatek
- [ ] Zapisany krótki komentarz: które kroki pamiętałeś bez problemu, a przy których musiałbyś jednak zajrzeć do materiału

!!! tip "Po co to zadanie"
    To najlepszy sprawdzian tego, czy naprawdę **rozumiesz** proces, czy tylko mechanicznie klikałeś przez instrukcję krok po kroku. Na egzaminie zawodowym nie będziesz miał tej strony otwartej obok siebie.

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. MBR czy GPT?"
    Nowoczesny serwer z dyskiem 8 TB — który styl partycjonowania jest wymagany?

??? question "2. Systemy plików"
    Jaki jest domyślny system plików w świeżo zainstalowanym Ubuntu?

??? question "3. Alokacja dysku"
    Czym różni się dysk wirtualny o alokacji dynamicznej od dysku o stałym rozmiarze?

??? question "4. Guest Additions"
    Do czego służą Dodatki Gościa i dlaczego warto je zainstalować zaraz po systemie?

??? question "5. Server bez GUI"
    Dlaczego serwery produkcyjne często instaluje się bez graficznego interfejsu użytkownika?
