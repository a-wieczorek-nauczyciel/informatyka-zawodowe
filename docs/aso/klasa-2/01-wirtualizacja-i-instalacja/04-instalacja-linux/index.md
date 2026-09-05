# Instalacja Linux

!!! info "Cel lekcji"
    Po tej lekcji będziesz umieć samodzielnie zainstalować dystrybucję Linuksa (Ubuntu) jako maszynę wirtualną w VirtualBox, i zauważyć kluczowe różnice względem instalacji Windows z poprzedniej lekcji.

!!! tip "Wykorzystaj ten sam plan co przy Windows"
    Parametry z lekcji "Planowanie instalacji" (styl partycjonowania, podział dysku) stosujesz teraz ponownie — tym razem dla Linuksa. Warto od razu zwracać uwagę na to, co jest **takie samo**, a co **inne** niż przy instalacji Windows.

## Czego potrzebujesz przed rozpoczęciem

- Zainstalowany VirtualBox
- Plik ISO z dystrybucją Ubuntu (Desktop lub Server — sprawdź, którą wersję udostępnił nauczyciel na zajęciach)
- Zaplanowane parametry maszyny wirtualnej

## Krok 1 — utworzenie maszyny wirtualnej

1. W VirtualBox kliknij **"Nowa"**
2. Nadaj nazwę (np. `Ubuntu-Server`) — VirtualBox rozpozna typ systemu Linux na podstawie nazwy
3. Ustaw RAM — Ubuntu Server potrafi działać sensownie już przy 1–2 GB, ale dla komfortu pracy zalecane 2 GB
4. Utwórz nowy dysk wirtualny VDI, alokacja dynamiczna

!!! tip "Linux jest zwykle lżejszy od Windows"
    Zauważ, że domyślne wymagania sprzętowe proponowane przez VirtualBox dla systemów Linux są zwykle niższe niż dla Windows — to odzwierciedla rzeczywistą różnicę w zapotrzebowaniu na zasoby między tymi systemami, szczególnie przy wersjach Server bez graficznego interfejsu.

## Krok 2 — podpięcie obrazu ISO

Identycznie jak przy Windows: **Ustawienia → Nośniki → wybierz plik ISO** z Ubuntu.

## Krok 3 — uruchomienie instalatora

1. Uruchom maszynę
2. Instalator Ubuntu wyświetli ekran wyboru języka, następnie układu klawiatury
3. Wybierz opcję instalacji (przy Ubuntu Server: zwykle "Install Ubuntu Server")

!!! info "Ubuntu Server nie ma interfejsu graficznego"
    Jeśli instalujesz wersję Server (a nie Desktop), po zakończeniu instalacji zobaczysz **tekstowy terminal**, nie pulpit graficzny — to zamierzone. Serwery produkcyjne rzadko potrzebują interfejsu graficznego, który tylko zużywa dodatkowe zasoby. Cała administracja odbywa się z poziomu wiersza poleceń.

## Krok 4 — partycjonowanie

Tu pojawia się pierwsza wyraźna różnica względem Windows:

1. Instalator Ubuntu zaproponuje **"Use entire disk"** (użyj całego dysku) jako opcję domyślną — dla naszych ćwiczeń edukacyjnych to wygodne uproszczenie
2. Jeśli chcesz ręcznie odwzorować swój plan z poprzedniej lekcji, wybierz opcję **manualnego partycjonowania** i twórz partycje zgodnie z notatkami

!!! warning "Linux inaczej nazywa partycje niż Windows"
    Zamiast liter dysków (C:, D:) Linux używa ścieżek w drzewie katalogów, a partycje są oznaczane np. `/dev/sda1`, `/dev/sda2`. Kluczowa partycja to `/` (root) — odpowiednik "dysku systemowego" w Windows. Osobną partycją bywa też `/home` (dane użytkowników) czy `swap` (odpowiednik pliku wymiany w Windows, ale jako osobna partycja, nie plik).

??? question "Zadanie 1: Porównaj nazewnictwo partycji"
    Zestaw ze sobą: partycję systemową Windows (z poprzedniej lekcji) i partycję root `/` w Linuksie. Czym są podobne funkcjonalnie, a czym różni się sposób ich nazywania?

## Krok 5 — konfiguracja użytkownika i sieci

1. Instalator poprosi o nazwę komputera (hostname), nazwę użytkownika i hasło
2. Przy Ubuntu Server instalator zwykle automatycznie skonfiguruje sieć przez DHCP — zobaczysz przydzielony adres IP już na etapie instalacji

??? question "Zadanie 2: Zainstaluj Ubuntu zgodnie z planem"
    Wykonaj instalację Ubuntu w VirtualBox. Dokumentuj proces zrzutami ekranu: tworzenie maszyny, ekran partycjonowania, moment zakończenia instalacji.

## Dodatki Gościa w Linuksie

Instalacja Dodatków Gościa w Linuksie wymaga kilku dodatkowych poleceń w terminalu (w przeciwieństwie do Windows, gdzie to prosty kreator):

```bash
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)
```

Następnie z menu VirtualBox: **Urządzenia → Wstaw obraz CD Dodatków Gościa**, zamontuj płytę i uruchom instalator zgodnie z instrukcją wyświetloną w terminalu.

??? question "Zadanie 3: Zainstaluj Dodatki Gościa w Linuksie"
    Zainstaluj Dodatki Gościa w swojej maszynie z Ubuntu, wykonując polecenia z tej lekcji. Zrób zrzut ekranu terminala pokazujący pomyślne zakończenie instalacji.

## Pierwsze polecenia w terminalu Linuksa

Jeśli zainstalowałeś Ubuntu Server (bez GUI), oto kilka pierwszych, przydatnych poleceń do sprawdzenia, że wszystko działa:

```bash
whoami          # pokazuje nazwę zalogowanego użytkownika
hostname        # pokazuje nazwę komputera
ip a            # pokazuje konfigurację sieciową (adresy IP)
df -h           # pokazuje wykorzystanie dysku w czytelnej formie
```

??? question "Zadanie 4: Uruchom pierwsze polecenia"
    Zaloguj się do swojej instalacji Ubuntu i wykonaj wszystkie cztery polecenia z tej lekcji. Zrób zrzut ekranu terminala z wynikami i krótko opisz (jednym zdaniem na polecenie), co każde z nich pokazuje.

## Zadania podsumowujące

??? question "Zadanie 5: Porównaj instalację Windows i Linux"
    Napisz krótkie porównanie (kilka zdań) doświadczenia instalacji obu systemów: co było podobne, co różniło się najbardziej, co było łatwiejsze/trudniejsze i dlaczego.

??? question "Zadanie 6: Sprawdź wersję systemu"
    W zainstalowanym Ubuntu użyj polecenia `lsb_release -a` (lub `cat /etc/os-release`), żeby sprawdzić dokładną wersję systemu. Zapisz wynik.

??? question "Zadanie 7: GUI czy bez GUI — uzasadnij wybór"
    Gdybyś miał zainstalować serwer plików dla małej firmy (10 pracowników), czy wybrałbyś Ubuntu Server (bez GUI), czy Ubuntu Desktop? Uzasadnij, biorąc pod uwagę zużycie zasobów i to, do czego serwer faktycznie będzie służył.
