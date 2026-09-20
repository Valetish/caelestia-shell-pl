# Caelestia Shell po polsku

Repozytorium dodaje 821 polskich tłumaczeń do Caelestia Shell. Obejmuje komunikaty
interfejsu: Nexus, panel główny, launcher, pasek zadań, panel boczny, ekran
blokady oraz komunikaty i powiadomienia generowane przez Caelestię.

Kod bazuje na Caelestii 2.5 i korzysta z jej istniejącego mechanizmu tłumaczeń.
Zmiany zgłoszono do głównego projektu w [PR #2058](https://github.com/caelestia-dots/shell/pull/2058).

## Co jest tłumaczone

- ustawienia Nexus i wszystkie ich podstrony;
- panel główny, prognoza pogody i informacje o wydajności;
- launcher i jego wbudowane działania;
- ustawienia sieci, Bluetooth, dźwięku i VPN;
- panel powiadomień, ekran blokady i komunikaty Caelestii;
- prawidłowe polskie formy liczby mnogiej.

Treść powiadomień pochodzących z innych programów pozostaje w języku, w którym
wysłał ją dany program.

## Test na Arch Linux (fish)

Poniższy sposób nie używa `sudo`, nie zastępuje oficjalnej instalacji i nie
dotyka konfiguracji w `~/.config/caelestia`. Kod, biblioteki i testowe ustawienia
trafiają do osobnego katalogu. Najpierw zainstaluj zależności wymienione w
głównym pliku README, a następnie wykonaj:

```fish
mkdir -p "$HOME/.local/src"
git clone --branch main https://github.com/Valetish/caelestia-shell-pl.git \
  "$HOME/.local/src/caelestia-shell-pl"
cd "$HOME/.local/src/caelestia-shell-pl"
```

Zbuduj i zainstaluj wersję testową tylko dla bieżącego użytkownika:

```fish
set CAELESTIA_PL_TEST_ROOT "$HOME/.local/share/caelestia-pl-test"
cmake -S . -B build-test -G Ninja -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_INSTALL_PREFIX="$CAELESTIA_PL_TEST_ROOT"
cmake --build build-test
cmake --install build-test
```

Najpierw zatrzymaj zwykłą Caelestię, aby dwie powłoki nie rezerwowały jednocześnie
miejsca na panele i nie obsługiwały tych samych skrótów. Następnie uruchom wersję
testową z oddzielną konfiguracją. Polecenie pozostaje podłączone do terminala;
zakończenie go skrótem `Ctrl+C` wyłącza testową powłokę.

```fish
caelestia shell -k
env XDG_CONFIG_HOME="$CAELESTIA_PL_TEST_ROOT/config" \
  CAELESTIA_LIB_DIR="$CAELESTIA_PL_TEST_ROOT/lib/caelestia" \
  QML2_IMPORT_PATH="$CAELESTIA_PL_TEST_ROOT/lib/qt6/qml:$QML2_IMPORT_PATH" \
  qs -p "$CAELESTIA_PL_TEST_ROOT/etc/xdg/quickshell/caelestia"
```

W działającej wersji testowej otwórz:

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

Po teście naciśnij `Ctrl+C` i uruchom ponownie zwykłą powłokę:

```fish
caelestia shell -d
```

### Stała instalacja na Arch Linux

Ta instalacja tworzy pakiet `caelestia-shell-pl` zarządzany przez Pacmana.
Pakiet obecnie korzysta z tagu `polish-v0.9`; nowsze commity na `main`
trafią do niego dopiero po aktualizacji pakietu.
Nie używaj samego `pacman -Sy`: Arch nie obsługuje częściowych
aktualizacji. Najpierw wykonaj pełną aktualizację systemu i pakietów AUR:

```fish
paru -Syu
paru -S --needed qt6-m3shapes-git
```

Następnie zbuduj i zainstaluj pakiet z repozytorium:

```fish
cd "$HOME/.local/src/caelestia-shell-pl"
git pull --ff-only
cd packaging/arch
makepkg -si
```

Pacman zapyta, czy usunąć konfliktujący pakiet `caelestia-shell`; odpowiedz
`t` (tak). Plik `~/.config/caelestia/shell.json` nie jest częścią pakietu i nie
zostanie nadpisany. Po instalacji przeładuj powłokę:

```fish
caelestia shell -k
caelestia shell -d
```

Powrót do oficjalnego wydania jest równie prosty: `paru -S caelestia-shell`.

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

Ukraiński i inne języki planujemy dodawać w osobnych pull requestach.

Projekt zachowuje licencję GPL-3.0 oryginalnego Caelestia Shell.
