# Instrukcje warunkowe

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zapisać instrukcję warunkową `if` z gałęziami `else if` i `else`, budować złożone warunki z operatorów porównania i logicznych, porównywać teksty i znaki, stosować operator warunkowy `?:`, instrukcję wyboru `switch` oraz wyrażenie `switch` z wzorcami.

## Instrukcja if

```csharp
if (warunek)
{
    // wykona się, gdy warunek jest prawdziwy
}
else
{
    // wykona się, gdy warunek jest fałszywy
}
```

- warunek zawsze w **nawiasach okrągłych**,
- warunek musi być typu **`bool`** — `if (liczba)` to błąd kompilacji, trzeba napisać `if (liczba != 0)`,
- gałąź `else` jest opcjonalna.

### Wiele gałęzi: else if

```csharp
int punkty = 72;
string ocena;

if (punkty >= 90)
{
    ocena = "celujący";
}
else if (punkty >= 75)
{
    ocena = "bardzo dobry";
}
else if (punkty >= 60)
{
    ocena = "dobry";
}
else if (punkty >= 50)
{
    ocena = "dostateczny";
}
else
{
    ocena = "niedostateczny";
}

Console.WriteLine(ocena);
```

Warunki są sprawdzane **od góry**. Wykonuje się tylko pierwsza gałąź, której warunek jest prawdziwy — dlatego progi układamy od najwyższego. Gdyby `punkty >= 50` stało na początku, każdy wynik od 50 wzwyż dostałby ocenę dostateczną.

!!! warning "Klamry zawsze"
    C# pozwala pominąć klamry, gdy w bloku jest jedna instrukcja. Nie rób tego: po dopisaniu drugiej linii tylko pierwsza będzie należała do `if`, mimo że wcięcie sugeruje coś innego.

    ```csharp
    if (punkty < 50)
        Console.WriteLine("Niezaliczone");
        Console.WriteLine("Popraw w piątek");   // wykona się ZAWSZE
    ```

!!! warning "Średnik po warunku"
    `if (x > 0);` kończy instrukcję `if` na pustym średniku. Blok w klamrach wykona się wtedy zawsze. Kompilator zgłosi tylko ostrzeżenie `CS0642`.

### Zagnieżdżanie

```csharp
if (zalogowany)
{
    if (czyAdministrator)
    {
        Console.WriteLine("Panel administratora");
    }
    else
    {
        Console.WriteLine("Panel użytkownika");
    }
}
else
{
    Console.WriteLine("Zaloguj się");
}
```

Zbyt głębokie zagnieżdżenia są trudne do czytania. Często da się je spłaszczyć, sprawdzając najpierw przypadki błędne:

```csharp
if (!int.TryParse(tekst, out int wiek))
{
    Console.WriteLine("To nie jest liczba.");
    return;                        // kończy program (lub metodę)
}
if (wiek < 0 || wiek > 120)
{
    Console.WriteLine("Niemożliwy wiek.");
    return;
}
Console.WriteLine($"Wiek: {wiek}");
```

## Budowanie warunków

| Operator | Znaczenie | Przykład |
|---|---|---|
| `==` `!=` | równe, różne | `odpowiedz == 'T'` |
| `<` `>` `<=` `>=` | porównania | `wiek >= 18` |
| `&&` | i | `wiek >= 13 && wiek <= 19` |
| <code>&#124;&#124;</code> | lub | <code>dzien == 6 &#124;&#124; dzien == 7</code> |
| `!` | nie | `!czyPelnoletni` |

Przedział sprawdzamy dwoma porównaniami: `x >= 1 && x <= 10`. Zapis `1 <= x <= 10` w C# nie działa.

**Obliczanie skrócone.** `&&` nie sprawdza drugiego warunku, jeśli pierwszy jest fałszywy; <code>&#124;&#124;</code> — jeśli pierwszy jest prawdziwy. To pozwala bezpiecznie pisać:

```csharp
int liczba = 10, dzielnik = 0;
if (dzielnik != 0 && liczba / dzielnik > 2)     // przy dzielnik == 0 dzielenie się nie wykona
{
    Console.WriteLine("Iloraz większy niż 2");
}

string? tekst = Console.ReadLine();
if (tekst != null && tekst.Length > 0)          // przy null nie odczytamy Length
{
    Console.WriteLine($"Wpisano {tekst.Length} znaków");
}
```

**Warunek w zmiennej.** Złożony warunek warto nazwać:

```csharp
bool przestepny = (rok % 4 == 0 && rok % 100 != 0) || rok % 400 == 0;
if (przestepny)
{
    Console.WriteLine($"{rok} jest przestępny");
}
```

## Porównywanie tekstów i znaków

```csharp
string haslo = "Tajne123";
Console.WriteLine(haslo == "tajne123");                                   // wielkość liter ma znaczenie
Console.WriteLine(haslo.Equals("tajne123", StringComparison.OrdinalIgnoreCase));  // bez rozróżniania

string odp = (Console.ReadLine() ?? "").Trim().ToLower();
if (odp == "tak" || odp == "t")
{
    Console.WriteLine("Potwierdzono.");
}
```

Klasa `char` ma metody do sprawdzania znaków:

```csharp
char z = 'a';
Console.WriteLine(char.IsLetter(z));      // litera?
Console.WriteLine(char.IsDigit(z));       // cyfra?
Console.WriteLine(char.IsUpper(z));       // wielka litera?
Console.WriteLine(char.IsWhiteSpace(z));  // spacja, tabulator, nowa linia?
Console.WriteLine(char.ToUpper(z));       // A
```

## Operator warunkowy ?:

Gdy na podstawie warunku wybierasz jedną z dwóch **wartości**:

```csharp
string wynik = punkty >= 50 ? "zaliczone" : "niezaliczone";
int wieksza = a > b ? a : b;
Console.WriteLine($"Masz {n} {(n == 1 ? "punkt" : "punktów")}");
```

Nie zagnieżdżaj go wielokrotnie — do wielu przypadków służy `switch`.

## Instrukcja switch

Wybór jednej z wielu ścieżek na podstawie **wartości**:

```csharp
Console.Write("Numer dnia (1–7): ");
int.TryParse(Console.ReadLine(), out int dzien);

switch (dzien)
{
    case 1:
        Console.WriteLine("poniedziałek");
        break;
    case 2:
        Console.WriteLine("wtorek");
        break;
    case 6:
    case 7:
        Console.WriteLine("weekend");
        break;
    default:
        Console.WriteLine("inny dzień lub błędny numer");
        break;
}
```

- każdy `case` kończy się `break` (albo `return`) — C# **nie pozwala** „przelecieć” do następnego przypadku,
- kilka wartości z tą samą akcją zapisujesz jedna pod drugą,
- `default` obsługuje wszystkie pozostałe wartości,
- `switch` działa dla liczb całkowitych, `char`, `string`, `bool`, typów wyliczeniowych (`enum`).

### Wzorce w case

```csharp
switch (temperatura)
{
    case < 0:
        Console.WriteLine("mróz");
        break;
    case >= 0 and < 15:
        Console.WriteLine("chłodno");
        break;
    case >= 15 and < 25:
        Console.WriteLine("przyjemnie");
        break;
    default:
        Console.WriteLine("upał");
        break;
}
```

Warunek dodatkowy dopisujemy słowem `when`:

```csharp
switch (polecenie)
{
    case "start" when zalogowany:
        Uruchom();
        break;
    case "start":
        Console.WriteLine("Najpierw się zaloguj.");
        break;
}
```

## Wyrażenie switch

Gdy chcesz na podstawie wartości **obliczyć wynik**, krótsze jest wyrażenie `switch`:

```csharp
string ocena = punkty switch
{
    >= 90 => "celujący",
    >= 75 => "bardzo dobry",
    >= 60 => "dobry",
    >= 50 => "dostateczny",
    _ => "niedostateczny"
};
```

- przypadki oddziela przecinek, a wynik wskazuje strzałka `=>`,
- `_` oznacza „wszystko inne”,
- całe wyrażenie kończy się średnikiem, bo jest częścią przypisania.

Wzorce można łączyć słowami `and`, `or`, `not`:

```csharp
string pora = miesiac switch
{
    12 or 1 or 2 => "zima",
    >= 3 and <= 5 => "wiosna",
    >= 6 and <= 8 => "lato",
    >= 9 and <= 11 => "jesień",
    _ => "błędny miesiąc"
};

double cena = (wiek, czyStudent) switch
{
    ( < 7, _) => 0,
    (_, true) => 12,
    ( >= 65, _) => 10,
    _ => 20
};
```

W drugim przykładzie sprawdzamy jednocześnie dwie wartości zapisane jako para (krotka).

## Którą konstrukcję wybrać

| Sytuacja | Konstrukcja |
|---|---|
| jeden warunek, dwie ścieżki | `if` / `else` |
| progi, przedziały, złożone warunki | `if` / `else if` |
| wybór jednej z dwóch wartości | `?:` |
| wiele konkretnych wartości, różne działania | instrukcja `switch` |
| wiele wartości lub przedziałów, obliczenie wyniku | wyrażenie `switch` |

## Zadania

??? question "Zadanie 1: Parzysta, dodatnia, podzielna"
    Program wczytuje liczbę całkowitą (z `TryParse`) i wypisuje: czy jest parzysta, czy dodatnia, ujemna czy zero, i czy jest podzielna przez 3 i 5 jednocześnie. Dla błędnego wejścia wypisuje komunikat i kończy działanie.

??? question "Zadanie 2: Rok przestępny"
    Program sprawdza rok podany przez użytkownika. Warunek zapisz w jednej zmiennej `bool`. Przetestuj na latach 2024, 1900, 2000, 2026.

??? question "Zadanie 3: Trójkąt"
    Program wczytuje trzy długości boków i sprawdza: czy da się zbudować trójkąt (każdy bok mniejszy od sumy dwóch pozostałych), a jeśli tak — czy jest równoboczny, równoramienny czy różnoboczny, oraz czy jest prostokątny.

??? question "Zadanie 4: Kalkulator"
    Program wczytuje dwie liczby i znak działania (`+`, `-`, `*`, `/`, `%`, `^`) i wypisuje wynik. Użyj instrukcji `switch`. Obsłuż dzielenie przez zero i nieznany znak.

??? question "Zadanie 5: Bilet"
    Cena biletu: dzieci do 6 lat — za darmo, uczniowie i studenci do 26 lat — 12 zł, seniorzy od 65 lat — 10 zł, pozostali — 20 zł; w poniedziałki wszystkie bilety płatne są o 20% tańsze. Napisz program raz z `if / else if`, raz z wyrażeniem `switch`.

??? question "Zadanie 6: Znak"
    Program wczytuje jeden znak i określa, czy to wielka litera, mała litera, cyfra, samogłoska (polska), spacja czy inny znak.

??? question "Zadanie 7: Znajdź błędy"
    Znajdź wszystkie błędy w kodzie, a potem popraw go tak, żeby się skompilował i działał zgodnie z intencją autora (ocena za punkty: 0–49 ndst, 50–74 dst, 75–100 bdb).

    ```csharp
    int p = int.Parse(Console.ReadLine())
    if (p >= 50);
    {
        Console.WriteLine("dst")
    }
    else if (p >= 75)
        Console.WriteLine("bdb");
    else if (0 <= p < 50)
        Console.WriteLine("ndst");
    ```
