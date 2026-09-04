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

??? question "Zadanie domowe (kliknij, żeby rozwinąć)"
    Napisz funkcję `czy_parzysta(liczba)`, która zwraca `True`, jeśli liczba jest parzysta, i `False` w przeciwnym razie. Przetestuj ją dla pięciu różnych liczb.
