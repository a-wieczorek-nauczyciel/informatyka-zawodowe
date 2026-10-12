# Zasady zabezpieczeń lokalnych i zasady grup

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć skonfigurować zasady haseł i blokady konta, rozróżnić zasady zabezpieczeń lokalnych od lokalnych zasad grup, wprowadzić przykładowe ograniczenia dla użytkowników oraz sprawdzić, które zasady obowiązują.

## Dwa narzędzia

| Narzędzie | Skrót | Co zawiera |
|---|---|---|
| **Local Security Policy (Zasady zabezpieczeń lokalnych)** | `secpol.msc` | zasady haseł, blokady konta, prawa użytkowników, opcje zabezpieczeń |
| **Local Group Policy Editor (Edytor lokalnych zasad grupy)** | `gpedit.msc` | setki ustawień systemu i użytkowników; zawiera też wszystko z `secpol.msc` |

`secpol.msc` to w praktyce wycinek `gpedit.msc`: **Computer Configuration → Windows Settings → Security Settings**.

Oba narzędzia wymagają Windows Pro, Education lub Enterprise.

## Zasady haseł

`secpol.msc` → **Account Policies → Password Policy**:

| Zasada | Znaczenie | Przykład |
|---|---|---|
| **Minimum password length** | najmniejsza liczba znaków | 12 |
| **Password must meet complexity requirements** | hasło musi mieć znaki z trzech z czterech grup: wielkie litery, małe litery, cyfry, znaki specjalne; nie może zawierać nazwy konta | Enabled |
| **Maximum password age** | po ilu dniach hasło trzeba zmienić | 90 |
| **Minimum password age** | ile dni musi minąć przed kolejną zmianą | 1 |
| **Enforce password history** | ile poprzednich haseł system pamięta, żeby nie można było ich powtórzyć | 5 |

!!! tip "Po co minimalny wiek hasła"
    Bez niego użytkownik, który musi zmienić hasło, zmieni je pięć razy pod rząd i wróci do starego — historia haseł przestaje mieć sens.

## Blokada konta

`secpol.msc` → **Account Policies → Account Lockout Policy**:

| Zasada | Znaczenie | Przykład |
|---|---|---|
| **Account lockout threshold** | po ilu błędnych próbach logowania konto się blokuje | 5 |
| **Account lockout duration** | na ile minut | 15 |
| **Reset account lockout counter after** | po ilu minutach licznik błędnych prób się zeruje | 15 |

Blokada utrudnia odgadywanie hasła metodą prób i błędów.

Podgląd zasad haseł i blokady z wiersza poleceń:

```powershell
net accounts
net accounts /minpwlen:12 /maxpwage:90      # zmiana wybranych wartości
```

## Lokalne zasady grup

`gpedit.msc` ma dwie gałęzie:

- **Computer Configuration** — dotyczy komputera, niezależnie od tego, kto się loguje,
- **User Configuration** — dotyczy użytkowników logujących się na tym komputerze.

W każdej są **Administrative Templates** — tysiące gotowych ustawień. Przykłady:

| Cel | Ścieżka w `gpedit.msc` |
|---|---|
| zablokować Panel sterowania i Ustawienia | User Configuration → Administrative Templates → Control Panel → **Prohibit access to Control Panel and PC settings** |
| zablokować wiersz poleceń | User Configuration → Administrative Templates → System → **Prevent access to the command prompt** |
| zablokować edytor rejestru | User Configuration → Administrative Templates → System → **Prevent access to registry editing tools** |
| ukryć dysk w Eksploratorze | User Configuration → Administrative Templates → Windows Components → File Explorer → **Hide these specified drives in My Computer** |
| wyłączyć porty USB dla pamięci masowych | Computer Configuration → Administrative Templates → System → Removable Storage Access → **All Removable Storage classes: Deny all access** |

Dwuklik na ustawieniu → **Enabled / Disabled / Not Configured** → **OK**.

!!! warning "Lokalne zasady dotyczą wszystkich"
    Ustawienie w **User Configuration** w `gpedit.msc` obejmuje **wszystkich** użytkowników komputera, łącznie z administratorem. Zablokowanie Panelu sterowania zablokuje go także Tobie. Testuj na snapshocie.

## Odświeżanie i sprawdzanie zasad

```powershell
gpupdate /force          # zastosuj zasady od razu
gpresult /r              # jakie zasady obowiązują użytkownika i komputer
gpresult /h raport.html  # pełny raport w HTML
```

## Zadania

??? question "Zadanie 1: Polityka haseł"
    Ustaw: minimalna długość 10 znaków, wymóg złożoności, maksymalny wiek 60 dni, minimalny wiek 1 dzień, historia 5 haseł. Spróbuj ustawić kontu `anowak` hasło `anowak123` i `Kadry2026!`. Opisz wyniki.

??? question "Zadanie 2: Blokada konta"
    Ustaw blokadę po 3 błędnych próbach na 10 minut. Trzy razy wpisz błędne hasło na koncie `anowak`. Sprawdź w `lusrmgr.msc`, jak wygląda zablokowane konto, i odblokuj je jako administrator.

??? question "Zadanie 3: net accounts"
    Wyświetl zasady poleceniem `net accounts` i porównaj z ustawieniami w `secpol.msc`. Zmień minimalną długość hasła poleceniem `net accounts` i sprawdź zmianę w GUI.

??? question "Zadanie 4: Ograniczenia użytkownika"
    W `gpedit.msc` zablokuj dostęp do Panelu sterowania i wiersza poleceń. Zaloguj się na konto `anowak` i sprawdź działanie. Wyświetl obowiązujące zasady poleceniem `gpresult /r`. Przywróć ustawienia.

??? question "Zadanie 5: Wybierz ustawienie"
    Szkoła chce, żeby uczniowie nie mogli kopiować plików na pendrive'y. Znajdź w `gpedit.msc` ustawienie, które to realizuje, i opisz, w której gałęzi go szukałeś i dlaczego właśnie tam.
