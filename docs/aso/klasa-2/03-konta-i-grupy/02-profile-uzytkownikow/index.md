# Profile użytkowników

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć wyjaśnić, czym jest profil użytkownika i gdzie się znajduje, rozróżnić profil lokalny, mobilny, obowiązkowy i tymczasowy oraz sprawdzić, skopiować i usunąć profil.

## Konto a profil

**Konto** to tożsamość: nazwa, hasło, członkostwo w grupach. **Profil** to „mieszkanie” tego konta na dysku: pulpit, dokumenty, ustawienia programów, tło, skróty.

Profil powstaje przy **pierwszym logowaniu**. Dlatego nowo utworzone konto, na które nikt się jeszcze nie zalogował, nie ma folderu w `C:\Users`.

## Co jest w profilu

```text
C:\Users\anowak
├── Desktop          pulpit
├── Documents        dokumenty
├── Downloads        pobrane pliki
├── Pictures, Music, Videos
├── AppData          ustawienia programów (folder ukryty)
│   ├── Local        dane tylko dla tego komputera
│   └── Roaming      dane, które „podróżują” z profilem mobilnym
└── NTUSER.DAT       ustawienia użytkownika z rejestru (plik ukryty)
```

Nowy profil jest kopią profilu **Default** (`C:\Users\Default`, folder ukryty). Zmiana profilu Default zmienia wygląd startowy każdego nowego użytkownika.

!!! tip "Pokaż ukryte pliki"
    W Eksploratorze: **View → Show → Hidden items**. Bez tego nie zobaczysz `AppData`, `NTUSER.DAT` ani `C:\Users\Default`.

Zmienne środowiskowe wskazują na profil zalogowanego użytkownika:

```powershell
$env:USERPROFILE      # C:\Users\anowak
$env:APPDATA          # C:\Users\anowak\AppData\Roaming
$env:USERNAME         # anowak
```

## Rodzaje profili

| Profil | Gdzie jest | Cechy |
|---|---|---|
| **lokalny** | na dysku komputera, `C:\Users\nazwa` | domyślny; działa tylko na tym komputerze |
| **mobilny (roaming)** | na serwerze, kopiowany na komputer przy logowaniu | użytkownik ma ten sam pulpit i ustawienia na każdym komputerze w sieci |
| **obowiązkowy (mandatory)** | zwykle na serwerze; plik `NTUSER.MAN` zamiast `NTUSER.DAT` | zmiany użytkownika **nie są zapisywane** — po wylogowaniu wszystko wraca do stanu początkowego |
| **tymczasowy (temporary)** | tworzony, gdy właściwego profilu nie da się wczytać | komunikat *You've been signed in with a temporary profile*; zmiany przepadają |

Profil obowiązkowy sprawdza się w pracowni szkolnej i kafejce: każdy uczeń zaczyna od tego samego, czystego pulpitu. Profile mobilne i obowiązkowe konfiguruje się w domenie — wrócimy do nich w klasie 3.

## Zarządzanie profilami

**System Properties:** ++win+r++ → `sysdm.cpl` → karta **Advanced** → **User Profiles → Settings…**

Widać tu listę profili z rozmiarem, typem (*Local* / *Roaming*) i datą modyfikacji. Przyciski:

- **Delete** — usuwa profil (folder i wpisy w rejestrze). Konto zostaje, a przy następnym logowaniu powstanie nowy, czysty profil.
- **Change Type** — zmiana między lokalnym a mobilnym (gdy profil mobilny jest dostępny).
- **Copy To** — w nowszych Windows dostępne tylko dla profilu Default.

!!! warning "Nie usuwaj folderu profilu w Eksploratorze"
    Ręczne skasowanie `C:\Users\nazwa` zostawia w rejestrze wpis o profilu. Przy następnym logowaniu użytkownik dostanie **profil tymczasowy**. Profil usuwaj przez `sysdm.cpl`.

PowerShell:

```powershell
Get-CimInstance Win32_UserProfile | Select-Object LocalPath, Loaded, LastUseTime
```

## Zadania

??? question "Zadanie 1: Narodziny profilu"
    Utwórz konto `profil-test`. Sprawdź, czy w `C:\Users` jest jego folder. Zaloguj się na to konto, wyloguj i sprawdź ponownie. Wyjaśnij wynik.

??? question "Zadanie 2: Zawartość profilu"
    Włącz pokazywanie ukrytych plików. Wypisz foldery i pliki znajdujące się w profilu `profil-test`. Sprawdź rozmiar profilu w `sysdm.cpl`.

??? question "Zadanie 3: Zmiany w profilu"
    Zaloguj się na `profil-test`, zmień tło pulpitu, utwórz plik na pulpicie, przypnij aplikację do paska zadań. Wyloguj się i zaloguj ponownie. Które z tych zmian przetrwały i gdzie są zapisane?

??? question "Zadanie 4: Usunięcie profilu"
    Usuń profil `profil-test` przez `sysdm.cpl` (konto zostaw). Zaloguj się ponownie. Opisz, co się zmieniło. Wyjaśnij, czym usunięcie profilu różni się od usunięcia konta.

??? question "Zadanie 5: Profil dla pracowni"
    Szkoła chce, żeby w pracowni każdy uczeń po zalogowaniu miał zawsze ten sam pulpit, a zmiany po wylogowaniu znikały. Jaki rodzaj profilu wybierzesz? Jaki plik decyduje o tym zachowaniu?
