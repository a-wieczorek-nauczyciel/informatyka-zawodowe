# Budowa programu

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wskazać w programie C# przestrzenie nazw, dyrektywy `using`, klasę, metodę `Main`, instrukcje i bloki; zapisać program w wersji pełnej i z instrukcjami najwyższego poziomu; nadawać nazwy zgodnie z regułami i konwencjami C#; korzystać z klasy `Console` oraz czytać błędy i ostrzeżenia kompilatora.

## Program w wersji pełnej

Tak wygląda kompletny program C# zapisany „klasycznie”:

```csharp linenums="1"
using System;

namespace Powitanie
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Jak masz na imię? ");
            string imie = Console.ReadLine() ?? "";
            Console.WriteLine($"Cześć, {imie}!");
        }
    }
}
```

| Linie | Element | Znaczenie |
|---|---|---|
| 1 | `using System;` | **dyrektywa using**: pozwala pisać `Console` zamiast `System.Console` |
| 3 | `namespace Powitanie` | **przestrzeń nazw**: grupuje typy, żeby nazwy z różnych bibliotek się nie myliły |
| 5 | `internal class Program` | **klasa**: każda metoda w C# należy do jakiegoś typu |
| 7 | `static void Main(string[] args)` | **metoda Main**: punkt wejścia — od niej zaczyna się wykonanie programu |
| 9–11 | instrukcje | kolejne polecenia, każde zakończone średnikiem |
| 4, 6, 8 … | `{ }` | **bloki**: wyznaczają zawartość przestrzeni nazw, klasy i metody |

Elementy nagłówka `Main`:

- `static` — metoda należy do klasy, a nie do obiektu, więc można ją wywołać bez tworzenia obiektu `Program`,
- `void` — metoda nic nie zwraca (może też zwracać `int` — kod zakończenia programu),
- `string[] args` — argumenty podane przy uruchomieniu, np. `dotnet run -- plik.txt 5`.

## Ten sam program z instrukcjami najwyższego poziomu

Od C# 9 można pominąć klasę i `Main` — kompilator dopisze je sam. Szablon `dotnet new console` tworzy właśnie taką wersję:

```csharp
Console.Write("Jak masz na imię? ");
string imie = Console.ReadLine() ?? "";
Console.WriteLine($"Cześć, {imie}!");
```

Zasady:

- instrukcje najwyższego poziomu mogą być **tylko w jednym pliku** projektu,
- deklaracje klas umieszczone w tym samym pliku muszą być **po** instrukcjach,
- argumenty uruchomienia są dostępne w zmiennej `args`, tak jak w `Main`.

Obie wersje kompilują się do tego samego kodu. W tym dziale używamy krótszej, ale w dokumentacji i starszych materiałach zobaczysz pełną.

## Przestrzenie nazw i using

**Przestrzeń nazw** to „nazwisko” typu. Pełna nazwa klasy `Console` to `System.Console`, a klasy `List<T>` — `System.Collections.Generic.List<T>`.

Bez dyrektywy `using` podajemy pełną nazwę:

```csharp
System.Text.StringBuilder sb = new System.Text.StringBuilder();
sb.Append("Ala ");
sb.Append("ma kota");
System.Console.WriteLine(sb.ToString());
```

Z dyrektywą `using` (zawsze na samym początku pliku) wystarczy krótka nazwa:

```csharp
using System.Text;

StringBuilder sb = new StringBuilder();
sb.Append("Ala ");
sb.Append("ma kota");
Console.WriteLine(sb.ToString());
```

Dzięki ustawieniu `ImplicitUsings` w `.csproj` najczęstsze przestrzenie nazw są dołączone automatycznie (tzw. *global using*), dlatego w nowym projekcie nie widać żadnego `using`, a `Console` i `List<T>` działają od razu.

Własną przestrzeń nazw w nowszym zapisie deklaruje się jedną linią na początku pliku:

```csharp
namespace Szkola.Oceny;

class Dziennik { }
```

## Instrukcje, bloki, wielkość liter

- **Instrukcja** kończy się średnikiem `;`. Kilka instrukcji może stać w jednej linii, a jedna instrukcja może zajmować kilka linii.
- **Blok** `{ }` grupuje instrukcje; wyznacza ciało klasy, metody, `if`, pętli.
- **Wcięcia** nie mają znaczenia dla kompilatora — służą tylko czytelności. Standardowo: 4 spacje na poziom.
- C# **rozróżnia wielkość liter**: `Console`, `console` i `CONSOLE` to trzy różne nazwy; zadziała tylko pierwsza.

## Komentarze

```csharp
// komentarz jednoliniowy

/* komentarz
   wieloliniowy */

/// <summary>
/// Komentarz dokumentacyjny: opis metody pokazywany
/// w podpowiedziach edytora.
/// </summary>
static double Pole(double r) => Math.PI * r * r;
```

## Nazwy (identyfikatory)

**Reguły** — kompilator ich wymaga:

- nazwa zaczyna się od litery lub `_`, dalej litery, cyfry i `_`,
- nie może być słowem kluczowym (`int`, `class`, `if`, `return` …) — chyba że poprzedzisz je `@`, np. `@class`,
- wielkość liter ma znaczenie.

**Konwencje** — tak piszą programiści C#, kompilator tego nie sprawdza:

| Co nazywamy | Styl | Przykład |
|---|---|---|
| klasy, metody, właściwości, przestrzenie nazw | **PascalCase** | `KontoBankowe`, `ObliczSume`, `Saldo` |
| zmienne lokalne, parametry | **camelCase** | `liczbaUczniow`, `masaKg` |
| pola prywatne | `_camelCase` lub camelCase | `_saldo` |
| stałe | PascalCase | `const int MaksLiczbaProb = 3;` |
| interfejsy | `I` + PascalCase | `IPorownywalny` |

Nazwy powinny mówić, **co** przechowują: `liczbaUczniow`, a nie `x` czy `lu`. Polskie znaki są dozwolone, ale w kodzie zwykle się ich unika.

## Wejście i wyjście: klasa Console

| Metoda / właściwość | Działanie |
|---|---|
| `Console.WriteLine(x)` | wypisuje wartość i przechodzi do nowej linii |
| `Console.Write(x)` | wypisuje bez nowej linii |
| `Console.ReadLine()` | czyta linię tekstu (`string`) po naciśnięciu Enter |
| `Console.ReadKey()` | czeka na naciśnięcie jednego klawisza |
| `Console.Clear()` | czyści ekran |
| `Console.ForegroundColor = ConsoleColor.Green;` | zmienia kolor tekstu |
| `Console.ResetColor()` | przywraca kolory |

Wartości wstawiamy do tekstu przez **interpolację**:

```csharp
string imie = "Ada";
int wiek = 17;
Console.WriteLine($"{imie} ma {wiek} lat, za rok {wiek + 1}.");
Console.WriteLine($"Średnia: {4.666:F2}");     // 4,67 lub 4.67 — zależnie od ustawień systemu
```

`Console.ReadLine()` zawsze zwraca **tekst**. Zamianę tekstu na liczbę i sprawdzanie poprawności danych omawiamy w lekcji o typach.

!!! note "Skąd ?? \"\""
    `Console.ReadLine()` może zwrócić `null` (np. gdy wejście zostanie zamknięte). Przy włączonym `Nullable` kompilator ostrzega przed tym. Zapis `Console.ReadLine() ?? ""` znaczy: „jeśli nic nie przyszło, użyj pustego tekstu”.

## Błędy i ostrzeżenia

Kompilator zgłasza dwa rodzaje komunikatów:

| | Błąd (*error*) | Ostrzeżenie (*warning*) |
|---|---|---|
| program powstaje? | nie | tak |
| znaczenie | kod jest niepoprawny | kod jest poprawny, ale podejrzany |
| przykład | `CS1002: ; expected` — brak średnika | `CS0168` — zmienna zadeklarowana, ale nieużyta; `CS8600` — możliwy `null` |

Ostrzeżeń nie ignorujemy — bardzo często wskazują prawdziwy błąd, który ujawni się dopiero podczas działania.

Najczęstsze błędy na początku:

| Kod | Komunikat (skrót) | Przyczyna |
|---|---|---|
| `CS1002` | `; expected` | brak średnika |
| `CS1513` | `} expected` | brak zamykającej klamry |
| `CS0103` | `The name 'x' does not exist` | literówka albo zmienna użyta poza swoim blokiem |
| `CS0029` | `Cannot implicitly convert type` | niezgodne typy |
| `CS0165` | `Use of unassigned local variable` | odczyt zmiennej, której nie nadano wartości |
| `CS0117` | `'Console' does not contain a definition for 'Writeline'` | zła wielkość liter w nazwie metody |

## Sprawdź się

??? question "1. Rozpoznaj elementy"
    W programie w wersji pełnej wskaż: dyrektywę, przestrzeń nazw, klasę, punkt wejścia, typ zwracany metody `Main`, parametr metody `Main` i trzy instrukcje.

??? question "2. Dwie wersje"
    Napisz program wypisujący trzy linie o sobie w wersji pełnej (z `namespace`, klasą i `Main`), a potem w wersji z instrukcjami najwyższego poziomu. Sprawdź, że oba działają tak samo.

??? question "3. Argumenty uruchomienia"
    Napisz program, który wypisuje liczbę argumentów i każdy z nich w osobnej linii. Uruchom go poleceniem `dotnet run -- ala ma kota`.

??? question "4. Poprawne nazwy"
    Które nazwy są niepoprawne i dlaczego: `liczba1`, `1liczba`, `_wynik`, `class`, `@class`, `średnia`, `moja-zmienna`, `MojaZmienna`? Które poprawne nazwy łamią konwencje C# dla zmiennej lokalnej?

??? question "5. Popsuj celowo"
    Wprowadź do działającego programu po kolei: brak średnika, `console` zamiast `Console`, brak zamykającej klamry, `int x = "5";`. Zapisz numer i treść każdego błędu oraz wiersz, który wskazał kompilator.

??? question "6. Kolorowa wizytówka"
    Napisz program, który pyta o imię, nazwisko i ulubiony przedmiot, czyści ekran i wypisuje wizytówkę w ramce z gwiazdek, z imieniem i nazwiskiem w kolorze zielonym.
