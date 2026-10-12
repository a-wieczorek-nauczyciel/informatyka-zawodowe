# Czym jest funkcja

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, czym jest funkcja, po co dzieli się program na funkcje, i napisać własną, prostą funkcję w Pythonie.

## Dlaczego w ogóle dzielimy program na funkcje

Wyobraź sobie, że piszesz program, w którym trzykrotnie musisz obliczyć pole trójkąta. Bez funkcji musiałbyś trzy razy przepisać ten sam wzór. Funkcja pozwala napisać go **raz** i używać go wielokrotnie, wywołując go po nazwie.

!!! tip "Definicja"
    Funkcja to nazwany, wydzielony fragment kodu, który wykonuje określone zadanie i można go wywołać (uruchomić) w dowolnym miejscu programu, dowolną liczbę razy.

## Prototyp funkcji

Prototyp (nagłówek) funkcji to jej "wizytówka" — mówi, jak funkcja się nazywa, jakich danych oczekuje i co zwraca.

```python
def pole_trojkata(podstawa, wysokosc):
    wynik = (podstawa * wysokosc) / 2
    return wynik
```

- `def` — słowo kluczowe rozpoczynające definicję funkcji
- `pole_trojkata` — nazwa funkcji (wybierasz ją sam, powinna opisywać, co funkcja robi)
- `(podstawa, wysokosc)` — parametry, czyli dane, które funkcja przyjmuje
- `return` — zwraca wynik do miejsca, z którego funkcja została wywołana

## Wywołanie funkcji

Samo zdefiniowanie funkcji niczego jeszcze nie robi — trzeba ją **wywołać**:

```python
wynik = pole_trojkata(6, 4)
print(wynik)
```

!!! warning "Częsty błąd"
    Zdefiniowanie funkcji (`def ...`) to nie to samo, co jej wywołanie. Wielu początkujących pisze funkcję i dziwi się, że "nic się nie dzieje" — bo nigdzie jej nie wywołali.


??? question "Zadanie na lekcję (kliknij, żeby rozwinąć)"
    Napisz funkcję `pole_prostokata(a, b)`, która przyjmuje dwa boki prostokąta i zwraca jego pole. Wywołaj ją dla kilku różnych wartości.

??? question "Zadanie na lekcję (kliknij, żeby rozwinąć)"
    Napisz funkcję `czy_parzysta(liczba)`, która zwraca `True`, jeśli liczba jest parzysta, i `False` w przeciwnym razie. Przetestuj ją dla pięciu różnych liczb.

## Zadania wprowadzające

Zanim zaczniesz pisać własne funkcje, upewnij się, że rozumiesz, jak działają gotowe. Te zadania nie wymagają pisania kodu od zera.

### Przewidź wynik

??? question "1. Co wypisze ten kod?"
```python
    def podwoj(liczba):
        return liczba * 2

    wynik = podwoj(7)
    print(wynik)
```
    Zanim uruchomisz kod, zapisz na kartce, co Twoim zdaniem się wypisze. Dopiero potem sprawdź w Codespace, czy się zgadza.

??? question "2. Co wypisze ten kod?"
```python
    def przywitaj(imie):
        print(f"Cześć, {imie}!")

    przywitaj("Ola")
    przywitaj("Kuba")
```
    Ile razy wykona się `print`? Co dokładnie się wypisze?

### Znajdź błąd

??? question "3. Ten kod nie działa — dlaczego?"
```python
    def pole_kwadratu(bok):
        wynik = bok * bok

    print(pole_kwadratu(4))
```
    Kod się uruchomi bez błędu, ale wynik będzie zaskakujący. Jakiego słowa kluczowego brakuje i dlaczego to zmienia wynik?

??? question "4. Ten kod zgłasza błąd — dlaczego?"
```python
    def pomnoz(a, b):
        return a * b

    wynik = pomnoz(5)
    print(wynik)
```
    Python zgłosi błąd przy wywołaniu funkcji. Co dokładnie jest nie tak?

### Dokończ kod

??? question "5. Uzupełnij brakujący fragment"
```python
    def odejmij(a, b):
        wynik = ___
        return wynik

    print(odejmij(10, 3))  # ma wypisać 7
```
    Co powinno znaleźć się w miejscu `___`?

??? question "6. Uzupełnij wywołanie funkcji"
```python
    def przedstaw_sie(imie, wiek):
        print(f"Mam na imię {imie} i mam {wiek} lat")

    ___  # wywołaj funkcję tak, żeby wypisała: "Mam na imię Adrian i mam 25 lat"
```

### Prześledź krok po kroku

??? question "7. Prześledź działanie krok po kroku"
```python
    def krok(liczba):
        liczba = liczba + 1
        print(liczba)
        return liczba

    x = 5
    x = krok(x)
    x = krok(x)
    print(x)
```
    Zapisz na kartce, co się wypisze **po kolei**, linijka po linijce — zanim uruchomisz kod. Ile razy wykona się `print` i jakie wartości pokaże?

## Zadania dodatkowe

Poćwicz pisanie własnych funkcji na kilku poziomach trudności. Zacznij od podstawowych, przejdź dalej dopiero gdy poprzednie wychodzą Ci bez zaglądania do notatek.

### Poziom podstawowy

??? question "1. Pole koła"
    Napisz funkcję `pole_kola(promien)`, która zwraca pole koła (wzór: π × r²). Możesz przyjąć π = 3.14159.

??? question "2. Zamiana stopni"
    Napisz funkcję `celsjusz_na_fahrenheit(stopnie)`, która przelicza temperaturę ze stopni Celsjusza na Fahrenheita (wzór: F = C × 9/5 + 32).

??? question "3. Pozdrowienie"
    Napisz funkcję `przywitaj(imie)`, która zwraca napis `"Cześć, [imie]!"`. Wywołaj ją dla trzech różnych imion.

### Poziom średni

??? question "4. Liczba pierwsza"
    Napisz funkcję `czy_pierwsza(liczba)`, która sprawdza, czy podana liczba jest liczbą pierwszą, i zwraca `True` lub `False`.

??? question "5. Silnia"
    Napisz funkcję `silnia(n)`, która oblicza silnię liczby `n` (np. silnia(5) = 5×4×3×2×1 = 120). Użyj pętli wewnątrz funkcji.

??? question "6. Największa z trzech"
    Napisz funkcję `najwieksza(a, b, c)`, która zwraca największą z trzech podanych liczb, bez używania wbudowanej funkcji `max()`.

### Poziom trudniejszy

??? question "7. Sprawdzanie hasła"
    Napisz funkcję `czy_silne_haslo(haslo)`, która zwraca `True`, jeśli hasło ma co najmniej 8 znaków **i** zawiera przynajmniej jedną cyfrę, w przeciwnym razie `False`.

??? question "8. Mini-kalkulator"
    Napisz cztery osobne funkcje: `dodaj(a, b)`, `odejmij(a, b)`, `pomnoz(a, b)`, `podziel(a, b)`. Napisz program, który pyta użytkownika o dwie liczby i działanie, a następnie wywołuje odpowiednią funkcję.
