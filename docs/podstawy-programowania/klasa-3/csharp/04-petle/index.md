# Pętle

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć dobrać rodzaj pętli do zadania, zapisać w C# pętle `for`, `while`, `do-while` i `foreach`, przerywać i pomijać obroty pętli oraz wyjaśnić, gdzie „żyje” zmienna zadeklarowana w pętli.

## for — gdy wiesz, ile razy

=== "Python"

    ```python
    for i in range(1, 6):
        print(i)
    ```

=== "C#"

    ```csharp
    for (int i = 1; i <= 5; i++)
    {
        Console.WriteLine(i);
    }
    ```

Nagłówek `for` ma trzy części oddzielone średnikami:

| Część | Przykład | Kiedy się wykonuje |
|---|---|---|
| inicjalizacja | `int i = 1` | raz, przed pętlą |
| warunek | `i <= 5` | przed każdym obrotem; fałsz kończy pętlę |
| krok | `i++` | po każdym obrocie |

```csharp
for (int i = 10; i > 0; i -= 2) { ... }   // 10, 8, 6, 4, 2
```

## while — dopóki warunek jest prawdziwy

```csharp
int liczba = 1;
while (liczba < 1000)
{
    liczba *= 2;
}
Console.WriteLine(liczba);   // pierwsza potęga dwójki ≥ 1000
```

## do-while — przynajmniej raz

Warunek sprawdzany jest **na końcu**, więc ciało wykona się co najmniej raz. W Pythonie takiej pętli nie ma.

Typowe zastosowanie: pytanie użytkownika, dopóki nie poda poprawnych danych.

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

## foreach — dla każdego elementu

=== "Python"

    ```python
    imiona = ["Ada", "Olek", "Ewa"]
    for imie in imiona:
        print(imie)
    ```

=== "C#"

    ```csharp
    string[] imiona = { "Ada", "Olek", "Ewa" };
    foreach (string imie in imiona)
    {
        Console.WriteLine(imie);
    }
    ```

`foreach` przechodzi przez tablicę, listę albo tekst (znak po znaku). Elementu w `foreach` nie można podmienić — do zmiany wartości w tablicy użyj `for` z indeksem.

## break i continue

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i % 3 == 0) continue;   // pomiń ten obrót
    if (i > 8) break;           // zakończ pętlę
    Console.WriteLine(i);
}
```

Działają tak samo jak w Pythonie.

## Zasięg zmiennych

Zmienna zadeklarowana **w bloku** `{ }` istnieje tylko w tym bloku. Zmienna zadeklarowana w nagłówku `for` istnieje tylko w pętli.

```csharp
int suma = 0;                      // zadeklarowana przed pętlą — widoczna po pętli
for (int i = 1; i <= 5; i++)
{
    int kwadrat = i * i;           // istnieje tylko w jednym obrocie pętli
    suma += kwadrat;
}
Console.WriteLine(suma);           // OK
// Console.WriteLine(kwadrat);     // błąd: kwadrat nie istnieje poza pętlą
// Console.WriteLine(i);           // błąd: i nie istnieje poza pętlą
```

W Pythonie zmienna utworzona w pętli istnieje dalej po jej zakończeniu. W C# — nie. Jeśli potrzebujesz wartości po pętli, zadeklaruj zmienną **przed** pętlą.

## Którą pętlę wybrać

| Sytuacja | Pętla |
|---|---|
| znana liczba powtórzeń, potrzebny licznik | `for` |
| przejście po wszystkich elementach kolekcji | `foreach` |
| powtarzaj, dopóki coś jest prawdą — może ani razu | `while` |
| wykonaj co najmniej raz (np. pobranie danych) | `do-while` |

## Zadania

??? question "Zadanie 1: Tabliczka mnożenia"
    Wypisz tabliczkę mnożenia od 1 do 10 w postaci tabeli 10 × 10. Użyj dwóch zagnieżdżonych pętli `for` i formatowania `{x,4}`, żeby kolumny były równe.

??? question "Zadanie 2: Pytaj ponownie"
    Zmień kalkulator BMI z poprzedniej lekcji tak, żeby po błędnym wejściu pytał ponownie, a nie kończył działania. Która czynność ma się powtarzać: pobranie danych czy cały program? Przetestuj na wpisach `kot`, `-5`, `0`, `72.5`.

??? question "Zadanie 3: Zgadywanka"
    Program losuje liczbę od 1 do 100 (`Random.Shared.Next(1, 101)`), a użytkownik zgaduje. Po każdej próbie program podpowiada „za mało” / „za dużo”. Na końcu wypisuje liczbę prób.

??? question "Zadanie 4: Suma cyfr"
    Program wczytuje liczbę całkowitą dodatnią i liczy sumę jej cyfr pętlą `while` (bez zamiany na tekst). Następnie zrób to samo pętlą `foreach` po znakach tekstu. Porównaj oba rozwiązania.

??? question "Zadanie 5: Znajdź błąd"
    Program ma wypisać ostatni kwadrat liczby z pętli, ale się nie kompiluje. Popraw go, zmieniając jak najmniej.

    ```csharp
    for (int i = 0; i < 3; i++)
    {
        int kwadrat = i * i;
    }
    Console.WriteLine(kwadrat);
    ```
