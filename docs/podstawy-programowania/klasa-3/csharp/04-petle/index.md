# Pętle

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć dobrać rodzaj pętli do zadania, zapisać pętle `for`, `while`, `do-while` i `foreach`, sterować nimi za pomocą `break` i `continue`, zagnieżdżać pętle, wyjaśnić zasięg zmiennych w pętli oraz rozpoznać i naprawić pętlę nieskończoną.

## Po co pętle

Pętla powtarza fragment kodu. Bez niej wypisanie liczb od 1 do 1000 wymagałoby tysiąca linii. Każda pętla ma:

- **warunek** — dopóki jest prawdziwy, pętla działa,
- **ciało** — instrukcje powtarzane w każdym **obrocie** (iteracji),
- **zmianę stanu** — coś, co sprawia, że warunek w końcu stanie się fałszywy.

## Pętla for — gdy znasz liczbę powtórzeń

```csharp
for (int i = 1; i <= 5; i++)
{
    Console.WriteLine($"Obrót numer {i}");
}
```

| Część nagłówka | Przykład | Kiedy się wykonuje |
|---|---|---|
| inicjalizacja | `int i = 1` | raz, przed pierwszym obrotem |
| warunek | `i <= 5` | przed każdym obrotem; fałsz kończy pętlę |
| krok | `i++` | po każdym obrocie |

Kolejność wykonania: inicjalizacja → warunek → ciało → krok → warunek → ciało → krok → … → warunek fałszywy → koniec.

Warianty:

```csharp
for (int i = 10; i >= 1; i--) { }       // odliczanie: 10, 9, … 1
for (int i = 0; i <= 100; i += 5) { }   // co 5: 0, 5, 10, … 100
for (int i = 1; i <= 1000; i *= 2) { }  // potęgi dwójki: 1, 2, 4, … 512
for (int i = 0; i < tekst.Length; i++)  // po indeksach tekstu lub tablicy
{
    Console.WriteLine(tekst[i]);
}
```

!!! tip "Liczenie od zera"
    Tablice i teksty w C# numerujemy od 0. Pętla po wszystkich elementach to `for (int i = 0; i < t.Length; i++)` — z `<`, a nie `<=`. Warunek `i <= t.Length` spowoduje wyjątek `IndexOutOfRangeException` w ostatnim obrocie.

## Pętla while — dopóki warunek jest prawdziwy

```csharp
int liczba = 1;
int potega = 0;
while (liczba < 1000)
{
    liczba *= 2;
    potega++;
}
Console.WriteLine($"2^{potega} = {liczba} to pierwsza potęga dwójki ≥ 1000");
```

Warunek sprawdzany jest **przed** każdym obrotem, więc ciało może nie wykonać się ani razu. Używamy jej, gdy nie wiadomo z góry, ile będzie obrotów.

Przykład: suma cyfr liczby.

```csharp
int n = 9475;
int suma = 0;
while (n > 0)
{
    suma += n % 10;   // ostatnia cyfra
    n /= 10;          // usuń ostatnią cyfrę
}
Console.WriteLine(suma);   // 25
```

## Pętla do-while — przynajmniej raz

Warunek sprawdzany jest **po** obrocie, więc ciało wykona się co najmniej raz. Typowe zastosowanie: pytanie użytkownika, dopóki nie poda poprawnych danych, albo menu programu.

```csharp
int wiek;
bool poprawne;
do
{
    Console.Write("Podaj wiek (0–120): ");
    poprawne = int.TryParse(Console.ReadLine(), out wiek) && wiek >= 0 && wiek <= 120;
    if (!poprawne)
    {
        Console.WriteLine("Niepoprawne dane, spróbuj jeszcze raz.");
    }
} while (!poprawne);

Console.WriteLine($"Wiek: {wiek}");
```

Po `while (...)` w `do-while` stoi **średnik**.

### Menu programu

```csharp
string wybor;
do
{
    Console.WriteLine("1 — powitanie, 2 — data, 0 — koniec");
    wybor = Console.ReadLine() ?? "";

    switch (wybor)
    {
        case "1": Console.WriteLine("Cześć!"); break;
        case "2": Console.WriteLine(DateTime.Now); break;
        case "0": break;
        default: Console.WriteLine("Nieznana opcja"); break;
    }
} while (wybor != "0");
```

## Pętla foreach — dla każdego elementu

```csharp
string[] imiona = { "Ada", "Olek", "Ewa" };
foreach (string imie in imiona)
{
    Console.WriteLine($"Cześć, {imie}!");
}

foreach (char znak in "Programowanie")
{
    if ("aeiouy".Contains(znak)) Console.Write(znak);
}
```

`foreach` przechodzi przez tablicę, listę, tekst i każdą inną kolekcję. Nie trzeba pilnować indeksów, więc nie da się wyjść poza zakres.

Ograniczenia:

- zmienna pętli jest **tylko do odczytu** — `imie = "X";` to błąd kompilacji,
- w trakcie `foreach` nie wolno dodawać ani usuwać elementów listy (wyjątek `InvalidOperationException`),
- nie znasz numeru bieżącego elementu.

W tych sytuacjach używamy `for`.

## break i continue

```csharp
for (int i = 1; i <= 100; i++)
{
    if (i % 3 == 0) continue;   // pomiń resztę tego obrotu, przejdź do następnego
    if (i > 20) break;          // zakończ całą pętlę
    Console.Write($"{i} ");
}
```

`break` i `continue` działają na **najbliższą** pętlę, w której się znajdują.

Wyszukiwanie z przerwaniem:

```csharp
int[] liczby = { 4, 8, 15, 16, 23, 42 };
int szukana = 16;
int pozycja = -1;
for (int i = 0; i < liczby.Length; i++)
{
    if (liczby[i] == szukana)
    {
        pozycja = i;
        break;
    }
}
Console.WriteLine(pozycja >= 0 ? $"Znaleziono na pozycji {pozycja}" : "Nie znaleziono");
```

## Pętle zagnieżdżone

```csharp
for (int wiersz = 1; wiersz <= 5; wiersz++)
{
    for (int kolumna = 1; kolumna <= wiersz; kolumna++)
    {
        Console.Write("*");
    }
    Console.WriteLine();
}
```

Wewnętrzna pętla wykonuje się w całości przy **każdym** obrocie zewnętrznej. Dla pętli 10 × 10 ciało wewnętrzne wykona się 100 razy, dla 1000 × 1000 — milion razy. Warto o tym pamiętać przy dużych danych.

## Zasięg zmiennych

Zmienna istnieje od miejsca deklaracji do końca bloku `{ }`, w którym ją zadeklarowano. Zmienna z nagłówka `for` istnieje tylko w pętli.

```csharp
int suma = 0;                      // przed pętlą — widoczna po pętli
for (int i = 1; i <= 5; i++)
{
    int kwadrat = i * i;           // tworzona od nowa w każdym obrocie
    suma += kwadrat;
}
Console.WriteLine(suma);           // OK
// Console.WriteLine(kwadrat);     // CS0103: nie istnieje w tym miejscu
// Console.WriteLine(i);           // CS0103
```

Jeśli wartość jest potrzebna po pętli, zadeklaruj zmienną **przed** pętlą.

## Pętla nieskończona

```csharp
int i = 0;
while (i < 10)
{
    Console.WriteLine(i);
    // brak i++ — warunek nigdy nie stanie się fałszywy
}
```

Program „wisi” i trzeba go przerwać klawiszami ++ctrl+c++. Przyczyny: brak zmiany zmiennej sterującej, zmiana w złą stronę (`i--` zamiast `i++`), warunek, który zawsze jest prawdziwy.

Celowa pętla nieskończona z wyjściem przez `break`:

```csharp
while (true)
{
    string polecenie = Console.ReadLine() ?? "";
    if (polecenie == "koniec") break;
    Console.WriteLine($"Wykonuję: {polecenie}");
}
```

## Którą pętlę wybrać

| Sytuacja | Pętla |
|---|---|
| znana liczba powtórzeń, potrzebny licznik | `for` |
| przejście po wszystkich elementach kolekcji, bez zmiany | `foreach` |
| powtarzaj, dopóki coś jest prawdą — może ani razu | `while` |
| wykonaj co najmniej raz (pobranie danych, menu) | `do-while` |

## Zadania

??? question "Zadanie 1: Ciągi"
    Wypisz pętlami: liczby parzyste od 2 do 50; liczby od 100 do 0 co 7; pierwsze 15 wyrazów ciągu Fibonacciego; wszystkie dzielniki liczby podanej przez użytkownika.

??? question "Zadanie 2: Tabliczka mnożenia"
    Wypisz tabliczkę mnożenia 10 × 10 z nagłówkami wierszy i kolumn. Kolumny wyrównaj formatem `{x,4}`.

??? question "Zadanie 3: Kalkulator BMI z powtarzaniem"
    Zmień kalkulator BMI tak, żeby po błędnym wejściu pytał ponownie, a po obliczeniu pytał, czy liczyć dalej (`t` / `n`). Która czynność ma się powtarzać i którą pętlę wybierzesz do każdej z nich?

??? question "Zadanie 4: Zgadywanka"
    Program losuje liczbę od 1 do 100 (`Random.Shared.Next(1, 101)`), a użytkownik zgaduje. Po każdej próbie podpowiada „za mało” / „za dużo”, a na końcu wypisuje liczbę prób. Gracz ma najwyżej 7 prób.

??? question "Zadanie 5: Liczby pierwsze"
    Wypisz wszystkie liczby pierwsze mniejsze od 200. Użyj pętli zagnieżdżonych i `break`, żeby przerwać sprawdzanie dzielników, gdy jeden zostanie znaleziony. Wystarczy sprawdzać dzielniki do pierwiastka z liczby — dlaczego?

??? question "Zadanie 6: Wzory z gwiazdek"
    Program wczytuje wysokość *n* i rysuje: trójkąt prostokątny, trójkąt równoramienny (choinkę) i pustą w środku ramkę *n* × *n*.

??? question "Zadanie 7: Statystyki wpisów"
    Program wczytuje liczby, dopóki użytkownik nie wpisze pustej linii. Na końcu wypisuje: ile było liczb, ich sumę, średnią, największą i najmniejszą. Niepoprawne wpisy pomija z komunikatem.
