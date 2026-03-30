# GrayLauncher — Инструкция по сборке

## Требования

| Компонент | Версия |
|-----------|--------|
| Qt        | 6.4+   |
| CMake     | 3.20+  |
| Компилятор | GCC 12+ / MSVC 2022 / Clang 15+ |
| Ninja     | любая  |

---

## 1. Установить Qt 6

### Windows
Скачать Qt Online Installer: https://www.qt.io/download-open-source  
Выбрать компоненты:
- Qt 6.x → MSVC 2022 64-bit (или MinGW 64-bit)
- Qt WebEngine
- Qt Concurrent

### Linux (Ubuntu/Debian)
```bash
sudo apt install qt6-base-dev qt6-webengine-dev qt6-concurrent-dev \
     libqt6network6 cmake ninja-build build-essential
```

### macOS
```bash
brew install qt cmake ninja
```

---

## 2. Шрифты (обязательно!)

Скачать и положить в `resources/fonts/`:
- **Orbitron** → https://fonts.google.com/specimen/Orbitron
  - `Orbitron-Regular.ttf`
  - `Orbitron-Bold.ttf`
- **Rajdhani** → https://fonts.google.com/specimen/Rajdhani
  - `Rajdhani-Regular.ttf`
  - `Rajdhani-SemiBold.ttf`
- **Share Tech Mono** → https://fonts.google.com/specimen/Share+Tech+Mono
  - `ShareTechMono-Regular.ttf`

Иконка `resources/icons/icon.png` — любой PNG 256×256.

---

## 3. Сборка

### Windows (MSVC)
```cmd
mkdir build && cd build
cmake .. -G "Ninja" -DCMAKE_BUILD_TYPE=Release ^
         -DCMAKE_PREFIX_PATH="C:\Qt\6.x\msvc2022_64"
ninja
```

### Linux / macOS
```bash
mkdir build && cd build
cmake .. -G "Ninja" -DCMAKE_BUILD_TYPE=Release \
         -DCMAKE_PREFIX_PATH="$(brew --prefix qt)" # только macOS
ninja
```

---

## 4. Запуск

```bash
./GrayLauncher          # Linux / macOS
GrayLauncher.exe        # Windows
```

Данные лаунчера хранятся в `~/.graylauncher/`

---

## Структура проекта

```
GrayLauncher/
├── CMakeLists.txt
├── README.md
├── include/
│   ├── Constants.h         ← все URL и пути
│   ├── Launcher.h          ← ядро: запуск игры
│   ├── MetaManager.h       ← meta.pineconemc.ru/v1/
│   ├── DownloadManager.h   ← параллельные загрузки (8 потоков)
│   ├── LogManager.h        ← лог + hst.sh
│   ├── JavaManager.h       ← авто-поиск Java
│   ├── AuthManager.h       ← offline + Mojang auth
│   ├── VersionManager.h    ← управление версиями
│   ├── ProfileManager.h    ← профили
│   └── ui/
│       ├── StyleHelper.h   ← QSS тема (Cyberpunk Dark)
│       ├── SideBar.h
│       ├── PlayPage.h
│       ├── SettingsPage.h
│       ├── ConsolePage.h
│       ├── AccountPage.h
│       └── ProgressOverlay.h
├── src/
│   ├── main.cpp
│   ├── MainWindow.cpp
│   ├── Launcher.cpp
│   ├── MetaManager.cpp
│   ├── DownloadManager.cpp
│   ├── LogManager.cpp
│   ├── JavaManager.cpp
│   ├── AuthManager.cpp
│   ├── VersionManager.cpp
│   ├── ProfileManager.cpp
│   └── ui/
│       ├── SideBar.cpp
│       ├── PlayPage.cpp
│       ├── SettingsPage.cpp
│       ├── ConsolePage.cpp
│       ├── AccountPage.cpp
│       ├── ProgressOverlay.cpp
│       └── StyleHelper.cpp
└── resources/
    ├── resources.qrc
    ├── fonts/          ← Google Fonts (см. выше)
    └── icons/
        └── icon.png
```

---

## API эндпоинты

| Назначение | URL |
|------------|-----|
| Метаданные версий | `https://meta.pineconemc.ru/v1/versions` |
| Метаданные версии | `https://meta.pineconemc.ru/v1/versions/{id}` |
| Ресурсы (assets) | `https://resources.download.minecraft.net/{hash[0:2]}/{hash}` |
| FML библиотеки | `https://files.pineconemc.ru/fmllibs/{path}` |
| Загрузка логов | `https://hst.sh` (POST `/documents`) |
| Библиотеки Mojang | `https://libraries.minecraft.net/{path}` |
