# Zadania utrwalające

Programy podsumowujące wprowadzenie do C#. Każdy zapisz jako osobny projekt konsolowy (`dotnet new console -n NazwaZadania`). Przy każdym zadaniu przygotuj **tabelę testów**: dane wejściowe, oczekiwany wynik, wynik programu — także dla błędnych danych.

!!! tip "Zanim oddasz program"
    Sprawdź, czy program: kompiluje się bez ostrzeżeń, nie przerywa się przy błędnych danych (używa `TryParse`), ma czytelne nazwy zmiennych i metod oraz jest podzielony na metody tam, gdzie to ma sens.

## Zadanie 1: Konwerter temperatur

- [ ] Menu: `1` — °C → °F, `2` — °F → °C, `0` — koniec; program działa w pętli do wyboru `0`
- [ ] Wczytywanie temperatury przez osobną metodę z `TryParse`
- [ ] Obliczenia w dwóch metodach zwracających `double`
- [ ] Wynik z jednym miejscem po przecinku
- [ ] Tabela testów: co najmniej 5 przypadków, w tym błędne wejście i błędny wybór z menu

## Zadanie 2: Statystyki klasy

- [ ] Program wczytuje liczbę uczniów, a potem punkty każdego ucznia (0–100) do tablicy
- [ ] Wypisuje: średnią, najwyższy i najniższy wynik, liczbę osób, które zdały (≥ 50)
- [ ] Każdy wynik zamienia na ocenę wyrażeniem `switch` i wypisuje listę „punkty → ocena”
- [ ] Obliczenia w osobnych metodach przyjmujących tablicę

## Zadanie 3: Gra w kości

- [ ] Gracz i komputer rzucają dwiema kośćmi (`Random.Shared.Next(1, 7)`), wygrywa wyższa suma
- [ ] Gra trwa do 5 wygranych jednej ze stron; po każdej rundzie wypisz wynik
- [ ] Historia sum gracza zapisywana w liście `List<int>`; na końcu wypisz ją i średnią
- [ ] Rzut kośćmi w osobnej metodzie

## Zadanie 4: Szyfr Cezara

- [ ] Program wczytuje tekst i przesunięcie (liczba całkowita)
- [ ] Każda litera alfabetu łacińskiego (wielka i mała) jest przesuwana o podaną liczbę pozycji z „zawinięciem” (`z` + 1 → `a`); pozostałe znaki bez zmian
- [ ] Metoda `Szyfruj(string tekst, int przesuniecie)` zwraca nowy tekst
- [ ] Odszyfrowanie to szyfrowanie z przesunięciem ujemnym — sprawdź to testem

## Zadanie 5: Python → C#

Przepisz poniższy program z Pythona na C#, zachowując jego działanie.

```python
def czy_palindrom(tekst):
    tekst = tekst.lower().replace(" ", "")
    return tekst == tekst[::-1]

slowa = []
while True:
    s = input("Słowo (puste kończy): ")
    if s == "":
        break
    slowa.append(s)

for s in slowa:
    wynik = "palindrom" if czy_palindrom(s) else "nie"
    print(f"{s}: {wynik}")
```

- [ ] Program w C# daje te same wyniki dla tych samych danych
- [ ] Komentarz: trzy miejsca, w których zapis w C# różni się najbardziej od Pythona, i dlaczego
