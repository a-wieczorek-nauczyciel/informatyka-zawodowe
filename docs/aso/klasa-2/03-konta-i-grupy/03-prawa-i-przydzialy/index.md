# Prawa, przywileje i przydziały dyskowe

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć odróżnić prawa użytkownika od uprawnień do plików, nadać lub odebrać przywilej w zasadach zabezpieczeń lokalnych oraz włączyć i skonfigurować przydziały dyskowe dla użytkowników.

## Prawa a uprawnienia

| | Prawa (przywileje) użytkownika | Uprawnienia (permissions) |
|---|---|---|
| dotyczą | **czynności w systemie** | **konkretnego obiektu**: pliku, folderu, drukarki |
| przykład | zmiana czasu systemowego, wyłączenie komputera, logowanie lokalne | odczyt folderu `C:\Kadry`, zapis do pliku |
| gdzie | `secpol.msc` → **User Rights Assignment** | właściwości obiektu → karta **Security** |

Uprawnienia do plików i folderów omawiamy w dziale [Zabezpieczanie zasobów](../../04-zabezpieczanie-zasobow/index.md).

## User Rights Assignment

++win+r++ → `secpol.msc` → **Local Policies → User Rights Assignment (Przypisywanie praw użytkownika)**.

Wybrane prawa:

| Prawo | Znaczenie |
|---|---|
| **Allow log on locally** | kto może zalogować się przy klawiaturze komputera |
| **Deny log on locally** | kto **nie może** zalogować się lokalnie (ma pierwszeństwo przed *Allow*) |
| **Allow log on through Remote Desktop Services** | kto może logować się zdalnie |
| **Change the system time** | kto może zmienić godzinę systemową |
| **Change the time zone** | kto może zmienić strefę czasową |
| **Shut down the system** | kto może wyłączyć komputer |
| **Back up files and directories** | kto może kopiować pliki niezależnie od uprawnień |
| **Take ownership of files or other objects** | kto może przejąć własność plików |

Jak zmienić: dwuklik na prawie → **Add User or Group…** → wpisz nazwę konta lub grupy → **OK**. Usunięcie: zaznacz wpis → **Remove**.

!!! tip "Nadawaj prawa grupom"
    Tak jak przy uprawnieniach: zamiast dopisywać pojedyncze konta, dodaj prawo grupie (np. `Kadry`) i zarządzaj członkostwem w grupie.

!!! warning "Ostrożnie z odbieraniem praw"
    Usunięcie grupy **Administrators** z prawa **Allow log on locally** może zablokować logowanie na wszystkie konta administratora. Przed zmianami w `secpol.msc` zrób snapshot maszyny.

Zmiany zwykle zaczynają działać po ponownym zalogowaniu użytkownika. Wymuszenie odświeżenia zasad:

```powershell
gpupdate /force
```

## Przydziały dyskowe (quota)

**Przydział dyskowy** ogranicza, ile miejsca na danym woluminie NTFS może zająć jeden użytkownik. Przydają się wszędzie tam, gdzie wielu użytkowników zapisuje pliki na jednym dysku — np. pracownia albo serwer plików.

Pojęcia:

- **limit (Limit disk space to)** — ile miejsca użytkownik może zająć,
- **poziom ostrzeżenia (Set warning level to)** — od jakiej zajętości system zapisuje ostrzeżenie,
- **odmawianie miejsca (Deny disk space to users exceeding quota limit)** — czy po przekroczeniu limitu zapis jest blokowany, czy tylko odnotowywany.

Przydział liczy się na podstawie **właściciela pliku** i dotyczy **całego woluminu**, a nie jednego folderu.

### Konfiguracja

1. W Eksploratorze: prawy przycisk na dysku (np. `E:`) → **Properties** → karta **Quota (Przydział)** → **Show Quota Settings**.
2. Zaznacz **Enable quota management**.
3. Zaznacz **Deny disk space to users exceeding quota limit**, jeśli limit ma blokować zapis.
4. Ustaw domyślne wartości dla nowych użytkowników: **Limit disk space to** (np. 500 MB) i **Set warning level to** (np. 400 MB).
5. **Quota Entries…** — limity dla konkretnych użytkowników: **Quota → New Quota Entry…**
6. **Apply**. System przeskanuje dysk i policzy zajętość każdego użytkownika.

!!! note "Administratorzy bez limitu"
    Członkowie grupy Administrators nie są blokowani przez przydziały. Test wykonuj na koncie użytkownika standardowego.

Z wiersza poleceń:

```powershell
fsutil quota query E:            # stan przydziałów na woluminie
```

## Zadania

??? question "Zadanie 1: Prawo zmiany czasu"
    Zaloguj się jako użytkownik standardowy i spróbuj zmienić godzinę systemową. Następnie jako administrator nadaj grupie `Kadry` prawo **Change the system time** i powtórz próbę na koncie z tej grupy. Udokumentuj wynik.

??? question "Zadanie 2: Zakaz logowania"
    Odbierz kontu `pzielinski` możliwość logowania lokalnego, nie usuwając go z grupy `Users`. Którego prawa użyłeś i dlaczego zadziałało mimo członkostwa w `Users`? Przywróć ustawienia.

??? question "Zadanie 3: Prawa a uprawnienia"
    Dla każdej czynności określ, czy wymaga prawa, czy uprawnienia: odczyt pliku `plan.docx`, wyłączenie komputera, zapis w folderze `C:\Kadry`, logowanie przez pulpit zdalny, usunięcie drukarki.

??? question "Zadanie 4: Przydział dyskowy"
    Dodaj maszynie wirtualnej drugi dysk (1 GB), utwórz na nim wolumin NTFS `E:`. Włącz przydziały: limit 100 MB, ostrzeżenie 80 MB, blokowanie po przekroczeniu. Zaloguj się jako użytkownik standardowy i spróbuj skopiować na `E:` pliki o łącznym rozmiarze ponad 100 MB. Udokumentuj komunikat i wpis w **Quota Entries**.

??? question "Zadanie 5: Indywidualny limit"
    Ustaw dla konta `anowak` limit 300 MB, a pozostałym użytkownikom zostaw limit domyślny. Pokaż oba wpisy w **Quota Entries**.
