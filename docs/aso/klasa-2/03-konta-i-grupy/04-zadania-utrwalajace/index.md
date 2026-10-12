# Zadania utrwalające

Zadania podsumowujące dział „Konta i grupy lokalne”. Wykonujesz je na maszynie wirtualnej z Windows i dokumentujesz zrzutami ekranu.

!!! tip "Snapshot przed startem"
    Przed rozpoczęciem zrób snapshot maszyny. Zmiany w prawach użytkownika mogą zablokować logowanie — snapshot pozwoli szybko wrócić.

## Zadanie 1: Firma w trzech działach

| Konto | Pełna nazwa | Grupa | Uwagi |
|---|---|---|---|
| `anowak` | Anna Nowak | `Kadry` | zmiana hasła przy pierwszym logowaniu |
| `bkowal` | Bartosz Kowal | `Kadry` | — |
| `cwisniewska` | Celina Wiśniewska | `Ksiegowosc` | hasło nie wygasa |
| `dlis` | Dawid Lis | `Ksiegowosc` | konto wyłączone |
| `ezajac` | Ewa Zając | `IT` | członek także grupy `Administrators` |

- [ ] Grupy `Kadry` i `Ksiegowosc` utworzone w GUI, grupa `IT` w PowerShell
- [ ] Konta z działu kadr utworzone w GUI, z księgowości poleceniem `net user`, z IT w PowerShell
- [ ] Zrzut ekranu: lista kont w `lusrmgr.msc`
- [ ] Zrzut ekranu: członkowie każdej grupy

## Zadanie 2: Profile

- [ ] Zaloguj się kolejno na `anowak` i `bkowal`, zmień każdemu tło pulpitu
- [ ] Zrzut ekranu: lista profili w `sysdm.cpl` z rozmiarami
- [ ] Usuń profil `bkowal` prawidłowym sposobem i pokaż, że konto nadal istnieje
- [ ] Komentarz: co straci użytkownik po usunięciu profilu, a czego nie

## Zadanie 3: Prawa

- [ ] Grupa `IT` ma prawo **Change the system time**
- [ ] Konto `dlis` ma zakaz logowania lokalnego (nawet po włączeniu konta)
- [ ] Zrzut ekranu: oba prawa w `secpol.msc`
- [ ] Zrzut ekranu: próba logowania na `dlis` po włączeniu konta

## Zadanie 4: Przydziały

- [ ] Wolumin `E:` z włączonymi przydziałami: limit 200 MB, ostrzeżenie 150 MB, blokowanie zapisu
- [ ] Indywidualny limit 500 MB dla `cwisniewska`
- [ ] Test na koncie `anowak`: próba zapisania ponad 200 MB
- [ ] Zrzut ekranu: **Quota Entries** z zajętością użytkowników

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. Konto a profil"
    Czym różni się konto użytkownika od jego profilu? Kiedy powstaje profil?

??? question "2. Grupy"
    Dlaczego uprawnienia nadajemy grupom, a nie pojedynczym użytkownikom?

??? question "3. Profil obowiązkowy"
    Czym profil obowiązkowy różni się od lokalnego? Gdzie się go stosuje?

??? question "4. Prawo a uprawnienie"
    Podaj przykład prawa użytkownika i przykład uprawnienia. Gdzie konfiguruje się każde z nich?

??? question "5. Przydział"
    Czy przydział dyskowy można ustawić dla jednego folderu? Uzasadnij.
