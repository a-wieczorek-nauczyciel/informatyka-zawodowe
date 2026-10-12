# Czym jest C#

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć scharakteryzować język C#: kto i po co go stworzył, jakie ma najważniejsze cechy (obiektowość, statyczne i silne typowanie, kod zarządzany), do czego się go używa oraz jak rozwijał się w kolejnych wersjach.

## Krótka definicja

**C#** (czyt. *si szarp*) to **obiektowy, statycznie i silnie typowany język programowania ogólnego przeznaczenia**, stworzony przez firmę Microsoft. Programy w C# działają na platformie **.NET**, która dostarcza im środowisko uruchomieniowe i bogatą bibliotekę gotowych klas.

Nazwa nawiązuje do zapisu muzycznego: krzyżyk (♯) podwyższa dźwięk o pół tonu, czyli „C#” to „C o krok wyżej”. W kodzie i nazwach plików używa się zwykłego znaku `#`.

## Skąd się wziął

| Rok | Wydarzenie |
|---|---|
| 1999–2000 | w Microsofcie powstaje nowy język dla nowej platformy .NET; zespołem kieruje **Anders Hejlsberg** (wcześniej twórca Turbo Pascala i Delphi, później także TypeScriptu) |
| 2002 | **C# 1.0** razem z **.NET Framework 1.0** — pierwsze wydanie dla Windows |
| 2002–2003 | język zostaje ustandaryzowany: **ECMA-334** i **ISO/IEC 23270** |
| 2014–2016 | Microsoft otwiera kod kompilatora (**Roslyn**) i platformy; powstaje **.NET Core**, działający także na Linuksie i macOS |
| od 2020 | jedna, wieloplatformowa linia **.NET 5, 6, 7 …**; nowa wersja co roku w listopadzie |

C# był projektowany jako język, który łączy wygodę języków zarządzanych (automatyczne zarządzanie pamięcią, kontrola typów) ze składnią znaną programistom C, C++ i Javy.

## Najważniejsze cechy języka

### Obiektowy

Program w C# składa się z **typów** — przede wszystkim klas — które łączą dane (pola, właściwości) i działania na nich (metody). Nawet najprostszy program jest w rzeczywistości metodą w klasie, a typy proste, takie jak `int`, mają swoje metody (`5.ToString()`). Programowanie obiektowe omawiamy szczegółowo w klasie 4.

### Statycznie typowany

Każda zmienna, parametr i wynik metody ma **typ znany w czasie kompilacji**. Kompilator sprawdza, czy operacje są zgodne z typami, **zanim program zostanie uruchomiony**:

```csharp
int punkty = 10;
punkty = "dziesięć";   // błąd kompilacji CS0029: nie można przekonwertować string na int
```

Błąd zostaje wykryty, zanim program trafi do użytkownika.

### Silnie typowany

Język nie zamienia typów „po cichu”, jeśli grozi to utratą informacji. Konwersję z `double` na `int` trzeba zapisać jawnie, a tekstu na liczbę — wywołaniem odpowiedniej metody:

```csharp
double cena = 9.99;
int a = cena;          // błąd kompilacji: możliwa utrata danych
int b = (int)cena;     // jawne rzutowanie: 9
```

### Zarządzany (managed)

Program w C# nie zarządza pamięcią ręcznie. Obiekty tworzy się słowem `new`, a gdy przestają być potrzebne, pamięć zwalnia automatycznie **garbage collector** (odśmiecacz pamięci). Środowisko uruchomieniowe pilnuje też, żeby program nie odczytał pamięci spoza tablicy ani nie potraktował obiektu jako innego typu. Taki kod nazywamy **kodem zarządzanym**.

### Wieloparadygmatowy

Oprócz programowania obiektowego C# wspiera elementy programowania **funkcyjnego** (wyrażenia lambda, LINQ), **generycznego** (typy generyczne, np. `List<int>`), **asynchronicznego** (`async` / `await`) i programowania opartego na **zdarzeniach**.

### Wieloplatformowy

Ten sam program działa na Windows, Linuksie i macOS, na procesorach x64 i ARM. Warunkiem jest obecność środowiska .NET albo publikacja programu razem z nim.

## Do czego używa się C#

| Obszar | Technologia .NET | Przykład |
|---|---|---|
| aplikacje i usługi internetowe | **ASP.NET Core** | sklepy internetowe, systemy rezerwacji, interfejsy API |
| gry | silnik **Unity** (skrypty w C#), Godot | gry na komputery, konsole i telefony |
| aplikacje okienkowe | **WPF**, **Windows Forms**, **WinUI** | programy księgowe, narzędzia firmowe |
| aplikacje mobilne i wieloplatformowe | **.NET MAUI** | jedna aplikacja na Android, iOS, Windows, macOS |
| chmura | Azure Functions, kontenery | usługi przetwarzające dane |
| narzędzia i automatyzacja | aplikacje konsolowe | skrypty administracyjne, przetwarzanie plików |

C# jest jednym z najpopularniejszych języków w firmach, szczególnie przy dużych systemach biznesowych, gdzie liczy się kontrola typów, wydajność i długie utrzymanie kodu.

## Wersje języka

Język rozwija się razem z platformą. Każda wersja dodaje nowe konstrukcje, ale stary kod nadal działa.

| Wersja | Rok | Najważniejsze nowości |
|---|---|---|
| C# 1.0 | 2002 | klasy, interfejsy, właściwości, zdarzenia |
| C# 2.0 | 2005 | typy generyczne (`List<T>`), typy dopuszczające `null` (`int?`) |
| C# 3.0 | 2007 | `var`, wyrażenia lambda, LINQ |
| C# 5.0 | 2012 | `async` / `await` |
| C# 6.0 | 2015 | interpolacja tekstu `$"..."`, operator `?.` |
| C# 7.x | 2017 | krotki, dopasowanie wzorców, zmienne `out` w wywołaniu |
| C# 8.0 | 2019 | wyrażenie `switch`, referencje dopuszczające `null` |
| C# 9.0 | 2020 | instrukcje najwyższego poziomu (program bez `Main`), rekordy |
| C# 10–11 | 2021–2022 | `global using`, przestrzenie nazw na poziomie pliku, surowe literały tekstowe |
| C# 12 | 2023 | wyrażenia kolekcji (`[1, 2, 3]`), konstruktory podstawowe |
| C# 13–14 | 2024–2025 | kolejne usprawnienia razem z .NET 9 i .NET 10 |

Na zajęciach używamy aktualnej wersji .NET z długim wsparciem. W starszych podręcznikach i odpowiedziach z internetu zobaczysz dłuższe zapisy tych samych rzeczy — warto wiedzieć, że oba działają.

## C# na tle innych języków

| Cecha | C# | Java | C++ | Python |
|---|---|---|---|---|
| typowanie | statyczne | statyczne | statyczne | dynamiczne |
| zarządzanie pamięcią | automatyczne (GC) | automatyczne (GC) | ręczne / RAII | automatyczne |
| wykonanie | CIL + JIT | bytecode + JIT | kod maszynowy | bytecode + interpreter |
| typowe zastosowania | biznes, web, gry, desktop | biznes, Android, web | systemy, gry, sterowniki | skrypty, dane, AI, web |

Składnia C# jest bardzo podobna do Javy i C++: klamry, średniki, `if`, `for`, `while` wyglądają niemal tak samo. Kto zna C#, łatwo czyta kod w tych językach.

## Sprawdź się

??? question "1. Definicja"
    Wyjaśnij własnymi słowami, co znaczy, że C# jest językiem obiektowym, statycznie typowanym i zarządzanym.

??? question "2. Statyczne a silne typowanie"
    Czym różni się statyczne typowanie od silnego? Podaj po jednym przykładzie kodu, który ilustruje każdą z tych cech.

??? question "3. Garbage collector"
    Co robi garbage collector i jakiego rodzaju błędów pozwala uniknąć w porównaniu z ręcznym zarządzaniem pamięcią?

??? question "4. Zastosowania"
    Wybierz jedną z technologii z tabeli zastosowań i znajdź w internecie znany program lub grę, która z niej korzysta. Zapisz nazwę i źródło informacji.

??? question "5. Wersje"
    Do której wersji C# trzeba było pisać program z klasą i metodą `Main`, a od której można pisać kod bezpośrednio w pliku `Program.cs`?
