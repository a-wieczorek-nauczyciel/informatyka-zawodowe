# Składniki statyczne

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zdefiniować pole statyczne (wspólne dla wszystkich obiektów klasy) oraz metodę statyczną, i wiedzieć, kiedy warto z nich skorzystać.

## Pole zwykłe a pole statyczne — kluczowa różnica

Dotychczasowe pola (`self.imie`, `self.saldo`) należą do **konkretnego obiektu** — każdy obiekt ma własną, niezależną kopię. Pole statyczne jest inne: **należy do samej klasy** i jest **współdzielone przez wszystkie obiekty** tej klasy.

```python
class Uczen:
    liczba_uczniow = 0   # pole statyczne — zdefiniowane POZA __init__, na poziomie klasy

    def __init__(self, imie):
        self.imie = imie          # pole zwykłe — należy do konkretnego obiektu
        Uczen.liczba_uczniow += 1 # zmiana pola statycznego przez nazwę klasy

uczen1 = Uczen("Marta")
uczen2 = Uczen("Tomek")

print(Uczen.liczba_uczniow)  # 2 — wspólne dla wszystkich obiektów
```

Za każdym razem, gdy tworzysz nowego `Uczen`, konstruktor zwiększa `liczba_uczniow` — a ponieważ to pole jest **jedno, wspólne dla całej klasy**, licznik poprawnie rośnie niezależnie od tego, ile obiektów stworzysz.

!!! tip "Jak odróżnić pole statyczne od zwykłego w kodzie"
    Pole statyczne definiuje się **wewnątrz klasy, ale poza `__init__`**, bezpośrednio pod `class NazwaKlasy:`. Odwołujesz się do niego przez nazwę klasy (`Uczen.liczba_uczniow`), a nie przez `self`, choć odczyt przez `self.liczba_uczniow` też zadziała (o czym więcej w dalszej części lekcji).

??? question "Zadanie 1: Licznik obiektów"
    Zdefiniuj klasę `Ksiazka` z polem statycznym `liczba_ksiazek` (start od 0), zwiększanym w konstruktorze przy każdym nowym obiekcie. Stwórz cztery obiekty i wypisz `Ksiazka.liczba_ksiazek`.

??? question "Zadanie 2: Rozpoznaj typ pola"
```python
    class Samochod:
        liczba_kol = 4

        def __init__(self, marka):
            self.marka = marka
```
    Które pole jest statyczne, a które zwykłe? Uzasadnij, powołując się na to, gdzie zostało zdefiniowane.

## Pułapka: modyfikacja pola statycznego przez obiekt

Uważaj na jedną subtelność — jeśli spróbujesz **zmienić** pole statyczne przez `self.pole = nowa_wartosc`, Python nie zmieni pola statycznego, tylko **stworzy nowe pole zwykłe** o tej samej nazwie, należące tylko do tego jednego obiektu.

```python
class Uczen:
    szkola = "Technikum Cosinus"

    def __init__(self, imie):
        self.imie = imie

uczen1 = Uczen("Marta")
uczen2 = Uczen("Tomek")

uczen1.szkola = "Inna szkoła"   # to NIE zmienia pola statycznego!

print(uczen1.szkola)  # "Inna szkoła" — nowe pole zwykłe, tylko dla uczen1
print(uczen2.szkola)  # "Technikum Cosinus" — pole statyczne, nienaruszone
print(Uczen.szkola)   # "Technikum Cosinus" — też nienaruszone
```

!!! warning "Częsty błąd na egzaminie i w praktyce"
    Żeby faktycznie zmienić pole statyczne dla **wszystkich** obiektów, trzeba odwołać się przez nazwę klasy: `Uczen.szkola = "Inna szkoła"`. Odwołanie przez `self` (albo przez nazwę konkretnego obiektu) do zapisu tworzy nowe, niezależne pole zamiast modyfikować wspólne.

??? question "Zadanie 3: Zaobserwuj pułapkę"
    Skorzystaj z klasy `Uczen` z przykładu powyżej. Stwórz dwa obiekty, zmień `szkola` przez `self` w jednym z nich, i wypisz wartość `szkola` obu obiektów oraz `Uczen.szkola`, żeby zobaczyć rozbieżność na własne oczy.

??? question "Zadanie 4: Popraw kod"
    Napraw kod z zadania 3 tak, żeby zmiana `szkola` faktycznie objęła **wszystkie** obiekty klasy `Uczen` (łącznie z tymi stworzonymi w przyszłości).

## Metoda statyczna — `@staticmethod`

Metoda statyczna to metoda, która **nie potrzebuje dostępu** do konkretnego obiektu (`self`) ani do samej klasy — działa niezależnie, jak zwykła funkcja, tyle że logicznie należy do danej klasy.

```python
class Kalkulator:
    @staticmethod
    def dodaj(a, b):
        return a + b

    @staticmethod
    def czy_parzysta(liczba):
        return liczba % 2 == 0

wynik = Kalkulator.dodaj(3, 5)   # wywołanie przez nazwę klasy, bez tworzenia obiektu
print(wynik)
```

Zwróć uwagę: `dodaj` **nie ma** `self` jako parametru — bo nie operuje na żadnym konkretnym obiekcie. Dekorator `@staticmethod` informuje Pythona, że ta metoda ma działać w ten sposób.

??? question "Zadanie 5: Metoda statyczna do walidacji"
    Zdefiniuj klasę `Walidator` z metodą statyczną `czy_pelnoletni(wiek)`, zwracającą `True`, jeśli `wiek >= 18`. Wywołaj ją bez tworzenia żadnego obiektu klasy `Walidator`.

??? question "Zadanie 6: Kiedy metoda statyczna ma sens"
    Wyjaśnij własnymi słowami: dlaczego `czy_parzysta` z przykładu powyżej **nie potrzebuje** dostępu do `self` ani do żadnego pola klasy `Kalkulator`? Co by się stało, gdyby ta metoda musiała korzystać z jakiegoś pola konkretnego obiektu — czy nadal mogłaby być statyczna?

## Metoda klasowa — `@classmethod`

Metoda klasowa jest podobna do statycznej, ale **ma dostęp do samej klasy** (przez konwencjonalnie nazwany parametr `cls`, odpowiednik `self`, tylko dla klasy zamiast obiektu). Często używa się jej do tworzenia obiektów w alternatywny sposób.

```python
class Uczen:
    liczba_uczniow = 0

    def __init__(self, imie):
        self.imie = imie
        Uczen.liczba_uczniow += 1

    @classmethod
    def pokaz_liczbe_uczniow(cls):
        print(f"Liczba uczniów: {cls.liczba_uczniow}")

Uczen("Marta")
Uczen("Tomek")
Uczen.pokaz_liczbe_uczniow()
```

??? question "Zadanie 7: Metoda klasowa odczytująca licznik"
    Dodaj do klasy `Ksiazka` z zadania 1 metodę klasową `pokaz_liczbe_ksiazek()`, wypisującą aktualną wartość `liczba_ksiazek` przez `cls`.

??? question "Zadanie 8: Staticmethod czy classmethod?"
    Masz zdefiniować metodę `ile_dni_do_wakacji()`, która nie korzysta z żadnego pola klasy ani obiektu — po prostu liczy dni na podstawie stałej daty wpisanej w kodzie. Czy powinna to być `@staticmethod`, czy `@classmethod`? Uzasadnij.

## Podsumowanie — kiedy czego użyć

| Element | Ma dostęp do | Kiedy stosować |
|---|---|---|
| Pole zwykłe (`self.pole`) | Tylko tego jednego obiektu | Dane różne dla każdego obiektu (np. imię ucznia) |
| Pole statyczne (`Klasa.pole`) | Wszystkich obiektów naraz | Dane wspólne dla całej klasy (np. licznik obiektów, stała wartość) |
| Metoda zwykła (`self`) | Pól konkretnego obiektu | Operacje na danych tego jednego obiektu |
| `@staticmethod` | Niczego automatycznie | Funkcja logicznie związana z klasą, ale niepotrzebująca jej danych |
| `@classmethod` (`cls`) | Pól/metod statycznych klasy | Operacje na danych wspólnych całej klasy, alternatywne sposoby tworzenia obiektów |

!!! info "To samo w Javie, dla porównania"
    W Javie identyczną rolę pełni słowo kluczowe `static` — `static int liczbaUczniow;` to dokładny odpowiednik pola statycznego z tej lekcji, a `static void pokazLiczbe()` to odpowiednik metody statycznej. Mechanizm istnieje praktycznie tak samo w obu językach — zmienia się tylko składnia (`@staticmethod` + brak `self` w Pythonie, `static` przed typem w Javie).

## Zadania utrwalające

??? question "Zadanie 9: System rejestracji pojazdów"
    Zdefiniuj klasę `Pojazd` z polem statycznym `licznik_rejestracji` (start od 0) i polem zwykłym `numer_rejestracyjny`, generowanym automatycznie w konstruktorze na podstawie licznika (np. `f"WA{Pojazd.licznik_rejestracji:04d}"`, czyli WA0001, WA0002...). Dodaj metodę klasową `ile_zarejestrowano()`. Stwórz pięć obiektów i sprawdź, czy numery generują się poprawnie po kolei.

??? question "Zadanie 10: Klasa Waluta z metodą statyczną"
    Zdefiniuj klasę `Waluta` z metodą statyczną `przelicz_na_euro(kwota_pln)`, zwracającą kwotę podzieloną przez stały kurs (np. 4.3). Wywołaj metodę bez tworzenia żadnego obiektu.

??? question "Zadanie 11: Porównaj z podejściem bez składników statycznych"
    Napisz klasę `Zamowienie` śledzącą **łączną wartość wszystkich zamówień** złożonych w programie (pole statyczne, zwiększane przy każdym nowym obiekcie o wartość zamówienia przekazaną w konstruktorze). Dodaj metodę klasową `pokaz_sume_wszystkich()`. Zastanów się i zapisz w komentarzu: jak musiałbyś rozwiązać ten problem, gdybyś **nie miał** pól statycznych do dyspozycji?
