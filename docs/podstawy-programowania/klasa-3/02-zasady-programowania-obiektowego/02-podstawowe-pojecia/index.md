# Podstawowe pojęcia

!!! info "Cel lekcji"
    Po tej lekcji będziesz znać i potrafił wyjaśnić pojęcia: klasa, obiekt, pole, metoda, dziedziczenie, hermetyzacja, polimorfizm — oraz rozpoznać je w prostym kodzie.

!!! tip "To wciąż fundament, nie pełna implementacja"
    W tej lekcji zobaczysz krótkie fragmenty kodu ilustrujące każde pojęcie. Pełną, szczegółową składnię (konstruktory, specyfikatory dostępu, wszystkie niuanse) poznasz w kolejnym dziale — **Klasy i obiekty**.

## Klasa

**Klasa** to szablon (przepis) opisujący, jakie dane i zachowania będą miały obiekty tego typu. Sama klasa to jeszcze nie konkretna "rzecz" — to plan, według którego tworzy się konkretne obiekty.

```python
class Pojazd:
    pass
```

To najprostsza możliwa klasa — na razie pusta, ale już zdefiniowana pod nazwą `Pojazd`.

??? question "Zadanie 1: Zaprojektuj pustą klasę"
    Zdefiniuj pustą klasę `Zwierze` (samo `class Zwierze: pass`, bez żadnych pól ani metod). Uruchom kod, żeby upewnić się, że nie ma błędów składniowych.

??? question "Zadanie 2: Rozpoznaj klasę w kodzie"
```python
    class KontoBankowe:
        pass

    class Ksiazka:
        pass
```
    Ile klas jest zdefiniowanych w tym fragmencie kodu? Wypisz ich nazwy.

## Obiekt

**Obiekt** (instancja) to konkretny "egzemplarz" stworzony na podstawie klasy. Jedna klasa może mieć wiele różnych obiektów.

```python
class Pojazd:
    pass

pojazd1 = Pojazd()
pojazd2 = Pojazd()
```

`pojazd1` i `pojazd2` to dwa **osobne** obiekty tej samej klasy — tak jak dwa różne samochody mogą być tego samego modelu.

??? question "Zadanie 3: Stwórz trzy obiekty"
    Korzystając z klasy `Zwierze` z zadania 1, stwórz trzy osobne obiekty: `zwierze1`, `zwierze2`, `zwierze3`. Sprawdź w kodzie (np. przez `print(zwierze1 == zwierze2)`), że to różne obiekty, mimo że pochodzą z tej samej klasy.

??? question "Zadanie 4: Ile obiektów?"
```python
    class Ksiazka:
        pass

    ksiazka1 = Ksiazka()
    ksiazka2 = Ksiazka()
    ksiazka3 = Ksiazka()
```
    Ile klas jest w tym kodzie, a ile obiektów? Wyjaśnij różnicę własnymi słowami.

## Pole

**Pole** (atrybut, właściwość) to dana należąca do obiektu — jego "cecha".

```python
class Pojazd:
    def __init__(self, marka, predkosc_max):
        self.marka = marka
        self.predkosc_max = predkosc_max

auto = Pojazd("Toyota", 180)
print(auto.marka)
print(auto.predkosc_max)
```

??? question "Zadanie 5: Dodaj pola do klasy"
    Rozbuduj klasę `Zwierze` tak, żeby miała pola `gatunek` i `wiek` (skorzystaj ze składni z przykładu powyżej, nawet jeśli `__init__` nie jest jeszcze w pełni omówione — potraktuj to jako wzór do naśladowania). Stwórz jeden obiekt i wypisz oba pola.

??? question "Zadanie 6: Znajdź pola w kodzie"
```python
    class Uczen:
        def __init__(self, imie, klasa):
            self.imie = imie
            self.klasa = klasa
```
    Jakie pola ma klasa `Uczen`? Wypisz ich nazwy.

## Metoda

**Metoda** to funkcja należąca do obiektu — to, co obiekt "potrafi zrobić".

```python
class Pojazd:
    def __init__(self, marka, predkosc_max):
        self.marka = marka
        self.predkosc_max = predkosc_max

    def opisz(self):
        print(f"{self.marka} rozwija maksymalnie {self.predkosc_max} km/h")

auto = Pojazd("Toyota", 180)
auto.opisz()
```

??? question "Zadanie 7: Dodaj metodę"
    Dodaj do klasy `Zwierze` (z pól `gatunek` i `wiek`) metodę `opisz()`, która wypisuje zdanie w stylu `"To jest pies w wieku 3 lat"`. Wywołaj ją.

??? question "Zadanie 8: Pole czy metoda?"
```python
    class Telefon:
        def __init__(self, model, pojemnosc_baterii):
            self.model = model
            self.pojemnosc_baterii = pojemnosc_baterii

        def wlacz(self):
            print(f"{self.model} się włącza")
```
    Które elementy tej klasy to pola, a które to metody? Wypisz je w dwóch osobnych listach.

## Dziedziczenie

**Dziedziczenie** pozwala jednej klasie (klasie pochodnej) przejąć pola i metody innej klasy (klasy bazowej), a następnie dodać własne albo zmienić odziedziczone.

```python
class Pojazd:
    def __init__(self, marka):
        self.marka = marka

    def opisz(self):
        print(f"To pojazd marki {self.marka}")

class Motocykl(Pojazd):
    def klaksonuj(self):
        print("Biip biip!")

moto = Motocykl("Honda")
moto.opisz()       # odziedziczone z Pojazd
moto.klaksonuj()   # własne, tylko w Motocykl
```

??? question "Zadanie 9: Stwórz klasę pochodną"
    Zdefiniuj klasę bazową `Zwierze` z polem `imie` i metodą `wydaj_dzwiek()` (wypisującą np. `"..."`). Następnie zdefiniuj klasę pochodną `Pies(Zwierze)`, która dodaje własną metodę `aportuj()`. Stwórz obiekt klasy `Pies` i wywołaj obie metody — jedną odziedziczoną, jedną własną.

??? question "Zadanie 10: Co zostało odziedziczone?"
```python
    class Ksztalt:
        def __init__(self, kolor):
            self.kolor = kolor

        def opisz_kolor(self):
            print(f"Ten kształt jest koloru {self.kolor}")

    class Kolo(Ksztalt):
        def policz_pole(self, promien):
            return 3.14 * promien * promien

    kolo1 = Kolo("czerwony")
```
    Czy `kolo1.opisz_kolor()` zadziała, mimo że metoda `opisz_kolor` jest zdefiniowana w klasie `Ksztalt`, a nie `Kolo`? Uzasadnij.

## Hermetyzacja

**Hermetyzacja** (enkapsulacja) to ukrywanie wewnętrznych szczegółów obiektu i udostępnianie tylko tego, co potrzebne na zewnątrz. W Pythonie sygnalizuje się to konwencją nazewniczą — pojedynczy podkreślnik `_` oznacza "nie ruszaj tego z zewnątrz, choć technicznie możesz", a podwójny `__` dodatkowo utrudnia przypadkowy dostęp.

```python
class KontoBankowe:
    def __init__(self, saldo_startowe):
        self._saldo = saldo_startowe   # konwencja: "wewnętrzne"

    def pokaz_saldo(self):
        print(f"Saldo: {self._saldo} zł")

    def wplac(self, kwota):
        if kwota > 0:
            self._saldo += kwota

konto = KontoBankowe(100)
konto.wplac(50)
konto.pokaz_saldo()
```

Zamiast pozwolić komukolwiek dowolnie zmieniać `_saldo` z zewnątrz (co mogłoby wprowadzić błędne dane, np. ujemne saldo bez kontroli), zmiana odbywa się przez metodę `wplac()`, która pilnuje poprawności.

??? question "Zadanie 11: Dodaj kontrolę"
    Rozbuduj klasę `KontoBankowe` o metodę `wyplac(kwota)`, która **zmniejsza** `_saldo`, ale tylko jeśli w koncie jest wystarczająco dużo pieniędzy (w przeciwnym razie wypisz komunikat o błędzie zamiast pozwolić na ujemne saldo).

??? question "Zadanie 12: Po co ta konwencja?"
    Wyjaśnij własnymi słowami, dlaczego lepiej zmieniać saldo konta przez metodę `wplac()`, a nie bezpośrednio linijką `konto._saldo = konto._saldo + 50` napisaną poza klasą.

## Polimorfizm

**Polimorfizm** oznacza, że różne klasy mogą mieć metodę o tej samej nazwie, ale każda wykonuje ją inaczej — a Ty możesz je wywoływać w jednolity sposób, nie martwiąc się, z jaką dokładnie klasą masz do czynienia.

```python
class Kot:
    def wydaj_dzwiek(self):
        print("Miau")

class Pies:
    def wydaj_dzwiek(self):
        print("Hau")

zwierzeta = [Kot(), Pies(), Kot()]

for zwierze in zwierzeta:
    zwierze.wydaj_dzwiek()
```

Pętla wywołuje `wydaj_dzwiek()` na każdym obiekcie, nie sprawdzając, czy to kot, czy pies — a mimo to każdy "odpowiada" inaczej, właściwie dla swojego typu.

??? question "Zadanie 13: Dodaj trzecią klasę do polimorfizmu"
    Dodaj do przykładu powyżej klasę `Krowa` z własną wersją `wydaj_dzwiek()` (np. `"Muu"`). Dodaj obiekt tej klasy do listy `zwierzeta` i sprawdź, czy pętla nadal działa bez żadnych zmian w samej pętli.

??? question "Zadanie 14: Polimorfizm z kształtami"
    Zdefiniuj klasy `Kwadrat` i `Kolo`, każda z metodą `policz_pole()` liczącą pole odpowiedniej figury (przyjmij, że bok/promień jest przekazywany w konstruktorze). Stwórz listę obiektów obu typów i w pętli wywołaj `policz_pole()` na każdym z nich, wypisując wynik.

## Podsumowanie pojęć

| Pojęcie | W skrócie |
|---|---|
| Klasa | Szablon opisujący dane i zachowania |
| Obiekt | Konkretny egzemplarz stworzony na podstawie klasy |
| Pole | Dana należąca do obiektu |
| Metoda | Funkcja należąca do obiektu |
| Dziedziczenie | Klasa przejmuje pola/metody innej klasy |
| Hermetyzacja | Ukrywanie wewnętrznych szczegółów, kontrolowany dostęp |
| Polimorfizm | Ta sama nazwa metody, różne działanie w różnych klasach |

??? question "Zadanie podsumowujące: znajdź wszystko w jednym miejscu"
```python
    class Pracownik:
        def __init__(self, imie, pensja):
            self.imie = imie
            self._pensja = pensja

        def podwyzka(self, kwota):
            self._pensja += kwota

        def przedstaw_sie(self):
            print(f"Nazywam się {self.imie}")

    class Kierownik(Pracownik):
        def przedstaw_sie(self):
            print(f"Jestem kierownikiem, nazywam się {self.imie}")
```
    Wskaż w tym kodzie: klasę bazową, klasę pochodną, przykład dziedziczenia, przykład hermetyzacji (konwencji) i przykład polimorfizmu (podpowiedź: porównaj metodę `przedstaw_sie` w obu klasach).
