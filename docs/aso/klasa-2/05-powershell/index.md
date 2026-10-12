---
hide:
  - navigation
---

# PowerShell

!!! info "Cel działu"
    Po tym dziale będziesz wiedzieć, czym jest powłoka i dlaczego PowerShell pracuje na obiektach, a nie na tekście. Będziesz umieć znaleźć polecenie i przeczytać jego pomoc, pracować na plikach, aliasach i parametrach, bezpiecznie sprawdzać skutki poleceń, łączyć polecenia w potok oraz pisać pierwsze skrypty `.ps1` ze zmiennymi, warunkami i pętlami.

!!! note "Środowisko"
    Pracujemy w **Windows PowerShell 5.1** na maszynie wirtualnej **Windows Server 2022** z angielskim interfejsem. Wszystkie ćwiczenia wykonujemy w folderze roboczym `C:\PS_Lab`. Na początku każdych zajęć:

    ```powershell
    New-Item -Path C:\PS_Lab -ItemType Directory -Force
    Set-Location C:\PS_Lab
    ```

<div class="grid cards" markdown>

-   **Lekcja 1 — Czym jest powłoka**

    ---

    Powłoka, GUI a CLI, historia PowerShella, tekst kontra obiekty, środowiska pracy, pierwszy skrypt

    [:octicons-arrow-right-24: Przejdź](01-czym-jest-powloka/index.md)

-   **Lekcja 2 — Pomoc, pliki, aliasy, parametry**

    ---

    Budowa cmdletu, Get-Help, Get-Command, pliki i foldery, aliasy, rodzaje parametrów

    [:octicons-arrow-right-24: Przejdź](02-pomoc-pliki-aliasy-parametry/index.md)

-   **Lekcja 3 — Bezpieczeństwo i potok**

    ---

    -WhatIf, -Confirm, potok obiektów, Get-Member, formatowanie i zapis wyniku, procesy, usługi, drukarki

    [:octicons-arrow-right-24: Przejdź](03-bezpieczenstwo-i-potok/index.md)

-   **Lekcja 4 — Dane i skrypty**

    ---

    Sort, Measure, Group, Select, Where, zmienne, tablice, if, pętle, budowanie skryptów, Execution Policy

    [:octicons-arrow-right-24: Przejdź](04-dane-i-skrypty/index.md)

-   **Mini-zadania 1–9**

    ---

    Zadania wykonywane na zajęciach po każdym bloku teorii — w jednym miejscu

    [:octicons-arrow-right-24: Przejdź](05-mini-zadania/index.md)

-   **Zadania utrwalające**

    ---

    Samodzielne zadania podsumowujące dział

    [:octicons-arrow-right-24: Przejdź](06-zadania-utrwalajace/index.md)

</div>
