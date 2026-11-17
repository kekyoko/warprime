# WarPrime

WarPrime — переименованная (ребрендинг) сборка бутстраппера **OpenWF** версии `0.14.0` для Warframe.
Вся видимая пользователю атрибутика заменена на **WarPrime**: название установщика, заголовок окна,
оверлей, меню и сообщения. Автообновление переведено на этот репозиторий.

> Это клиентская модификация игры. Используйте на свой страх и риск.
> Проект не связан с Digital Extremes и не одобрен ими.

## Быстрая установка

1. Скачайте установщик: **[WarPrime Setup.exe](https://github.com/kekyoko/warprime/raw/main/WarPrime%20Setup.exe)**
2. Положите `WarPrime Setup.exe` в папку, где находится `Warframe.x64.exe`
   (обычно `C:\Program Files (x86)\Steam\steamapps\common\Warframe` или папка лаунчера).
3. Запустите `WarPrime Setup.exe` и дождитесь окончания загрузки.
4. Запускайте игру через созданный ярлык/файл `Launch with WarPrime.bat`
   (он появляется рядом с `Warframe.x64.exe`).

Установщик создаёт папку `WarPrime`, скачивает скрипт установки и загружает актуальную
`dwmapi.dll` из этого репозитория.

## Ручная установка

Если установщик не подходит, можно поставить DLL вручную:

1. Скачайте [`dwmapi.dll`](https://github.com/kekyoko/warprime/raw/main/client-drop-in/0.14.0/dwmapi.dll).
2. Положите файл рядом с `Warframe.x64.exe`.
3. Создайте папку `WarPrime` рядом с `Warframe.x64.exe`.

DLL также работает под именами `wtsapi32.dll` и `version.dll` — бутстраппер поддерживает все три
варианта прокси-библиотеки. Не держите DLL в одной папке с другими `.exe`.

## Автообновление

Бутстраппер и скрипты обновления обращаются к файлам в этом репозитории:

| Файл | Назначение |
| --- | --- |
| `client-drop-in/meta` | JSON с версией, SHA-256 и манифестом (используется DLL и PowerShell-скриптом) |
| `client-drop-in/latest.txt` | Последняя версия (для Linux/Wine-скрипта) |
| `client-drop-in/latest_hotfix.txt` | Имя последнего хотфикса (пусто, если хотфикса нет) |
| `client-drop-in/sha256.txt` | SHA-256 актуальной DLL |
| `client-drop-in/sha256_hotfix.txt` | SHA-256 хотфикса (пусто, если хотфикса нет) |
| `client-drop-in/0.14.0/dwmapi.dll` | Собственно файл бутстраппера |
| `scripts/Download-Latest-DLL.ps1` | Скрипт установки/обновления для Windows |
| `scripts/Download-Latest-DLL.sh` | Скрипт установки/обновления для Linux/Wine |

Текущая версия: **0.14.0**

SHA-256 `client-drop-in/0.14.0/dwmapi.dll`:

```
884f36d50d9f892b3f50aeac372661882196063483d03a0706777291e8432eb6
```

## Структура репозитория

```
WarPrime Setup.exe                  установщик
client-drop-in/
  meta                              метаданные для автообновления
  0.14.0/dwmapi.dll                 бутстраппер (прокси-DLL)
  latest.txt
  latest_hotfix.txt
  sha256.txt
  sha256_hotfix.txt
scripts/
  Download-Latest-DLL.ps1
  Download-Latest-DLL.sh
source/
  warframe-dll-0.14.0-warprime.zip  исходники DLL (WarPrime)
  Bootstrapper-Setup-warprime.zip   исходники установщика
```

## Сборка из исходников

### Бутстраппер (`dwmapi.dll`)

Нужны: PHP 8+ с поддержкой хеша `joaat`, LLVM (`llvm-rc`), [Sun](https://github.com/Sainan/Sun),
Git (для подмодулей).

```bat
git submodule update --init --recursive
php archive.php
tools\pluto.exe tools\make_version_rc.pluto
llvm-rc version.rc
sun _release
```

На выходе — `dwmapi.dll`. Заголовок версии задаётся в `main.hpp`
(`BOOTSTRAPPER_TITLE "WarPrime Bootstrapper v0.14.0"`).

### Установщик (`WarPrime Setup.exe`)

```bat
cd Bootstrapper-Setup
sun _release
```

На выходе — `WarPrime Setup.exe`.

## Благодарности и происхождение кода

Проект основан на открытых исходниках:

- **OpenWF** — <https://onlyg.it/OpenWF> (идея, клиентский бутстраппер, скрипты и переводы);
- **warframe-dll** — <https://github.com/spaceninjaserver/warframe-dll>;
- **Soup** — <https://github.com/calamity-inc/Soup>;
- **Pluto** — <https://github.com/PlutoLang/Pluto>;
- **Sideloadify** — <https://github.com/Sainan/Sideloadify>.

Права на исходный код принадлежат их авторам. Изменения в этом репозитории касаются
названия, оформления и адресов обновления. Подробнее — в [NOTICE.md](NOTICE.md).
