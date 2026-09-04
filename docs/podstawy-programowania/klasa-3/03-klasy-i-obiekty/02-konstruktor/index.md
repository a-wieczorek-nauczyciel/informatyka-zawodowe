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

### Dlaczego Python tego nie ma — problem z kopiowaniem obiektów

Kiedy przypisujesz jeden obiekt do drugiej zmiennej w Pythonie, **nie kopiujesz obiektu** — kopiujesz tylko odniesienie (referencję) do tego samego obiektu w pamięci.

```python
class Pracownik:
    def __init__(self, imie, pensja):
        self.imie = imie
        self.pensja = pensja

pracownik1 = Pracownik("Kowalski", 4500)
pracownik2 = pracownik1   # to NIE jest kopia!

pracownik2.pensja = 6000
print(pracownik1.pensja)  # wypisze 6000, nie 4500!
```

Zmiana `pracownik2` wpłynęła na `pracownik1`, bo to **ten sam obiekt** pod dwiema różnymi nazwami. To zaskakuje wielu uczniów — i to jest dokładnie ten problem, który konstruktor kopiujący rozwiązuje w innych językach.

### Jak to wygląda w Javie

W Javie (podobnie jak w C++) można napisać **konstruktor kopiujący** — konstruktor przyjmujący jako argument inny obiekt tej samej klasy i tworzący na jego podstawie **nowy, niezależny** obiekt o tych samych wartościach pól.

```java
public class Pracownik {
    String imie;
    double pensja;

    // zwykły konstruktor
    public Pracownik(String imie, double pensja) {
        this.imie = imie;
        this.pensja = pensja;
    }

    // konstruktor kopiujący
    public Pracownik(Pracownik inny) {
        this.imie = inny.imie;
        this.pensja = inny.pensja;
    }
}

Pracownik p1 = new Pracownik("Kowalski", 4500);
Pracownik p2 = new Pracownik(p1);  // konstruktor kopiujący — NOWY obiekt

p2.pensja = 6000;
System.out.println(p1.pensja);  // nadal 4500 — p1 i p2 to różne obiekty
```

!!! tip "Dlaczego to jest ważne"
    Konstruktor kopiujący daje **pełną kontrolę** nad tym, jak dokładnie obiekt ma być skopiowany — co przydaje się szczególnie, gdy klasa ma pola będące innymi obiektami (nie tylko liczbami czy tekstem), bo wtedy trzeba decydować, czy kopiować "płytko" (te same wewnętrzne obiekty) czy "głęboko" (osobne kopie wszystkiego w środku).

!!! info "W jakich jeszcze językach to występuje"
    **C++** ma konstruktor kopiujący jako mechanizm **wbudowany** — kompilator potrafi go nawet wygenerować automatycznie, jeśli programista go nie napisze. **C#** nie generuje go automatycznie, ale programista może napisać własny konstruktor przyjmujący obiekt tego samego typu, podobnie jak w Javie. Java, tak jak pokazano powyżej, **nie generuje** konstruktora kopiującego automatycznie — trzeba go napisać samodzielnie, jeśli jest potrzebny.

### Jak zrobić to samo w Pythonie

Python nie ma tego mechanizmu wbudowanego w składnię klasy, ale problem rozwiązuje się jedną z dwóch metod:

**Sposób 1 — moduł `copy`:**
```python
import copy

pracownik1 = Pracownik("Kowalski", 4500)
pracownik2 = copy.copy(pracownik1)  # tworzy NOWY, niezależny obiekt

pracownik2.pensja = 6000
print(pracownik1.pensja)  # nadal 4500
```

**Sposób 2 — własna metoda kopiująca:**
```python
class Pracownik:
    def __init__(self, imie, pensja):
        self.imie = imie
        self.pensja = pensja

    def kopiuj(self):
        return Pracownik(self.imie, self.pensja)

pracownik1 = Pracownik("Kowalski", 4500)
pracownik2 = pracownik1.kopiuj()
```

!!! warning "Częsta pomyłka na egzaminie zawodowym"
    Jeśli w pytaniu egzaminacyjnym pojawi się "konstruktor kopiujący", pytanie najpewniej dotyczy C++ lub Javy jako języka referencyjnego — nie próbuj szukać dosłownie analogicznej składni w Pythonie, bo jej tam nie ma. Kluczowe jest zrozumienie **problemu**, który to rozwiązuje (niezależna kopia obiektu), a nie zapamiętanie jednej konkretnej składni.

??? question "Zadanie 5: Zaobserwuj problem"
    Zdefiniuj dowolną prostą klasę (np. `Ksiazka` z polem `tytul`). Stwórz obiekt, przypisz go do drugiej zmiennej (`ksiazka2 = ksiazka1`), zmień pole w `ksiazka2`, i wypisz `ksiazka1` — zaobserwuj, że też się zmieniło. Zapisz w komentarzu, dlaczego tak się dzieje.

??? question "Zadanie 6: Napraw problem"
    To samo zadanie co wyżej, ale tym razem użyj `copy.copy()` (albo własnej metody `kopiuj()`) zamiast zwykłego przypisania. Sprawdź, że tym razem zmiana w kopii **nie** wpływa na oryginał.

??? question "Zadanie 7: Przepisz konstruktor kopiujący z Javy"
    Spójrz na przykład klasy `Pracownik` w Javie z tej lekcji. Napisz jego odpowiednik w Pythonie — zdefiniuj klasę z dwoma polami i metodą `kopiuj()`, która tworzy nowy obiekt na podstawie istniejącego (tak jak konstruktor kopiujący w Javie).

## Zadania utrwalające

??? question "Zadanie 8: Konstruktor z pełną walidacją"
    Zdefiniuj klasę `Produkt` z konstruktorem przyjmującym `nazwa` i `cena`. Konstruktor ma: ustawić `cena` na `0`, jeśli podano wartość ujemną, oraz automatycznie obliczyć i zapisać pole `cena_z_vat` jako `cena * 1.23`.

??? question "Zadanie 9: Kopiowanie obiektu ze zmienną listą"
    Zdefiniuj klasę `Koszyk` z polem `produkty` (pusta lista w konstruktorze) i metodą `dodaj(nazwa)`. Stwórz obiekt, dodaj kilka produktów, skopiuj go metodą `copy.copy()`, dodaj produkt do kopii i sprawdź, czy oryginalny koszyk też się zmienił. Zapisz wniosek — czy `copy.copy()` w pełni rozwiązuje problem przy polu będącym listą? (podpowiedź: poszukaj różnicy między `copy.copy()` a `copy.deepcopy()`).

??? question "Zadanie 10: Porównanie językowe"
    Własnymi słowami wyjaśnij: dlaczego w Javie i C++ konstruktor kopiujący jest osobnym, jawnym mechanizmem językowym, a w Pythonie trzeba go "symulować" przez moduł `copy` lub własną metodę?
