# Interfejsy

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zdefiniować interfejs i zaimplementować go w klasie, zaimplementować kilka interfejsów w jednej klasie, korzystać z obiektów przez typ interfejsu, użyć interfejsów z biblioteki .NET (`IComparable<T>`, `IEnumerable<T>`) oraz wybrać między klasą abstrakcyjną a interfejsem.

## Problem: wspólne zachowanie bez wspólnego przodka

W grze chcemy zapisywać stan wielu rzeczy: postaci, ekwipunku, ustawień, mapy. Nie mają one wspólnej klasy bazowej — postać to nie ustawienia. Do tego postać ma jednocześnie dziedziczyć po `Postac` i umieć się zapisywać, a C# pozwala tylko na **jedną** klasę bazową.

**Interfejs** opisuje, **co** obiekt potrafi, nie mówiąc, **jak** to robi i czym ten obiekt jest.

## Definicja i implementacja

```csharp
// IZapisywalny.cs
interface IZapisywalny
{
    string Zapisz();               // tylko sygnatura, bez ciała
    void Wczytaj(string dane);
}

// Ustawienia.cs
class Ustawienia : IZapisywalny
{
    public int Glosnosc { get; set; } = 50;
    public bool PelnyEkran { get; set; }

    public string Zapisz() => $"{Glosnosc};{PelnyEkran}";

    public void Wczytaj(string dane)
    {
        string[] czesci = dane.Split(';');
        Glosnosc = int.Parse(czesci[0]);
        PelnyEkran = bool.Parse(czesci[1]);
    }
}
```

- nazwy interfejsów zaczynamy od **`I`**: `IZapisywalny`, `IPorownywalny`, `IKsztalt`,
- składowe interfejsu są domyślnie **publiczne** — nie piszemy `public`,
- interfejs nie ma pól ani konstruktorów,
- klasa, która deklaruje interfejs (`: IZapisywalny`), **musi** zaimplementować wszystkie jego składowe jako `public` — inaczej błąd `CS0535`.

Interfejs to **kontrakt**: klasa obiecuje, że ma określone metody. Kod korzystający z interfejsu polega tylko na tej obietnicy.

## Obiekt przez typ interfejsu

```csharp
List<IZapisywalny> doZapisu = new()
{
    new Ustawienia(),
    new Gracz("Ada"),
    new Ekwipunek()
};

foreach (IZapisywalny obiekt in doZapisu)
{
    Console.WriteLine(obiekt.Zapisz());     // polimorfizm przez interfejs
}
```

Nie można utworzyć obiektu interfejsu (`new IZapisywalny()`), ale można mieć zmienną, parametr i listę typu interfejsu.

## Wiele interfejsów w jednej klasie

Klasa może mieć **jedną** klasę bazową i **dowolnie wiele** interfejsów. Klasa bazowa stoi na liście pierwsza:

```csharp
interface IAtakujacy
{
    int Atak();
}

interface ILeczacy
{
    void Ulecz(Postac cel);
}

class Paladyn : Postac, IAtakujacy, ILeczacy, IZapisywalny
{
    public Paladyn(string imie) : base(imie, 120) { }

    public int Atak() => 15;
    public void Ulecz(Postac cel) => cel.DodajZycie(20);
    public string Zapisz() => $"Paladyn;{Imie};{Zycie}";
    public void Wczytaj(string dane) { /* … */ }
}
```

To odpowiednik dziedziczenia wielokrotnego: `Paladyn` **jest** postacią i jednocześnie **potrafi** atakować, leczyć i zapisywać się.

```csharp
ILeczacy uzdrowiciel = new Paladyn("Artur");   // przez interfejs widać tylko Ulecz
if (uzdrowiciel is IAtakujacy a)
{
    Console.WriteLine(a.Atak());
}
```

## Interfejsy z biblioteki .NET

### IComparable&lt;T&gt; — porównywanie i sortowanie

`List<T>.Sort()` musi wiedzieć, jak porównać dwa obiekty. Liczby i teksty to wiedzą; własne klasy — nie. Uczymy je, implementując `IComparable<T>`:

```csharp
// Program.cs
List<Uczen> klasa = new()
{
    new Uczen("Nowak", 4.2),
    new Uczen("Kowalska", 5.1),
    new Uczen("Lis", 3.8)
};
klasa.Sort();                       // korzysta z CompareTo
klasa.ForEach(Console.WriteLine);

// Uczen.cs
class Uczen : IComparable<Uczen>
{
    public string Nazwisko { get; }
    public double Srednia { get; }

    public Uczen(string nazwisko, double srednia)
    {
        Nazwisko = nazwisko;
        Srednia = srednia;
    }

    // wynik < 0: this przed other; 0: równe; > 0: this po other
    public int CompareTo(Uczen? other)
    {
        if (other is null) return 1;
        return other.Srednia.CompareTo(Srednia);     // malejąco po średniej
    }

    public override string ToString() => $"{Nazwisko} ({Srednia:F2})";
}
```

### Inne często spotykane interfejsy

| Interfejs | Co obiecuje | Gdzie się przydaje |
|---|---|---|
| `IComparable<T>` | `CompareTo` — naturalny porządek | `Sort()`, `Max()`, `Min()` |
| `IEquatable<T>` | `Equals(T)` — równość wartości | `Contains`, `IndexOf`, słowniki |
| `IEnumerable<T>` | można przejść elementy pętlą | `foreach`, zapytania LINQ |
| `IDisposable` | `Dispose()` — zwolnienie zasobu | instrukcja `using` przy plikach i połączeniach |
| `IReadOnlyList<T>` | odczyt elementów i liczby elementów | udostępnianie kolekcji bez prawa zmiany |

`List<T>`, tablice i `Dictionary<K,V>` implementują `IEnumerable<T>` — dlatego każdą z nich można przejść `foreach`. Metoda przyjmująca `IEnumerable<int>` zadziała dla tablicy, listy i każdej innej kolekcji liczb.

```csharp
static int Suma(IEnumerable<int> liczby)
{
    int s = 0;
    foreach (int x in liczby) s += x;
    return s;
}

Suma(new[] { 1, 2, 3 });
Suma(new List<int> { 4, 5 });
```

## Klasa abstrakcyjna czy interfejs

| | Klasa abstrakcyjna | Interfejs |
|---|---|---|
| relacja | „**jest** rodzajem” | „**potrafi**” |
| ile może mieć klasa | jedną | wiele |
| pola i konstruktory | tak | nie |
| zaimplementowane metody | tak | zasadniczo nie (od C# 8 możliwe metody domyślne — rzadko używane) |
| modyfikatory dostępu składowych | dowolne | publiczne |
| typowe użycie | wspólny kod i stan dla rodziny klas | wspólna umiejętność różnych, niespokrewnionych klas |

Przykład łączący oba: `abstract class Pojazd` (marka, przebieg, `Jedz()`) i interfejsy `IElektryczny` (`Laduj()`), `IPrzewozacyLadunek` (`Zaladuj()`). `SamochodElektryczny : Pojazd, IElektryczny`, `Ciezarowka : Pojazd, IPrzewozacyLadunek`, a `Rower : Pojazd` nie implementuje żadnego.

## Zadania

??? question "Zadanie 1: Kształty przez interfejs"
    Zdefiniuj interfejs `IKsztalt` z metodami `Pole()` i `Obwod()` i właściwością `Nazwa`. Zaimplementuj go w klasach `Kolo`, `Prostokat`, `Trojkat`. Napisz metodę `static IKsztalt Najwiekszy(List<IKsztalt> lista)` i użyj jej.

??? question "Zadanie 2: Sortowanie produktów"
    Klasa `Produkt` (nazwa, cena) implementuje `IComparable<Produkt>`, porządkując rosnąco po cenie, a przy równej cenie — alfabetycznie po nazwie. Posortuj listę 8 produktów i wypisz ją.

??? question "Zadanie 3: Inteligentny dom"
    Interfejsy `IWlaczalny` (`Wlacz`, `Wylacz`, `CzyWlaczony`), `IRegulowany` (`UstawPoziom(int poziom)` 0–100), `IRaportujacy` (`Raport()` → `string`). Klasy: `Zarowka` (włączalna, regulowana), `Termostat` (regulowany, raportujący), `Glosnik` (wszystkie trzy). Program: wyłącz wszystkie urządzenia włączalne, ustaw poziom 50 we wszystkich regulowanych, wypisz raporty.

??? question "Zadanie 4: Abstrakcja czy interfejs?"
    Dla każdego przypadku wybierz klasę abstrakcyjną, interfejs albo oba i uzasadnij: wspólne cechy wszystkich kont bankowych; możliwość wydrukowania (faktura, raport, bilet); rodzina postaci w grze z punktami życia; możliwość porównywania obiektów; pojazdy, z których część jest elektryczna.

??? question "Zadanie 5: Kontrakt w praktyce"
    Napisz interfejs `IMagazyn` (`Dodaj`, `Usun`, `Ilosc`, `WszystkieProdukty`) i dwie implementacje: `MagazynWLiscie` (przechowuje produkty w `List<string>`) i `MagazynWSlowniku` (w `Dictionary<string, int>` z ilościami). Program testowy działa na zmiennej `IMagazyn` i działa z obiema implementacjami bez żadnych zmian — sprawdź to.
