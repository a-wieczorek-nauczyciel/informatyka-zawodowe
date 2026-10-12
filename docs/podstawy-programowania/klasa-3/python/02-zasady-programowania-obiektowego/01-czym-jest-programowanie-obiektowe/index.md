# Czym jest programowanie obiektowe

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, czym różni się podejście obiektowe od proceduralnego, i rozpoznać sytuacje, w których programowanie obiektowe jest naturalnym wyborem.

## Podejście proceduralne — to, co już znasz

W dziale "Funkcje" pisałeś programy, w których **dane i funkcje istnieją osobno**. Dane trzymasz w zmiennych i listach, a funkcje operują na tych danych, gdy je do nich przekażesz.

```python
imie = "Marta"
wiek = 17
oceny = [4, 5, 3, 5]

def oblicz_srednia(lista_ocen):
    return sum(lista_ocen) / len(lista_ocen)

print(oblicz_srednia(oceny))
```

To działa dobrze dla jednego ucznia. Ale co, jeśli masz **trzydziestu** uczniów, każdy z imieniem, wiekiem i listą ocen? Musiałbyś trzymać osobne zmienne i listy dla każdego, albo skomplikowane struktury łączące je ręcznie — i za każdym razem pamiętać, które dane do kogo należą.

## Podejście obiektowe — dane i zachowania razem

Programowanie obiektowe (OOP — *object-oriented programming*) to inny sposób organizacji kodu: zamiast trzymać dane i funkcje osobno, **łączysz je razem w jedną całość zwaną obiektem**.

```python
class Uczen:
    def __init__(self, imie, wiek, oceny):
        self.imie = imie
        self.wiek = wiek
        self.oceny = oceny

    def oblicz_srednia(self):
        return sum(self.oceny) / len(self.oceny)

uczen1 = Uczen("Marta", 17, [4, 5, 3, 5])
print(uczen1.oblicz_srednia())
```

Teraz `imie`, `wiek`, `oceny` i metoda `oblicz_srednia` są **częścią tego samego obiektu** — `uczen1`. Chcesz drugiego ucznia? Tworzysz kolejny obiekt, a nie kolejny zestaw osobnych zmiennych.

!!! tip "Nie musisz jeszcze rozumieć każdej linijki tej składni"
    `class`, `__init__`, `self` — dokładnie to omówimy w kolejnym dziale, "Klasy i obiekty". Na razie skup się na **idei**: dane i zachowania (funkcje, które na nich operują) są teraz częścią jednej całości.

## Dlaczego to się przydaje

- **Porządek** — każdy obiekt "wie", jakie ma dane i co potrafi zrobić, bez rozgrzebywania kodu w wielu miejscach
- **Skalowanie** — dodanie stu kolejnych uczniów to sto obiektów tego samego typu, nie sto nowych zestawów zmiennych
- **Odwzorowanie rzeczywistości** — świat wokół nas naturalnie składa się z "rzeczy" mających cechy i zachowania (uczeń, samochód, konto bankowe) — obiekty pozwalają to odzwierciedlić wprost w kodzie

## Kiedy podejście proceduralne wystarczy

OOP nie zawsze jest potrzebne. Prosty skrypt liczący pole trójkąta nie potrzebuje klasy — funkcja w zupełności wystarczy. OOP zaczyna się opłacać, gdy w programie pojawia się **wiele podobnych "rzeczy"**, z których każda ma własne dane i zachowania — tak jak wielu uczniów, wiele kont bankowych, wiele pojazdów w systemie wypożyczalni.

!!! warning "Częste błędne przekonanie"
    OOP nie jest "lepsze" ani "bardziej zaawansowane" w sposób absolutny — to inne narzędzie do innych sytuacji. Doświadczeni programiści mieszają oba podejścia w jednym projekcie, dobierając to, co pasuje do konkretnego fragmentu problemu.

## Zadania wprowadzające

### Rozpoznaj podejście

??? question "1. Proceduralne czy obiektowe?"
```python
    imie_psa = "Reksio"
    rasa_psa = "Labrador"

    def przedstaw_psa(imie, rasa):
        print(f"{imie} to {rasa}")

    przedstaw_psa(imie_psa, rasa_psa)
```
    Czy ten kod jest napisany w podejściu proceduralnym, czy obiektowym? Po czym to poznajesz?

??? question "2. Proceduralne czy obiektowe?"
```python
    class Pies:
        def __init__(self, imie, rasa):
            self.imie = imie
            self.rasa = rasa

        def przedstaw_sie(self):
            print(f"{self.imie} to {self.rasa}")

    reksio = Pies("Reksio", "Labrador")
    reksio.przedstaw_sie()
```
    A ten? Co dokładnie się zmieniło względem poprzedniego przykładu?

### Zidentyfikuj kandydatów na obiekty

??? question "3. System biblioteczny"
    Wyobraź sobie program zarządzający biblioteką szkolną — śledzi książki i czytelników wypożyczających je. Jakie "rzeczy" w tym systemie nadawałyby się na obiekty (klasy)? Wypisz przynajmniej dwie i uzasadnij.

??? question "4. Sklep internetowy"
    Program obsługujący sklep internetowy musi śledzić produkty, zamówienia i klientów. Które z nich potraktowałbyś jako osobne klasy? Czy widzisz tu coś, co **nie** powinno być osobną klasą, tylko zwykłą zmienną?

### Rozróżnij cechy od zachowań

??? question "5. Cechy i zachowania samochodu"
    Pomyśl o samochodzie jako potencjalnym obiekcie w programie. Wypisz osobno: co byłoby jego **cechami** (danymi, polami) i co byłoby jego **zachowaniami** (metodami, czyli tym, co potrafi zrobić).

??? question "6. Cechy i zachowania konta bankowego"
    To samo dla konta bankowego — jakie dane (pola) i jakie działania (metody) miałby obiekt reprezentujący konto?

## Zadania podstawowe

??? question "7. Przepisz na słowa"
    Spójrz jeszcze raz na klasę `Uczen` z tej lekcji. Opisz **własnymi słowami** (bez kodu), co się dzieje w linijce `uczen1 = Uczen("Marta", 17, [4, 5, 3, 5])`.

??? question "8. Znajdź podobieństwo"
    Kod z tej lekcji tworzy obiekt `uczen1` na podstawie klasy `Uczen`. Gdybyś chciał dodać drugiego ucznia, `uczen2`, jak wyglądałaby ta linijka kodu? Nie musisz znać jeszcze pełnej składni klas — napisz, jak *przypuszczasz*, że mogłaby wyglądać, bazując na przykładzie `uczen1`.

??? question "9. Znajdź przykład z życia"
    Podaj własny przykład (inny niż uczeń, pies, samochód czy konto bankowe) sytuacji z codziennego życia, którą dobrze byłoby zamodelować jako klasę w programie. Wypisz przynajmniej dwie cechy i dwa zachowania takiego obiektu.

## Zadania średnio-trudne

??? question "10. Zaprojektuj klasę na papierze — Film"
    Nie pisząc kodu, zaprojektuj na kartce (albo w pliku tekstowym) klasę `Film`, która mogłaby być częścią programu do zarządzania domową kolekcją filmów. Wypisz: nazwę klasy, listę pól (danych) i listę metod (co obiekt potrafi zrobić), np. `wypozycz()`, `oceń()`.

??? question "11. Zaprojektuj klasę na papierze — Pracownik"
    To samo dla klasy `Pracownik` w programie kadrowym firmy. Pomyśl, jakie dane firma musiałaby przechowywać o pracowniku i jakie operacje mogłaby na nich wykonywać.

??? question "12. Znajdź błąd w projektowaniu"
    Kolega projektuje klasę `Zamowienie` w sklepie internetowym i umieszcza w niej pole `lista_wszystkich_produktow_w_sklepie`. Czy to dobry pomysł? Uzasadnij, czy to pole rzeczywiście "należy" do pojedynczego zamówienia, czy raczej do czegoś innego w systemie.

!!! tip "Nie ma tu jeszcze kodowania — i to celowe"
    Te zadania uczą **myślenia obiektowego**, zanim nauczysz się pełnej składni klas w Pythonie. To ważny krok — dobrze zaprojektowane klasy "na papierze" to fundament, na którym w kolejnym dziale nauczysz się pisać działający kod.
