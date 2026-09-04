# Zadania utrwalające

Zadania podsumowujące cały dział "Funkcje. Funkcje biblioteczne". Każde z nich wymaga napisania **co najmniej dwóch–trzech współpracujących ze sobą funkcji** — to jest sedno tego zestawu, nie pojedyncza funkcja na krzyż.

## Poziom podstawowy

??? question "1. Konwerter jednostek"
    Napisz program z funkcjami:

    - `km_na_mile(km)` — przelicza kilometry na mile
    - `kg_na_funty(kg)` — przelicza kilogramy na funty
    - `main()` — pyta użytkownika, co chce przeliczyć, i wywołuje odpowiednią funkcję

??? question "2. Sprawdzanie ocen"
    Napisz program z funkcjami:

    - `oblicz_srednia(oceny)` — przyjmuje listę ocen i zwraca średnią
    - `czy_zdal(srednia)` — zwraca `True`, jeśli średnia jest co najmniej 3.0
    - `main()` — wczytuje kilka ocen, oblicza średnią i wypisuje, czy uczeń zdał

## Poziom średni

??? question "3. Quiz matematyczny"
    Napisz program-quiz, który:

    1. Ma funkcję `wylosuj_dzialanie()`, korzystającą z modułu `random`, żeby wylosować dwie liczby i jedno działanie (+, -, ×)
    2. Ma funkcję `sprawdz_odpowiedz(poprawna, odpowiedz_gracza)`, zwracającą `True` lub `False`
    3. Ma funkcję `main()`, która trzykrotnie losuje działanie, pyta użytkownika o wynik i wypisuje, ile odpowiedzi było poprawnych

??? question "4. Generator haseł"
    Napisz program z funkcjami:

    - `wygeneruj_haslo(dlugosc)` — korzystając z modułu `random`, losuje hasło o zadanej długości ze zbioru liter i cyfr
    - `czy_silne_haslo(haslo)` — sprawdza, czy wygenerowane hasło ma co najmniej 8 znaków i zawiera cyfrę
    - `main()` — generuje hasła, aż trafi się jedno silne, i je wypisuje

??? question "5. Kalkulator statystyk"
    Napisz program z funkcjami:

    - `najmniejsza(liczby)`, `najwieksza(liczby)`, `srednia(liczby)` — każda przyjmuje listę liczb
    - `main()` — wczytuje od użytkownika kilka liczb, wywołuje wszystkie trzy funkcje i wypisuje wyniki w czytelnej formie

## Poziom trudniejszy

??? question "6. Prosta gra w zgadywanie liczby"
    Napisz program z funkcjami:

    - `wylosuj_liczbe(min, max)` — korzystając z `random`, losuje liczbę z zadanego zakresu
    - `podpowiedz(liczba_szukana, zgadywana)` — zwraca `"za mało"`, `"za dużo"` albo `"trafiono"`
    - `main()` — losuje liczbę od 1 do 100, pozwala użytkownikowi zgadywać, aż trafi, licząc liczbę prób

??? question "7. Analiza tekstu"
    Napisz program z funkcjami:

    - `policz_slowa(tekst)` — zwraca liczbę słów w tekście
    - `policz_samogloski(tekst)` — zwraca liczbę samogłosek
    - `najdluzsze_slowo(tekst)` — zwraca najdłuższe słowo w tekście
    - `main()` — wczytuje zdanie od użytkownika i wypisuje wszystkie trzy statystyki

??? question "8. Mini-system rezerwacji"
    Napisz program z funkcjami:

    - `dodaj_rezerwacje(lista, imie, godzina)` — dodaje nową rezerwację do listy
    - `czy_wolny_termin(lista, godzina)` — sprawdza, czy dana godzina jest jeszcze wolna
    - `main()` — pozwala dodać kilka rezerwacji, odrzucając próbę dodania zajętego terminu

!!! tip "Wskazówka"
    Jeśli utkniesz przy którymś zadaniu, spróbuj najpierw rozpisać na kartce, jakich funkcji potrzebujesz i co każda z nich ma przyjmować i zwracać — zanim zaczniesz pisać kod.
