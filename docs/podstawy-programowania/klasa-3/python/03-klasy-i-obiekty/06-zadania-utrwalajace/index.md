# Zadania utrwalające

Zadania podsumowujące cały dział "Klasy i obiekty". Łączą pola, metody, konstruktory, specyfikatory dostępu i składniki statyczne — a w pytaniach teoretycznych też wątki z Javy i C++ (konstruktor kopiujący, destruktor, klasy zaprzyjaźnione).

## Poziom podstawowy

??? question "1. Klasa Film"
    Zdefiniuj klasę `Film` z konstruktorem przyjmującym `tytul` i `rok_produkcji`. Dodaj metodę `opisz()`, wypisującą oba pola w czytelnej formie. Stwórz trzy obiekty i wywołaj `opisz()` na każdym.

??? question "2. Klasa Zwierze z polem statycznym"
    Zdefiniuj klasę `Zwierze` z polem statycznym `liczba_zwierzat` (zliczającym wszystkie stworzone obiekty) i polem zwykłym `gatunek`. Stwórz pięć obiektów różnych gatunków i wypisz `Zwierze.liczba_zwierzat`.

??? question "3. Konto z konstruktorem walidującym"
    Zdefiniuj klasę `KontoOszczednosciowe` z konstruktorem przyjmującym `saldo_startowe`. Jeśli wartość jest ujemna, konstruktor ma ustawić saldo na `0` i wypisać ostrzeżenie. Przetestuj z poprawną i niepoprawną wartością startową.

## Poziom średni

??? question "4. Hermetyzacja + metoda statyczna"
    Zdefiniuj klasę `Pracownik` z polem `__pensja` (prywatnym, dwa podkreślniki) i metodą `podwyzka(procent)`, zwiększającą pensję o podany procent. Dodaj metodę statyczną `Pracownik.oblicz_podatek(kwota)`, zwracającą 19% podanej kwoty (bez tworzenia obiektu). Przetestuj oba mechanizmy.

??? question "5. Kopiowanie obiektu z listą wewnątrz"
    Zdefiniuj klasę `Projekt` z polami `nazwa` i `zadania` (pusta lista w konstruktorze) oraz metodą `dodaj_zadanie(opis)`. Stwórz obiekt, dodaj kilka zadań, skopiuj go przez `copy.deepcopy()`, dodaj zadanie tylko do kopii i sprawdź, czy oryginał pozostał nienaruszony.

??? question "6. Klasa z metodą klasową jako alternatywnym konstruktorem"
    Zdefiniuj klasę `Data` z polami `dzien`, `miesiac`, `rok`. Dodaj metodę klasową `z_tekstu(cls, tekst)`, przyjmującą tekst w formacie `"15-03-2024"` i zwracającą nowy obiekt `Data` na jego podstawie (rozbij tekst metodą `.split("-")`). Przetestuj tworzenie obiektu obydwoma sposobami — zwykłym konstruktorem i przez `Data.z_tekstu(...)`.

## Poziom trudniejszy

??? question "7. System rezerwacji sal (statyczne + hermetyzacja + konstruktor)"
    Zdefiniuj klasę `Sala` z polem statycznym `liczba_sal` i polami zwykłymi `numer` (generowany automatycznie na podstawie licznika, tak jak w lekcji o składnikach statycznych) i `_czy_zajeta` (chronione, start `False`). Dodaj metody `zarezerwuj()` (ustawia `_czy_zajeta` na `True`, tylko jeśli sala nie jest już zajęta — w przeciwnym razie wypisz komunikat) i `zwolnij()`. Stwórz kilka sal i przetestuj podwójną rezerwację tej samej sali.

??? question "8. Klasa z pełną kopią głęboką"
    Zdefiniuj klasę `Zamowienie` z polami `numer` i `pozycje` (lista słowników, każdy z kluczami `nazwa` i `ilosc`). Napisz własną metodę `kopiuj()` (odpowiednik konstruktora kopiującego), która tworzy **w pełni niezależną** kopię — łącznie z niezależną kopią listy `pozycje` (nie samym przypisaniem). Sprawdź, że zmiana pozycji w kopii nie wpływa na oryginał.

??? question "9. Zaprojektuj system z wykorzystaniem wszystkiego naraz"
    Zaprojektuj i zaimplementuj klasę `Biblioteka`, łączącą wszystko z tego działu:

    - pole statyczne zliczające łączną liczbę wypożyczeń we wszystkich obiektach
    - pole chronione `_ksiazki` (lista) inicjowane w konstruktorze
    - metodę `dodaj_ksiazke(tytul)`
    - metodę `wypozycz(tytul)`, zwiększającą licznik statyczny i wypisującą potwierdzenie (albo komunikat o błędzie, jeśli książki nie ma na liście)
    - metodę klasową `pokaz_wszystkie_wypozyczenia()`

## Pytania teoretyczne (bez kodowania)

??? question "P1. Konstruktor kopiujący"
    Czym jest konstruktor kopiujący i w jakich językach występuje jako mechanizm wbudowany w język? Jak rozwiązuje się ten sam problem w Pythonie?

??? question "P2. Destruktor"
    Dlaczego C++ wymaga destruktora, a Python i Java radzą sobie bez jego pełnoprawnego odpowiednika? Jaki mechanizm jest współcześnie zalecany w obu tych językach zamiast destruktora?

??? question "P3. Klasy zaprzyjaźnione"
    Czym jest `friend` w C++ i dlaczego ani Python, ani Java nie mają tego mechanizmu? Co Java oferuje jako najbliższy (choć niepełny) odpowiednik?

??? question "P4. Specyfikatory dostępu"
    Czym różni się `private` w Javie od podwójnego podkreślnika w Pythonie? Dlaczego mówimy, że Java to "wymusza", a Python tylko "sygnalizuje"?

??? question "P5. Pole statyczne w praktyce"
    Podaj własny przykład (inny niż z lekcji) sytuacji, w której pole statyczne byłoby lepszym rozwiązaniem niż pole zwykłe. Uzasadnij.

!!! tip "To domyka dział Klasy i obiekty"
    Po tym zestawie zadań masz pełne pokrycie zarówno praktyczne (kod w Pythonie), jak i teoretyczne (porównania z Javą/C++, przydatne na egzaminie zawodowym) całego tematu. Kolejny naturalny krok w programie to **Dziedziczenie klas** — tam wrócicie do pojęć poznanych w poprzednim dziale, tym razem w pełnej, praktycznej implementacji.
