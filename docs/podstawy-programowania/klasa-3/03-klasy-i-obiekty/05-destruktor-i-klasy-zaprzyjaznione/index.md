# Destruktor i klasy zaprzyjaźnione

!!! info "Cel lekcji"
    Po tej lekcji będziesz wiedział, czym jest destruktor i dlaczego w Pythonie prawie się go nie używa, a także będziesz rozumiał ideę klas zaprzyjaźnionych — mechanizmu, który w ogóle nie istnieje w Pythonie ani w Javie, a jest cechą charakterystyczną C++.

## Destruktor

### Dlaczego C++ potrzebuje destruktora

W C++ programista **ręcznie zarządza pamięcią** — gdy tworzy obiekt, sam decyduje, kiedy go usunąć i zwolnić zajmowaną przez niego pamięć. Destruktor to specjalna metoda wywoływana automatycznie w momencie zniszczenia obiektu, używana głównie do "posprzątania" (np. zamknięcia pliku, zwolnienia zasobu).

```cpp
class Plik {
public:
    Plik(std::string nazwa) {
        // otwórz plik
    }

    ~Plik() {  // destruktor — tylda + nazwa klasy
        // zamknij plik, zwolnij zasoby
    }
};
```

### Python i Java — automatyczny garbage collector

**Python i Java nie wymagają ręcznego zarządzania pamięcią.** Obie mają mechanizm zwany *garbage collector* (odśmiecacz pamięci), który automatycznie wykrywa, kiedy obiekt nie jest już nigdzie używany, i sam zwalnia zajmowaną przez niego pamięć — bez udziału programisty.

!!! info "Jak to wygląda w Javie"
    Java **nie ma prawdziwego destruktora**. Kiedyś istniała metoda `finalize()`, wywoływana (niegwarantowanie, kiedy dokładnie) przed usunięciem obiektu przez garbage collector — ale została uznana za zawodną i **usunięta w nowszych wersjach Javy**. Współczesna Java rozwiązuje problem "posprzątania po sobie" inaczej — przez mechanizm `try-with-resources` i interfejs `AutoCloseable`, gdzie programista jawnie definiuje metodę `close()` i sam decyduje, kiedy ją wywołać (albo robi to automatycznie blok `try-with-resources`), zamiast polegać na niepewnym momencie działania garbage collectora.

```java
    try (Plik plik = new Plik("dane.txt")) {
        // praca z plikiem
    } // automatyczne wywołanie plik.close() na końcu bloku
```

### `__del__` w Pythonie — istnieje, ale prawie nikt go nie używa

Python ma odpowiednik destruktora — metodę `__del__`, wywoływaną, gdy obiekt jest usuwany przez garbage collector.

```python
class Plik:
    def __init__(self, nazwa):
        self.nazwa = nazwa
        print(f"Otwieram {self.nazwa}")

    def __del__(self):
        print(f"Zamykam {self.nazwa}")

plik = Plik("dane.txt")
del plik  # wymusza usunięcie obiektu, wywołuje __del__
```

!!! warning "Dlaczego `__del__` jest rzadko używany w praktyce"
    Podobnie jak Javowe `finalize()`, moment wywołania `__del__` **nie jest gwarantowany** — Python decyduje, kiedy dokładnie usunąć obiekt z pamięci, i nie zawsze da się to precyzyjnie przewidzieć. Do "sprzątania" zasobów (zamykania plików, połączeń) standardowo używa się w Pythonie instrukcji `with`, działającej na tej samej zasadzie co Javowe `try-with-resources`:
```python
    with open("dane.txt") as plik:
        zawartosc = plik.read()
    # plik zostaje zamknięty automatycznie na końcu bloku with
```

!!! tip "Podsumowanie"
    Trzy języki, trzy podejścia: **C++** wymaga jawnego destruktora, bo programista sam zarządza pamięcią. **Java** kiedyś miała `finalize()`, dziś zaleca `try-with-resources`. **Python** ma `__del__`, ale w praktyce też zaleca się `with`. Wspólny mianownik: nowoczesny kod (Java, Python) **unika polegania na destruktorze** i zamiast tego jawnie zamyka zasoby w kontrolowanym momencie.

??? question "Zadanie 1: Zaobserwuj __del__"
    Zdefiniuj klasę `Zasob` z `__init__` wypisującym `"Tworzę zasób"` i `__del__` wypisującym `"Usuwam zasób"`. Stwórz obiekt, a potem wywołaj na nim `del nazwa_obiektu`. Sprawdź, czy oba komunikaty się pojawiły.

??? question "Zadanie 2: Napisz odpowiednik przez `with`"
    Napisz prosty program otwierający i czytający dowolny plik tekstowy za pomocą `with open(...)`, zamiast polegać na `__del__`. Wyjaśnij w komentarzu, dlaczego to podejście jest bardziej przewidywalne.

??? question "Zadanie 3: Porównanie trzech języków"
    Własnymi słowami wyjaśnij różnicę w zarządzaniu pamięcią między C++ a Pythonem/Javą. Dlaczego C++ **musi** mieć destruktor, a Python i Java mogą się bez niego (w praktyce) obejść?

---

## Klasy i funkcje zaprzyjaźnione (friend)

!!! danger "Ten mechanizm nie istnieje ani w Pythonie, ani w Javie"
    W przeciwieństwie do konstruktora kopiującego czy destruktora (które mają swój odpowiednik lub namiastkę w innych językach), **`friend` to unikalna cecha C++**. Ani Python, ani Java nie mają tego mechanizmu w żadnej formie — nawet w uproszczonej. Poznajesz to wyłącznie teoretycznie, bo może pojawić się w pytaniu na egzaminie zawodowym odwołującym się do C++.

### Czym jest `friend` w C++

Domyślnie prywatne (`private`) pola klasy są niedostępne z zewnątrz — nawet dla innej, niepowiązanej klasy czy funkcji. `friend` pozwala **jawnie zrobić wyjątek**: wskazana klasa lub funkcja dostaje dostęp do prywatnych składowych, mimo że normalnie by go nie miała.

```cpp
class Konto {
private:
    double saldo;

    friend class Bank;  // klasa Bank dostaje dostęp do prywatnych pól Konto

public:
    Konto(double saldo) : saldo(saldo) {}
};

class Bank {
public:
    void ustawSaldo(Konto& konto, double nowe_saldo) {
        konto.saldo = nowe_saldo;  // działa TYLKO dzięki friend — normalnie byłoby zablokowane
    }
};
```

Podobnie można zaprzyjaźnić pojedynczą **funkcję** (niekoniecznie całą klasę) — `friend void jakasFunkcja(Konto& k);` daje dostęp tylko tej jednej funkcji.

### Po co to komu — i dlaczego to kontrowersyjne

`friend` bywa używane, gdy dwie klasy są ze sobą **ściśle powiązane** i sztuczne trzymanie się pełnej hermetyzacji utrudniałoby pisanie kodu bardziej, niż by pomagało (klasyczny przykład: klasa `Macierz` i `Wektor`, które muszą często sięgać do swoich wewnętrznych danych). Jednocześnie wielu programistów uważa `friend` za **osłabienie hermetyzacji** — bo łamie zasadę, że tylko sama klasa kontroluje dostęp do swoich prywatnych danych, i część projektów świadomie go unika.

### Dlaczego Python i Java nie mają tego mechanizmu

- **Python** i tak nie ma **prawdziwego** `private` (pamiętasz konwencję `_`/`__` z poprzedniej lekcji) — skoro nic nie jest naprawdę zablokowane, nie ma czego "odblokowywać" przez `friend`.
- **Java** rozwiązuje podobny problem inaczej — przez **dostęp pakietowy** (*package-private*): pole lub metoda bez żadnego specyfikatora (bez `public`, `private`, `protected`) jest dostępna dla **wszystkich klas w tym samym pakiecie**, nawet niepowiązanych dziedziczeniem. To nie jest to samo co `friend` (bo obejmuje *cały pakiet*, a nie jedną wybraną klasę), ale rozwiązuje pokrewny problem — kontrolowane rozluźnienie hermetyzacji między ściśle powiązanymi klasami.

```java
class Konto {
    double saldo;  // brak specyfikatora = dostęp pakietowy, NIE jest to friend, ale najbliższy odpowiednik

    Konto(double saldo) {
        this.saldo = saldo;
    }
}

class Bank {
    void ustawSaldo(Konto konto, double noweSaldo) {
        konto.saldo = noweSaldo;  // działa, bo Bank i Konto są w tym samym pakiecie
    }
}
```

!!! info "Podsumowanie różnic"
    | Język | Mechanizm | Zakres dostępu |
    |---|---|---|
    | C++ | `friend` | Dokładnie wskazana klasa lub funkcja |
    | Java | dostęp pakietowy (brak specyfikatora) | Wszystkie klasy w tym samym pakiecie |
    | Python | brak odpowiednika | Konwencja `_`/`__` i tak niczego nie blokuje |

??? question "Zadanie 4: Prześledź przykład C++"
    Spójrz na przykład klas `Konto` i `Bank` w C++ powyżej. Co by się stało, gdyby usunąć linijkę `friend class Bank;`? Który fragment kodu przestałby działać i dlaczego?

??? question "Zadanie 5: Zaprojektuj sytuację wymagającą friend"
    Wymyśl własny przykład dwóch ściśle powiązanych klas (inny niż Konto/Bank), gdzie sensowne byłoby użycie `friend` w C++. Opisz, jakie dane jedna klasa musiałaby udostępnić drugiej i dlaczego zwykła hermetyzacja by tu przeszkadzała.

??? question "Zadanie 6: Porównaj z dostępem pakietowym w Javie"
    Wyjaśnij własnymi słowami, dlaczego dostęp pakietowy w Javie **nie jest** pełnym odpowiednikiem `friend` z C++. Jaka jest kluczowa różnica w precyzji kontroli dostępu między tymi dwoma mechanizmami?

## Zadania utrwalające

??? question "Zadanie 7: Pytanie egzaminacyjne — destruktor"
    Wyjaśnij, czym jest destruktor, w jakim języku jest rzeczywiście potrzebny i dlaczego, oraz jak Python i Java radzą sobie bez jego pełnego odpowiednika.

??? question "Zadanie 8: Pytanie egzaminacyjne — friend"
    Wyjaśnij, czym są klasy i funkcje zaprzyjaźnione w C++, do czego służą, oraz dlaczego nie mają odpowiednika ani w Pythonie, ani w Javie.

??? question "Zadanie 9: Zastosuj `with` zamiast destruktora"
    Napisz klasę `Licznik`, która przy tworzeniu obiektu wypisuje `"Start"`, a Ty ręcznie (bez polegania na `__del__`) dodaj metodę `zakoncz()`, którą trzeba jawnie wywołać, żeby wypisała `"Koniec"`. Zapisz w komentarzu, dlaczego to podejście jest bardziej przewidywalne niż poleganie na `__del__`.
