# Hermetyzacja

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć ukryć dane obiektu modyfikatorami dostępu, udostępnić je przez właściwości (automatyczne, z walidacją, tylko do odczytu, obliczane), zdecydować, co zrobić z niepoprawnymi danymi, chronić kolekcje wewnątrz klasy, stosować składowe statyczne oraz zaprojektować klasę, która nie może wejść w niepoprawny stan.

## Problem: każdy może wszystko

```csharp
// Program.cs
Konto k = new Konto();
k.Saldo = -1000;   // kto to sprawdzi?

// Konto.cs
class Konto
{
    public decimal Saldo;
}
```

Gdy pole jest publiczne, każdy fragment programu może wpisać do niego dowolną wartość. Reguła „saldo nie może być ujemne” musiałaby być sprawdzana w każdym miejscu, które zmienia saldo — a wystarczy jedno przeoczenie.

**Hermetyzacja** (enkapsulacja) odwraca tę sytuację: **obiekt sam pilnuje swojego stanu**. Z zewnątrz nie widać, jak jest zbudowany w środku, a każda zmiana danych przechodzi przez kod, który sprawdza jej poprawność.

1. **Ukryj dane** — pola oznacz jako `private`.
2. **Udostępnij interfejs** — publiczne metody i właściwości to jedyna droga do danych.
3. **Pilnuj reguł** — obiekt nie może wejść w niepoprawny stan.

## Modyfikatory dostępu

| Modyfikator | Kto ma dostęp | Uwagi |
|---|---|---|
| `public` | każdy kod | interfejs klasy — to, co świadomie udostępniamy |
| `private` | tylko kod wewnątrz tej klasy | **domyślny** dla składowych klasy |
| `protected` | klasa i jej klasy pochodne | przy dziedziczeniu |
| `internal` | kod w tym samym projekcie (zestawie) | **domyślny** dla samej klasy |

**Zasada:** zaczynaj od `private` i otwieraj tylko to, co musi być dostępne z zewnątrz. Odwrotnie jest trudniej — ujawnioną składową ktoś zdąży użyć.

```csharp
// Program.cs
Konto k = new Konto();
k.Wplac(100);
// k.saldo = -1000;   // błąd CS0122: 'saldo' jest niedostępne

// Konto.cs
class Konto
{
    private decimal saldo;              // ukryte

    public decimal PobierzSaldo()       // tylko odczyt
    {
        return saldo;
    }

    public bool Wplac(decimal kwota)    // zmiana z walidacją
    {
        if (kwota <= 0) return false;
        saldo += kwota;
        return true;
    }
}
```

## Właściwości

Metody `PobierzSaldo` / `UstawSaldo` działają, ale są niewygodne. W C# do kontrolowanego dostępu służą **właściwości** (*properties*): z zewnątrz wyglądają jak pola, a w środku mają akcesory `get` i `set`.

### Właściwość z polem i walidacją

```csharp
// Program.cs
Produkt p = new Produkt();
p.Cena = 19.99m;      // wywołuje set z value = 19.99
p.Cena = -5;          // set odrzuca wartość
Console.WriteLine(p.Cena);   // wywołuje get

// Produkt.cs
class Produkt
{
    private decimal cena;

    public decimal Cena
    {
        get { return cena; }
        set
        {
            if (value > 0)          // value — wartość, którą ktoś przypisuje
            {
                cena = value;
            }
        }
    }
}
```

### Właściwość automatyczna

Gdy nie trzeba walidacji, kompilator sam tworzy ukryte pole:

```csharp
public string Nazwa { get; set; } = "";
```

### Rodzaje dostępu

| Zapis | Odczyt | Zapis |
|---|---|---|
| `{ get; set; }` | każdy | każdy |
| `{ get; private set; }` | każdy | tylko klasa |
| `{ get; }` | każdy | tylko w konstruktorze (lub inicjalizatorze pola) |
| `{ get; init; }` | każdy | w konstruktorze lub inicjalizatorze obiektu `new X { … }` |

### Właściwość obliczana

Nie ma własnego pola — wynik liczy się przy każdym odczycie:

```csharp
public bool CzyDebet => Saldo < 0;
public string PelneImie => $"{Imie} {Nazwisko}";
public decimal Wartosc => Cena * Ilosc;
```

### Pełny przykład

```csharp
// Konto.cs
class Konto
{
    public string Wlasciciel { get; }                 // tylko do odczytu
    public decimal Saldo { get; private set; }        // czyta każdy, zmienia klasa

    private decimal limit;
    public decimal Limit                              // z walidacją
    {
        get => limit;
        set { if (value >= 0) limit = value; }
    }

    public bool CzyDebet => Saldo < 0;                // obliczana

    public Konto(string wlasciciel)
    {
        Wlasciciel = string.IsNullOrWhiteSpace(wlasciciel) ? "Nieznany" : wlasciciel;
    }

    public bool Wplac(decimal kwota)
    {
        if (kwota <= 0) return false;
        Saldo += kwota;
        return true;
    }

    public bool Wyplac(decimal kwota)
    {
        if (kwota <= 0 || kwota > Saldo + Limit) return false;
        Saldo -= kwota;
        return true;
    }
}
```

## Co zrobić z niepoprawnymi danymi

| Podejście | Gdzie stosujemy | Przykład |
|---|---|---|
| zignorować zmianę | w `set` właściwości | `set { if (value >= 0) limit = value; }` |
| zwrócić `bool` | w metodach wykonujących operację | `bool Wyplac(decimal kwota)` — `false`, gdy nie wolno |
| zamienić na bezpieczną wartość | w konstruktorze | `Stan = stan >= 0 ? stan : 0;` |
| zgłosić wyjątek | gdy dalsza praca nie ma sensu | `throw new ArgumentException("Cena musi być dodatnia");` |

Ważne, żeby **każda** droga do zmiany danych była objęta walidacją i żeby wywołujący mógł się dowiedzieć, czy operacja się udała. Ciche ignorowanie błędu w `set` jest najprostsze, ale najmniej czytelne dla użytkownika klasy. Wyjątki i ich obsługę omówimy osobno.

??? question "Czy to się skompiluje?"
    ```csharp
    // Konto.cs
    class Konto
    {
        private decimal saldo;
        public decimal Saldo { get { return saldo; } }
        public void Wplac(decimal kwota)
        {
            if (kwota > 0) saldo += kwota;
        }
    }

    // Program.cs
    Konto k = new Konto();
    k.Wplac(100);
    k.Wplac(-50);
    k.Saldo = 10;
    Console.WriteLine(k.Saldo);
    ```

    Która linia jest błędem i dlaczego? Co wypisze program po jej usunięciu? Jak zmienić klasę, żeby `k.Saldo = 10` było dozwolone tylko dla wartości dodatnich?

## Kolekcje w klasie

??? question "Czy ta klasa jest hermetyczna?"
    ```csharp
    // Dziennik.cs
    class Dziennik
    {
        public List<string> Wpisy { get; } = new List<string>();

        public void Dodaj(string tekst)
        {
            if (tekst != "") Wpisy.Add(tekst);
        }
    }

    // Program.cs
    Dziennik d = new Dziennik();
    d.Dodaj("");
    d.Wpisy.Add("");
    Console.WriteLine(d.Wpisy.Count);
    ```

    Co wypisze program? Metoda `Dodaj` odrzuca pusty tekst — dlaczego to nie wystarcza? Co chroni `{ get; }`: referencję do listy czy jej zawartość?

**Zasada:** nie wystawiaj zmiennej kolekcji na zewnątrz. `{ get; }` chroni tylko **referencję** — nikt nie podmieni listy na inną, ale każdy może zmienić jej zawartość.

```csharp
class Dziennik
{
    private readonly List<string> wpisy = new List<string>();

    public int LiczbaWpisow => wpisy.Count;

    public IReadOnlyList<string> Wpisy => wpisy;     // widok tylko do odczytu

    public bool Dodaj(string tekst)
    {
        if (string.IsNullOrWhiteSpace(tekst)) return false;
        wpisy.Add(tekst.Trim());
        return true;
    }

    public bool Usun(int numer)
    {
        if (numer < 0 || numer >= wpisy.Count) return false;
        wpisy.RemoveAt(numer);
        return true;
    }
}
```

- lista jest `private`, więc zmienia ją tylko klasa,
- `readonly` — pola nie da się przypisać na nowo po konstruktorze,
- `IReadOnlyList<string>` pozwala czytać elementy i liczyć je, ale nie ma metod `Add` ani `Remove`.

## Składowe statyczne

Składowa oznaczona `static` należy do **klasy**, a nie do pojedynczego obiektu — jest jedna, wspólna dla wszystkich obiektów.

```csharp
// Konto.cs
class Konto
{
    private static int liczba = 0;        // jedno pole na całą klasę

    public int Numer { get; }             // osobne w każdym obiekcie

    public Konto()
    {
        liczba++;
        Numer = liczba;
    }

    public static int LiczbaKont => liczba;

    public static bool CzyPoprawnyNumer(string numer) => numer.Length == 26 && numer.All(char.IsDigit);
}

// Program.cs
Konto k1 = new Konto();
Konto k2 = new Konto();
Console.WriteLine(k2.Numer);            // 2
Console.WriteLine(Konto.LiczbaKont);    // 2 — przez nazwę klasy
```

- składową statyczną wywołujemy przez **nazwę klasy**: `Konto.LiczbaKont`, tak jak `Math.Sqrt(16)`,
- metoda statyczna **nie ma** `this` — nie widzi pól konkretnego obiektu,
- typowe zastosowania: liczniki, numeracja, stałe, metody pomocnicze,
- **klasa statyczna** (`static class Matematyka`) ma wyłącznie składowe statyczne i nie można tworzyć jej obiektów.

## Jak projektować klasę

1. Zacznij od **odpowiedzialności**: jedno zdanie, za co klasa odpowiada.
2. Wypisz **dane** (rzeczowniki) i **operacje** (czasowniki).
3. Zapisz **reguły**, które muszą być zawsze prawdziwe (*niezmienniki*): „saldo ≥ 0”, „ilość > 0”.
4. Wszystkie pola `private`; dostęp przez właściwości i metody, które pilnują reguł.
5. **Konstruktor** ustawia stan, w którym reguły są spełnione.
6. Dodaj `ToString()`, żeby łatwo sprawdzać obiekt.
7. Napisz program testowy z **przypadkami brzegowymi**: zero, wartości ujemne, puste teksty, `null`, granice zakresów.

## Zadania

??? question "Zadanie 1: Termometr"
    Klasa `Termometr` z właściwością `Temperatura` (zakres −50 … 50, wartości spoza zakresu odrzucane), właściwościami obliczanymi `Fahrenheit` i `CzyMroz`, metodą `ToString()`. Pokaż w programie testowym, że nie da się ustawić temperatury 80.

??? question "Zadanie 2: Licznik obiektów"
    Do klasy `Uczen` z poprzedniej lekcji dodaj statyczny licznik utworzonych uczniów i numer w dzienniku nadawany automatycznie. Lista ocen ma być prywatna; dostęp przez `DodajOcene`, `LiczbaOcen` i `IReadOnlyList<int> Oceny`.

??? question "Zadanie 3: Hasło"
    Klasa `Konto` (użytkownika) z loginem tylko do odczytu i prywatnym hasłem. Metody: `ZmienHaslo(string stare, string nowe)` — zmienia tylko przy poprawnym starym haśle i gdy nowe ma co najmniej 8 znaków, w tym cyfrę; `Zaloguj(string haslo)` → `bool`. Hasła nie da się odczytać z zewnątrz.

??? question "Zadanie 4: Popraw klasę"
    Ta klasa łamie hermetyzację w co najmniej trzech miejscach. Znajdź je i popraw.

    ```csharp
    class Magazyn
    {
        public List<string> Produkty = new List<string>();
        public int Pojemnosc;
        public int Zajete;

        public void Dodaj(string p)
        {
            Produkty.Add(p);
            Zajete++;
        }
    }
    ```

??? question "Zadanie 5: Bank"
    Napisz klasę `KontoBankowe` zgodnie ze specyfikacją z [zadań i projektów](../08-zadania-i-projekty/index.md) (zadanie 1) i sprawdź wszystkie przypadki brzegowe.
