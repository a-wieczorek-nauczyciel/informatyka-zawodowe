# Zadania utrwalające

Zadania podsumowujące dział „Zabezpieczanie zasobów”. Wykonujesz je na dwóch maszynach wirtualnych w jednej sieci (serwer plików `PC01` i klient `PC02`) i dokumentujesz zrzutami ekranu.

!!! tip "Snapshot przed startem"
    Zrób snapshot obu maszyn. Zasady grup i odebrane uprawnienia łatwo przywrócić ze snapshotu, trudniej ręcznie.

## Zadanie 1: Struktura folderów firmy

Na `PC01` utwórz strukturę i uprawnienia NTFS:

| Folder | Kadry | Ksiegowosc | IT |
|---|---|---|---|
| `C:\Firma\Kadry` | Modify | — | Full control |
| `C:\Firma\Ksiegowosc` | — | Modify | Full control |
| `C:\Firma\Wspolne` | Modify | Modify | Full control |
| `C:\Firma\Wspolne\Regulaminy` | Read | Read | Full control |

- [ ] Grupa `Users` nie ma dostępu do folderów działów
- [ ] `Regulaminy` nie dziedziczy uprawnień z `Wspolne`
- [ ] Uprawnienia działu kadr i księgowości nadane w GUI, pozostałe poleceniem `icacls`
- [ ] Zrzut ekranu: wynik `icacls` dla każdego folderu
- [ ] Zrzut ekranu: **Effective Access** dla jednego konta z każdej grupy na `C:\Firma\Kadry`

## Zadanie 2: Udział sieciowy

- [ ] `C:\Firma` udostępniony jako `Firma` z uprawnieniem udziału *Authenticated Users: Change*
- [ ] `C:\Firma\Ksiegowosc` dodatkowo udostępniony jako udział ukryty `Ksiegowosc$`
- [ ] Na `PC02` zamapowany dysk `F:` → `\\PC01\Firma` jako użytkownik z grupy `Kadry`
- [ ] Zrzut ekranu z `PC02`: udana praca w `Kadry`, odmowa dostępu do `Ksiegowosc`, brak możliwości zapisu w `Regulaminy`
- [ ] Zrzut ekranu z `PC01`: aktywne sesje i otwarte pliki

## Zadanie 3: Kopia uprawnień

- [ ] Uprawnienia `C:\Firma` zapisane do pliku poleceniem `icacls /save`
- [ ] Celowo usunięty wpis grupy `Kadry` z `C:\Firma\Kadry`
- [ ] Uprawnienia odtworzone z pliku
- [ ] Zrzut ekranu: stan przed, po zepsuciu i po odtworzeniu

## Zadanie 4: Zasady zabezpieczeń

- [ ] Hasła: min. 10 znaków, złożoność, historia 5, maks. wiek 60 dni
- [ ] Blokada: 5 prób, 15 minut
- [ ] Dla wszystkich użytkowników zablokowany edytor rejestru
- [ ] Zrzut ekranu: wynik `net accounts` i `gpresult /r`

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. Deny"
    Użytkownik ma *Allow Modify* przez jedną grupę i *Deny Write* przez drugą. Co może zrobić z plikiem?

??? question "2. Udział i NTFS"
    Udział: *Everyone: Read*. NTFS: *Kadry: Full control*. Co może zrobić członek grupy Kadry przez sieć, a co lokalnie?

??? question "3. (OI)(CI)"
    Co oznacza zapis `(OI)(CI)M` w wyniku `icacls`?

??? question "4. Udział ukryty"
    Jak utworzyć udział ukryty i jak się z nim połączyć?

??? question "5. Złożoność hasła"
    Czy hasło `Szkola2026` spełnia wymóg złożoności Windows? Uzasadnij.
