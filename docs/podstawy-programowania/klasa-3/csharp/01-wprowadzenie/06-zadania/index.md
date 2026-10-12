# Rozgrzewka i zadania

## Rozgrzewka: co zrobi ten kod?

Nie uruchamiaj kodu. Zapisz w zeszycie przewidywany wynik każdej linii albo słowo „błąd” i zaznacz, czy błąd wykryje kompilator, czy pojawi się w trakcie działania. Dopiero potem sprawdź.

??? question "Fragment 1: dzielenie i reszta"
    ```csharp
    Console.WriteLine(7 / 2 * 2.0);
    Console.WriteLine(-7 / 2);
    Console.WriteLine(-7 % 3);
    ```

    Co wypisze każda linia? Co decyduje o typie wyniku w pierwszej linii?

??? question "Fragment 2: char, int i string"
    ```csharp
    char c = 'A';
    c++;
    int n = 'A' + 1;
    string s = "A" + 1;

    Console.WriteLine(c);
    Console.WriteLine(n);
    Console.WriteLine(s);
    ```

    Czy któraś z czterech pierwszych linii zgłosi błąd? Co wypiszą trzy ostatnie? Czym w C# jest `'A'`, a czym `"A"`?

??? question "Fragment 3: tekst, ułamki i granice liczb"
    ```csharp
    string a = "kot";
    a.ToUpper();
    Console.WriteLine(a);

    double x = 0.1 + 0.2;
    Console.WriteLine(x == 0.3);

    int m = int.MaxValue;
    m++;
    Console.WriteLine(m);
    ```

    Co wypiszą trzy wywołania `WriteLine`? Czy któraś linia zgłosi błąd? Dlaczego `a` się nie zmieniło?

??? question "Fragment 4: gdzie żyje zmienna?"
    ```csharp
    for (int i = 0; i < 3; i++)
    {
        int kwadrat = i * i;
    }

    Console.WriteLine(kwadrat);
    Console.WriteLine(i);
    ```

    Czy ten kod się skompiluje? Jak go naprawić, żeby wypisać ostatni kwadrat?

## Zadania podsumowujące wprowadzenie

??? question "Zadanie 1: Mapa pojęć"
    Narysuj schemat (na kartce lub w dowolnym narzędziu), na którym połączysz strzałkami i podpiszesz: plik `.cs`, Roslyn, CIL, metadane, zestaw `.dll`, CLR, JIT, garbage collector, BCL, kod maszynowy, procesor. Przy każdej strzałce napisz, co się dzieje.

??? question "Zadanie 2: Projekt od zera w terminalu"
    Bez używania przycisków edytora, tylko poleceniami `dotnet`: utwórz projekt `Info`, który wypisuje wersję .NET (`Environment.Version`), nazwę systemu (`Environment.OSVersion`), nazwę komputera (`Environment.MachineName`) i bieżący katalog (`Environment.CurrentDirectory`). Zbuduj go w trybie Release i uruchom zbudowany plik `.dll`.

??? question "Zadanie 3: Tłumacz błędów"
    Przygotuj tabelę pięciu błędów kompilacji, które samodzielnie wywołasz w programie. Dla każdego zapisz: kod (np. `CS0103`), oryginalny komunikat, wyjaśnienie po polsku i sposób naprawy.

??? question "Zadanie 4: Argumenty"
    Napisz program `Kalkulator`, uruchamiany np. poleceniem `dotnet run -- 12 + 30`, który wypisuje wynik działania podanego w argumentach. Na razie obsłuż tylko dodawanie i sprawdź, czy podano dokładnie trzy argumenty.

??? question "Zadanie 5: Prezentacja technologii"
    Wybierz jedną technologię .NET (ASP.NET Core, Unity, .NET MAUI, WPF) i przygotuj krótką notatkę: do czego służy, jak wygląda w niej najprostszy program, jakie znane aplikacje z niej korzystają. Podaj źródła.
