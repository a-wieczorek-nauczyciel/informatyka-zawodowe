# Projekt i narzędzia

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć utworzyć, zbudować i uruchomić projekt konsolowy poleceniem `dotnet`, odczytać zawartość pliku `.csproj`, wyjaśnić przeznaczenie folderów `bin` i `obj`, dodać pakiet NuGet oraz korzystać z debugera: punktów przerwania, wykonywania krok po kroku i podglądu zmiennych.

## Polecenie dotnet

Wszystko, co robi środowisko programistyczne, da się zrobić w terminalu poleceniem `dotnet`. Ta sama wiedza przyda się w Visual Studio, VS Code, Codespaces i na serwerze bez interfejsu graficznego.

| Polecenie | Co robi |
|---|---|
| `dotnet new list` | lista dostępnych szablonów projektów |
| `dotnet new console -n Kalkulator` | nowy projekt konsolowy w folderze `Kalkulator` |
| `dotnet build` | kompiluje projekt (wynik w `bin/Debug/...`) |
| `dotnet run` | kompiluje (jeśli trzeba) i uruchamia |
| `dotnet watch run` | uruchamia i automatycznie restartuje po każdym zapisie pliku |
| `dotnet clean` | usuwa wyniki kompilacji |
| `dotnet add package Nazwa` | dodaje pakiet NuGet |
| `dotnet publish -c Release` | przygotowuje wersję do przekazania użytkownikom |

```bash
dotnet new console -n Kalkulator
cd Kalkulator
dotnet run
```

## Szablony projektów

| Szablon | Co tworzy |
|---|---|
| `console` | aplikacja konsolowa — z niej korzystamy |
| `classlib` | biblioteka klas (`.dll` do użycia w innych projektach) |
| `web`, `webapi`, `mvc` | aplikacje internetowe ASP.NET Core |
| `wpf`, `winforms` | aplikacje okienkowe dla Windows |
| `xunit`, `nunit` | projekt testów jednostkowych |
| `sln` | plik rozwiązania łączący kilka projektów |

## Co jest w folderze projektu

```text
Kalkulator/
├── Kalkulator.csproj    ustawienia projektu
├── Program.cs           kod programu
├── bin/                 wyniki kompilacji (powstaje po dotnet build)
│   └── Debug/
│       └── net10.0/
│           ├── Kalkulator.dll                 zestaw z kodem CIL
│           ├── Kalkulator.exe                 plik uruchamiający (Windows)
│           ├── Kalkulator.pdb                 informacje dla debugera
│           ├── Kalkulator.runtimeconfig.json  wymagana wersja .NET
│           └── Kalkulator.deps.json           zależności
└── obj/                 pliki pośrednie kompilatora i pobrane informacje o pakietach
```

!!! tip "Czego nie wysyłać do repozytorium"
    Foldery `bin` i `obj` powstają przy każdej kompilacji, więc nie dodaje się ich do Gita. Plik `.gitignore` dla .NET utworzysz poleceniem `dotnet new gitignore`.

## Plik projektu .csproj

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

| Ustawienie | Znaczenie |
|---|---|
| `Sdk="Microsoft.NET.Sdk"` | rodzaj projektu i zestaw reguł budowania |
| `OutputType` = `Exe` | wynikiem jest program do uruchomienia (`Library` = biblioteka) |
| `TargetFramework` | wersja .NET, dla której budujemy (`net8.0`, `net10.0` …) |
| `ImplicitUsings` | automatycznie dołącza najczęstsze przestrzenie nazw (`System`, `System.IO`, `System.Collections.Generic`, `System.Linq` i inne) |
| `Nullable` | włącza ostrzeżenia, gdy zmienna może zawierać `null`, a używamy jej bez sprawdzenia |

Po dodaniu pakietu NuGet w pliku pojawi się sekcja:

```xml
<ItemGroup>
  <PackageReference Include="Spectre.Console" Version="0.49.1" />
</ItemGroup>
```

## Rozwiązanie z kilkoma projektami

Większy program dzieli się na kilka projektów połączonych w **rozwiązanie** (*solution*, plik `.sln` lub `.slnx`), np. aplikacja konsolowa + biblioteka z logiką + projekt testów.

```bash
dotnet new sln -n Sklep
dotnet new console -n Sklep.App
dotnet new classlib -n Sklep.Logika
dotnet sln add Sklep.App/Sklep.App.csproj Sklep.Logika/Sklep.Logika.csproj
dotnet add Sklep.App/Sklep.App.csproj reference Sklep.Logika/Sklep.Logika.csproj
```

## Środowiska programistyczne

| Narzędzie | Opis |
|---|---|
| **Visual Studio** (Windows) | pełne IDE Microsoftu: projektant okien, debuger, profiler; wersja Community bezpłatna dla uczniów |
| **Visual Studio Code** + rozszerzenie **C# Dev Kit** | lekki edytor na każdy system; korzysta z polecenia `dotnet` |
| **JetBrains Rider** | IDE dla .NET na każdy system, bezpłatne do nauki |
| **GitHub Codespaces** | VS Code w przeglądarce z gotowym .NET SDK — tego używamy na zajęciach |

Wszystkie korzystają z tego samego SDK. Projekt utworzony w jednym narzędziu otworzysz w każdym innym.

## Debugowanie

**Debuger** pozwala zatrzymać program w wybranym miejscu i podejrzeć wartości zmiennych.

| Czynność | VS Code / Visual Studio |
|---|---|
| punkt przerwania (*breakpoint*) | kliknięcie na lewym marginesie obok numeru linii albo ++f9++ |
| uruchomienie z debugerem | ++f5++ |
| następna linia, bez wchodzenia do metody (*Step Over*) | ++f10++ |
| wejście do wywoływanej metody (*Step Into*) | ++f11++ |
| wyjście z metody (*Step Out*) | ++shift+f11++ |
| kontynuacja do następnego punktu przerwania | ++f5++ |
| zakończenie | ++shift+f5++ |

Podczas zatrzymania panel **Variables / Locals** pokazuje wartości zmiennych, a w **Watch** możesz wpisać dowolne wyrażenie, np. `suma / licznik`.

!!! tip "Debuger zamiast Console.WriteLine"
    Dopisywanie `Console.WriteLine` w wielu miejscach, żeby zobaczyć wartości, działa, ale jest wolne. Jeden punkt przerwania pokazuje wszystkie zmienne naraz i pozwala iść linia po linii.

## Sprawdź się

??? question "1. Pierwszy projekt z terminala"
    Utwórz w terminalu projekt `Powitanie`, zbuduj go poleceniem `dotnet build`, a potem uruchom plik `.dll` bezpośrednio poleceniem `dotnet bin/Debug/<wersja>/Powitanie.dll`. Czym różni się to od `dotnet run`?

??? question "2. Plik .csproj"
    Otwórz plik `.csproj` swojego projektu. Wyjaśnij każde ustawienie. Zmień `Nullable` na `disable` i sprawdź, co stanie się z ostrzeżeniami przy `Console.ReadLine()`.

??? question "3. Foldery bin i obj"
    Usuń foldery `bin` i `obj`, a potem wykonaj `dotnet run`. Co się stało? Dlaczego tych folderów nie dodajemy do repozytorium?

??? question "4. Pakiet NuGet"
    Dodaj do projektu pakiet `Spectre.Console` i wypisz tekst w kolorze, korzystając z przykładu z dokumentacji pakietu. Pokaż, co zmieniło się w pliku `.csproj`.

??? question "5. Debuger"
    Napisz program, który w pętli sumuje liczby od 1 do 10. Ustaw punkt przerwania w pętli, uruchom debuger i zapisz wartość sumy po trzecim i po siódmym obrocie pętli, korzystając tylko z debugera.
