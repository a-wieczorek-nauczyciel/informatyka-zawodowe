---
hide:
  - navigation
---

# Klasa 3 — Administracja Systemami Operacyjnymi

Serwery sieciowe i ich konfiguracja. W pierwszym semestrze pracujemy na Windows Server 2022, w drugim na Ubuntu Server.

<div class="grid cards" markdown>

-   **Podstawy sieci i adres statyczny**

    ---

    Adresacja IPv4, maska, brama, DNS; konfiguracja adresu statycznego na Windows Server 2022

    [:octicons-arrow-right-24: Przejdź](01-podstawy-sieci/index.md)

-   **Serwer DHCP**

    ---

    Automatyczna konfiguracja adresów: DORA, zakresy, wykluczenia, rezerwacje, opcje

    [:octicons-arrow-right-24: Przejdź](02-dhcp/index.md)

-   **Serwer DNS**

    ---

    Rozwiązywanie nazw: strefy, rekordy, strefa wyszukiwania wstecznego, przekazywanie

    [:octicons-arrow-right-24: Przejdź](03-dns/index.md)

</div>

## Środowisko laboratoryjne

Wszystkie zadania z tego semestru wykonujemy na dwóch maszynach wirtualnych w VirtualBox, połączonych **siecią wewnętrzną** (odizolowaną od sieci szkolnej):

| Maszyna | System | Karta sieciowa VirtualBox | Adres IPv4 |
|---|---|---|---|
| `SRV01` | Windows Server 2022 (Desktop Experience) | Internal Network `lab` | `192.168.10.10/24` (statyczny) |
| `CLIENT01` | Windows 10/11 | Internal Network `lab` | z serwera DHCP |

Brama domyślna w sieci laboratoryjnej: `192.168.10.1` (w naszym labie nie istnieje fizycznie — konfigurujemy ją tak, jak w prawdziwej sieci firmowej).

!!! warning "Dlaczego sieć wewnętrzna, a nie mostkowana"
    Serwer DHCP uruchomiony w trybie **Bridged** zacząłby rozdawać adresy komputerom w całej szkolnej sieci i sparaliżowałby ją. Tryb **Internal Network** sprawia, że Twój serwer widzi tylko Twojego klienta.
