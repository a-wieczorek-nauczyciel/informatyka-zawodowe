# Personalizacja

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć skonfigurować ustawienia personalne systemu według podanej specyfikacji: wygląd, pasek zadań i menu Start, język, region, datę i godzinę oraz domyślne aplikacje.

## Dlaczego administrator zajmuje się wyglądem

Na egzaminie zawodowym i w pracy często dostajesz specyfikację w rodzaju: „ustaw tło w kolorze …, przypnij do paska zadań …, ustaw format daty …”. Liczy się **dokładność** — zrobienie dokładnie tego, co zapisano, i umiejętność szybkiego znalezienia właściwej opcji.

## Gdzie co jest

| Co ustawiasz | Gdzie (Windows 11) |
|---|---|
| tło, kolory, motyw, tryb ciemny | **Settings → Personalization → Background / Colors / Themes** |
| ekran blokady | **Settings → Personalization → Lock screen** |
| menu Start, przypięte aplikacje | **Settings → Personalization → Start** |
| pasek zadań: ikony, wyrównanie, ukrywanie | **Settings → Personalization → Taskbar** |
| język systemu i klawiatury | **Settings → Time & language → Language & region** |
| strefa czasowa, data i godzina, format daty | **Settings → Time & language → Date & time** / **Language & region → Regional format** |
| rozdzielczość, skalowanie | **Settings → System → Display** |
| domyślna przeglądarka, odtwarzacz | **Settings → Apps → Default apps** |
| ułatwienia dostępu: rozmiar tekstu, kontrast | **Settings → Accessibility** |
| wygaszacz ekranu | **Settings → Personalization → Lock screen → Screen saver** |
| ikony pulpitu (Ten komputer, Kosz) | **Settings → Personalization → Themes → Desktop icon settings** |

!!! warning "Personalizacja wymaga aktywacji"
    W nieaktywowanym Windows część opcji personalizacji jest zablokowana. W maszynie wirtualnej możesz ominąć to np. przez **Control Panel** albo zmieniając tło prawym przyciskiem na pliku obrazu → **Set as desktop background**.

## Skróty, które przyspieszają pracę

| Skrót | Otwiera |
|---|---|
| ++win+i++ | Settings |
| ++win+r++ → `control` | klasyczny Control Panel |
| ++win+r++ → `desk.cpl` | ustawienia ekranu |
| ++win+r++ → `intl.cpl` | region, formaty daty i liczb |
| ++win+r++ → `timedate.cpl` | data i godzina |
| ++win+r++ → `sysdm.cpl` | właściwości systemu: nazwa komputera, profile, zmienne środowiskowe |

## Część ustawień przez PowerShell

```powershell
Get-TimeZone                                         # bieżąca strefa czasowa
Set-TimeZone -Id "Central European Standard Time"    # strefa dla Polski
Get-Culture                                          # format regionalny
Set-Culture pl-PL                                    # polski format daty i liczb (po ponownym zalogowaniu)
Rename-Computer -NewName "PC-SALA-12" -Restart       # nazwa komputera
```

## Zadania

??? question "Zadanie 1: Specyfikacja stanowiska"
    Skonfiguruj maszynę wirtualną według specyfikacji:

    - tryb ciemny, kolor akcentu zielony,
    - pasek zadań wyrównany do lewej, ukryte pole wyszukiwania,
    - przypięte do paska zadań: Eksplorator plików, Notatnik, Windows PowerShell,
    - na pulpicie ikony „Ten komputer” i „Kosz”,
    - wygaszacz ekranu „Puste” po 5 minutach z ekranem logowania po wznowieniu.

    Udokumentuj każdy punkt zrzutem ekranu.

??? question "Zadanie 2: Region i czas"
    Ustaw strefę czasową dla Polski, polski format regionalny oraz format daty krótkiej `dd.MM.yyyy`. Dodaj drugi układ klawiatury (angielski US) i sprawdź przełączanie skrótem ++win+space++.

??? question "Zadanie 3: Znajdź bez szukania"
    Bez używania wyszukiwarki w menu Start otwórz kolejno: ustawienia rozdzielczości ekranu, formaty regionalne i właściwości systemu. Zapisz, jakiego skrótu użyłeś w każdym przypadku.

??? question "Zadanie 4: Nazwa komputera"
    Zmień nazwę komputera na `PC-<twoje inicjały>-01` w PowerShellu. Po restarcie sprawdź nazwę dwoma różnymi sposobami.
