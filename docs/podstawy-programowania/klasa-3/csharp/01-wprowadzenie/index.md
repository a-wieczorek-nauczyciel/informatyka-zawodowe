# Wprowadzenie: od Pythona do C#

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, czym są C#, .NET, CIL i JIT, odróżnić typowanie statyczne od sposobu wykonania programu, utworzyć i uruchomić projekt konsolowy oraz wskazać miejsca, w których przyzwyczajenia z Pythona mogą zawieść.

## Rozgrzewka: co zrobi ten kod?

Nie uruchamiaj kodu. Zapisz w zeszycie przewidywany wynik każdej linii albo słowo „błąd”. Przy każdej linii zastanów się: a co zrobiłby Python?

??? question "Fragment 1: dzielenie i reszta"
    ```csharp
    Console.WriteLine(7 / 2 * 2.0);
    Console.WriteLine(-7 / 2);
    Console.WriteLine(-7 % 3);
    ```

    Co wypisze każda linia? Co dałby Python dla `7 // 2`, `-7 // 2` i `-7 % 3`? Co decyduje o typie wyniku w pierwszej linii?

??? question "Fragment 2: char, int i string"
    ```csharp
    char c = 'A';
    c++;
    int n = 'A' + 1;
    string s = "A" + 1;

    Console.WriteLine(c);
    Console.WriteLine(n);
    Console.WriteLine(s);
    ```

    Czy któraś z czterech pierwszych linii zgłosi błąd? Co wypiszą trzy ostatnie? Czym w C# jest `'A'`, a czym `"A"`?

??? question "Fragment 3: tekst, ułamki i granice liczb"
    ```csharp
    string a = "kot";
    a.ToUpper();
    Console.WriteLine(a);

    double x = 0.1 + 0.2;
    Console.WriteLine(x == 0.3);

    int m = int.MaxValue;
    m++;
    Console.WriteLine(m);
    ```

    Co wypiszą trzy wywołania `WriteLine`? Czy któraś linia zgłosi błąd? Dlaczego `a` się nie zmieniło?

??? question "Fragment 4: gdzie żyje zmienna?"
    ```csharp
    for (int i = 0; i < 3; i++)
    {
        int kwadrat = i * i;
    }

    Console.WriteLine(kwadrat);
    Console.WriteLine(i);
    ```

    Czy ten kod się skompiluje? W Pythonie podobny kod by zadziałał — dlaczego tu jest inaczej? Jak go naprawić, żeby wypisać ostatni kwadrat?

## Od kodu do uruchomionego programu

![Droga programu C#: kod, kompilator, CIL, JIT, kod maszynowy, procesor](../../../../assets/img/pp/csharp-cil-jit.svg)

Nasz zapis nie trafia bezpośrednio do procesora. **Kompilator C#** sprawdza składnię i typy, a potem tworzy **kod pośredni (CIL)**. Podczas uruchamiania **JIT** zamienia CIL na instrukcje konkretnego procesora.

| Pojęcie | Co to jest |
|---|---|
| **C#** | język, w którym zapisujemy program |
| **.NET** | platforma: środowisko uruchomieniowe + biblioteki (np. `Console`, `Math`) |
| **CIL** | kod pośredni, niezależny od konkretnego procesora |
| **metadane** | opis typów i metod programu zapisany obok CIL |
| **JIT** | kompilator działający podczas uruchamiania: zamienia CIL na kod maszynowy |
| **środowisko uruchomieniowe** | uruchamia program i zapewnia usługi, np. zarządzanie pamięcią (*garbage collector*) |

To podstawowy model wykonania. Istnieją też inne sposoby wdrażania .NET, np. kompilacja z góry (*Native AOT*), ale na razie zostajemy przy tym.

### A jak jest w Pythonie?

| Typowy CPython | Typowy .NET |
|---|---|
| kod `.py` → bytecode | kod `.cs` → CIL |
| bytecode wykonuje interpreter (maszyna wirtualna) | JIT zamienia CIL na kod maszynowy podczas działania |

Dlatego nie mówimy, że „Python nie jest kompilowany”. Różnica jest w tym, **co powstaje i jak jest wykonywane**, a nie w tym, że u jednego jest kompilacja, a u drugiego jej nie ma.

## Typowanie i wykonanie to dwie różne sprawy

- **Typowanie statyczne** mówi, **kiedy** sprawdzamy zgodność typów: kompilator zna typy zmiennych i odrzuca niedozwolone operacje **przed uruchomieniem**.
- **Kompilacja i wykonanie** mówią, **jak** przekształcamy kod do postaci, którą można wykonać: C# → CIL → JIT → kod maszynowy.

```csharp
int punkty = "dużo";   // błąd wykryty przez kompilator, zanim program ruszy
```

W Pythonie zmienna może w trakcie działania programu przechowywać raz liczbę, raz tekst. W C# zmienna ma **jeden typ na zawsze**, ustalony przy deklaracji.

```csharp
var licznik = 10;      // kompilator sam ustala typ: int
licznik = "dziesięć";  // błąd: licznik jest int
```

`var` nie wyłącza sprawdzania typów. Kompilator sam ustala typ i pilnuje go tak samo jak przy `int`.

## Pierwszy program

=== "Python"

    ```python
    imie = input("Jak masz na imię? ")
    print(f"Cześć, {imie}!")
    ```

=== "C#"

    ```csharp
    Console.Write("Jak masz na imię? ");
    string imie = Console.ReadLine() ?? "";
    Console.WriteLine($"Cześć, {imie}!");
    ```

Zapis `?? ""` znaczy: „jeśli nie ma żadnego tekstu (wartość `null`), użyj pustego napisu”. Bez niego kompilator wyświetli ostrzeżenie, bo `Console.ReadLine()` może nic nie zwrócić.

Najważniejsze różnice w zapisie:

| | Python | C# |
|---|---|---|
| koniec instrukcji | koniec linii | średnik `;` |
| bloki kodu | wcięcia | nawiasy klamrowe `{ }` (wcięcia tylko dla czytelności) |
| komentarz | `# ...` | `// ...` oraz `/* ... */` |
| wypisanie | `print()` | `Console.WriteLine()` / `Console.Write()` (bez nowej linii) |
| wczytanie | `input()` | `Console.ReadLine()` |
| tekst z wartościami | `f"...{x}..."` | `$"...{x}..."` |
| wielkość liter | ważna | ważna: `Console` to nie `console` |

## Tworzenie i uruchamianie projektu

W GitHub Codespaces lub w terminalu z zainstalowanym .NET SDK:

```bash
dotnet new console -n Powitanie    # nowy projekt konsolowy w folderze Powitanie
cd Powitanie
dotnet run                         # kompilacja i uruchomienie
```

Powstaje projekt z plikami:

```text
Powitanie/
├── Powitanie.csproj   ustawienia projektu (wersja .NET, opcje kompilatora)
└── Program.cs         kod programu
```

Plik `Program.cs` zawiera od razu działający program:

```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

Kod zapisany bezpośrednio w `Program.cs`, bez klasy i metody `Main`, to **instrukcje najwyższego poziomu** (*top-level statements*). W starszych materiałach zobaczysz dłuższą wersję tego samego programu:

```csharp
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

Oba zapisy działają tak samo — kompilator sam dopisuje klasę i metodę `Main`.

!!! tip "Czytaj błędy kompilatora"
    Błąd kompilacji ma numer (np. `CS0029`), plik, wiersz i opis. Zanim zapytasz, co jest nie tak, przeczytaj go: zwykle wskazuje dokładnie miejsce i przyczynę.

## Sprawdź się

??? question "1. C# i .NET"
    Czy C# i .NET to to samo? Wyjaśnij własnymi słowami.

??? question "2. CIL"
    Czy CIL to już kod maszynowy? Co robi JIT?

??? question "3. Kiedy wykryjemy błąd"
    Na którym etapie zostanie wykryte przypisanie `"kot"` do zmiennej typu `int` — w Pythonie i w C#?

??? question "4. Typowanie a wykonanie"
    Czy typowanie i sposób wykonania programu to ta sama kwestia? Uzasadnij.

??? question "5. Pierwszy projekt"
    Utwórz projekt `Wizytowka`, który pyta o imię, nazwisko i klasę, a potem wypisuje je w trzech liniach z opisami. Uruchom go poleceniem `dotnet run`.
