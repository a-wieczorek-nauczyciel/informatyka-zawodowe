# Konstruktor

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć napisać konstruktor z instrukcjami inicjującymi (nie tylko prostymi przypisaniami) oraz rozumieć, czym jest konstruktor kopiujący — mimo że w Pythonie nie istnieje on jako osobny mechanizm językowy.

## Konstruktor w Pythonie — `__init__`

Konstruktor to specjalna metoda wywoływana **automatycznie** w momencie tworzenia obiektu. W Pythonie tę rolę pełni `__init__`.

```python
class Pracownik:
    def __init__(self, imie, pensja):
        self.imie = imie
        self.pensja = pensja

pracownik1 = Pracownik("Kowalski", 4500)
```

W chwili wykonania `Pracownik("Kowalski", 4500)` Python sam wywołuje `__init__`, przekazując nowo tworzony obiekt jako `self`.

??? question "Zadanie 1: Konstruktor klasy Samochod"
    Zdefiniuj klasę `Samochod` z konstruktorem przyjmującym `marka` i `rok_produkcji`. Stwórz jeden obiekt i wypisz oba pola.

??? question "Zadanie 2: Rozpoznaj wywołanie konstruktora"
```python
    class Bilet:
        def __init__(self, numer_miejsca):
            self.numer_miejsca = numer_miejsca

    b1 = Bilet(14)
    b2 = Bilet(15)
```
    Ile razy zostanie wywołany konstruktor `__init__` w tym kodzie? Skąd to wiadomo?

## Instrukcje inicjujące — konstruktor to nie tylko przypisania

"Instrukcje inicjujące" to każdy kod wykonujący się przy tworzeniu obiektu — nie tylko `self.pole = wartosc`. Konstruktor może też: obliczać coś na podstawie argumentów, walidować dane wejściowe, albo ustawiać wartości domyślne.

```python
class KontoBankowe:
    def __init__(self, wlasciciel, saldo_startowe=0):
        if saldo_startowe < 0:
            print("Saldo startowe nie może być ujemne — ustawiam 0")
            saldo_startowe = 0
        self.wlasciciel = wlasciciel
        self.saldo = saldo_startowe
        self.historia = []  # pole, które nie pochodzi wprost z argumentu

konto1 = KontoBankowe("Nowak")
konto2 = KontoBankowe("Kowalska", 500)
```

Tutaj konstruktor **waliduje** dane (`if saldo_startowe < 0`), **ustawia wartość domyślną** (`saldo_startowe=0`) i **inicjuje pole niepochodzące bezpośrednio z argumentu** (`self.historia = []`).

??? question "Zadanie 3: Konstruktor z walidacją"
    Zdefiniuj klasę `Prostokat` z konstruktorem przyjmującym `bok_a` i `bok_b`. Jeśli którykolwiek bok jest ujemny lub równy zero, konstruktor ma wypisać komunikat o błędzie i ustawić oba boki na `1`. Przetestuj z poprawnymi i niepoprawnymi wartościami.

??? question "Zadanie 4: Konstruktor z polem obliczanym"
    Zdefiniuj klasę `Prostokat` (jeśli robisz to jako kontynuację zadania 3) tak, żeby konstruktor od razu obliczał i zapisywał pole `pole_powierzchni` jako `self.pole_powierzchni = bok_a * bok_b` — bez osobnej metody, wynik ma być gotowy zaraz po stworzeniu obiektu.

## Konstruktor kopiujący

!!! warning "Tego mechanizmu nie ma w Pythonie"
    **Konstruktor kopiujący jako osobny, językowy mechanizm nie istnieje w Pythonie.** To jest jeden z tych tematów z podstawy programowej, który wymaga wytłumaczenia na przykładzie innego języka — tutaj użyjemy Javy, żebyś zrozumiał ideę, mimo że w swoim kodzie w Pythonie zastosujesz to inaczej.

### Dlaczego Python tego nie ma — problem z
