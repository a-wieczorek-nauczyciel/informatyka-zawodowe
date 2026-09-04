# Specyfikatory dostępu

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć zastosować konwencję ukrywania pól w Pythonie (`_`, `__`) oraz rozumieć, czym różni się to od prawdziwych specyfikatorów dostępu (`private`, `protected`, `public`) znanych z Javy czy C++.

## Po co w ogóle ograniczać dostęp do pól

Specyfikatory dostępu określają, **skąd** można odwołać się do pola lub metody klasy — czy tylko z wewnątrz samej klasy, czy też z zewnątrz (np. z innego pliku, przez obiekt). To narzędzie do wymuszania hermetyzacji, o której mówiliśmy w poprzednim dziale: chronisz dane obiektu przed niekontrolowaną, przypadkową zmianą z zewnątrz.

## Jak to wygląda w Javie — prawdziwe specyfikatory

Java ma trzy główne specyfikatory dostępu, wpisywane wprost przed nazwą pola lub metody:

```java
public class KontoBankowe {
    private double saldo;        // dostępne TYLKO wewnątrz tej klasy
    protected String wlasciciel; // dostępne w tej klasie i klasach pochodnych
    public String numerKonta;    // dostępne z każdego miejsca w programie

    public void wplac(double kwota) {
        if (kwota > 0) {
            saldo += kwota;
        }
    }
}
```

- `private` — pole/metoda widoczne **wyłącznie** wewnątrz tej samej klasy. Próba `konto.saldo` z zewnątrz **nie skompiluje się** — Java to blokuje na poziomie kompilatora.
- `protected` — widoczne w tej klasie i w klasach po niej dziedziczących.
- `public` — widoczne z każdego miejsca w programie, bez ograniczeń.

To jest **wymuszone przez sam język** — próba złamania tej zasady kończy się błędem kompilacji, zanim program w ogóle zacznie działać.

!!! info "W jakich jeszcze językach to występuje"
    Prawdziwe, wymuszone przez kompilator specyfikatory dostępu (`private`, `protected`, `public`) są standardem w **C++, C#, Javie** i wielu innych językach silnie obiektowych. To jeden z filarów hermetyzacji w tych językach.

## Jak to wygląda w Pythonie — konwencja, nie wymuszenie

!!! warning "Python nie ma prawdziwego private"
    W Pythonie **nie istnieje** mechanizm blokujący dostęp do pola z zewnątrz klasy tak, jak robi to Java. Zamiast tego jest **konwencja nazewnicza** — umowa między programistami, którą Python w większości przypadków tylko sygnalizuje, ale nie wymusza.

```python
class KontoBankowe:
    def __init__(self, wlasciciel, saldo):
        self.numer_konta = "PL123"      # brak podkreślnika = "publiczne"
        self._wlasciciel = wlasciciel   # jeden podkreślnik = "chronione" (konwencja)
        self.__saldo = saldo            # dwa podkreślniki = "prywatne" (utrudnione, nie zablokowane)

    def wplac(self, kwota):
        if kwota > 0:
            self.__saldo += kwota

    def pokaz_saldo(self):
        print(f"Saldo: {self.__saldo} zł")
```

- **brak podkreślnika** (`numer_konta`) — odpowiednik `public`. Dostęp z zewnątrz jest w pełni oczekiwany i normalny.
- **jeden podkreślnik** (`_wlasciciel`) — odpowiednik `protected`. To sygnał dla innych programistów: "nie ruszaj tego z zewnątrz, choć technicznie możesz". Python **niczego tutaj nie blokuje**.
- **dwa podkreślniki** (`__saldo`) — odpowiednik `private`. Python **utrudnia** (ale nie uniemożliwia całkowicie) dostęp z zewnątrz przez mechanizm zwany *name mangling* — w praktyce nazwa pola zostaje "zniekształcona" wewnętrznie, więc `konto.__saldo` z zewnątrz zgłosi błąd, ale wciąż istnieje sposób, żeby się do niego dostać, gdyby ktoś naprawdę chciał.

```python
konto = KontoBankowe("Kowalski", 1000)

print(konto.numer_konta)     # działa bez problemu — "publiczne"
print(konto._wlasciciel)     # zadziała! Python tego nie blokuje, tylko odradza
print(konto.__saldo)         # zgłosi błąd — nazwa została "zniekształcona"
```

!!! tip "Kluczowa różnica do zapamiętania"
    W Javie `private` to **twardy mur** stawiany przez kompilator. W Pythonie `__` to **znak ostrzegawczy**, którego można (choć nie powinno się) zignorować. To filozoficzna różnica między tymi językami: Python zakłada, że programiści są "dorośli" i będą przestrzegać konwencji, zamiast wymuszać to na poziomie języka.

??? question "Zadanie 1: Zastosuj konwencję"
    Zdefiniuj klasę `Uczen` z polem publicznym `imie` i polem chronionym konwencją `_oceny` (lista). Dodaj metodę `dodaj_ocene(ocena)`, która dopisuje ocenę do `_oceny`. Stwórz obiekt i pokaż, że `_oceny` da się odczytać z zewnątrz, mimo podkreślnika.

??? question "Zadanie 2: Zaobserwuj name mangling"
    Zdefiniuj klasę `Sejf` z polem `__kod` (dwa podkreślniki), ustawianym w konstruktorze. Spróbuj odczytać `sejf.__kod` z zewnątrz klasy — zapisz w komentarzu, jaki błąd otrzymujesz i dlaczego.

??? question "Zadanie 3: Porównaj Javę i Pythona"
    Spójrz na przykład klasy `KontoBankowe` w Javie z tej lekcji. Wyjaśnij własnymi słowami: co by się stało, gdyby ktoś w Javie spróbował napisać `konto.saldo = 1000000;` z zewnątrz klasy (przy `private double saldo`)? A co by się stało w analogicznej sytuacji w Pythonie, gdyby pole nazywało się `_saldo` (jeden podkreślnik)?

## Dlaczego mimo braku wymuszenia warto stosować konwencję

Nawet jeśli Python "pozwala" na złamanie zasady, konsekwentne trzymanie się konwencji (`_` dla wewnętrznych, brak podkreślnika dla publicznych) sprawia, że Twój kod jest **czytelny dla innych programistów** — od razu widać, które pola są "bezpieczne" do używania z zewnątrz, a które są szczegółem implementacji, mogącym się zmienić.

```python
class Silnik:
    def __init__(self, moc):
        self._moc = moc          # "nie ruszaj bezpośrednio"
        self.dziala = False      # "to jest bezpieczne do użycia z zewnątrz"

    def uruchom(self):
        self.dziala = True
        print(f"Silnik o mocy {self._moc} KM uruchomiony")
```

??? question "Zadanie 4: Popraw kod"
    Poniższy kod nie stosuje żadnej konwencji nazewniczej, mimo że jedno z pól powinno być traktowane jako wewnętrzne. Przepisz go, dodając odpowiedni podkreślnik tam, gdzie to sensowne, i uzasadnij wybór:
```python
    class Zamek:
        def __init__(self, haslo):
            self.haslo = haslo
            self.otwarty = False
```

??? question "Zadanie 5: Zaprojektuj klasę z trzema poziomami"
    Zdefiniuj klasę `Serwer` z trzema polami: `nazwa` (publiczne), `_status` (chronione konwencją), `__haslo_admina` (prywatne, dwa podkreślniki). Dodaj metodę `zmien_haslo(nowe_haslo)`, która jako jedyna może zmieniać `__haslo_admina`. Przetestuj dostęp do wszystkich trzech pól z zewnątrz klasy i zapisz, co zadziałało bez problemu, a co wymagało przejścia przez metodę.

## Zadania utrwalające

??? question "Zadanie 6: System logowania"
    Zdefiniuj klasę `Uzytkownik` z polami: `login` (publiczne) i `__haslo` (prywatne). Dodaj metodę `sprawdz_haslo(proba)`, zwracającą `True` lub `False` w zależności od tego, czy `proba` zgadza się z `__haslo`. Nie dodawaj żadnej metody pozwalającej bezpośrednio odczytać hasło z zewnątrz — pokaż, że sprawdzenie poprawności jest możliwe bez ujawniania samego hasła.

??? question "Zadanie 7: Klasa Termostat z walidacją przez hermetyzację"
    Zdefiniuj klasę `Termostat` z polem `_temperatura` (chronionym). Dodaj metodę `ustaw_temperature(wartosc)`, która zmienia `_temperatura` tylko, jeśli wartość mieści się w zakresie 10–30 stopni (w przeciwnym razie wypisz komunikat o błędzie i nie zmieniaj pola). Przetestuj z poprawną i niepoprawną wartością.

??? question "Zadanie 8: Napisz odpowiednik w Javie"
    Na podstawie klasy `Uzytkownik` z zadania 6, napisz (na kartce lub w pliku tekstowym, nie musisz uruchamiać) jej odpowiednik w Javie, używając prawdziwego specyfikatora `private` dla pola z hasłem. Porównaj, czym różni się to od podejścia w Pythonie — co Java daje "za darmo", czego Python wymaga jako świadomej dyscypliny programisty?
