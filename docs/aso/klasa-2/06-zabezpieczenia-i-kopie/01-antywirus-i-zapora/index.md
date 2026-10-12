# Antywirus i zapora

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć rozpoznać podstawowe rodzaje zagrożeń, skonfigurować Microsoft Defender i Windows Defender Firewall, utworzyć regułę zapory oraz dobrać zabezpieczenie do zagrożenia.

## Zagrożenia

| Zagrożenie | Jak działa |
|---|---|
| **wirus** | dopisuje się do plików i rozprzestrzenia się, gdy uruchomisz zainfekowany plik |
| **robak (worm)** | rozprzestrzenia się sam przez sieć, bez udziału użytkownika |
| **koń trojański** | udaje użyteczny program, a w tle wykonuje szkodliwe działania |
| **ransomware** | szyfruje pliki i żąda okupu za klucz |
| **spyware / keylogger** | zbiera dane: hasła, naciśnięcia klawiszy, historię przeglądania |
| **phishing** | fałszywa wiadomość lub strona wyłudzająca dane logowania |
| **atak sieciowy** | próba połączenia z usługą na komputerze z zewnątrz |

## Warstwy ochrony

Żadne pojedyncze zabezpieczenie nie chroni przed wszystkim. Stosujemy kilka warstw:

| Zabezpieczenie | Chroni przed |
|---|---|
| **antywirus** | wirusami, trojanami, ransomware, spyware w plikach |
| **zapora (firewall)** | niechcianymi połączeniami sieciowymi |
| **aktualizacje systemu** | wykorzystaniem znanych luk w oprogramowaniu |
| **konto standardowe zamiast administratora** | instalacją złośliwego oprogramowania bez wiedzy użytkownika |
| **kopie zapasowe** | utratą danych — także po ataku ransomware |
| **świadomość użytkownika** | phishingiem i socjotechniką |

## Windows Security

**Start → Windows Security (Zabezpieczenia Windows)** — jedno okno ze wszystkimi zabezpieczeniami:

- **Virus & threat protection** — antywirus Microsoft Defender,
- **Firewall & network protection** — zapora,
- **App & browser control** — SmartScreen (ostrzega przed nieznanymi programami i stronami),
- **Device security** — m.in. TPM, Secure Boot,
- **Protection history** — wykryte i usunięte zagrożenia.

### Microsoft Defender

**Virus & threat protection**:

- **Scan options** — **Quick scan** (miejsca, w których najczęściej ukrywa się złośliwe oprogramowanie), **Full scan** (cały dysk), **Custom scan** (wybrany folder), **Microsoft Defender Offline scan** (skan przed uruchomieniem systemu),
- **Manage settings** — **Real-time protection** (ochrona w czasie rzeczywistym), **Cloud-delivered protection**, **Exclusions** (wykluczenia),
- **Protection updates** — aktualizacja definicji wirusów,
- **Ransomware protection → Controlled folder access** — chronione foldery, do których nieznane programy nie mogą zapisywać.

PowerShell:

```powershell
Get-MpComputerStatus | Select-Object AntivirusEnabled, RealTimeProtectionEnabled, AntivirusSignatureLastUpdated
Update-MpSignature                                  # aktualizacja definicji
Start-MpScan -ScanType QuickScan                    # szybkie skanowanie
Start-MpScan -ScanType CustomScan -ScanPath C:\Pobrane
Get-MpThreatDetection                               # historia wykrytych zagrożeń
```

!!! tip "Bezpieczny test antywirusa"
    Plik testowy **EICAR** to standardowy, nieszkodliwy ciąg znaków, który każdy antywirus wykrywa jako „wirusa”. Służy do sprawdzenia, czy ochrona działa — bez żadnego ryzyka.

## Windows Defender Firewall

Zapora decyduje, które połączenia sieciowe są dozwolone. Domyślnie:

- połączenia **przychodzące** — blokowane, chyba że reguła na nie pozwala,
- połączenia **wychodzące** — dozwolone.

### Profile sieci

| Profil | Kiedy |
|---|---|
| **Domain** | komputer w domenie Active Directory |
| **Private** | zaufana sieć: dom, firma, pracownia |
| **Public** | niezaufana sieć: kawiarnia, lotnisko — najostrzejsze ustawienia |

Każdy profil ma osobne reguły i można go włączyć lub wyłączyć osobno.

### Reguły — wf.msc

++win+r++ → `wf.msc` → **Windows Defender Firewall with Advanced Security**:

- **Inbound Rules** — reguły dla ruchu przychodzącego,
- **Outbound Rules** — reguły dla ruchu wychodzącego.

**Nowa reguła:** **Inbound Rules → New Rule…** → typ (*Program*, *Port*, *Predefined*, *Custom*) → protokół i port (np. TCP 8080) → akcja (*Allow* / *Block*) → profile → nazwa.

Przykład: zezwolenie na ping (domyślnie zablokowany) — reguła **File and Printer Sharing (Echo Request - ICMPv4-In)** → **Enable Rule**.

PowerShell:

```powershell
Get-NetFirewallProfile | Select-Object Name, Enabled
New-NetFirewallRule -DisplayName "Serwer WWW 8080" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow
New-NetFirewallRule -DisplayName "Blokuj Notatnik" -Direction Outbound -Program "C:\Windows\System32\notepad.exe" -Action Block
Enable-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)"
Remove-NetFirewallRule -DisplayName "Serwer WWW 8080"
```

!!! warning "Nie wyłączaj zapory „żeby działało”"
    Gdy coś nie łączy się przez sieć, włącz **jedną konkretną regułę** dla potrzebnego programu lub portu. Wyłączenie całej zapory otwiera komputer na wszystkie połączenia.

## Aktualizacje

**Settings → Windows Update**: **Check for updates**, **Update history**, **Pause updates**, **Advanced options → Active hours** (godziny, w których system nie uruchomi się ponownie sam).

## Zadania

??? question "Zadanie 1: Stan ochrony"
    Sprawdź w Windows Security i poleceniem `Get-MpComputerStatus`: czy ochrona w czasie rzeczywistym jest włączona i kiedy ostatnio zaktualizowano definicje. Zaktualizuj je.

??? question "Zadanie 2: Skanowanie"
    Utwórz folder `C:\Pobrane` i przeskanuj go skanem niestandardowym — raz w Windows Security, raz w PowerShell. Sprawdź **Protection history**.

??? question "Zadanie 3: Ping przez zaporę"
    Mając dwie maszyny w jednej sieci, sprawdź, czy ping działa. Włącz odpowiednią regułę zapory na maszynie docelowej i sprawdź ponownie. Zrób zrzut ekranu reguły w `wf.msc`.

??? question "Zadanie 4: Własna reguła"
    Utwórz regułę blokującą połączenia wychodzące przeglądarki Edge. Sprawdź, że strony się nie otwierają. Wyłącz regułę i sprawdź ponownie.

??? question "Zadanie 5: Dobierz zabezpieczenie"
    Dla każdej sytuacji wskaż najważniejsze zabezpieczenie: (a) pracownik otworzył załącznik „faktura.pdf.exe”, (b) ktoś z sieci próbuje połączyć się z udostępnionym folderem, (c) ransomware zaszyfrował dokumenty, (d) w przeglądarce jest znana od miesiąca luka.
