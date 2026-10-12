# Złożone typy danych

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć posługiwać się tablicami jedno- i wielowymiarowymi, tablicami dynamicznymi `List<T>`, słownikiem `Dictionary<TKey, TValue>`, kolejką `Queue<T>` i stosem `Stack<T>`, definiować własne struktury (`struct`) i typy wyliczeniowe (`enum`), wyjaśnić, co dzieje się przy kopiowaniu typu referencyjnego, oraz dobrać strukturę danych do problemu.

## Dlaczego typy złożone

Typ prosty przechowuje jedną wartość. Gdy danych jest wiele — oceny całej klasy, lista zakupów, plansza do gry — potrzebujemy typów, które przechowują **zbiór** wartości albo **kilka powiązanych** wartości naraz.

| Typ | Co przechowuje | Rozmiar | Dostęp |
|---|---|---|---|
| tablica `T[]` | ciąg elementów jednego typu | stały | po indeksie |
| `List<T>` | ciąg elementów jednego typu | zmienny | po indeksie |
| `Dictionary<K, V>` | pary klucz → wartość | zmienny | po kluczu |
| `Queue<T>` | elementy w kolejce | zmienny | pierwszy wchodzi — pierwszy wychodzi |
| `Stack<T>` | elementy na stosie | zmienny | ostatni wchodzi — pierwszy wychodzi |
| `struct` | kilka powiązanych pól | stały | po nazwie pola |
| `enum` | jedna wartość z zamkniętego zbioru nazw | — | po nazwie |

## Tablice jednowymiarowe

```csharp
int[] oceny = new int[5];                 // 5 elementów o wartości domyślnej 0
int[] punkty = { 12, 7, 19, 3 };          // od razu z wartościami
string[] dni = new string[] { "pon", "wt", "śr" };

punkty[0] = 15;                           // zapis
Console.WriteLine(punkty[2]);             // odczyt: 19
Console.WriteLine(punkty.Length);         // liczba elementów: 4
Console.WriteLine(punkty[^1]);            // ostatni element (indeks od końca)
```

- indeksy zaczynają się od **0**, ostatni ma numer `Length - 1`,
- rozmiaru tablicy nie można zmienić po utworzeniu,
- nowe elementy dostają **wartość domyślną** typu: `0`, `false`, `'\0'`, `null`,
- odwołanie poza zakres powoduje wyjątek `IndexOutOfRangeException`.

### Przechodzenie po tablicy

```csharp
// foreach — odczyt każdego elementu
int suma = 0;
foreach (int p in punkty)
{
    suma += p;
}
double srednia = (double)suma / punkty.Length;

// for — gdy potrzebny indeks albo zmiana elementów
for (int i = 0; i < punkty.Length; i++)
{
    punkty[i] = punkty[i] * 2;
}
```

### Metody klasy Array

```csharp
Array.Sort(punkty);                           // sortowanie rosnąco
Array.Reverse(punkty);                        // odwrócenie kolejności
int poz = Array.IndexOf(punkty, 19);          // pozycja elementu lub -1
int[] kopia = (int[])punkty.Clone();          // niezależna kopia
Array.Fill(oceny, 1);                         // wypełnienie jedną wartością
Console.WriteLine(string.Join(", ", punkty)); // wypisanie całej tablicy
```

### Tablica to typ referencyjny

```csharp
int[] a = { 1, 2, 3 };
int[] b = a;          // kopiuje referencję, nie elementy
b[0] = 100;
Console.WriteLine(a[0]);   // 100 — a i b to ta sama tablica
```

Żeby mieć niezależną kopię, użyj `Clone()` albo skopiuj elementy w pętli.

## Tablice wielowymiarowe

### Prostokątna: `[,]`

```csharp
int[,] plansza = new int[3, 4];      // 3 wiersze, 4 kolumny
plansza[1, 2] = 7;

int[,] macierz =
{
    { 1, 2, 3 },
    { 4, 5, 6 }
};

for (int w = 0; w < macierz.GetLength(0); w++)        // liczba wierszy
{
    for (int k = 0; k < macierz.GetLength(1); k++)    // liczba kolumn
    {
        Console.Write($"{macierz[w, k],4}");
    }
    Console.WriteLine();
}
```

### Postrzępiona (tablica tablic): `[][]`

Każdy wiersz może mieć inną długość:

```csharp
int[][] trojkat = new int[4][];
for (int i = 0; i < trojkat.Length; i++)
{
    trojkat[i] = new int[i + 1];
}
trojkat[3][2] = 5;
```

## List&lt;T&gt; — tablica dynamiczna

`T` to typ elementów, np. `List<int>`, `List<string>`, `List<double>`.

```csharp
List<string> zakupy = new List<string>();
zakupy.Add("mleko");
zakupy.Add("chleb");
zakupy.AddRange(new[] { "jajka", "masło" });

Console.WriteLine(zakupy.Count);          // 4
Console.WriteLine(zakupy[1]);             // chleb

zakupy.Insert(0, "kawa");                 // wstaw na pozycję 0
zakupy.Remove("mleko");                   // usuń pierwsze wystąpienie wartości
zakupy.RemoveAt(0);                       // usuń element na pozycji 0
bool jest = zakupy.Contains("jajka");
int indeks = zakupy.IndexOf("masło");
zakupy.Sort();
zakupy.Reverse();
zakupy.Clear();
```

| Operacja | Tablica | `List<T>` |
|---|---|---|
| liczba elementów | `Length` | `Count` |
| dodanie elementu | niemożliwe | `Add`, `Insert` |
| usunięcie | niemożliwe | `Remove`, `RemoveAt` |
| sortowanie | `Array.Sort(t)` | `lista.Sort()` |
| kiedy | znana, stała liczba elementów | liczba elementów zmienia się w trakcie działania |

Nowsze zapisy tworzenia listy: `List<int> l = new();` oraz `List<int> l = [3, 1, 2];`.

## Dictionary&lt;TKey, TValue&gt; — słownik

Przechowuje pary **klucz → wartość**. Klucze są unikalne; dostęp po kluczu jest bardzo szybki, niezależnie od liczby elementów.

```csharp
Dictionary<string, int> stan = new Dictionary<string, int>();
stan["jabłka"] = 12;          // dodanie lub nadpisanie
stan["gruszki"] = 5;
stan.Add("śliwki", 30);       // Add zgłosi wyjątek, jeśli klucz już istnieje

Console.WriteLine(stan["jabłka"]);    // 12

if (stan.ContainsKey("banany"))
{
    Console.WriteLine("Banany są w magazynie.");
}

if (stan.TryGetValue("gruszki", out int ile))
{
    Console.WriteLine($"Gruszek: {ile}");
}

stan.Remove("śliwki");

foreach (KeyValuePair<string, int> para in stan)
{
    Console.WriteLine($"{para.Key}: {para.Value}");
}
```

Odczyt nieistniejącego klucza przez `stan["banany"]` powoduje wyjątek `KeyNotFoundException` — dlatego używamy `TryGetValue` albo `ContainsKey`.

Typowe zastosowanie — zliczanie:

```csharp
Dictionary<char, int> licznik = new();
foreach (char z in "abrakadabra")
{
    if (licznik.ContainsKey(z)) licznik[z]++;
    else licznik[z] = 1;
}
```

## Queue&lt;T&gt; — kolejka

Zasada **FIFO** (*First In, First Out*): pierwszy, który wszedł, pierwszy wychodzi — jak kolejka w sklepie albo kolejka wydruków.

```csharp
Queue<string> wydruki = new Queue<string>();
wydruki.Enqueue("raport.pdf");     // dodaj na koniec
wydruki.Enqueue("zdjecie.png");
wydruki.Enqueue("list.docx");

Console.WriteLine(wydruki.Peek());     // podgląd pierwszego: raport.pdf
string pierwszy = wydruki.Dequeue();   // zdejmij pierwszego
Console.WriteLine(wydruki.Count);      // 2
```

## Stack&lt;T&gt; — stos

Zasada **LIFO** (*Last In, First Out*): ostatni, który wszedł, pierwszy wychodzi — jak stos talerzy albo przycisk „cofnij” w edytorze.

```csharp
Stack<string> historia = new Stack<string>();
historia.Push("wpisano: Ala");     // połóż na wierzch
historia.Push("wpisano: ma");
historia.Push("wpisano: kota");

Console.WriteLine(historia.Peek());    // wpisano: kota
string ostatnia = historia.Pop();      // zdejmij z wierzchu
```

`Dequeue`, `Pop` i `Peek` na pustej kolejce lub stosie zgłaszają wyjątek — przed nimi sprawdź `Count > 0` albo użyj `TryDequeue` / `TryPop`.

## Struktury: struct

**Struktura** łączy kilka powiązanych wartości w jeden typ. Jest **typem wartościowym**: przypisanie kopiuje wszystkie pola.

```csharp
Punkt a = new Punkt(1, 2);
Punkt b = a;          // kopia
b.X = 10;
Console.WriteLine($"{a.X} {b.X}");    // 1 10 — a się nie zmieniło

struct Punkt
{
    public int X;
    public int Y;

    public Punkt(int x, int y)
    {
        X = x;
        Y = y;
    }

    public double OdlegloscOdPoczatku() => Math.Sqrt(X * X + Y * Y);
}
```

Struktur używamy do małych, prostych danych: punkt, kolor RGB, data, ułamek. Do większych modeli z zachowaniem (konto, gracz, zamówienie) używamy **klas** — to temat programowania obiektowego.

## Typy wyliczeniowe: enum

**Typ wyliczeniowy** to zamknięty zbiór nazwanych wartości. Zamiast zapamiętywać, że 0 to poniedziałek, a 3 to czwartek, nadajemy wartościom nazwy.

```csharp
DzienTygodnia dzis = DzienTygodnia.Sroda;

if (dzis == DzienTygodnia.Sobota || dzis == DzienTygodnia.Niedziela)
{
    Console.WriteLine("Weekend");
}

Console.WriteLine(dzis);          // Sroda
Console.WriteLine((int)dzis);     // 2

string opis = dzis switch
{
    DzienTygodnia.Sobota or DzienTygodnia.Niedziela => "wolne",
    _ => "zajęcia"
};

enum DzienTygodnia
{
    Poniedzialek, Wtorek, Sroda, Czwartek, Piatek, Sobota, Niedziela
}
```

Zalety: kompilator nie pozwoli przypisać wartości spoza zbioru, edytor podpowiada nazwy, a kod czyta się jak zdanie. Zamiana tekstu na wartość: `Enum.TryParse("Piatek", out DzienTygodnia d)`.

## Jak wybrać strukturę danych

| Problem | Struktura |
|---|---|
| temperatury z 7 dni tygodnia | `double[]` (7 elementów, stała liczba) |
| plansza do gry w statki | `char[,]` lub `int[,]` |
| uczestnicy zapisujący się na wycieczkę | `List<string>` |
| numer telefonu po nazwisku | `Dictionary<string, string>` |
| obsługa klientów w kolejności przyjścia | `Queue<T>` |
| historia ruchów do cofania | `Stack<T>` |
| współrzędne punktu | `struct` |
| stan zamówienia: nowe, opłacone, wysłane | `enum` |

## Zadania

??? question "Zadanie 1: Oceny"
    Program wczytuje 6 ocen (1–6) do tablicy, odrzucając niepoprawne wpisy. Wypisuje oceny, średnią z dwoma miejscami po przecinku, najwyższą i najniższą (znalezione pętlą, bez gotowych metod) oraz liczbę ocen niedostatecznych.

??? question "Zadanie 2: Odwracanie w miejscu"
    Odwróć kolejność elementów tablicy bez `Array.Reverse` i bez drugiej tablicy, zamieniając miejscami elementy z początku i końca.

??? question "Zadanie 3: Kółko i krzyżyk"
    Napisz grę dla dwóch graczy na planszy `char[3, 3]`. Po każdym ruchu wyświetl planszę i sprawdź, czy ktoś wygrał (wiersz, kolumna, przekątna) albo czy jest remis.

??? question "Zadanie 4: Lista zakupów"
    Program w pętli wyświetla menu: dodaj, usuń, pokaż posortowaną listę, wyczyść, koniec. Nie pozwala dodać produktu, który już jest na liście (bez względu na wielkość liter).

??? question "Zadanie 5: Słownik częstości"
    Program wczytuje zdanie i wypisuje, ile razy wystąpiło każde słowo (bez rozróżniania wielkości liter), posortowane od najczęstszego. Wskazówka: `zdanie.Split(' ')`.

??? question "Zadanie 6: Kolejka w przychodni"
    Symulacja: polecenie `+ Nazwisko` dodaje pacjenta do kolejki, `>` wywołuje następnego, `?` pokazuje, kto czeka. Użyj `Queue<string>`. Obsłuż wywołanie, gdy kolejka jest pusta.

??? question "Zadanie 7: Nawiasy"
    Program sprawdza, czy nawiasy `()`, `[]`, `{}` w podanym tekście są poprawnie sparowane, np. `{[()]}` — tak, `([)]` — nie. Użyj `Stack<char>`.

??? question "Zadanie 8: Struktura i enum"
    Zdefiniuj `enum Kolor { Czerwony, Zielony, Niebieski }` i `struct Pionek` z polami `X`, `Y`, `Kolor`. Utwórz tablicę 4 pionków, przesuń każdy o losowe pole i wypisz ich pozycje. Sprawdź, co się stanie, gdy skopiujesz pionek do innej zmiennej i zmienisz kopię.
