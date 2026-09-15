# Caelestia Shell po polsku

To jest polska wersja rozwojowa Caelestia Shell. Obejmuje 840 komunikatów
interfejsu: Nexus, panel główny, launcher, pasek zadań, panel boczny, ekran
blokady oraz komunikaty i powiadomienia generowane przez Caelestię.

> [!IMPORTANT]
> Projekt bazuje na bieżącej gałęzi rozwojowej Caelestii. Mechanizm tłumaczeń
> jest nowszy niż ostatnie stabilne wydanie, dlatego przed instalacją warto
> zachować dotychczasowy pakiet lub konfigurację.

## Co jest tłumaczone

- ustawienia Nexus i wszystkie ich podstrony;
- panel główny, prognoza pogody i informacje o wydajności;
- launcher i jego wbudowane działania;
- ustawienia sieci, Bluetooth, dźwięku i VPN;
- panel powiadomień, ekran blokady i komunikaty Caelestii;
- prawidłowe polskie formy liczby mnogiej.

Treść powiadomień pochodzących z innych programów pozostaje w języku, w którym
wysłał ją dany program. Caelestia nie może bezpiecznie tłumaczyć np. wiadomości
z przeglądarki, komunikatora lub klienta poczty.

## Instalacja testowa na Arch Linux

Najpierw zainstaluj zależności wymienione w głównym pliku README. Następnie
sklonuj ten fork do katalogu konfiguracji Quickshell. Jeżeli masz już w tym
miejscu ręcznie zainstalowaną Caelestię, najpierw zmień nazwę jej katalogu i
zachowaj go jako kopię zapasową.

```bash
mkdir -p "$HOME/.config/quickshell"
git clone --branch main https://github.com/Valetish/caelestia-shell-pl.git \
  "$HOME/.config/quickshell/caelestia"
cd "$HOME/.config/quickshell/caelestia"
```

Zbuduj i zainstaluj fork tak samo jak oficjalną wersję źródłową:

```bash
cmake -B build -G Ninja -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/
cmake --build build
sudo cmake --install build
```

Uruchom powłokę ponownie:

```bash
caelestia shell -d
```

Jeżeli nie korzystasz z Caelestia CLI, użyj `qs -c caelestia -n -d`.
Następnie otwórz:

**Ustawienia → Język i region → Język interfejsu → polski**

W razie potrzeby język można ustawić ręcznie w
`~/.config/caelestia/shell.json`:

```json
{
  "general": {
    "language": "pl"
  }
}
```

Usunięcie właściwości `language` przywraca automatyczne dopasowanie do języka
systemu.

### Powrót do oficjalnej wersji

Przywróć kopię katalogu `~/.config/quickshell/caelestia`, jeżeli została
utworzona, a następnie przeinstaluj oficjalny pakiet `caelestia-shell` lub
ponownie wykonaj instalację z oficjalnego repozytorium. Twoje ustawienia w
`~/.config/caelestia` pozostają oddzielne od kodu powłoki.

## Sprawdzanie tłumaczenia

```bash
fish scripts/trs.fish raw
msgcmp --use-fuzzy plugin/src/Caelestia/I18n/trs/pl.po plugin/src/Caelestia/I18n/trs/raw.pot
fish scripts/trs.fish compile pl /tmp/caelestia-pl.mo
python3 scripts/trs-check.py --strict
```

Repozytorium zawiera również automatyczny test GitHub Actions, który sprawdza
kompletność katalogu, format parametrów i poprawność kompilacji.

## Rozwój

Głównym celem jest dopracowanie polskiej wersji i przekazanie jej do
oficjalnego projektu Caelestia. Kolejne języki mogą być dodawane jako osobne
katalogi PO, np. `uk.po` dla ukraińskiego.

Projekt zachowuje licencję GPL-3.0 oryginalnego Caelestia Shell.
