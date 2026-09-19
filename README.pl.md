# Caelestia Shell po polsku

To jest polska wersja rozwojowa Caelestia Shell. Obejmuje ponad 800 komunikatów
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

## Bezpieczny test na Arch Linux

Poniższy sposób nie używa `sudo`, nie zastępuje oficjalnej instalacji i nie
dotyka konfiguracji w `~/.config/caelestia`. Kod, biblioteki i testowe ustawienia
trafiają do osobnego katalogu. Najpierw zainstaluj zależności wymienione w
głównym pliku README, a następnie wykonaj:

```bash
mkdir -p "$HOME/.local/src"
git clone --branch main https://github.com/Valetish/caelestia-shell-pl.git \
  "$HOME/.local/src/caelestia-shell-pl"
cd "$HOME/.local/src/caelestia-shell-pl"
```

Zbuduj i zainstaluj wersję testową tylko dla bieżącego użytkownika:

```bash
CAELESTIA_PL_TEST_ROOT="$HOME/.local/share/caelestia-pl-test"
cmake -B build-test -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_INSTALL_PREFIX="$CAELESTIA_PL_TEST_ROOT"
cmake --build build-test
cmake --install build-test
```

Uruchom ją z oddzielną konfiguracją. Polecenie pozostaje podłączone do terminala;
zakończenie go skrótem `Ctrl+C` wyłącza testową powłokę.

```bash
XDG_CONFIG_HOME="$CAELESTIA_PL_TEST_ROOT/config" \
CAELESTIA_LIB_DIR="$CAELESTIA_PL_TEST_ROOT/lib/caelestia" \
QML2_IMPORT_PATH="$CAELESTIA_PL_TEST_ROOT/lib/qt6/qml${QML2_IMPORT_PATH:+:$QML2_IMPORT_PATH}" \
qs -p "$CAELESTIA_PL_TEST_ROOT/etc/xdg/quickshell/caelestia"
```

Następnie otwórz:

**Ustawienia → Język i region → Język interfejsu → polski**

Podczas tego testu ustawienia są zapisywane w
`~/.local/share/caelestia-pl-test/config/caelestia/shell.json`, a nie w Twojej
zwykłej konfiguracji. W razie potrzeby język można ustawić tam ręcznie:

```json
{
  "general": {
    "language": "pl"
  }
}
```

Usunięcie właściwości `language` przywraca automatyczne dopasowanie do języka
systemu.

### Instalacja systemowa

Standardowe `sudo cmake --install build` zastępuje systemowe pliki Caelestii.
Używaj go dopiero po udanym teście i zachowaj możliwość ponownego zainstalowania
oficjalnego pakietu. Sama instalacja nie nadpisuje pliku
`~/.config/caelestia/shell.json`, ale przed systemową zmianą i tak warto zrobić
jego kopię zapasową.

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
