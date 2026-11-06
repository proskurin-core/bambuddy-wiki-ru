---
title: Установщик для Windows
description: Установите Bambuddy непосредственно в Windows с помощью автономного установщика .exe
---

# Установщик для Windows {#windows-installer}

Установщик Windows — автономный `.exe`, включающий всё необходимое для Bambuddy: встроенный Python 3.13, готовый фронтенд React, ffmpeg и NSSM для управления службой Windows. **Python, Node, Docker, Git и другие среды выполнения на целевом компьютере не нужны.**

Он устанавливает Bambuddy как службу Windows, доступную по адресу `http://localhost:8000`. Поэтому панель доступна после перезагрузки без ручного запуска.

---

## :material-download: Загрузка {#download}

Последняя стабильная версия:

[:material-download: bambuddy-windows-x64-setup.exe](https://github.com/maziggy/bambuddy/releases/latest/download/bambuddy-windows-x64-setup.exe){ .md-button .md-button--primary }

Ежедневные бета-сборки содержат новейшие функции, но проверены меньше. На [странице релизов](https://github.com/maziggy/bambuddy/releases) выберите последний предварительный выпуск с тегом `v<version>-daily.<date>`.

!!! info "Только Windows 10 / 11 x64"
    Установщик предназначен для 64-разрядных Windows 10 и 11. ARM64 (Surface Pro X, Snapdragon) пока не поддерживается: для большинства зависимостей Bambuddy нет пакетов ARM64 Windows.

---

## :material-shield-alert: Предупреждение SmartScreen при первом запуске {#smartscreen-warning-on-first-run}

До получения разрешения на [подпись кода SignPath OSS](https://signpath.io/open-source) установщик остаётся неподписанным. Windows SmartScreen покажет предупреждение:

> **Система Windows защитила ваш компьютер** — Microsoft Defender SmartScreen предотвратил запуск неопознанного приложения.

Чтобы продолжить, нажмите **Подробнее** → **Выполнить в любом случае**. Установщик собирается публично в GitHub Actions ([windows-installer.yml](https://github.com/maziggy/bambuddy/blob/main/.github/workflows/windows-installer.yml)), поэтому происхождение сборки можно проверить.

После внедрения подписи предупреждение исчезнет, когда первые подписанные выпуски за короткое время наберут репутацию SmartScreen.

---

## :material-cog-play: Порядок установки {#installer-flow}

После запуска `.exe`:

1. **Запрос UAC** — права администратора необходимы для регистрации службы Windows и записи в `C:\Program Files\` и `C:\ProgramData\`. Запрос появляется только при установке; для обычной работы права администратора не нужны.
2. **Лицензия и каталог установки** — по умолчанию `C:\Program Files\Bambuddy\`.
3. **Необязательное правило брандмауэра** — флажок добавляет входящее правило Windows Firewall для TCP `8000`, чтобы устройства локальной сети могли открыть панель. По умолчанию включено. Снимите флажок, если нужен доступ только с этого компьютера через loopback.
4. **Необязательный ярлык на рабочем столе** — по умолчанию отключён.
5. **Установка** — файлы копируются, служба Bambuddy регистрируется и запускается.
6. **Открытие браузера** — браузер по умолчанию переходит на `http://localhost:8000`.

---

## :material-folder-cog: Структура установки {#installation-layout}

```text
C:\Program Files\Bambuddy\        Install root (managed by installer)
├── python\                       Embedded Python 3.13 + venv
├── app\
│   ├── backend\                  Backend source
│   ├── static\                   Frontend bundle (served by FastAPI)
│   └── gcode_viewer\             3D preview iframe assets
├── bin\
│   ├── nssm.exe                  Windows service supervisor
│   ├── ffmpeg.exe                Timelapse / camera processing
│   └── ffprobe.exe
├── service\                      Service install/uninstall scripts
├── bambuddy.ico                  App icon
└── unins000.exe                  Uninstaller

C:\ProgramData\Bambuddy\          Data root (preserved on uninstall)
├── data\
│   ├── bambuddy.db               SQLite database
│   ├── archive\                  Archived 3MF files + thumbnails
│   └── plate_calibration\        Plate detection cache
└── logs\
    ├── service-stdout.log        Стандартный вывод uvicorn, записанный NSSM
    ├── service-stderr.log        NSSM-captured uvicorn stderr
    └── *.log                     Bambuddy application logs
```

Благодаря разделению `Program Files` (после установки только для чтения, заменяется при обновлении) и `ProgramData` (постоянные данные пользователя) **обновления не затрагивают базу данных и архивы**.

---

## :material-cog-play: Служба Windows {#windows-service}

Bambuddy работает как служба Windows `Bambuddy`, настроенная на автоматический запуск при загрузке. Служба работает от имени `LocalSystem`.

| Параметр | Значение |
|---------|-------|
| Имя службы | `Bambuddy` |
| Отображаемое имя | `Bambuddy` |
| Тип запуска | Автоматический |
| Учётная запись | `LocalSystem` |
| Перезапуск при сбое | Да (управляется NSSM, задержка 5 секунд) |

!!! info "Почему LocalSystem?"
    Необязательной функции виртуального принтера нужно привязывать привилегированные порты (322 / 990 / 8883) для прокси камеры RTSPS, файлового сервера FTPS и брокера MQTT, эмулирующих принтер Bambu для загрузок из слайсера. В Windows порты ниже 1024 может привязать только `LocalSystem` или учётная запись с аналогичными правами. Если виртуальный принтер не используется, эти права не задействуются; они позволяют включить функцию позднее без смены учётной записи службы.

### Управление службой {#managing-the-service}

```powershell
Get-Service Bambuddy            # Status
Start-Service Bambuddy          # Start
Stop-Service Bambuddy           # Stop
Restart-Service Bambuddy        # Restart
```

Или через графический интерфейс: откройте `services.msc` и найдите `Bambuddy`.

---

## :material-update: Обновление {#updating}

Для обновления **скачайте и запустите новый установщик поверх существующей установки**. Inno Setup обнаружит предыдущую установку, остановит службу Bambuddy, заменит файлы программы, повторно зарегистрирует и запустит службу.

- Данные в `C:\ProgramData\Bambuddy\` не изменяются.
- Настройки (уведомления, интеграции, токены OAuth) сохраняются.
- Служба останавливается примерно на 5 секунд при замене файлов.

```text
1. Скачайте новый bambuddy-windows-x64-setup.exe
2. Запустите его (появится запрос UAC)
3. Пройдите мастер установки (существующая конфигурация будет обнаружена)
4. Служба автоматически перезапустится с новой версией
```

Выполнять `Stop-Service` вручную не нужно: установщик делает это перед установкой. Если всё же появится ошибка «файл используется», остановите службу вручную (`Stop-Service Bambuddy` в PowerShell администратора) и нажмите «Повторить».

---

## :material-trash-can: Удаление {#uninstalling}

Стандартный порядок удаления в Windows:

- **Параметры → Приложения** → найдите «Bambuddy» → **Удалить**, или
- **Панель управления → Программы и компоненты** → Bambuddy → **Удалить**

Программа удаления останавливает службу, удаляет правило Windows Firewall и каталог `C:\Program Files\Bambuddy\`, а также отменяет регистрацию службы.

!!! info "По умолчанию ваши данные сохраняются"
    `C:\ProgramData\Bambuddy\` (база данных, архивы и журналы) при удалении **не удаляется**. При последующей установке Bambuddy данные сохранятся. Чтобы удалить всё, после деинсталляции удалите папку `C:\ProgramData\Bambuddy\` вручную.

---

## :material-file-document: Журналы {#logs}

В Windows Bambuddy записывает три потока журнала:

| Файл | Источник |
|------|--------|
| `C:\ProgramData\Bambuddy\logs\service-stdout.log` | Стандартный вывод uvicorn, записанный NSSM |
| `C:\ProgramData\Bambuddy\logs\service-stderr.log` | Стандартный вывод ошибок uvicorn и трассировки Python, записанные NSSM |
| `C:\ProgramData\Bambuddy\logs\bambuddy.log` | Собственный журнал приложения Bambuddy с ротацией |

Быстрый доступ к журналам:

```powershell
# Tail the service stdout
Get-Content "C:\ProgramData\Bambuddy\logs\service-stdout.log" -Tail 100 -Wait

# Tail Bambuddy's app log
Get-Content "C:\ProgramData\Bambuddy\logs\bambuddy.log" -Tail 100 -Wait
```

Ярлык **Bambuddy → Bambuddy Logs** в меню «Пуск» открывает папку журналов в проводнике для быстрой диагностики.

---

## :material-firewall: Брандмауэр Windows {#material-firewall-windows-firewall}

Если при установке выбран пункт добавления правила брандмауэра, создаётся входящее правило TCP для порта 8000:

| Параметр | Значение |
|---------|-------|
| Имя | `Bambuddy Dashboard` |
| Направление | Входящее |
| Протокол | TCP |
| Порт | 8000 |
| Действие | Разрешить |

Это делает панель доступной с других устройств локальной сети по адресу `http://<windows-host-ip>:8000`. Программа удаления удаляет правило.

!!! warning "По умолчанию аутентификация Bambuddy отключена"
    При активном правиле любой пользователь локальной сети может открыть панель Bambuddy без учётных данных. Прежде чем использовать доступ по сети, особенно в общей или гостевой сети, откройте **Настройки → Безопасность** на панели и включите аутентификацию.

---

## :material-lan: Виртуальный принтер и сетевые интерфейсы {#virtual-printer-network-interfaces}

При добавлении виртуального принтера список **Интерфейс привязки** показывает все IPv4-адреса хоста Windows: отдельную запись для каждого адреса, а не адаптера. Поэтому у сетевой карты с несколькими адресами будет несколько вариантов привязки. Дополнительные адреса после первого помечены `[alias]`. В Windows могут появиться записи, которых нет в Linux:

- `Ethernet`, `Wi-Fi` — физические адаптеры локальной сети
- `vEthernet (WSL)`, `vEthernet (Default Switch)` — виртуальные коммутаторы Hyper-V
- `Tailscale` — если установлен Tailscale
- `Bluetooth Network Connection` — если подключение активно

Это сделано намеренно: Bambuddy не фильтрует виртуальные адаптеры Windows, поскольку они могут понадобиться для привязки виртуального принтера. Например, интерфейс Tailscale используется для удалённой нарезки через tailnet, а WSL — для нарезки инструментами Linux внутри WSL.

В большинстве случаев выбирайте физический адаптер локальной сети в той же подсети, что и принтер Bambu.

Каждому включённому виртуальному принтеру нужен собственный адрес. Добавьте дополнительные адреса на используемый адаптер из PowerShell с правами администратора:

```powershell
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 192.168.1.101 -PrefixLength 24
```

Адреса сохраняются после перезагрузки и появляются в списке после обновления страницы настроек. Подробная инструкция приведена на [странице виртуального принтера](../features/virtual-printer.md#dedicated-bind-ip).

!!! warning "Не добавляйте сетевые карты в одну подсеть"
    Второй сетевой адаптер вместо второго адреса работает только в отдельной подсети или VLAN. Несколько адаптеров одного компьютера в одной подсети вызывают ARP flux: `route print -4` показывает маршрут для каждого адаптера, `netstat -a` показывает каждый порт в состоянии LISTENING на каждом IP-адресе, но другие компьютеры могут достичь только одного из них. Режим weak host не помогает.

---

## :material-alert-circle: Устранение неполадок {#troubleshooting}

### Ошибка занятого файла при обновлении {#installer-file-in-use-error-during-update}

Если при обновлении появляются ошибки «доступ запрещён» или «файл используется», прежняя служба Bambuddy всё ещё удерживает `python.exe` или его DLL. Обычно установщик останавливает службу автоматически, но если этого не произошло:

```powershell
# Admin PowerShell
Stop-Service Bambuddy
```

Затем нажмите **Повторить** в окне установщика.

### Служба не запускается после установки {#service-wont-start-after-install}

Сначала проверьте журнал ошибок NSSM:

```powershell
Get-Content "C:\ProgramData\Bambuddy\logs\service-stderr.log" -Tail 50
```

Распространённые причины:

- **Порт 8000 уже занят** другим приложением. Остановите его или измените порт Bambuddy через переменную окружения. Удобный интерфейс смены порта запланирован для v1.1; пока измените `C:\Program Files\Bambuddy\service\install-service.bat` и снова запустите его от администратора для регистрации службы на другом порту.
- **Антивирус блокирует встроенный Python**, помещая `python.exe` в карантин. Добавьте `C:\Program Files\Bambuddy\` в исключения антивируса.

### Браузер не открывается после установки {#browser-doesnt-open-at-end-of-install}

Автоматическое открытие может не сработать при необычной настройке браузера по умолчанию, например при использовании программы запуска для конфиденциальности. Используйте ярлык **Bambuddy → Open Bambuddy Dashboard** в меню «Пуск» или откройте `http://localhost:8000` в любом браузере.

### Список виртуального принтера пуст {#virtual-printer-dropdown-is-empty}

Если при добавлении виртуального принтера интерфейсы не отображаются, перезапустите службу и обновите страницу. В некоторых конфигурациях Windows (при строгих правилах брандмауэра или защите конечных устройств) `psutil.net_if_addrs()` возвращает устаревшие данные до перезапуска службы.

### Полное удаление {#removing-everything-cleanly}

```powershell
# 1. Uninstall via Settings → Apps
# 2. Then wipe persistent data:
Remove-Item "C:\ProgramData\Bambuddy" -Recurse -Force
```

---

## :material-source-branch: Сборка из исходного кода {#build-from-source}

Установщик собирается в CI с помощью публичного процесса GitHub Actions [`.github/workflows/windows-installer.yml`](https://github.com/maziggy/bambuddy/blob/main/.github/workflows/windows-installer.yml). Компоненты сборки:

- [`installers/windows/build.py`](https://github.com/maziggy/bambuddy/blob/main/installers/windows/build.py) — подготавливает встроенный Python, зависимости, фронтенд, включённый NSSM и ffmpeg
- [`installers/windows/bambuddy.iss`](https://github.com/maziggy/bambuddy/blob/main/installers/windows/bambuddy.iss) — скрипт компилятора Inno Setup
- [`installers/windows/vendor/nssm.exe`](https://github.com/maziggy/bambuddy/tree/main/installers/windows/vendor) — включённый в проект NSSM 2.24

Для локальной сборки нужны Windows 10/11, Python 3.11+, Node.js 22 и Inno Setup 6. Порядок сборки описан в [`installers/windows/README.md`](https://github.com/maziggy/bambuddy/blob/main/installers/windows/README.md).

---

## :material-arrow-right-circle: Следующие шаги {#next-steps}

После установки:

1. Откройте Bambuddy по адресу `http://localhost:8000`
2. Добавьте первый принтер Bambu — см. [Настройка первого принтера](first-printer.md)
3. При необходимости включите аутентификацию в разделе **Настройки → Безопасность**
