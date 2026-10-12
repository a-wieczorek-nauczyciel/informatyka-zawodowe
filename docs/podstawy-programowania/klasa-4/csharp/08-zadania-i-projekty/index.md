# Zadania i projekty

Większe programy złożone z kilku współpracujących klas. Każdy to osobny projekt (`dotnet new console -n Nazwa`), a każda klasa — osobny plik. Do każdego zadania dołącz **diagram klas UML** i **listę przypadków brzegowych**, które sprawdziłeś.

!!! tip "Jak pracować nad większym programem"
    1. Przeczytaj specyfikację i narysuj diagram klas.
    2. Napisz jedną klasę i od razu ją sprawdź krótkim kodem w `Program.cs`.
    3. Dopiero gdy klasa działa, przejdź do następnej.
    4. Na końcu uruchom pełny program testowy i przypadki brzegowe.

## Zadanie 1: Bank

**Temat:** klasy, konstruktory, hermetyzacja, składowe statyczne.

Utwórz projekt `Bank` i plik `KontoBankowe.cs`.

| Składowa | Rodzaj | Zachowanie |
|---|---|---|
| `Wlasciciel` | właściwość `string`, tylko do odczytu | pusty lub biały tekst zamień na „Nieznany” |
| `Numer` | właściwość `int`, tylko do odczytu | nadawany automatycznie: 1, 2, 3 … |
| `Saldo` | właściwość `decimal`, `private set` | nigdy nie jest ujemne |
| `LiczbaKont` | właściwość `static` | ile kont utworzono |
| konstruktory | `(wlasciciel)` i `(wlasciciel, saldoPoczatkowe)` | pierwszy woła drugi z saldem 0; ujemne saldo początkowe zamień na 0 |
| `Wplac(kwota)` | metoda `bool` | kwota musi być dodatnia |
| `Wyplac(kwota)` | metoda `bool` | kwota dodatnia i nie większa niż saldo |
| `Przelej(cel, kwota)` | metoda `bool` | cel nie jest `null` ani tym samym kontem; wypłata z tego konta i wpłata na cel |
| `ToString()` | nadpisana | np. `Konto #1 (Ada): 500,00 zł` |

Program testowy:

```csharp
KontoBankowe ada = new KontoBankowe("Ada", 500m);
KontoBankowe olek = new KontoBankowe("Olek");
Console.WriteLine(ada);
Console.WriteLine(olek);

Console.WriteLine(ada.Wplac(250m));
Console.WriteLine(ada.Wplac(-10m));
Console.WriteLine(ada.Wyplac(1000m));
Console.WriteLine(ada.Przelej(olek, 300m));

Console.WriteLine(ada);
Console.WriteLine(olek);
Console.WriteLine(KontoBankowe.LiczbaKont);
```

Przypadki brzegowe:

- [ ] wpłata i wypłata równa 0
- [ ] wypłata dokładnie całego salda
- [ ] przelew na to samo konto
- [ ] przelew na `null`
- [ ] saldo początkowe ujemne
- [ ] pusta nazwa właściciela
- [ ] przelew większy niż saldo — czy saldo odbiorcy się nie zmieniło?
- [ ] z zewnątrz nie da się ustawić salda ani numeru konta

## Zadanie 2: Sklep z koszykiem

**Temat:** kilka współpracujących klas, prywatna kolekcja, właściwości obliczane.

Projekt `Sklep`, pliki `Produkt.cs`, `PozycjaKoszyka.cs`, `Koszyk.cs`.

| Klasa | Składowa | Zachowanie |
|---|---|---|
| `Produkt` | `Nazwa`, `Cena`, `Stan` | `Nazwa` tylko do odczytu (pusta: „Bez nazwy”); `Cena` i `Stan` z `private set` |
| | konstruktor `(nazwa, cena, stan)` | cena ≤ 0 zamień na 1; stan ujemny na 0 |
| | `ZmienCene`, `Dostaw`, `Sprzedaj` | zwracają `bool`; wartości muszą być dodatnie, sprzedaż nie większa niż stan |
| | `ToString()` | np. `Chleb: 4,50 zł (stan: 10)` |
| `PozycjaKoszyka` | `Produkt`, `Ilosc`, `Wartosc` | `Wartosc` obliczana: cena × ilość; `Ilosc` z `private set` |
| | `ZwiekszIlosc(ile)`, `ToString()` | np. `Chleb x3 = 13,50 zł` |
| `Koszyk` | `Dodaj(produkt, ile)` | `false` dla `null`, ilości ≤ 0 lub gdy łączna ilość w koszyku przekroczy `Stan`; ten sam produkt łączy się w jedną pozycję |
| | `Suma`, `LiczbaPozycji` | właściwości obliczane |
| | `Zaplac()`, `Wypisz()` | `Zaplac` sprzedaje zawartość, czyści koszyk i zwraca sumę |

Kolekcja pozycji ma być **prywatna** w klasie `Koszyk`.

```csharp
Produkt chleb = new Produkt("Chleb", 4.50m, 10);
Produkt mleko = new Produkt("Mleko", 3.20m, 5);

Koszyk koszyk = new Koszyk();
Console.WriteLine(koszyk.Dodaj(chleb, 2));
Console.WriteLine(koszyk.Dodaj(mleko, 1));
Console.WriteLine(koszyk.Dodaj(chleb, 1));
Console.WriteLine(koszyk.Dodaj(mleko, 10));
Console.WriteLine(koszyk.LiczbaPozycji);
koszyk.Wypisz();

decimal razem = koszyk.Zaplac();
Console.WriteLine($"Zapłacono: {razem:F2} zł");
Console.WriteLine(chleb);
Console.WriteLine(mleko);
Console.WriteLine(koszyk.LiczbaPozycji);
```

Przypadki brzegowe:

- [ ] dodanie `null` zamiast produktu
- [ ] ilość 0 i ujemna
- [ ] produkt ze stanem 0
- [ ] łączna ilość produktu w koszyku przekracza stan
- [ ] produkt z ceną 0 albo pustą nazwą
- [ ] `Zaplac` na pustym koszyku

## Zadanie 3: Kadry i płace

**Temat:** dziedziczenie, klasa abstrakcyjna, polimorfizm.

| Klasa | Opis |
|---|---|
| `Pracownik` (abstrakcyjna) | imię, nazwisko, pensja podstawowa (> 0), unikalny identyfikator nadawany automatycznie, abstrakcyjna metoda `Wyplata()`, wirtualna `Opis()` |
| `PracownikEtatowy` | wypłata = pensja podstawowa |
| `Kierownik : PracownikEtatowy` | dodatek funkcyjny; lista podwładnych (prywatna); wypłata = pensja + dodatek + 100 zł za każdego podwładnego |
| `Handlowiec` | prowizja procentowa od sprzedaży w miesiącu (`DodajSprzedaz`), wypłata = pensja + prowizja |
| `Zleceniobiorca` | stawka godzinowa i liczba godzin, brak pensji podstawowej |
| `Firma` | prywatna lista pracowników; `Zatrudnij`, `Zwolnij(id)`, `ListaPlac()`, `SumaWyplat`, `NajlepiejOplacany()` |

- [ ] lista płac wypisuje każdego pracownika z rodzajem umowy i wypłatą — bez sprawdzania typów `is`
- [ ] `ListaPlac()` sortuje pracowników malejąco po wypłacie (`IComparable<Pracownik>` lub porównanie w `Sort`)
- [ ] kierownik nie może być własnym podwładnym; ten sam pracownik nie może być dodany dwa razy
- [ ] diagram klas z zaznaczonym dziedziczeniem i klasą abstrakcyjną

## Zadanie 4: Mini-projekt — aplikacja z hierarchią klas

**Temat:** samodzielne zaprojektowanie programu obiektowego.

Wybierz jeden temat albo zaproponuj własny:

- **Wypożyczalnia** — pojazdy różnych rodzajów (samochód, rower, hulajnoga) z różnym cennikiem, klienci, wypożyczenia i zwroty,
- **Biblioteka** — książki, czasopisma, audiobooki; czytelnicy z limitem wypożyczeń; kary za przetrzymanie,
- **Gra RPG** — postacie różnych klas, przedmioty, walka turowa, zapis stanu przez interfejs,
- **Zoo** — zwierzęta, wybiegi, karmienie według gatunków, raport dzienny.

Wymagania:

- [ ] co najmniej 5 klas, w tym klasa abstrakcyjna i co najmniej 2 poziomy dziedziczenia
- [ ] co najmniej jeden własny interfejs zaimplementowany przez niespokrewnione klasy
- [ ] co najmniej jedna metoda wirtualna lub abstrakcyjna wykorzystana polimorficznie
- [ ] pełna hermetyzacja: brak publicznych pól, kolekcje prywatne, walidacja danych
- [ ] menu tekstowe w pętli, odporne na błędne dane
- [ ] diagram klas UML i krótki opis projektu (do czego służy każda klasa)
- [ ] prezentacja: pokaż działanie i wyjaśnij jedną decyzję projektową (np. dlaczego interfejs, a nie klasa bazowa)
