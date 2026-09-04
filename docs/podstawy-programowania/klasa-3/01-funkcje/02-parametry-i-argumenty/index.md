# Parametry i argumenty

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć rozróżnić parametr od argumentu, używać wartości domyślnych i argumentów nazwanych, oraz wyjaśnić, jak Python naprawdę przekazuje dane do funkcji.

## Parametr a argument — to nie to samo

Te dwa słowa często są mylone, ale różnica jest prosta:

- **Parametr** — nazwa zmiennej w definicji funkcji (czyli "gniazdo", do którego coś wstawisz)
- **Argument** — konkretna wartość, którą przekazujesz przy wywołaniu funkcji (czyli to, co faktycznie wstawiasz)

```python
def przywitaj(imie):      # imie -> parametr
    print(f"Cześć, {imie}!")

przywitaj("Adrian")       # "Adrian" -> argument
```

## Wartości domyślne

Możesz nadać parametrowi wartość domyślną — jeśli ktoś wywoła funkcję bez podania tego argumentu, użyta zostanie wartość domyślna.

```python
def przywitaj(imie, jezyk="polski"):
    if jezyk == "polski":
        print(f"Cześć, {imie}!")
    else:
        print(f"Hello, {imie}!")

przywitaj("Adrian")              # użyje domyślnego "polski"
przywitaj("Adrian", "angielski") # nadpisze wartość domyślną
```

!!! warning "Częsty błąd"
    Parametry z wartością domyślną muszą znajdować się **na końcu** listy parametrów, po parametrach bez wartości domyślnej. `def f(a=1, b)` nie zadziała — trzeba `def f(b, a=1)`.

## Argumenty nazwane (keyword arguments)

Możesz przekazywać argumenty po nazwie parametru, niezależnie od kolejności:

```python
def opisz_osobe(imie, wiek):
    print(f"{imie} ma {wiek} lat")

opisz_osobe(wiek=18, imie="Adrian")  # zadziała mimo odwróconej kolejności
```

## Zwracanie wartości — `return`

Funkcja może, ale nie musi, zwracać wartość przez `return`. Bez `return` funkcja zwraca `None`.

```python
def kwadrat(liczba):
    return liczba * liczba

def wypisz_kwadrat(liczba):
    print(liczba * liczba)   # nic nie zwraca, tylko wypisuje

a = kwadrat(5)        # a = 25
b = wypisz_kwadrat(5)  # wypisze 25 na ekranie, ale b = None
```

!!! tip "Return kończy działanie funkcji natychmiast"
    Gdy Python napotka `return`, natychmiast kończy wykonywanie funkcji — cokolwiek jest napisane po nim w tej samej gałęzi kodu, nie zostanie wykonane.

## Jak Python naprawdę przekazuje argumenty

To jest miejsce, w którym warto uważać. Python nie działa dokładnie tak jak niektóre inne języki — nie ma tu prostego podziału na "przez wartość" i "przez referencję". Zamiast tego liczy się **typ danych**, które przekazujesz:

- **Typy niemutowalne** (int, float, str, tuple) — funkcja dostaje kopię wartości. Zmiana wewnątrz funkcji **nie wpływa** na zmienną na zewnątrz.
- **Typy mutowalne** (list, dict, set) — funkcja dostaje odniesienie do tego samego obiektu. Zmiana **wewnątrz** funkcji **wpływa** na oryginalną zmienną na zewnątrz.

```python
def zmien_liczbe(x):
    x = x + 1

liczba = 5
zmien_liczbe(liczba)
print(liczba)   # nadal 5 — int jest niemutowalny

def dodaj_element(lista):
    lista.append(4)

moja_lista = [1, 2, 3]
dodaj_element(moja_lista)
print(moja_lista)   # [1, 2, 3, 4] — lista jest mutowalna, zmiana "wyszła na zewnątrz"
```

!!! warning "Częsty błąd i pułapka na egzaminie"
    Wielu uczniów zakłada, że skoro `x = x + 1` nic nie zmieniło na zewnątrz, to funkcje "nigdy nic nie zmieniają". To nieprawda — zależy to od typu danych. Ta różnica bywa pytana wprost na egzaminie zawodowym.

??? question "Zadanie na lekcję (kliknij, żeby rozwinąć)"
    Napisz funkcję `oblicz_cene(cena, rabat=0)`, która zwraca cenę po rabacie procentowym (domyślnie brak rabatu). Wywołaj ją raz bez rabatu i raz z rabatem, używając argumentu nazwanego.

??? question "Zadanie domowe (kliknij, żeby rozwinąć)"
    Napisz funkcję `dodaj_do_koszyka(koszyk, produkt)`, która dodaje produkt do listy `koszyk`. Sprawdź eksperymentalnie (i zapisz wniosek w komentarzu w kodzie), czy zmiana widoczna jest poza funkcją — i wyjaśnij dlaczego, opierając się na tym, czego nauczyłeś się w tej lekcji.

## Zadania dodatkowe

### Poziom podstawowy

??? question "1. Powitanie z domyślnym językiem"
    Napisz funkcję `powitanie(imie, pora_dnia="dzień")`, zwracającą napis w stylu `"Dzień dobry, Adrian!"`. Wywołaj ją z różnymi porami dnia.

??? question "2. Potęgowanie"
    Napisz funkcję `potega(podstawa, wykladnik=2)`, która domyślnie podnosi liczbę do kwadratu, ale pozwala też podać inny wykładnik.

??? question "3. Formatowanie ceny"
    Napisz funkcję `sformatuj_cene(kwota, waluta="zł")`, zwracającą tekst w stylu `"19.99 zł"`.

### Poziom średni

??? question "4. Notatka z argumentami nazwanymi"
    Napisz funkcję `utworz_notatke(tytul, tresc, priorytet="niski")`. Wywołaj ją trzykrotnie, za każdym razem przekazując argumenty w innej kolejności, korzystając z argumentów nazwanych.

??? question "5. Licznik wywołań (mutowalność)"
    Napisz funkcję `dodaj_wynik(lista_wynikow, wynik)`, dopisującą wynik do przekazanej listy. Wywołaj ją kilka razy z tą samą listą i wypisz jej zawartość na końcu — zaobserwuj, że lista "pamięta" wszystkie dopisane wartości.

??? question "6. Zamiana bez efektu ubocznego"
    Napisz funkcję `powieksz(liczba)`, która **zwraca** liczbę pomnożoną przez 2 (bez modyfikowania oryginalnej zmiennej). Wywołaj ją i przypisz wynik do nowej zmiennej — pokaż, że oryginał się nie zmienił.

### Poziom trudniejszy

??? question "7. Aktualizacja słownika ucznia"
    Napisz funkcję `dodaj_ocene(uczen, przedmiot, ocena)`, gdzie `uczen` to słownik (`dict`) z kluczem `"oceny"` będącym listą. Funkcja ma dopisać nową ocenę do listy wewnątrz słownika. Sprawdź, czy zmiana jest widoczna poza funkcją, i zapisz dlaczego.

??? question "8. Porównanie dwóch podejść"
    Napisz dwie wersje funkcji podwajającej wszystkie elementy listy: jedną, która **modyfikuje** listę wejściową w miejscu (bez `return`), i drugą, która **zwraca nową listę**, nie ruszając oryginalnej. Zademonstruj różnicę w działaniu obu wersji.
