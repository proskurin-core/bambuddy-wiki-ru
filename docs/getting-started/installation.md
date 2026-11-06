---
title: Установка
description: Установите Bambuddy в своей системе
---

# Установка {#installation}

> Запустите Bambuddy примерно за минуту: одна команда, одна вкладка браузера — и всё готово.

---

## :material-youtube: Видеоруководство {#video-walkthrough}

Предпочитаете смотреть, а не читать? **[@Help3d](https://www.youtube.com/@Help3d)** показывает весь процесс: от первоначальной настройки Raspberry Pi до печати через виртуальные принтеры, включая HTTPS через Tailscale, SSH, установку и импорт сертификата.

<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; border-radius: 12px; box-shadow: 0 4px 24px rgba(0, 0, 0, 0.3); max-width: 900px; margin: 1rem auto;">
  <iframe
    src="https://www.youtube-nocookie.com/embed/MTIPwPii8HA"
    title="Bambuddy — полное руководство (автор @Help3d)"
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; border: 0;"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen
    loading="lazy"></iframe>
</div>

<p style="text-align: center; font-size: 0.9rem; opacity: 0.8;">Видео <a href="https://www.youtube.com/@Help3d" target="_blank" rel="noopener">@Help3d</a>: настройка Pi, SSH, установка, HTTPS через Tailscale, виртуальные принтеры и импорт сертификата.</p>

---

## :material-rocket: Быстрая установка (рекомендуется) {#quick-install-recommended}

Самый простой способ установки Bambuddy: интерактивные скрипты выполнят все действия за вас.

=== ":material-linux: Linux"

    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/install.sh -o install.sh && chmod +x install.sh && ./install.sh
    ```

=== ":material-apple: macOS"

    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/install.sh -o install.sh && chmod +x install.sh && ./install.sh
    ```

    !!! warning "Не используйте `sudo` в macOS"
        Запустите скрипт от своего обычного пользователя, **без** `sudo`. Homebrew не работает от root, а агент launchd, работающий от вашего имени, не сможет управлять виртуальным окружением, созданным root. Установщик самостоятельно повышает права только там, где это необходимо. По умолчанию в macOS Bambuddy устанавливается в `~/bambuddy`, не записывая файлы в `/opt` и не требуя прав root.

=== ":material-microsoft-windows: Windows"

    Скачайте автономный установщик: Python, Node, Docker и Git на целевом компьютере не нужны:

    [:material-download: bambuddy-windows-x64-setup.exe](https://github.com/maziggy/bambuddy/releases/latest/download/bambuddy-windows-x64-setup.exe){ .md-button .md-button--primary }

    Запустите его и подтвердите однократный запрос UAC для установки с правами администратора. Bambuddy регистрируется как служба Windows, доступная на `http://localhost:8000`; по завершении установки браузер откроется автоматически.

    Для обновления запустите новый установщик поверх существующей установки. База данных и архивы в `C:\ProgramData\Bambuddy\` сохранятся.

    [:material-arrow-right: Полное руководство по установщику Windows](windows-installer.md)

Скрипт выполнит следующее:

- Запросит путь установки, порт, адрес привязки, часовой пояс и другие параметры
- Определит менеджер пакетов и установит зависимости
- Настроит виртуальное окружение Python
- Соберёт фронтенд (Node.js 22)
- Создаст службу systemd/launchd
- Автоматически запустит Bambuddy

!!! info "Поддерживаемые системы"
    - **Debian/Ubuntu** (apt) с доступным Python 3.11 или новее: Debian 12+, Ubuntu 22.04+ (на 22.04 установщик добавляет `python3.11`), Raspberry Pi OS bookworm+. В Debian 11 / Raspberry Pi OS bullseye нет Python 3.11: используйте Docker.
    - **RHEL/Fedora/CentOS** (dnf/yum)
    - **Arch Linux** (pacman)
    - **openSUSE** (zypper)
    - **macOS** (Homebrew)
    - **Windows 10/11** (PowerShell 5.1+, Git/Python via `winget`)

!!! tip "Автоматическая установка"
    Для автоматизации или CI используйте флаг `--yes`, чтобы принять все значения по умолчанию:
    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/install.sh | bash -s -- --yes
    ```

    Также можно указать параметры с помощью флагов:
    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/install.sh | bash -s -- --path /srv/bambuddy --port 3000 --yes
    ```

---

## :material-check-all: Требования {#requirements}

Перед началом убедитесь, что выполнены следующие требования:

| Требование | Описание |
|------------|---------|
| **Python** | 3.11+ (рекомендуется 3.12 или 3.13) — нужен только для ручной или нативной установки; Docker не требует Python на хосте |
| **Сеть** | Та же локальная сеть, что и у принтера Bambu Lab |
| **Принтер** | Включён режим разработчика ([см. руководство](index.md#enabling-developer-mode)) |
| **SD-карта** | Вставлена в принтер (необходима для передачи файлов) |

!!! warning "Необходима SD-карта"
    Для правильной работы Bambuddy в принтер должна быть вставлена SD-карта. Она необходима для передачи файлов, загрузки заданий на печать и архивации.

!!! tip "Установка через Docker"
    Если вы предпочитаете контейнеры, ознакомьтесь с [руководством по установке Docker](docker.md) — это ещё проще!

---

## :material-download: Ручная установка {#manual-install}

Хотите выполнить установку самостоятельно? Следуйте этим инструкциям.

=== ":material-apple: macOS"

    ```bash
    # Install prerequisites (if needed)
    brew install python@3.12

    # Clone and setup
    git clone https://github.com/maziggy/bambuddy.git
    cd bambuddy
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.lock

    # Run (--loop asyncio avoids a uvloop TLS bug that can truncate
    # Virtual Printer FTP uploads on slow storage — see #1896)
    uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 --loop asyncio
    ```

=== ":material-ubuntu: Ubuntu/Debian"

    ```bash
    # Install prerequisites (Ubuntu 22.04, whose python3 is 3.10:
    # install python3.11 python3.11-venv and use python3.11 below)
    sudo apt update
    sudo apt install python3 python3-venv python3-pip git

    # Clone and setup
    git clone https://github.com/maziggy/bambuddy.git
    cd bambuddy
    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.lock

    # Run (--loop asyncio avoids a uvloop TLS bug that can truncate
    # Virtual Printer FTP uploads on slow storage — see #1896)
    uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 --loop asyncio
    ```

Откройте в браузере `http://localhost:8000`. Если Bambuddy работает на другом компьютере, замените `localhost` именем или IP-адресом сервера. :tada:

---

## :material-cog: Запуск в качестве службы {#running-as-a-service}

Для рабочего использования запускайте Bambuddy как системную службу с автоматическим запуском.

=== ":material-linux: systemd (Linux)"

    Создайте файл службы:

    ```bash
    sudo nano /etc/systemd/system/bambuddy.service
    ```

    Добавьте следующее содержимое, указав пути для своей системы:

    ```ini
    [Unit]
    Description=BamBuddy Print Archive
    After=network.target

    [Service]
    Type=simple
    User=YOUR_USERNAME
    Group=YOUR_USERNAME
    WorkingDirectory=/home/YOUR_USERNAME/bambuddy
    Environment="PATH=/home/YOUR_USERNAME/bambuddy/venv/bin"

    # Kill any zombie ffmpeg processes before starting/after stopping
    # The - prefix ignores errors if no ffmpeg processes exist
    ExecStartPre=-/usr/bin/pkill -9 ffmpeg
    ExecStopPost=-/usr/bin/pkill -9 ffmpeg

    # Ensure directories exist and have correct permissions
    # The + prefix runs the command as root even though User= is set
    ExecStartPre=+/bin/mkdir -p /home/YOUR_USERNAME/bambuddy/logs
    ExecStartPre=+/bin/mkdir -p /home/YOUR_USERNAME/bambuddy/archive
    ExecStartPre=+/bin/chown -R YOUR_USERNAME:YOUR_USERNAME /home/YOUR_USERNAME/bambuddy/logs
    ExecStartPre=+/bin/chown -R YOUR_USERNAME:YOUR_USERNAME /home/YOUR_USERNAME/bambuddy/archive

    # --loop asyncio required: uvloop can truncate VP FTP uploads on slow storage (#1896)
    ExecStart=/home/YOUR_USERNAME/bambuddy/venv/bin/uvicorn backend.app.main:app --host 0.0.0.0 --port 8000 --loop asyncio
    Restart=always
    RestartSec=10

    # Required for Virtual Printer mode — see the note below
    AmbientCapabilities=CAP_NET_BIND_SERVICE

    [Install]
    WantedBy=multi-user.target
    ```

    !!! warning "Не удаляйте строку `AmbientCapabilities`"
        Виртуальный принтер открывает порты 990 (FTPS) и 322 (RTSP), оба ниже 1024. Служба обычного пользователя не может сделать это без данного разрешения. Без него Bambuddy работает, но сокеты виртуального принтера не открываются и слайсер не видит принтер; явной причины при этом не видно. Ошибка привязки записывается одной строкой в `journalctl -u bambuddy`.

        Если у существующей службы этой строки нет, добавьте её и выполните `sudo systemctl daemon-reload && sudo systemctl restart bambuddy`.

    !!! tip "Зачем завершать ffmpeg?"
        Bambuddy использует ffmpeg для трансляции с камеры. При перезапуске службы процессы ffmpeg иногда остаются без родительского процесса. Команды `ExecStartPre` и `ExecStopPost` обеспечивают корректный перезапуск.

    Включите и запустите службу:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable bambuddy
    sudo systemctl start bambuddy
    ```

    Проверьте состояние:

    ```bash
    sudo systemctl status bambuddy
    ```

    Просмотрите журнал:

    ```bash
    sudo journalctl -u bambuddy -f
    ```

=== ":material-apple: launchd (macOS)"

    Создайте файл plist:

    ```bash
    nano ~/Library/LaunchAgents/com.bambuddy.plist
    ```

    Добавьте следующее содержимое:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.bambuddy</string>
        <key>ProgramArguments</key>
        <array>
            <string>/Users/YOUR_USERNAME/bambuddy/venv/bin/uvicorn</string>
            <string>backend.app.main:app</string>
            <string>--host</string>
            <string>0.0.0.0</string>
            <string>--port</string>
            <string>8000</string>
            <!-- the loop asyncio flag below is required: uvloop can truncate VP FTP uploads, #1896 -->
            <string>--loop</string>
            <string>asyncio</string>
        </array>
        <key>WorkingDirectory</key>
        <string>/Users/YOUR_USERNAME/bambuddy</string>
        <key>RunAtLoad</key>
        <true/>
        <key>KeepAlive</key>
        <true/>
    </dict>
    </plist>
    ```

    Загрузите и запустите службу:

    ```bash
    launchctl load ~/Library/LaunchAgents/com.bambuddy.plist
    ```

    !!! warning "Если принтер недоступен только при запуске Bambuddy через launchd"

        macOS привязывает разрешение на доступ к локальной сети к подписи кода. Процесс launchd проверяется самостоятельно и не наследует разрешение Терминала. На компьютерах Intel Homebrew распространяет Python без подписи, поэтому привязать разрешение не к чему: соединения с принтером обрываются без ошибки и запроса разрешения. Веб-интерфейс работает, принтер остаётся отключённым, а диагностика сообщает о недоступности всех портов при успешной проверке подсети.

        Признак проблемы — немедленное подключение при запуске той же команды из Терминала. Чтобы проверить и исправить это:

        ```bash
        # Which interpreter macOS actually judges
        BASE=$(venv/bin/python3 -c 'import os, sys; print(os.path.realpath(sys._base_executable))')
        codesign -dv "$BASE"          # "code object is not signed at all" means this is the problem
        codesign --force --sign - "$BASE"
        launchctl unload ~/Library/LaunchAgents/com.bambuddy.plist
        launchctl load ~/Library/LaunchAgents/com.bambuddy.plist
        ```

        `install.sh` и `install/update_macos.sh` выполняют это автоматически. Программа обновления проверяет подпись при каждом запуске, поскольку `brew upgrade python` устанавливает новый неподписанный двоичный файл по новому пути с версией. Подписывайте только неподписанные файлы. На Apple Silicon каждый двоичный файл уже имеет ad-hoc подпись, идентификатор которой является хешем файла; повторная подпись меняет его и отзывает работавшее разрешение.

        На Apple Silicon у того же симптома другая причина и другое решение: подпись существует, но после обновления Python её идентификатор изменился, поэтому macOS видит новое приложение. Разрешите ему доступ в **Системные настройки > Конфиденциальность и безопасность > Локальная сеть**.

---

## :material-tune: Настройка {#configuration}

Настройте Bambuddy с помощью переменных окружения или файла `.env`:

```bash
cp .env.example .env
nano .env
```

### Переменные окружения {#environment-variables}

| Переменная | По умолчанию | Описание |
|----------|---------|-------------|
| `DEBUG` | `false` | Включить режим отладки (подробный журнал) |
| `LOG_LEVEL` | `INFO` | Уровень журнала: `DEBUG`, `INFO`, `WARNING`, `ERROR` |
| `LOG_TO_FILE` | `true` | Записывать журнал в `logs/bambuddy.log` |

### Настройки для рабочего использования (по умолчанию) {#production-settings-default}

- Журналирование уровня INFO
- Подавлены лишние сообщения библиотек SQLAlchemy и HTTP
- Журналы записываются в `logs/bambuddy.log` (ротация при 5 МБ, 3 резервные копии)

### Настройки для разработки {#development-settings}

Для отладки создайте файл `.env`:

```bash
DEBUG=true
LOG_TO_FILE=true
```

Это включает:

- Журналирование уровня DEBUG (очень подробное)
- Запись всех SQL-запросов
- Возможность диагностики подключения принтера

---

## :material-network: Требования к сети {#network-requirements}

Убедитесь, что межсетевой экран разрешает следующие соединения:

| Порт | Протокол | Направление | Назначение |
|------|----------|-----------|---------|
| 8000 | HTTP | Входящее | Веб-интерфейс Bambuddy |
| 8883 | MQTT/TLS | Исходящее | Связь с принтером |
| 990 | FTPS | Исходящее | Передача файлов с принтера |
| 2024-2026 | TCP | Исходящее | Проприетарные порты слайсера (A1/P1S) |

!!! tip "Доступ с других устройств"
    Для доступа к Bambuddy с других устройств сети используйте IP-адрес сервера вместо `localhost`. Например: `http://192.168.1.100:8000`

---

## :material-shield-lock: Обратный прокси {#reverse-proxy}

Bambuddy обслуживает обычный HTTP на порту 8000. Используйте перед ним обратный прокси для HTTPS, удобного имени хоста или доступа через интернет. Это относится к быстрой, ручной установке и Docker; меняется только адрес назначения (`localhost:8000` для установки на хосте и имя контейнера для Docker Compose).

!!! warning "Необходима поддержка WebSocket"
    Обновления состояния принтера в реальном времени используют WebSocket. Прокси должен передавать заголовки `Upgrade`/`Connection` и разрешать длительные соединения.

!!! warning "Не проксируйте порты FTP, MQTT, SSDP и привязки"
    Nginx и Caddy проксируют только HTTP(S). Порты FTP/MQTT/SSDP и привязки виртуального принтера должны оставаться напрямую доступными в локальной сети — см. [Виртуальный принтер → Необходимые порты](../features/virtual-printer.md#required-ports).

### Nginx {#nginx}

```nginx
server {
    listen 443 ssl http2;
    server_name bambuddy.yourdomain.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://localhost:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # WebSocket / long poll timeout
        proxy_read_timeout 86400;
    }
}
```

### Caddy {#caddy}

Caddy — более простой вариант с автоматическим HTTPS через Let's Encrypt. WebSocket и необходимые заголовки (`X-Forwarded-For`, `X-Forwarded-Proto`, `Host`) работают без дополнительной настройки.

=== "Публичный домен (автоматический HTTPS)"

    `/etc/caddy/Caddyfile`:

    ```caddy
    bambuddy.yourdomain.com {
        reverse_proxy localhost:8000
    }
    ```

    Caddy автоматически получает и обновляет сертификат Let's Encrypt, если порты 80 и 443 доступны из интернета, а DNS указывает на ваш сервер.

=== "Локальная сеть (самоподписанный сертификат)"

    `/etc/caddy/Caddyfile`:

    ```caddy
    bambuddy.local {
        reverse_proxy localhost:8000
        tls internal
    }
    ```

    `tls internal` заставляет Caddy выпустить сертификат собственного локального центра сертификации. Установите корневой сертификат Caddy на клиентах (`caddy trust` на том же компьютере или скопируйте `/var/lib/caddy/.local/share/caddy/pki/authorities/local/root.crt` на другие устройства), чтобы избежать предупреждений браузера.

=== "Docker Compose"

    ```yaml
    services:
      caddy:
        image: caddy:2
        restart: unless-stopped
        ports:
          - "80:80"
          - "443:443"
        volumes:
          - ./Caddyfile:/etc/caddy/Caddyfile:ro
          - caddy_data:/data
          - caddy_config:/config
        networks:
          - bambuddy_net

    volumes:
      caddy_data:
      caddy_config:
    ```

    В Caddyfile укажите имя службы Bambuddy в Compose вместо localhost:

    ```caddy
    bambuddy.yourdomain.com {
        reverse_proxy bambuddy:8000
    }
    ```

После изменения Caddyfile перезагрузите конфигурацию без простоя:

```bash
sudo systemctl reload caddy
# or, in Docker:
docker compose exec caddy caddy reload --config /etc/caddy/Caddyfile
```

!!! tip "Загрузка больших файлов"
    Если загрузка 3MF/G-code по медленному соединению прерывается по тайм-ауту, увеличьте ограничение тела запроса Caddy, добавив `request_body { max_size 500MB }` в блок сайта.

---

## :material-update: Обновление {#updating}

!!! warning "Однократное замечание для перехода с 0.2.2.x на 0.2.3"
    Кнопка **Обновить** в приложении не всегда надёжно выполняет именно эту миграцию. Выполните это обновление из командной строки по приведённым ниже путям. После перехода на 0.2.3 кнопка обновления снова будет работать для последующих версий.

### Рекомендуется: `update.sh` {#recommended-updatesh}

```bash
sudo /opt/bambuddy/install/update.sh
```

`update.sh` останавливает службу, создаёт снимок базы через встроенный API резервного копирования, обновляет код до `origin/main`, устанавливает зависимости Python, пересобирает фронтенд и перезапускает службу. При ошибке на любом этапе выполняется автоматический откат.

### Ручное обновление {#manual-update}

Если вы хотите выполнить шаги самостоятельно:

```bash
cd /opt/bambuddy
sudo systemctl stop bambuddy
sudo -u bambuddy git fetch origin
sudo -u bambuddy git reset --hard origin/main
sudo -u bambuddy venv/bin/pip install -r requirements.lock
sudo systemctl start bambuddy
```

Если каталог установки отличается, замените `/opt/bambuddy` своим путём. Миграции схемы базы данных запускаются автоматически при старте.

!!! tip "Установлено из ZIP-архива GitHub?"
    В такой установке нет каталога `.git`, поэтому оба описанных способа недоступны. Порядок резервного копирования и переустановки см. в [UPDATING.md](https://github.com/maziggy/bambuddy/blob/main/UPDATING.md#installed-from-a-github-zip-or-tarball-download).

---

## :material-folder-cog: Сборка фронтенда из исходного кода {#build-frontend-from-source}

Репозиторий включает готовые файлы фронтенда. Для сборки из исходного кода:

```bash
# Install Node.js 18+ first
cd frontend
npm install
npm run build
cd ..
```

---

## :checkered_flag: Следующие шаги {#next-steps}

Теперь, когда Bambuddy установлен:

<div class="quick-start" markdown>

[:material-printer-3d: **Добавить принтер**<br><small>Подключите первый принтер</small>](first-printer.md)

[:material-docker: **Попробовать Docker**<br><small>Ещё более простая установка</small>](docker.md)

[:material-help-circle: **Устранение неполадок**<br><small>Проблемы с установкой?</small>](../reference/troubleshooting.md)

</div>
