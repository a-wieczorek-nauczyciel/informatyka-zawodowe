# Zadania utrwalające

Zadania podsumowujące dział „Zabezpieczenia systemu Windows i kopie bezpieczeństwa”. Wykonujesz je na maszynie wirtualnej z dodatkowym dyskiem `E:` (np. 10 GB) i dokumentujesz zrzutami ekranu.

## Zadanie 1: Zabezpieczenie stanowiska

- [ ] Ochrona w czasie rzeczywistym włączona, definicje zaktualizowane
- [ ] Folder `C:\Firma` dodany do **Controlled folder access**
- [ ] Szybkie skanowanie wykonane w PowerShell
- [ ] Zapora włączona we wszystkich profilach; ping do maszyny dozwolony tylko w profilu **Private**
- [ ] Zrzut ekranu: wynik `Get-MpComputerStatus` i `Get-NetFirewallProfile`
- [ ] Zrzut ekranu: włączona reguła ICMPv4 w `wf.msc` z widocznym profilem

## Zadanie 2: Reguła dla usługi

- [ ] Reguła przychodząca zezwalająca na TCP 8080 tylko w profilu **Private**
- [ ] Reguła wychodząca blokująca wybrany program
- [ ] Obie reguły utworzone — jedna w `wf.msc`, druga w PowerShell
- [ ] Zrzut ekranu: obie reguły na liście

## Zadanie 3: Komplet kopii

- [ ] Punkt przywracania „przed zadaniem 3”
- [ ] Historia plików włączona na dysku `E:`
- [ ] Kopia `C:\Firma` poleceniem `robocopy` z uprawnieniami i logiem do `E:\Kopia`
- [ ] Obraz systemu zapisany na `E:`
- [ ] Zrzut ekranu każdej kopii i pliku logu

## Zadanie 4: Awaria i odtworzenie

- [ ] Usuń folder `C:\Firma\Kadry` (z pominięciem kosza: ++shift+delete++)
- [ ] Odtwórz go z kopii `robocopy` z zachowaniem uprawnień NTFS
- [ ] Zmień i zapisz plik w `Dokumenty`, potem przywróć poprzednią wersję z historii plików
- [ ] Zrzut ekranu: `icacls` odtworzonego folderu — uprawnienia takie same jak przed awarią

## Zadanie 5: Krótki quiz sprawdzający (bez VM)

??? question "1. Warstwy"
    Dlaczego sam antywirus nie wystarczy do ochrony komputera? Podaj trzy inne zabezpieczenia.

??? question "2. Profile zapory"
    Czym różni się profil **Public** od **Private** i dlaczego w kawiarni powinien być aktywny **Public**?

??? question "3. Przyrostowa i różnicowa"
    Czym różni się kopia przyrostowa od różnicowej? Która szybciej się wykonuje, a która szybciej przywraca?

??? question "4. Punkt przywracania"
    Czy punkt przywracania ochroni dokumenty użytkownika przed ransomware? Uzasadnij.

??? question "5. 3-2-1"
    Wyjaśnij regułę 3-2-1 na przykładzie zdjęć z wakacji przechowywanych na laptopie.
