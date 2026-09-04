# Pola i metody

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć samodzielnie zdefiniować pola i metody klasy, stworzyć obiekt (instancję) i odwołać się do jego pól oraz metod z zewnątrz.

## Definiowanie pól klasy

Pola definiuje się zwykle wewnątrz specjalnej metody `__init__`, którą Python wywołuje automatycznie przy tworzeniu nowego obiektu. Do pól odwołujemy się przez `self`.

```python
class Rower:
    def __init__(self, marka, liczba_biegow):
        self.marka = marka
        self.liczba_biegow = liczba_biegow
```

- `self` — reprezentuje **konkretny obiekt**, który jest właśnie tworzony lub na którym operujemy. To pierwszy parametr każdej metody w klasie, ale nie podaje się go przy wywołaniu — Python robi to automatycznie.
- `self.marka = marka` — zapisuje wartość przekazaną jako argument do pola `marka` należącego do tego konkretnego obiektu.

??? question "Zadanie 1: Klasa Telefon"
    Zdefiniuj klasę `Telefon` z polami `model` i `pojemnosc_pamieci`. Nie dodawaj jeszcze żadnych metod.

??? question "Zadanie 2: Rozpoznaj pola w kodzie"
```python
    class Bilet:
        def __init__(self, numer_miejsca, cena):
            self.numer_miejsca = numer_miejsca
            self.cena = cena
```
    Ile pól ma klasa `Bilet`? Jak się nazywają?

## Tworzenie obiektów (instancji)

Obiekt tworzy się, wywołując nazwę klasy jak funkcję, podając argumenty wymagane przez `__init__` (pomijając `self` — ten Python uzupełnia sam).

```python
rower1 = Rower("Kross", 21)
rower2 = Rower("Trek", 18)
```

`rower1` i `rower2` to dwa niezależne obiekty — zmiana pola w jednym nie wpływa na drugi.

??? question "Zadanie 3: Stwórz obiekty klasy Telefon"
    Korzystając z klasy `Telefon` z zadania 1, stwórz dwa obiekty o różnych wartościach pól.

??? question "Zadanie 4: Niezależność obiektów"
    Stwórz dwa obiekty klasy `Rower`. Zmień pole `liczba_biegow` w pierwszym obiekcie (np. `rower1.liczba_biegow = 24`). Wypisz `liczba_biegow` obu obiektów — sprawdź, czy zmiana w jednym wpłynęła na drugi, i zapisz wniosek.

## Odwoływanie się do pól obiektu

Do pola obiektu odwołujesz się przez kropkę: `nazwa_obiektu.nazwa_pola`.

```python
print(rower1.marka)
print(rower1.liczba_biegow)

rower1.liczba_biegow = 24  # można też zmienić wartość pola
```

??? question "Zadanie 5: Wypisz wszystkie pola"
    Dla obu obiektów `Telefon` z zadania 3, wypisz oba pola każdego z nich w czytelnej formie, np. `"iPhone 15 — 128 GB"`.

??? question "Zadanie 6: Zmień pole z zewnątrz"
    Stwórz obiekt klasy `Bilet` (z zadania 2). Zmień jego pole `cena` na nową wartość, odwołując się do niego z zewnątrz klasy (bez żadnej metody). Wypisz cenę przed i po zmianie.

## Definiowanie metod

Metoda to funkcja zdefiniowana wewnątrz klasy. Zawsze przyjmuje `self` jako pierwszy parametr — dzięki temu ma dostęp do pól tego konkretnego obiektu, na którym została wywołana.

```python
class Rower:
    def __init__(self, marka, liczba_biegow):
        self.marka = marka
        self.liczba_biegow = liczba_biegow

    def opisz(self):
        print(f"{self.marka}, liczba biegów: {self.liczba_biegow}")

    def zmien_bieg(self, nowy_bieg):
        if 1 <= nowy_bieg <= self.liczba_biegow:
            print(f"Zmieniono na bieg {nowy_bieg}")
        else:
            print("Nieprawidłowy numer biegu")
```

??? question "Zadanie 7: Dodaj metodę opisz()"
    Dodaj do klasy `Telefon` metodę `opisz()`, wypisującą zdanie w stylu `"iPhone 15 ma 128 GB pamięci"`.

??? question "Zadanie 8: Metoda z parametrem"
    Dodaj do klasy `Bilet` metodę `zastosuj_znizke(procent)`, która zmniejsza pole `cena` o podany procent. Przetestuj ją.

## Wywoływanie metod obiektu

Metodę wywołuje się tak samo jak odwołujesz się do pola — przez kropkę, ale z nawiasami (bo to wywołanie funkcji).

```python
rower1 = Rower("Kross", 21)
rower1.opisz()
rower1.zmien_bieg(5)
rower1.zmien_bieg(30)
```

!!! warning "Częsty błąd"
    Zapomnienie nawiasów przy wywołaniu metody (`rower1.opisz` zamiast `rower1.opisz()`) nie da błędu, ale też nic nie wykona — Python potraktuje to jako odwołanie do samej metody jako obiektu, a nie jej wywołanie. Jeśli coś "nie działa po cichu", to pierwsze miejsce do sprawdzenia.

??? question "Zadanie 9: Wywołaj wszystkie metody"
    Stwórz obiekt klasy `Telefon`, wywołaj jego metodę `opisz()`. Stwórz obiekt klasy `Bilet`, wywołaj `zastosuj_znizke()` z dowolnym procentem, a potem wypisz nową cenę.

## Metody odwołujące się do innych pól i metod tego samego obiektu

Metoda może korzystać z innych pól i wywoływać inne metody tego samego obiektu, zawsze przez `self`.

```python
class KontoBankowe:
    def __init__(self, wlasciciel, saldo):
        self.wlasciciel = wlasciciel
        self.saldo = saldo

    def wplac(self, kwota):
        self.saldo += kwota
        self.pokaz_stan()

    def pokaz_stan(self):
        print(f"{self.wlasciciel}: saldo {self.saldo} zł")
```

??? question "Zadanie 10: Metoda wywołująca inną metodę"
    Dodaj do klasy `Bilet` metodę `podsumowanie()`, która wypisuje numer miejsca i cenę, a następnie w metodzie `zastosuj_znizke()` (z zadania 8) wywołaj `podsumowanie()` na końcu, żeby od razu pokazać wynik po zastosowaniu zniżki.

## Zadania utrwalające

??? question "Zadanie 11: Klasa Pracownik"
    Zdefiniuj klasę `Pracownik` z polami `imie` i `stanowisko`. Dodaj metody `opisz()` (wypisuje imię i stanowisko) i `awansuj(nowe_stanowisko)` (zmienia pole `stanowisko` i wypisuje komunikat o awansie). Stwórz obiekt, wywołaj `opisz()`, potem `awansuj()`, potem znowu `opisz()`, żeby zobaczyć zmianę.

??? question "Zadanie 12: Klasa Zamowienie"
    Zdefiniuj klasę `Zamowienie` z polami `numer` i `wartosc`. Dodaj metodę `dodaj_produkt(cena)`, zwiększającą `wartosc` o podaną kwotę, oraz metodę `pokaz_wartosc()`. Stwórz obiekt, dodaj kilka produktów po kolei i sprawdź, czy wartość zamówienia poprawnie rośnie.

??? question "Zadanie 13: Kilka obiektów naraz"
    Zdefiniuj klasę `Uczen` z polami `imie` i `oceny` (pusta lista na start). Dodaj metody `dodaj_ocene(ocena)` i `srednia()`. Stwórz **listę trzech obiektów** `Uczen`, dodaj każdemu po kilka ocen (różne wartości), a następnie w pętli wypisz imię i średnią każdego z nich.
