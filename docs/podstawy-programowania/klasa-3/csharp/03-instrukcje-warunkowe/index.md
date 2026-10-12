# Instrukcje warunkowe

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zapisać w C# instrukcję warunkową `if` z gałęziami `else if` i `else`, łączyć warunki operatorami logicznymi, stosować instrukcję wyboru `switch` i wyrażenie `switch` oraz operator warunkowy `?:`.

## if / else if / else

=== "Python"

    ```python
    punkty = 72

    if punkty >= 90:
        print("celujący")
    elif punkty >= 75:
        print("bardzo dobry")
    elif punkty >= 50:
        print("dostateczny")
    else:
        print("niedostateczny")
    ```

=== "C#"

    ```csharp
    int punkty = 72;

    if (punkty >= 90)
    {
        Console.WriteLine("celujący");
    }
    else if (punkty >= 75)
    {
        Console.WriteLine("bardzo dobry");
    }
    else if (punkty >= 50)
    {
        Console.WriteLine("dostateczny");
    }
    else
    {
        Console.WriteLine("niedostateczny");
    }
    ```

Różnice:

- warunek zawsze w **nawiasach okrągłych**,
- `elif` to w C# `else if`,
- blok to **nawiasy klamrowe**, a nie wcięcie,
- warunek musi być typu `bool`. W Pythonie `if liczba:` działa dla każdej liczby różnej od zera, w C# `if (liczba)` to błąd kompilacji — trzeba napisać `if (liczba != 0)`.

!!! warning "Klamry przy jednej instrukcji"
    C# pozwala pominąć klamry, gdy w bloku jest jedna instrukcja. Nie rób tego: po dopisaniu drugiej linii tylko pierwsza będzie należała do `if`, mimo że wcięcie sugeruje coś innego.

    ```csharp
    if (punkty < 50)
        Console.WriteLine("Niezaliczone");
        Console.WriteLine("Popraw w piątek");   // wykona się ZAWSZE
    ```

## Łączenie warunków

| C# | Python | Znaczenie |
|---|---|---|
| `&&` | `and` | oba warunki prawdziwe |
| <code>&#124;&#124;</code> | `or` | co najmniej jeden prawdziwy |
| `!` | `not` | zaprzeczenie |

```csharp
int wiek = 17;
bool zgodaRodzica = true;

if (wiek >= 18 || zgodaRodzica)
{
    Console.WriteLine("Możesz wziąć udział");
}

if (wiek >= 13 && wiek <= 19)
{
    Console.WriteLine("Nastolatek");
}
```

W Pythonie można napisać `13 <= wiek <= 19`. W C# trzeba rozbić to na dwa porównania połączone `&&`.

`&&` i `||` sprawdzają drugi warunek tylko wtedy, gdy to potrzebne. Dzięki temu bezpieczny jest zapis:

```csharp
if (dzielnik != 0 && liczba / dzielnik > 2) { ... }
```

## Operator warunkowy ?:

```csharp
string wynik = punkty >= 50 ? "zaliczone" : "niezaliczone";
```

Odpowiednik w Pythonie: `wynik = "zaliczone" if punkty >= 50 else "niezaliczone"`.

## switch — wybór jednej z wielu wartości

```csharp
Console.Write("Numer dnia (1–7): ");
int dzien = int.Parse(Console.ReadLine() ?? "0");

switch (dzien)
{
    case 1:
        Console.WriteLine("poniedziałek");
        break;
    case 6:
    case 7:
        Console.WriteLine("weekend");
        break;
    default:
        Console.WriteLine("inny dzień");
        break;
}
```

- każdy `case` kończy się `break` — C# nie pozwala „przelecieć” do następnego przypadku,
- kilka wartości z tą samą akcją zapisujesz jedna pod drugą (`case 6:` i `case 7:`),
- `default` obsługuje wszystkie pozostałe wartości.

W Pythonie odpowiednikiem jest `match` / `case` (od wersji 3.10).

## Wyrażenie switch

Gdy chcesz na podstawie wartości **obliczyć wynik**, krótszy jest zapis z wyrażeniem `switch`:

```csharp
string ocena = punkty switch
{
    >= 90 => "celujący",
    >= 75 => "bardzo dobry",
    >= 50 => "dostateczny",
    _ => "niedostateczny"
};
```

Wzorce sprawdzane są **od góry**; `_` oznacza „wszystko inne”.

```csharp
string pora = miesiac switch
{
    12 or 1 or 2 => "zima",
    >= 3 and <= 5 => "wiosna",
    >= 6 and <= 8 => "lato",
    >= 9 and <= 11 => "jesień",
    _ => "błędny miesiąc"
};
```

## Zadania

??? question "Zadanie 1: Parzysta czy nieparzysta"
    Program wczytuje liczbę całkowitą (z `TryParse`) i wypisuje, czy jest parzysta, czy nieparzysta. Dla wpisu, który nie jest liczbą, wypisuje komunikat o błędzie.

??? question "Zadanie 2: Rok przestępny"
    Rok jest przestępny, jeśli dzieli się przez 4 i nie dzieli się przez 100, albo dzieli się przez 400. Napisz program sprawdzający rok podany przez użytkownika, z warunkiem zapisanym w jednym `if`. Przetestuj na latach 2024, 1900, 2000, 2026.

??? question "Zadanie 3: Kalkulator"
    Program wczytuje dwie liczby i znak działania (`+`, `-`, `*`, `/`) i wypisuje wynik. Użyj `switch`. Obsłuż dzielenie przez zero i nieznany znak działania.

??? question "Zadanie 4: Bilet"
    Cena biletu: dzieci do 6 lat — za darmo, uczniowie do 18 lat — 12 zł, seniorzy od 65 lat — 10 zł, pozostali — 20 zł. Napisz program raz z `if / else if`, raz z wyrażeniem `switch`. Który zapis jest czytelniejszy?

??? question "Zadanie 5: Znajdź błędy"
    Ten kod napisała osoba przyzwyczajona do Pythona. Znajdź wszystkie błędy, a potem popraw kod tak, żeby się skompilował.

    ```csharp
    int x = 5
    if x > 3 and x < 10:
        Console.WriteLine("w przedziale")
    elif (x == 10)
        Console.WriteLine("dziesięć");
    ```
