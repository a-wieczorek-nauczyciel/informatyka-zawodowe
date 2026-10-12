# Typy, operatory, wejście i wyjście

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć dobrać typ danych do wartości, rozróżnić typy całkowite i zmiennoprzecinkowe, stosować operatory zgodnie z ich priorytetem, zamieniać typy między sobą oraz bezpiecznie odczytać liczbę wpisaną przez użytkownika.

## Typy proste

| Typ | Do czego | Przykład | Zakres / uwagi |
|---|---|---|---|
| `int` | liczba całkowita | `int punkty = 10;` | ok. ±2,1 miliarda |
| `long` | duża liczba całkowita | `long ludnosc = 8_000_000_000;` | ok. ±9,2 · 10¹⁸ |
| `double` | liczba z ułamkiem | `double masa = 72.5;` | ok. 15–16 cyfr znaczących, wyniki przybliżone |
| `decimal` | kwoty pieniężne | `decimal cena = 19.99m;` | dokładne ułamki dziesiętne, przyrostek `m` |
| `bool` | tak / nie | `bool wygrana = false;` | tylko `true` lub `false` |
| `char` | pojedynczy znak | `char odpowiedz = 'A';` | apostrofy `' '` |
| `string` | tekst | `string imie = "Ada";` | cudzysłowy `" "` |

Typ mówi, jakie dane przechowujemy i jakie operacje mają sens: liczby mnożymy, tekst sklejamy, a odpowiedź „tak/nie” przechowujemy jako `bool`.

!!! warning "Python ma jeden typ całkowity, C# kilka"
    W Pythonie `int` nie ma górnej granicy. W C# `int` ma 32 bity. Po przekroczeniu `int.MaxValue` wartość domyślnie „przekręca się” na liczbę ujemną — bez żadnego komunikatu.

### Stałe i liczby w kodzie

```csharp
const double Vat = 0.23;      // stała: nie można jej zmienić
int milion = 1_000_000;       // podkreślenia dla czytelności
double d = 1e-3;              // 0.001
```

## Operatory

| Grupa | Operatory | Uwagi |
|---|---|---|
| arytmetyczne | `+ - * / %` | `/` dla dwóch liczb całkowitych daje wynik całkowity |
| inkrementacja | `++ --` | `x++` zwiększa o 1 |
| przypisania | `= += -= *= /= %=` | `x += 5` to `x = x + 5` |
| porównania | `== != < > <= >=` | wynik to `bool` |
| logiczne | <code>&& &#124;&#124; !</code> | i, lub, nie (w Pythonie: `and`, `or`, `not`) |
| łańcuchowe | `+` | sklejanie tekstu |
| warunkowy | `warunek ? a : b` | wartość `a` albo `b` |
| bitowe | <code>& &#124; ^ ~ &lt;&lt; >></code> | operacje na pojedynczych bitach |

### Dzielenie całkowite

O wyniku dzielenia decydują **typy** liczb:

```csharp
Console.WriteLine(7 / 2);      // oba int → wynik int (część ułamkowa jest odcinana)
Console.WriteLine(7 / 2.0);    // jeden double → wynik double
Console.WriteLine(7 % 2);      // reszta z dzielenia
```

W C# dzielenie całkowite **odcina część ułamkową** (zaokrągla w stronę zera), a w Pythonie `//` zaokrągla **w dół**. Dla liczb dodatnich wynik jest ten sam, dla ujemnych — nie.

### Priorytet operatorów

Od najwyższego: `!` i `++` → `* / %` → `+ -` → `< > <= >=` → `== !=` → `&&` → `||` → `?:` → przypisania. W razie wątpliwości użyj nawiasów.

### Plus: tekst czy dodawanie?

??? question "Co zrobi ten kod?"
    ```csharp
    Console.WriteLine("72" + "2");
    Console.WriteLine(72 + 2);
    Console.WriteLine("72" + 2 + 3);
    Console.WriteLine(2 + 3 + "72");
    ```

    Co wypisze każda linia? Dlaczego trzecia i czwarta dają różne wyniki? Co by się stało w Pythonie przy `"72" + 2`?

## Konwersje typów

```csharp
int calk = 7;
double ulamek = calk;          // niejawna: int mieści się w double

double cena = 9.99;
int obcieta = (int)cena;       // jawna (rzutowanie): część ułamkowa odcięta

string tekst = calk.ToString();          // liczba → tekst
double zaokr = Math.Round(2.567, 2);     // 2.57
```

Konwersja, w której można stracić informację, musi być **jawna** — inaczej kompilator zgłosi błąd.

## Wejście z konsoli jest tekstem

`Console.ReadLine()` zawsze zwraca tekst (`string`). Żeby wykonać obliczenia, trzeba z tekstu odczytać liczbę. Ten krok nazywamy **parsowaniem**.

| Sposób | Zwraca | Gdy tekst nie jest liczbą |
|---|---|---|
| `double.Parse(tekst)` | liczbę `double` | wyjątek `FormatException`, program się przerywa |
| `double.TryParse(tekst, out x)` | `bool` (udało się?) + liczba przez `out` | zwraca `false`, program działa dalej |

Dane od użytkownika są **niezaufane**: mogą zawierać litery, spacje, przecinek zamiast kropki albo nic. Dlatego w programach z wejściem używamy `TryParse` i sprawdzamy jego wynik.

```csharp
using System.Globalization;

Console.Write("Podaj masę, używając kropki: ");
string tekst = Console.ReadLine() ?? "";

bool poprawne = double.TryParse(
    tekst,
    NumberStyles.Float,
    CultureInfo.InvariantCulture,
    out double masa
);

Console.WriteLine($"Czy poprawne: {poprawne}");
Console.WriteLine($"Odczytana wartość: {masa}");
```

| Element | Znaczenie |
|---|---|
| `TryParse` | próbuje odczytać liczbę |
| `poprawne` | czy próba się udała |
| `out double masa` | miejsce na odczytaną liczbę |
| `InvariantCulture` | kropka jako separator, niezależnie od ustawień komputera |
| `?? ""` | jeśli brak tekstu, użyj pustego |

=== "Python"

    ```python
    tekst = input("Podaj wiek: ")
    try:
        wiek = int(tekst)
        print(f"Za rok: {wiek + 1}")
    except ValueError:
        print("To nie jest liczba")
    ```

=== "C#"

    ```csharp
    Console.Write("Podaj wiek: ");
    string tekst = Console.ReadLine() ?? "";
    if (int.TryParse(tekst, out int wiek))
    {
        Console.WriteLine($"Za rok: {wiek + 1}");
    }
    else
    {
        Console.WriteLine("To nie jest liczba");
    }
    ```

## Formatowanie wyniku

```csharp
double bmi = 22.2222;
Console.WriteLine($"BMI: {bmi:F2}");        // dwa miejsca po przecinku
Console.WriteLine($"Cena: {19.5m:C}");      // format walutowy wg ustawień komputera
Console.WriteLine($"|{"ab",5}|{"cd",-5}|"); // wyrównanie do prawej i do lewej
```

Separator dziesiętny w wyniku (przecinek lub kropka) zależy od ustawień regionalnych komputera.

## Zadania

??? question "Zadanie 1: Przewiduj, potem testuj"
    Dla programu z `TryParse` (masa) zapisz w tabeli, co wypisze dla wpisów: `72.5`, `kot`, `72 kg`, pusty Enter, `-5`, `72,5`. Dopiero potem uruchom i porównaj. Co zawiera `masa`, gdy parsowanie się nie udało?

??? question "Zadanie 2: Kalkulator BMI"
    Napisz program, który pobiera masę w kilogramach i wzrost w centymetrach, sprawdza, czy oba wejścia są liczbami i czy są większe od zera, oblicza BMI = masa / (wzrost w metrach)² i wypisuje wynik z dwoma miejscami po przecinku. Używaj fikcyjnych danych. Testy: `80` i `200`; `72` i `180`; `kot` i `180`; `72` i `0`; `-10` i `180`; puste i `180`.

??? question "Zadanie 3: Czy to się skompiluje?"
    ```csharp
    // (a)
    double masa = 80;
    double wzrostM = 2.0;
    int bmi = masa / (wzrostM * wzrostM);

    // (b)
    int wzrostCm = 175;
    double wzrostM2 = wzrostCm / 100;
    Console.WriteLine(wzrostM2);
    ```

    Czy (a) się skompiluje? Co wypisze (b)? Jak naprawić (b)?

??? question "Zadanie 4: Sklep"
    Napisz program, który wczytuje cenę netto produktu (`decimal`) i liczbę sztuk (`int`), a następnie wypisuje wartość netto, VAT 23% i wartość brutto, każdą z dwoma miejscami po przecinku. Dlaczego do pieniędzy lepiej nadaje się `decimal` niż `double`?

??? question "Zadanie 5: Sekundy"
    Program wczytuje liczbę sekund i wypisuje ją w formacie `godziny:minuty:sekundy`, np. `3725` → `1:02:05`. Użyj dzielenia całkowitego i reszty.
