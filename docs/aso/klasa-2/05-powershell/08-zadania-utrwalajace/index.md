# Zadania utrwalające

Zadania podsumowujące dział „PowerShell” i pliki wsadowe. Wykonujesz je na maszynie wirtualnej z Windows Server 2022, **wyłącznie w konsoli** (PowerShell lub `cmd`), bez okien konfiguracyjnych systemu. Każdy krok dokumentujesz zrzutem ekranu, na którym widać polecenie i jego wynik.

!!! tip "Snapshot przed startem"
    Przed rozpoczęciem zrób snapshot maszyny, np. `przed-powershell`. Jeśli coś pójdzie nie tak, wrócisz do czystego stanu bez reinstalacji systemu.

## Zadanie 1: Struktura folderów jedną linijką

- [ ] W `C:\PS_Lab\Firma` utwórz foldery `Dzial1` … `Dzial10` — **jednym poleceniem**, bez wpisywania każdej nazwy
- [ ] W każdym z nich utwórz pusty plik `notatki.txt` — również jednym poleceniem
- [ ] Policz poleceniem PowerShell, ile plików `notatki.txt` powstało
- [ ] Zrzut ekranu: wynik `Get-ChildItem C:\PS_Lab\Firma -Recurse`

## Zadanie 2: Konta działu handlowego

- [ ] Utworzona grupa lokalna `Handel` z opisem „Dział handlowy”
- [ ] Utworzone konta `mlis`, `kwrona`, `tsowa` z pełnymi imionami i nazwiskami
- [ ] Wszystkie trzy konta należą do grup `Handel` i `Users`
- [ ] Konto `tsowa` jest wyłączone (osoba zaczyna pracę za miesiąc)
- [ ] Zrzut ekranu: członkowie grupy `Handel`
- [ ] Zrzut ekranu: lista kont lokalnych z kolumnami `Name`, `Enabled`, `Description`

## Zadanie 3: Raport stanu systemu

Przygotuj plik `C:\PS_Lab\raport.txt`, w którym każda sekcja ma nagłówek:

- [ ] nazwa komputera i wersja PowerShella
- [ ] pięć procesów zużywających najwięcej pamięci, z pamięcią w MB (kolumna wyliczana)
- [ ] liczba usług w stanie `Running` i w stanie `Stopped` (`Group-Object`)
- [ ] wolne miejsce na każdym dysku w GB
- [ ] Zrzut ekranu: zawartość pliku wyświetlona poleceniem `Get-Content`

## Zadanie 4: Skrypt zakładający konta z pliku

Napisz skrypt `C:\PS_Lab\nowi.ps1`, który:

- [ ] wczytuje plik `pracownicy.txt` z loginami (jeden login w linii)
- [ ] dla każdego loginu sprawdza, czy konto już istnieje
- [ ] jeśli nie istnieje — tworzy je i wypisuje komunikat, jeśli istnieje — wypisuje ostrzeżenie (`Write-Warning`)
- [ ] na końcu wypisuje, ile kont utworzono
- [ ] Zrzut ekranu: dwa kolejne uruchomienia skryptu i różnica w wyniku

## Zadanie 5: Sprzątanie z zabezpieczeniem

- [ ] Przygotuj folder `C:\PS_Lab\Logi` z pięcioma plikami `.log`; trzem z nich ustaw datę modyfikacji sprzed 30 dni
- [ ] Napisz skrypt, który wyświetla pliki starsze niż 14 dni i usuwa je **z parametrem `-WhatIf`**
- [ ] Zrzut ekranu: wynik skryptu — komunikaty *What if* tylko dla trzech starych plików
- [ ] Usuń `-WhatIf` i uruchom skrypt naprawdę; pokaż, że zostały dwa pliki

## Zadanie 6: Strażnik usługi

- [ ] Napisz skrypt, który pyta użytkownika o nazwę usługi (`Read-Host`)
- [ ] Jeśli usługa nie istnieje — wypisuje czytelny komunikat
- [ ] Jeśli jest zatrzymana — uruchamia ją i potwierdza nowy stan
- [ ] Jeśli działa — tylko informuje
- [ ] Zrzuty ekranu dla wszystkich trzech przypadków (usługę `Spooler` zatrzymaj przed testem)

## Zadanie 7: Raport w pliku wsadowym

Napisz plik `raport.bat`, który:

- [ ] przyjmuje jako parametr nazwę pliku wynikowego; bez parametru zapisuje do `C:\PS_Lab\raport.txt`
- [ ] zapisuje do pliku: użytkownika, nazwę komputera, datę i godzinę, wynik `ipconfig` i listę kont (`net user`)
- [ ] na końcu wypisuje na ekran liczbę linii pliku (`find /c /v ""`)
- [ ] Zrzut ekranu: uruchomienie z parametrem i bez parametru

## Zadanie 8: Ten sam skrypt w .bat i .ps1

Napisz dwie wersje programu „przelicznik jednostek”: `przelicz.bat` i `przelicz.ps1`. Program pyta o rodzaj przeliczenia (1 — km na mile, 2 — kg na funty, 3 — °C na °F) i wartość, a następnie wypisuje wynik.

- [ ] w `.bat` wybór przez `goto` z etykietami, w `.ps1` — przez `switch`
- [ ] w `.bat` obliczenia przez `set /a` na liczbach całkowitych (np. wynik w setnych częściach), w `.ps1` — z zaokrągleniem do 2 miejsc
- [ ] wersja `.ps1` przyjmuje też parametry `-Rodzaj` i `-Wartosc` i wtedy nie zadaje pytań
- [ ] Komentarz: trzy różnice między wersjami, które najbardziej utrudniały pisanie wersji `.bat`

## Zadanie 9: Strażnik usług ze switch

Napisz `uslugi.ps1` z parametrem `-Akcja` (`ValidateSet`: `pokaz`, `uruchom`, `zatrzymaj`) i obowiązkowym `-Nazwa`.

- [ ] instrukcja `switch` wybiera działanie
- [ ] przed zatrzymaniem usługi skrypt pyta o potwierdzenie (`-Confirm`)
- [ ] dla nieistniejącej usługi wypisuje czytelny komunikat zamiast czerwonego błędu
- [ ] Zrzuty ekranu dla wszystkich trzech akcji na usłudze `Spooler`

