# Tablice i listy

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć utworzyć tablicę i listę w C#, odczytać i zmienić ich elementy, przejść po nich pętlą, dodawać i usuwać elementy listy oraz wybrać właściwą strukturę do zadania.

## Lista w Pythonie, tablica i lista w C#

W Pythonie `list` może rosnąć i przechowywać elementy dowolnych typów. W C# są dwie podstawowe struktury:

| | Tablica `int[]` | Lista `List<int>` |
|---|---|---|
| rozmiar | **stały**, ustalony przy tworzeniu | zmienia się: można dodawać i usuwać |
| typ elementów | jeden, wskazany w deklaracji | jeden, wskazany w `< >` |
| liczba elementów | `.Length` | `.Count` |
| kiedy | znana z góry liczba danych | liczba danych zmienia się w trakcie działania |

## Tablice

```csharp
int[] oceny = new int[5];            // 5 elementów, każdy równy 0
int[] punkty = { 12, 7, 19, 3 };     // od razu z wartościami
string[] dni = { "pon", "wt", "śr" };

punkty[0] = 15;                      // indeksy od zera
Console.WriteLine(punkty[2]);        // 19
Console.WriteLine(punkty.Length);    // 4
Console.WriteLine(punkty[^1]);       // ostatni element: 3
```

!!! warning "Indeks poza tablicą"
    `punkty[4]` dla tablicy czteroelementowej powoduje wyjątek `IndexOutOfRangeException` i program się przerywa. Indeks od końca zapisujesz jako `^1`, a nie `-1` jak w Pythonie.

### Przechodzenie po tablicy

=== "Python"

    ```python
    punkty = [12, 7, 19, 3]
    suma = 0
    for p in punkty:
        suma += p
    print(suma / len(punkty))
    ```

=== "C#"

    ```csharp
    int[] punkty = { 12, 7, 19, 3 };
    int suma = 0;
    foreach (int p in punkty)
    {
        suma += p;
    }
    Console.WriteLine((double)suma / punkty.Length);
    ```

Rzutowanie `(double)` jest potrzebne, bo `suma / punkty.Length` to dzielenie dwóch liczb całkowitych.

Gdy potrzebujesz indeksu albo chcesz **zmieniać** elementy — użyj `for`:

```csharp
for (int i = 0; i < punkty.Length; i++)
{
    punkty[i] = punkty[i] * 2;
}
```

### Przydatne operacje

```csharp
Array.Sort(punkty);                         // sortowanie rosnąco
Array.Reverse(punkty);                      // odwrócenie kolejności
int poz = Array.IndexOf(punkty, 19);        // pozycja elementu (-1, gdy brak)
Console.WriteLine(string.Join(", ", punkty));   // wypisanie całej tablicy
```

## Listy

```csharp
List<string> zakupy = new List<string>();   // pusta lista tekstów
zakupy.Add("mleko");
zakupy.Add("chleb");
zakupy.Add("jajka");

Console.WriteLine(zakupy.Count);            // 3
Console.WriteLine(zakupy[1]);               // chleb

zakupy.Remove("mleko");                     // usuń element o tej wartości
zakupy.RemoveAt(0);                         // usuń element na pozycji 0
zakupy.Insert(0, "masło");                  // wstaw na pozycję 0
bool jest = zakupy.Contains("jajka");       // czy element jest na liście
zakupy.Sort();
zakupy.Clear();                             // usuń wszystko
```

| Python | C# (`List<T>`) |
|---|---|
| `lista = []` | `List<int> lista = new List<int>();` |
| `lista.append(x)` | `lista.Add(x)` |
| `lista.insert(i, x)` | `lista.Insert(i, x)` |
| `lista.remove(x)` | `lista.Remove(x)` |
| `del lista[i]` | `lista.RemoveAt(i)` |
| `x in lista` | `lista.Contains(x)` |
| `len(lista)` | `lista.Count` |
| `lista.sort()` | `lista.Sort()` |

!!! note "Krótszy zapis"
    W nowych wersjach C# listę możesz utworzyć krócej: `List<int> liczby = new();` albo od razu z wartościami: `List<int> liczby = [3, 1, 2];`.

## Typowe algorytmy na kolekcjach

```csharp
int[] t = { 4, 9, 2, 7 };

int max = t[0];
int min = t[0];
int suma = 0;
foreach (int x in t)
{
    if (x > max) max = x;
    if (x < min) min = x;
    suma += x;
}
Console.WriteLine($"max {max}, min {min}, suma {suma}");
```

## Zadania

??? question "Zadanie 1: Oceny"
    Program wczytuje 5 ocen (1–6) do tablicy, odrzucając niepoprawne wpisy. Następnie wypisuje wszystkie oceny, średnią z dwoma miejscami po przecinku, najwyższą i najniższą ocenę. Najwyższą i najniższą znajdź pętlą, bez gotowych metod.

??? question "Zadanie 2: Lista zakupów"
    Program w pętli wyświetla menu: `1 — dodaj`, `2 — usuń`, `3 — pokaż`, `0 — koniec`, i wykonuje operację na liście tekstów. Przy usuwaniu sprawdź, czy element jest na liście.

??? question "Zadanie 3: Bez duplikatów"
    Program wczytuje słowa, dopóki użytkownik nie wpisze pustej linii. Do listy trafia tylko słowo, którego jeszcze na niej nie ma. Na końcu wypisz listę posortowaną alfabetycznie.

??? question "Zadanie 4: Odwracanie"
    Odwróć kolejność elementów tablicy **bez** użycia `Array.Reverse` i bez tworzenia drugiej tablicy. Wskazówka: zamieniaj elementy z początku i końca.

??? question "Zadanie 5: Tablica czy lista"
    Dla każdej sytuacji wybierz tablicę albo listę i uzasadnij: temperatury z 7 dni tygodnia, uczestnicy zapisujący się na wycieczkę, 12 miesięcy roku, historia ruchów w grze.
