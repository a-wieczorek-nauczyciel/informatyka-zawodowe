# Metody

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć podzielić program na metody, zdefiniować metodę z parametrami i wartością zwracaną, rozróżnić metodę zwracającą wynik od metody `void`, przekazywać argumenty przez wartość oraz przez `ref` i `out`, a także przeciążać metody.

## Funkcja w Pythonie, metoda w C#

W Pythonie piszesz **funkcje** (`def`). W C# każda funkcja należy do jakiejś klasy, dlatego nazywamy ją **metodą**. W programie z instrukcjami najwyższego poziomu (`Program.cs`) możesz jednak pisać metody bezpośrednio w pliku — kompilator sam umieści je w klasie.

=== "Python"

    ```python
    def oblicz_bmi(masa_kg, wzrost_cm):
        wzrost_m = wzrost_cm / 100
        return masa_kg / (wzrost_m * wzrost_m)

    bmi = oblicz_bmi(72, 180)
    print(f"{bmi:.2f}")
    ```

=== "C#"

    ```csharp
    double bmi = ObliczBmi(72, 180);
    Console.WriteLine($"{bmi:F2}");

    static double ObliczBmi(double masaKg, double wzrostCm)
    {
        double wzrostM = wzrostCm / 100.0;
        return masaKg / (wzrostM * wzrostM);
    }
    ```

## Budowa metody

```text
static   double     ObliczBmi  (double masaKg, double wzrostCm)
  │         │           │                    │
  │         │           │                    └── parametry: typ i nazwa każdego
  │         │           └── nazwa (w C#: WielkąLiterąNaPoczątkuKażdegoSłowa)
  │         └── typ zwracany (void = nic nie zwraca)
  └── modyfikator
```

Para „nazwa + typy parametrów” to **sygnatura** metody.

| | Python | C# |
|---|---|---|
| definicja | `def nazwa(a, b):` | `static int Nazwa(int a, int b) { ... }` |
| typy parametrów | opcjonalne podpowiedzi | **obowiązkowe** |
| typ wyniku | niepodawany | **obowiązkowy** (`void`, gdy brak wyniku) |
| styl nazw | `oblicz_bmi` | `ObliczBmi` |

Kompilator pilnuje typów: `ObliczBmi("72", 180)` to błąd kompilacji, a metoda typu `double` musi zwracać wartość w każdej ścieżce wykonania.

## Metoda zwracająca wynik a metoda void

```csharp
static int Kwadrat(int x)            // oblicza i zwraca wynik
{
    return x * x;
}

static void Powitaj(string imie)     // wykonuje czynność, nic nie zwraca
{
    Console.WriteLine($"Cześć, {imie}!");
}

int k = Kwadrat(5);      // wynik zapisujemy w zmiennej
Powitaj("Ada");          // wywołanie jako osobna instrukcja
```

!!! tip "Dobra metoda robi jedną rzecz"
    `ObliczBmi` tylko liczy — nie pyta użytkownika o dane i nic nie wypisuje. Dzięki temu można jej użyć w programie konsolowym, w aplikacji okienkowej i w testach. Wczytywanie danych i wypisywanie wyników zostaw w innych miejscach programu.

## Parametry domyślne i nazwane

```csharp
static double Brutto(double netto, double vat = 0.23)
{
    return netto * (1 + vat);
}

Brutto(100);              // vat = 0.23
Brutto(100, 0.08);        // vat = 0.08
Brutto(vat: 0.05, netto: 100);   // argumenty nazwane, dowolna kolejność
```

## Przekazywanie argumentów: wartość, ref, out

Domyślnie metoda dostaje **kopię** wartości typu prostego. Zmiana parametru nie zmienia zmiennej w miejscu wywołania.

```csharp
static void Zwieksz(int x) { x++; }
static void ZwiekszRef(ref int x) { x++; }

int a = 5;
Zwieksz(a);            // a nadal 5
ZwiekszRef(ref a);     // a = 6
```

| Słowo | Znaczenie |
|---|---|
| (brak) | metoda dostaje kopię wartości |
| `ref` | metoda pracuje na oryginalnej zmiennej; zmienna musi mieć wartość przed wywołaniem |
| `out` | metoda **musi** przypisać wartość; służy do zwrócenia dodatkowego wyniku |
| `in` | przekazanie bez kopiowania, tylko do odczytu |

`out` znasz już z `int.TryParse(tekst, out int liczba)`: metoda zwraca `bool`, a liczbę „oddaje” przez `out`. Własna metoda z `out`:

```csharp
static bool Podziel(int a, int b, out int wynik)
{
    if (b == 0)
    {
        wynik = 0;
        return false;
    }
    wynik = a / b;
    return true;
}

if (Podziel(10, 3, out int w))
{
    Console.WriteLine(w);
}
```

## Przeciążanie metod

**Przeciążanie** to kilka metod o tej samej nazwie, różniących się parametrami. Kompilator wybiera wersję na podstawie argumentów.

```csharp
Console.WriteLine(Obliczenia.Pole(3));          // koło
Console.WriteLine(Obliczenia.Pole(3, 4));       // prostokąt

static class Obliczenia
{
    public static double Pole(double r) => Math.PI * r * r;
    public static double Pole(double a, double b) => a * b;
}
```

Zapis `=> wyrażenie` to skrócona metoda, która od razu zwraca wynik wyrażenia. Przeciążać można metody w klasie — metod zapisanych bezpośrednio w `Program.cs` nie da się przeciążyć. W Pythonie przeciążania w tej postaci nie ma; zamiast tego używa się parametrów domyślnych.

## Zadania

??? question "Zadanie 1: Wydziel metodę"
    W kalkulatorze BMI wydziel obliczenie do metody `ObliczBmi(double masaKg, double wzrostCm)`. Jakie dane dostaje metoda? Co zwraca? Dlaczego nie ma w niej `Console.ReadLine()`?

??? question "Zadanie 2: Co zwróci metoda?"
    Przewidź, a potem sprawdź, co zwrócą wywołania: `ObliczBmi(80, 200)`, `ObliczBmi(70, 0)`, `ObliczBmi(0, 180)`. Czy któreś zgłosi wyjątek? Dlaczego walidacja jest potrzebna przed wywołaniem metody?

??? question "Zadanie 3: Metoda do wczytywania"
    Napisz metodę `static double WczytajLiczbe(string komunikat, double min, double max)`, która pyta użytkownika tak długo, aż poda liczbę z zakresu `min`–`max`, i zwraca ją. Użyj jej w kalkulatorze BMI do pobrania masy i wzrostu.

??? question "Zadanie 4: Zestaw metod"
    Napisz metody: `CzyPierwsza(int n)` → `bool`, `Silnia(int n)` → `long`, `Nwd(int a, int b)` → `int`. Program główny wczytuje liczbę i wypisuje wyniki wszystkich trzech metod (dla NWD wczytaj drugą liczbę).

??? question "Zadanie 5: min i max jednocześnie"
    Napisz metodę `static void MinMax(int[] t, out int min, out int max)`. Wywołaj ją dla tablicy liczb wczytanych od użytkownika. Dlaczego tutaj `out` jest wygodniejsze niż `return`?

??? question "Zadanie 6: Przeciążanie"
    W klasie statycznej `Formatowanie` napisz trzy przeciążone metody `Opisz`: dla `int`, dla `double` (z dwoma miejscami po przecinku) i dla `bool` (zwraca „tak” / „nie”). Wywołaj każdą z nich.
