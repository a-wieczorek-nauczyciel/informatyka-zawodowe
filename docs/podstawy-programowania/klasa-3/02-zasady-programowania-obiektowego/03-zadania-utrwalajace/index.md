# Zadania utrwalające

Zadania podsumowujące cały dział "Zasady programowania obiektowego". Każde z nich wymaga połączenia **kilku pojęć naraz** — klasy, obiektu, pól, metod, a w trudniejszych też dziedziczenia, hermetyzacji i polimorfizmu.

## Poziom podstawowy

??? question "1. Klasa Ksiazka"
    Zdefiniuj klasę `Ksiazka` z polami `tytul` i `autor`. Dodaj metodę `opisz()`, wypisującą zdanie w stylu `"Lalka, autor: Bolesław Prus"`. Stwórz dwa obiekty tej klasy i wywołaj na nich metodę `opisz()`.

??? question "2. Klasa Prostokat"
    Zdefiniuj klasę `Prostokat` z polami `bok_a` i `bok_b`. Dodaj metodę `policz_pole()`, zwracającą pole prostokąta. Stwórz obiekt i wypisz jego pole.

??? question "3. Klasa Produkt"
    Zdefiniuj klasę `Produkt` z polami `nazwa` i `cena`. Dodaj metodę `czy_drogi()`, zwracającą `True`, jeśli cena przekracza 100 zł. Stwórz trzy obiekty o różnych cenach i sprawdź działanie metody dla każdego.

## Poziom średni

??? question "4. Dziedziczenie — pojazdy"
    Zdefiniuj klasę bazową `Pojazd` z polem `marka` i metodą `opisz()`. Zdefiniuj klasę pochodną `Samochod(Pojazd)`, dodając pole `liczba_drzwi` i własną metodę `otworz_bagaznik()`. Stwórz obiekt klasy `Samochod` i wywołaj zarówno metodę odziedziczoną, jak i własną.

??? question "5. Hermetyzacja — licznik wejść"
    Zdefiniuj klasę `LicznikWejsc` z polem `_liczba_wejsc` (zaczynającym się od 0) i metodami `zarejestruj_wejscie()` (zwiększa licznik o 1) oraz `pokaz_licznik()`. Nie pozwól, żeby licznik dało się zmniejszyć bezpośrednio — cała logika ma iść przez metody klasy.

??? question "6. Polimorfizm — instrumenty"
    Zdefiniuj klasy `Gitara`, `Pianino` i `Perkusja`, każda z metodą `zagraj()` wypisującą inny komunikat (np. `"Brzdęk!"`, `"Dźwięk pianina"`, `"Bum!"`). Stwórz listę obiektów wszystkich trzech typów i w pętli wywołaj `zagraj()` na każdym.

## Poziom trudniejszy

??? question "7. System pracowników (dziedziczenie + hermetyzacja)"
    Zdefiniuj klasę bazową `Pracownik` z polami `imie` i `_pensja` (chronioną konwencją), metodą `podwyzka(kwota)` (zwiększa `_pensja`, tylko jeśli `kwota` jest dodatnia) i metodą `pokaz_pensje()`. Zdefiniuj klasę pochodną `Menedzer(Pracownik)`, dodając pole `zespol` (lista nazwisk podwładnych) i metodę `dodaj_do_zespolu(imie)`. Stwórz obiekt `Menedzer`, przetestuj podwyżkę i dodanie osoby do zespołu.

??? question "8. Biblioteka (klasa + obiekty + hermetyzacja)"
    Zdefiniuj klasę `Ksiazka` z polami `tytul` i `_czy_wypozyczona` (domyślnie `False`). Dodaj metody `wypozycz()` (ustawia `_czy_wypozyczona` na `True`, ale tylko jeśli książka nie jest już wypożyczona — w przeciwnym razie wypisz komunikat) i `zwroc()`. Stwórz listę kilku obiektów `Ksiazka` i przetestuj wypożyczanie tej samej książki dwa razy z rzędu.

??? question "9. Figury geometryczne (dziedziczenie + polimorfizm)"
    Zdefiniuj klasę bazową `Figura` z metodą `policz_pole()`, która na razie tylko wypisuje `"Nie zaimplementowano"`. Zdefiniuj klasy pochodne `Kwadrat(Figura)` i `Kolo(Figura)`, każda z własną, poprawną wersją `policz_pole()`. Stwórz listę obiektów obu typów i w pętli wywołaj `policz_pole()` na każdym — zaobserwuj, że mimo tej samej nazwy metody, każdy obiekt liczy inaczej.

!!! tip "Wskazówka do zadań 7–9"
    Jeśli nie jesteś pewien składni dziedziczenia (`class Cos(Bazowa):`) albo hermetyzacji (`self._pole`), wróć do lekcji "Podstawowe pojęcia" — tam są dokładnie te same wzorce na prostszych przykładach.

## Pytania sprawdzające (bez kodowania)

Zanim przejdziesz do kolejnego działu, upewnij się, że potrafisz odpowiedzieć na te pytania bez zaglądania do notatek:

??? question "P1. Klasa a obiekt"
    Jaka jest różnica między klasą a obiektem? Podaj przykład jednej klasy i dwóch różnych obiektów tej klasy.

??? question "P2. Pole a metoda"
    Czym różni się pole od metody? Podaj po jednym przykładzie z życia (np. dla klasy `Samochod`).

??? question "P3. Po co dziedziczenie"
    Dlaczego dziedziczenie jest przydatne? Jaki problem rozwiązuje w porównaniu do pisania każdej klasy od zera?

??? question "P4. Po co hermetyzacja"
    Dlaczego nie wystawia się wszystkich pól obiektu "na zewnątrz" bez żadnej kontroli? Podaj przykład sytuacji, w której brak hermetyzacji mógłby doprowadzić do błędu w programie.

??? question "P5. Polimorfizm w praktyce"
    Wyjaśnij, na czym polega polimorfizm, posługując się przykładem innym niż te z lekcji (możesz wymyślić własny).
