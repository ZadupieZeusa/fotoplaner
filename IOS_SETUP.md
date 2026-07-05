# Uruchomienie PhotoFlow na iOS (iPhone + iPad)

Kod aplikacji jest już gotowy pod iOS (Flutter + wszystkie użyte pakiety —
sqflite, path_provider, file_picker, share_plus, shared_preferences —
wspierają iOS natywnie). Braki to wyłącznie platformowy folder `ios/`,
którego nie da się bezpiecznie wygenerować bez Xcode/Flutter SDK na Macu.

Xcode **nie jest wtyczką do Android Studio** — to osobna aplikacja Apple.

## 1. Zainstaluj Xcode
- Mac App Store → wyszukaj "Xcode" → Zainstaluj (ok. 15–40 GB, może potrwać).
- Po instalacji w Terminalu:
  ```
  sudo xcode-select --switch /Applications/Xcode.app
  sudo xcodebuild -license accept
  xcodebuild -runFirstLaunch
  ```

## 2. Zainstaluj CocoaPods (menedżer zależności iOS)
```
sudo gem install cocoapods
```
(jeśli masz Homebrew, alternatywnie: `brew install cocoapods`)

## 3. Sprawdź Fluttera
```
flutter doctor
```
Powinieneś zobaczyć zielony ✓ przy "Xcode".

## 4. Wygeneruj platformę iOS w projekcie
W katalogu projektu (`~/Claude/Projects/PhotoFlow_Android`):
```
flutter create --platforms=ios .
```
To doda folder `ios/` z poprawnym projektem Xcode i podpięciem
identyfikatora aplikacji (`com.albertpigula.photoflow_android` —
Flutter przejmie go automatycznie z konfiguracji Androida).

## 5. Zainstaluj pody
```
cd ios && pod install && cd ..
```

## 6. Otwórz projekt w Xcode i skonfiguruj podpisywanie
```
open ios/Runner.xcworkspace
```
W zakładce **Signing & Capabilities**:
- Team: wybierz swoje Apple ID (darmowe konto wystarczy do testów na własnym
  urządzeniu, do App Store potrzebny płatny Apple Developer Program — 99$/rok).
- Bundle Identifier: zostaw `com.albertpigula.photoflow_android`.
- Deployment Target: iOS 13.0+ (wystarczy dla używanych pakietów).

## 7. Uruchom
- Symulator: wybierz np. "iPad Pro" lub "iPhone 15" w Xcode/VS Code i uruchom `flutter run`.
- Fizyczne urządzenie: podłącz kablem, zaufaj komputerowi na iPhonie/iPadzie,
  wybierz urządzenie jako target i uruchom `flutter run`.

## Co już przygotowałem w kodzie pod iOS/iPad
- **share_plus**: naprawiony crash na iPadzie przy eksporcie backupu — iOS
  wymaga punktu zaczepienia (`sharePositionOrigin`) dla arkusza udostępniania.
- **Orientacje**: dodano `portraitDown` — iPad naturalnie wspiera pracę "do góry nogami".
- **Kanban**: kolumny są już w poziomym scrollu o stałej szerokości — na
  większym ekranie iPada widać po prostu więcej kolumn naraz, bez zmian w kodzie.
- Reszta UI to czysty Material/Flutter bez kodu specyficznego dla Androida —
  nie wymaga przeróbek, żeby działać na iOS.

## Ewentualne dalsze kroki (opcjonalnie, nie wymagane do działania)
- Własna ikona aplikacji i launch screen pod iOS (obecnie domyślne Flutter).
- Adaptacyjny layout pod iPada (np. boczne menu zamiast dolnego paska
  w trybie landscape) — obecny layout działa poprawnie, ale jest "telefonowy".
