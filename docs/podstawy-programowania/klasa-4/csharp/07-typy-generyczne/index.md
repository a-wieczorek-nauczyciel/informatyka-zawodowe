# Typy generyczne

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, po co są typy generyczne, korzystać z generycznych kolekcji .NET, napisać własną klasę i metodę generyczną, ograniczyć parametr typu klauzulą `where` oraz porównać typy generyczne C# z szablonami klas w C++.

## Problem: ten sam kod dla różnych typów

Potrzebujemy pudełka, które przechowuje jedną rzecz. Dla liczb, tekstów i graczy:

```csharp
class PudelkoInt    { public int Zawartosc; }
class PudelkoString { public string Zawartosc = ""; }
class PudelkoGracz  { public Gracz? Zawartosc; }
```

Trzy razy ten sam kod. Można spróbować z `object`, który przyjmie wszystko:

```csharp
// Program.cs
Pudelko p = new Pudelko { Zawartosc = 42 };
int x = (int)p.Zawartosc!;          // rzutowanie przy każdym odczycie
string s = (string)p.Zawartosc!;    // skompiluje się, ale rzuci wyjątek w trakcie działania

// Pudelko.cs
class Pudelko { public object? Zawartosc; }
```

Tracimy kontrolę typów — błąd wyjdzie dopiero podczas działania programu.

## Rozwiązanie: parametr typu

**Typ generyczny** ma **parametr typu** — zwykle oznaczany `T` — za który przy użyciu podstawiamy konkretny typ:

```csharp
// Pudelko.cs
class Pudelko<T>
{
    public T Zawartosc { get; set; }

    public Pudelko(T zawartosc)
    {
        Zawartosc = zawartosc;
    }

    public bool CzyZawiera(T wartosc) => Equals(Zawartosc, wartosc);
}

// Program.cs
Pudelko<int> liczba = new Pudelko<int>(42);
Pudelko<string> tekst = new Pudelko<string>("Ala");
Pudelko<Gracz> gracz = new Pudelko<Gracz>(new Gracz("Ada"));

int x = liczba.Zawartosc;           // bez rzutowania
// string s = liczba.Zawartosc;     // błąd kompilacji — typ jest pilnowany
```

Jedna definicja, wiele typów, pełna kontrola kompilatora.

## Znasz to już: kolekcje generyczne

| Typ | Parametry typu | Przykład |
|---|---|---|
| `List<T>` | typ elementu | `List<double>` |
| `Dictionary<TKey, TValue>` | typ klucza i wartości | `Dictionary<string, int>` |
| `Queue<T>`, `Stack<T>` | typ elementu | `Queue<Zamowienie>` |
| `HashSet<T>` | typ elementu (bez powtórzeń) | `HashSet<string>` |
| `IComparable<T>`, `IEnumerable<T>` | typ porównywany / zwracany | `IComparable<Uczen>` |

Typy generyczne pojawiły się w C# 2.0 (2005) właśnie po to, żeby kolekcje były bezpieczne i szybkie.

## Kilka parametrów typu

```csharp
// Program.cs
var wynik = new Para<string, int>("Ada", 95);
var punkt = new Para<double, double>(1.5, -2);

// Para.cs
class Para<TPierwszy, TDrugi>
{
    public TPierwszy Pierwszy { get; }
    public TDrugi Drugi { get; }

    public Para(TPierwszy pierwszy, TDrugi drugi)
    {
        Pierwszy = pierwszy;
        Drugi = drugi;
    }

    public override string ToString() => $"({Pierwszy}, {Drugi})";
}
```

## Metody generyczne

Parametr typu może mieć też sama metoda — w zwykłej klasie:

```csharp
// Program.cs
int x = 1, y = 2;
Narzedzia.Zamien(ref x, ref y);           // T = int, kompilator sam ustalił typ

string s1 = "a", s2 = "b";
Narzedzia.Zamien<string>(ref s1, ref s2); // typ można też podać jawnie

Narzedzia.Wypisz(new List<string> { "chleb", "mleko" });

// Narzedzia.cs
static class Narzedzia
{
    public static void Zamien<T>(ref T a, ref T b)
    {
        T tmp = a;
        a = b;
        b = tmp;
    }

    public static void Wypisz<T>(IEnumerable<T> elementy)
    {
        foreach (T e in elementy)
            Console.WriteLine($"- {e}");
    }
}
```

Zwykle nie trzeba pisać `<int>` przy wywołaniu — kompilator wnioskuje typ z argumentów.

## Ograniczenia: where

Wewnątrz klasy generycznej o typie `T` nic nie wiadomo, więc nie można np. porównać `a > b` ani wywołać `a.Pole()`. **Ograniczenia** mówią kompilatorowi, jaki musi być `T`:

```csharp
static T Najwiekszy<T>(List<T> lista) where T : IComparable<T>
{
    T max = lista[0];
    foreach (T e in lista)
    {
        if (e.CompareTo(max) > 0) max = e;     // wolno, bo T implementuje IComparable<T>
    }
    return max;
}

Najwiekszy(new List<int> { 3, 9, 2 });          // 9
Najwiekszy(new List<string> { "kot", "ala" });  // "kot"
Najwiekszy(listaUczniow);                       // działa, jeśli Uczen : IComparable<Uczen>
```

| Ograniczenie | Znaczenie |
|---|---|
| `where T : IComparable<T>` | `T` implementuje wskazany interfejs |
| `where T : Ksztalt` | `T` jest klasą `Ksztalt` lub pochodną |
| `where T : class` | `T` jest typem referencyjnym |
| `where T : struct` | `T` jest typem wartościowym |
| `where T : new()` | `T` ma konstruktor bezparametrowy — można pisać `new T()` |

Przykład z hierarchią klas:

```csharp
// Program.cs
var kola = new Kolekcja<Kolo>();
kola.Dodaj(new Kolo(1));
// kola.Dodaj(new Prostokat(1, 2));   // błąd — to kolekcja kół

// Kolekcja.cs
class Kolekcja<T> where T : Ksztalt
{
    private readonly List<T> elementy = new();

    public void Dodaj(T k) => elementy.Add(k);

    public double SumaPol()
    {
        double s = 0;
        foreach (T k in elementy) s += k.Pole();   // wolno, bo T to Ksztalt
        return s;
    }
}
```

## Typy generyczne a szablony C++

W programie nauczania typy generyczne występują jako odpowiednik **szablonów klas** z C++ (`template<typename T> class Pudelko`). Idea jest ta sama — jedna definicja dla wielu typów. Różnice:

| | Szablony C++ | Typy generyczne C# |
|---|---|---|
| kiedy powstaje kod dla konkretnego typu | podczas kompilacji, osobna kopia dla każdego typu | typ generyczny istnieje w CIL; konkretyzacja przy JIT |
| sprawdzanie operacji na `T` | dopiero przy użyciu z konkretnym typem | od razu, na podstawie ograniczeń `where` |
| zapis | `template<typename T>` | `class Nazwa<T>` |

## Zadania

??? question "Zadanie 1: Stos generyczny"
    Napisz własną klasę `MojStos<T>` opartą na prywatnej `List<T>`, z metodami `Push`, `Pop`, `Peek`, `CzyPusty` i właściwością `Liczba`. `Pop` i `Peek` na pustym stosie zwracają `false` przez metody `TryPop(out T wynik)` i `TryPeek(out T wynik)`. Przetestuj dla `int`, `string` i `Gracz`.

??? question "Zadanie 2: Metody generyczne"
    W klasie `Narzedzia` napisz: `Zamien<T>`, `Odwroc<T>(List<T> lista)` (bez `Reverse`), `IleRazy<T>(List<T> lista, T szukany)`, `Najmniejszy<T>(List<T> lista) where T : IComparable<T>`.

??? question "Zadanie 3: Rejestr"
    Klasa `Rejestr<TKlucz, TWartosc>` opakowuje `Dictionary` i dodaje historię zmian: każda operacja `Ustaw(klucz, wartosc)` zapisuje w liście wpis „klucz: stara → nowa”. Metody: `Ustaw`, `Pobierz` (z `TryGetValue`), `Historia()`.

??? question "Zadanie 4: Ranking"
    Klasa `Ranking<T> where T : IComparable<T>` przechowuje najwyżej *n* najlepszych elementów (n podane w konstruktorze). Dodanie elementu słabszego od wszystkich przy pełnym rankingu nic nie zmienia. Użyj jej dla wyników liczbowych i dla obiektów `Uczen`.

??? question "Zadanie 5: Dlaczego nie object?"
    Napisz dwie wersje klasy `Pudelko` — z `object` i generyczną. Pokaż przykład błędu, który w wersji z `object` ujawni się dopiero w trakcie działania, a w wersji generycznej — przy kompilacji.
