# Metody

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć podzielić program na metody, zdefiniować metodę z parametrami i wartością zwracaną, rozróżnić metodę zwracającą wynik od metody `void`, przekazywać argumenty przez wartość oraz słowami `ref`, `out`, `in` i `params`, stosować parametry domyślne i nazwane, przeciążać metody oraz projektować metody, które łatwo testować.

## Po co metody

**Metoda** to nazwany fragment kodu, który wykonuje jedno zadanie i może być wywoływany wiele razy. Dzięki metodom:

- **nie powtarzamy kodu** — obliczenie zapisane raz wywołujemy w wielu miejscach,
- **dzielimy problem na części** — każdą metodę piszemy i sprawdzamy osobno,
- **nazywamy czynności** — `ObliczBmi(masa, wzrost)` mówi więcej niż wzór wpisany w środek programu,
- **łatwiej poprawiamy błędy** — poprawka w jednym miejscu działa wszędzie.

W C# każda metoda należy do jakiegoś **typu** (klasy lub struktury). Dlatego zamiast „funkcja” mówimy „metoda”.

## Budowa metody

```csharp
static double ObliczBmi(double masaKg, double wzrostCm)
{
    double wzrostM = wzrostCm / 100.0;
    return masaKg / (wzrostM * wzrostM);
}
```

| Element | W przykładzie | Znaczenie |
|---|---|---|
| modyfikator | `static` | metoda należy do klasy, nie do obiektu (szczegóły w klasie 4) |
| typ zwracany | `double` | typ wyniku; `void` — metoda nic nie zwraca |
| nazwa | `ObliczBmi` | czasownik, PascalCase |
| lista parametrów | `(double masaKg, double wzrostCm)` | typ i nazwa każdego parametru |
| ciało | `{ … }` | instrukcje metody |
| `return` | `return masaKg / …;` | kończy metodę i przekazuje wynik |

**Sygnatura** metody to jej nazwa i typy parametrów (w kolejności): `ObliczBmi(double, double)`. Typ zwracany do sygnatury nie należy.

**Parametr** to zmienna w definicji metody (`masaKg`). **Argument** to wartość przekazana przy wywołaniu (`72`).

## Gdzie zapisujemy metody

### W programie z instrukcjami najwyższego poziomu

Metody można zapisać bezpośrednio w `Program.cs` — kompilator umieści je w niewidocznej klasie. Można je wywoływać przed miejscem definicji.

```csharp
double bmi = ObliczBmi(72, 180);
Console.WriteLine($"BMI: {bmi:F2}");

static double ObliczBmi(double masaKg, double wzrostCm)
{
    double wzrostM = wzrostCm / 100.0;
    return masaKg / (wzrostM * wzrostM);
}
```

### W klasie

W większych programach metody grupujemy w klasach, najlepiej w osobnych plikach:

```csharp
// Program.cs
Console.WriteLine(Geometria.PoleKola(2));
Console.WriteLine(Geometria.ObwodKola(2));

// Geometria.cs
static class Geometria
{
    public static double PoleKola(double r) => Math.PI * r * r;
    public static double ObwodKola(double r) => 2 * Math.PI * r;
}
```

Słowo `public` sprawia, że metodę widać poza klasą. Wywołanie: `NazwaKlasy.NazwaMetody(...)` — tak samo jak `Math.Sqrt(16)` czy `Console.WriteLine()`.

## Wynik metody

### return

```csharp
static int Wieksza(int a, int b)
{
    if (a > b)
    {
        return a;      // metoda kończy się tutaj
    }
    return b;
}
```

- metoda z typem innym niż `void` musi zwrócić wartość **w każdej ścieżce** — inaczej błąd `CS0161`,
- `return` natychmiast kończy metodę, więc kod po nim się nie wykona,
- w metodzie `void` samo `return;` kończy metodę wcześniej.

### Metoda void

```csharp
static void WypiszRamke(string tekst)
{
    string linia = new string('*', tekst.Length + 4);
    Console.WriteLine(linia);
    Console.WriteLine($"* {tekst} *");
    Console.WriteLine(linia);
}

WypiszRamke("Witaj");
```

Wynik metody zwracającej wartość zapisujemy w zmiennej albo używamy w wyrażeniu. Metodę `void` wywołujemy jako osobną instrukcję.

### Zapis skrócony

Metodę, która tylko zwraca wynik jednego wyrażenia, można zapisać ze strzałką `=>`:

```csharp
static int Kwadrat(int x) => x * x;
static bool CzyParzysta(int x) => x % 2 == 0;
```

## Zmienne lokalne i zasięg

Zmienne zadeklarowane w metodzie (i jej parametry) istnieją tylko podczas wykonywania tej metody. Każde wywołanie ma własne, nowe zmienne.

```csharp
static int Licz()
{
    int licznik = 0;    // przy każdym wywołaniu znowu 0
    licznik++;
    return licznik;
}

Console.WriteLine(Licz());   // 1
Console.WriteLine(Licz());   // 1
```

Metoda nie widzi zmiennych lokalnych innych metod. Wszystko, czego potrzebuje, powinna dostać przez **parametry**, a wynik oddać przez **return**.

## Przekazywanie argumentów

### Przez wartość (domyślnie)

Metoda dostaje **kopię** wartości typu prostego. Zmiana parametru nie zmienia zmiennej w miejscu wywołania.

```csharp
static void Zwieksz(int x) { x++; }

int a = 5;
Zwieksz(a);
Console.WriteLine(a);   // 5
```

### Typy referencyjne

Przy tablicy, liście czy obiekcie metoda dostaje kopię **referencji** — wskazuje ten sam obiekt. Zmiana elementów jest widoczna na zewnątrz:

```csharp
static void Podwoj(int[] t)
{
    for (int i = 0; i < t.Length; i++) t[i] *= 2;
}

int[] liczby = { 1, 2, 3 };
Podwoj(liczby);
Console.WriteLine(string.Join(", ", liczby));   // 2, 4, 6
```

### ref, out, in

| Słowo | Znaczenie | Zmienna przed wywołaniem | Metoda |
|---|---|---|---|
| `ref` | metoda pracuje na oryginalnej zmiennej | musi mieć wartość | może czytać i zmieniać |
| `out` | metoda oddaje przez parametr dodatkowy wynik | nie musi mieć wartości | **musi** przypisać wartość |
| `in` | przekazanie bez kopiowania, tylko do odczytu | musi mieć wartość | może tylko czytać |

```csharp
static void Zamien(ref int x, ref int y)
{
    int tmp = x;
    x = y;
    y = tmp;
}

int p = 1, q = 2;
Zamien(ref p, ref q);
Console.WriteLine($"{p} {q}");   // 2 1
```

`out` znasz już z `int.TryParse(tekst, out int liczba)`: metoda zwraca `bool` (czy się udało), a liczbę oddaje przez `out`. Własna metoda w tym stylu:

```csharp
static bool Podziel(int a, int b, out int iloraz, out int reszta)
{
    if (b == 0)
    {
        iloraz = 0;
        reszta = 0;
        return false;
    }
    iloraz = a / b;
    reszta = a % b;
    return true;
}

if (Podziel(17, 5, out int il, out int r))
{
    Console.WriteLine($"17 = 5 · {il} + {r}");
}
```

Słowa `ref` i `out` piszemy **zarówno w definicji, jak i przy wywołaniu** — żeby było widać, że zmienna może się zmienić.

### params — zmienna liczba argumentów

```csharp
static int Suma(params int[] liczby)
{
    int s = 0;
    foreach (int x in liczby) s += x;
    return s;
}

Suma(1, 2);           // 3
Suma(1, 2, 3, 4, 5);  // 15
Suma();               // 0
```

## Parametry domyślne i nazwane

```csharp
static decimal Brutto(decimal netto, decimal vat = 0.23m, int zaokraglenie = 2)
{
    return Math.Round(netto * (1 + vat), zaokraglenie);
}

Brutto(100m);                       // vat = 0.23, zaokrąglenie = 2
Brutto(100m, 0.08m);                // vat = 0.08
Brutto(100m, zaokraglenie: 0);      // pomijamy vat, podajemy zaokrąglenie po nazwie
Brutto(vat: 0.05m, netto: 100m);    // dowolna kolejność argumentów nazwanych
```

Parametry z wartością domyślną muszą stać **na końcu** listy.

## Przeciążanie metod

**Przeciążanie** to kilka metod o tej samej nazwie i **różnych sygnaturach** (liczbie lub typach parametrów). Kompilator wybiera wersję na podstawie argumentów.

```csharp
Console.WriteLine(Figury.Pole(3));          // koło o promieniu 3
Console.WriteLine(Figury.Pole(3, 4));       // prostokąt 3 × 4
Console.WriteLine(Figury.Pole(3, 4, 5));    // trójkąt o bokach 3, 4, 5

static class Figury
{
    public static double Pole(double r) => Math.PI * r * r;

    public static double Pole(double a, double b) => a * b;

    public static double Pole(double a, double b, double c)
    {
        double p = (a + b + c) / 2;                       // wzór Herona
        return Math.Sqrt(p * (p - a) * (p - b) * (p - c));
    }
}
```

Metody różniące się **tylko** typem zwracanym nie są przeciążeniem — to błąd kompilacji. Przeciążać można metody w klasach; metod zapisanych bezpośrednio w `Program.cs` (funkcji lokalnych) — nie.

`Console.WriteLine` to przykład przeciążania z biblioteki: ma osobne wersje dla `int`, `double`, `string`, `bool`, `char` i innych typów.

## Dobra metoda

- **robi jedną rzecz** — `ObliczBmi` liczy, nie pyta użytkownika i nic nie wypisuje,
- **ma nazwę-czasownik**, która mówi, co robi: `ObliczSrednia`, `CzyPierwsza`, `WczytajLiczbe`,
- **dostaje dane przez parametry, oddaje wynik przez return** — nie korzysta z ukrytych zmiennych,
- **jest krótka** — jeśli nie mieści się na ekranie, zwykle da się ją podzielić,
- **jest opisana**, gdy działanie nie jest oczywiste:

```csharp
/// <summary>
/// Oblicza największy wspólny dzielnik algorytmem Euklidesa.
/// </summary>
/// <param name="a">pierwsza liczba, dodatnia</param>
/// <param name="b">druga liczba, dodatnia</param>
/// <returns>NWD liczb a i b</returns>
static int Nwd(int a, int b)
{
    while (b != 0)
    {
        int r = a % b;
        a = b;
        b = r;
    }
    return a;
}
```

Taką metodę łatwo sprawdzić — wystarczy kilka wywołań ze znanym wynikiem:

```csharp
Console.WriteLine(Nwd(12, 18) == 6);
Console.WriteLine(Nwd(17, 5) == 1);
Console.WriteLine(Nwd(100, 25) == 25);
```

## Zadania

??? question "Zadanie 1: Wydziel metody w kalkulatorze BMI"
    Podziel kalkulator BMI na metody: `WczytajLiczbe(string komunikat, double min, double max)` — pyta, dopóki użytkownik nie poda liczby z zakresu; `ObliczBmi(double masaKg, double wzrostCm)` — tylko oblicza; `WypiszWynik(double bmi)`. Program główny ma mieć najwyżej 5 linii.

??? question "Zadanie 2: Co zwróci metoda?"
    Przewidź, a potem sprawdź, co zwrócą wywołania: `ObliczBmi(80, 200)`, `ObliczBmi(70, 0)`, `ObliczBmi(0, 180)`. Czy któreś zgłosi wyjątek? Dlaczego walidacja jest potrzebna przed wywołaniem metody?

??? question "Zadanie 3: Biblioteka matematyczna"
    W klasie statycznej `Matematyka` napisz metody: `CzyPierwsza(int n)`, `Silnia(int n)` → `long`, `Nwd(int a, int b)`, `Nww(int a, int b)` (korzystając z `Nwd`), `SumaCyfr(int n)`, `Odwroc(int n)` (np. 1234 → 4321). Dla każdej przygotuj trzy wywołania sprawdzające ze znanym wynikiem.

??? question "Zadanie 4: Statystyki tablicy"
    Napisz metody przyjmujące tablicę `int[]`: `Suma`, `Srednia`, `Max`, `Min`, `IlePowyzej(int[] t, int prog)`. Następnie jedną metodę `Statystyki(int[] t, out int min, out int max, out double srednia)`. Porównaj oba podejścia.

??? question "Zadanie 5: Zamiana i sortowanie"
    Napisz metodę `Zamien(ref int a, ref int b)` i użyj jej w metodzie `Sortuj3(ref int a, ref int b, ref int c)`, która ustawia trzy zmienne rosnąco.

??? question "Zadanie 6: Przeciążanie"
    W klasie `Tekst` napisz przeciążone metody `Powtorz`: `(string s, int n)` — zwraca tekst powtórzony n razy; `(char c, int n)` — znak powtórzony n razy; `(string s)` — tekst powtórzony 2 razy. Dodaj parametr domyślny `separator` w pierwszej wersji.

??? question "Zadanie 7: Średnia z params"
    Napisz metodę `Srednia(params double[] liczby)`, która dla braku argumentów zwraca 0. Wywołaj ją z różną liczbą argumentów i z gotową tablicą.
