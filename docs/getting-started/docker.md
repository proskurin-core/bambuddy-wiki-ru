---
title: Установка через Docker
description: Разверните Bambuddy с помощью Docker одной командой
---

# Установка через Docker {#docker-installation}

Docker — самый простой способ запустить Bambuddy. Достаточно одной команды!

---

## :rocket: Быстрый старт {#quick-start}

=== ":material-script: Скрипт установки (Linux / macOS)"

    Интерактивный скрипт запросит параметры и выполнит настройку. Требуется совместимая с bash оболочка в Linux, macOS или WSL в Windows. **Для нативной Windows используйте вкладку готового образа ниже.**

    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/docker-install.sh -o docker-install.sh && chmod +x docker-install.sh && ./docker-install.sh
    ```

    Скрипт выполнит следующие действия:

    - Запросит путь установки, порт, адрес привязки и часовой пояс
    - Скачает docker-compose.yml (или клонирует репозиторий при сборке из исходного кода)
    - Создаст файл .env с вашими настройками
    - Запустит контейнер

    !!! tip "Автоматическая установка"
        Для автоматизации или CI используйте флаг `--yes`, чтобы принять все значения по умолчанию:
        ```bash
        curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/docker-install.sh | bash -s -- --yes
        ```

        Либо задайте параметры флагами:
        ```bash
        curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/docker-install.sh | bash -s -- --path /srv/bambuddy --port 3000 --yes
        ```

=== ":material-download: Готовый образ (вручную, также работает в Windows)"

    Самый быстрый способ — сборка не нужна. Работает в Linux, macOS и нативном PowerShell Windows (WSL не требуется):

    ```bash
    # Linux / macOS / WSL
    mkdir bambuddy && cd bambuddy
    curl -O https://raw.githubusercontent.com/maziggy/bambuddy/main/docker-compose.yml
    docker compose up -d
    ```

    ```powershell
    # Native Windows PowerShell
    mkdir bambuddy
    cd bambuddy
    Invoke-WebRequest -Uri https://raw.githubusercontent.com/maziggy/bambuddy/main/docker-compose.yml -OutFile docker-compose.yml
    docker compose up -d
    ```

    ```bash
    # Alternative option for QNAP when using docker-compose.yml is not working. Remember to set correct TZ (timezone)
    export CONFIG_PATH="/share/configs" # Your config directory path
    docker pull ghcr.io/maziggy/bambuddy:daily
    docker run -d \
       --name bambuddy \
       --restart=unless-stopped \
       --net=host \
       -e PUID=$(id -u) \
       -e PGID=$(id -g) \
       -v "$CONFIG_PATH/bambuddy/data:/app/data" \
       -v "$CONFIG_PATH/bambuddy/logs:/app/logs" \
       -e TZ=Europe/Warsaw \
       ghcr.io/maziggy/bambuddy:daily
    ```

    !!! success "Поддержка разных архитектур"
        Готовые образы доступны для:

        - **linux/amd64** — серверы и компьютеры Intel/AMD, большинство VPS
        - **linux/arm64** — Raspberry Pi 4/5, Mac с Apple Silicon, AWS Graviton

        Docker автоматически скачает подходящий образ.

=== ":material-source-branch: Сборка из исходного кода"

    Чтобы собрать образ локально или изменить код:

    ```bash
    git clone https://github.com/maziggy/bambuddy.git
    cd bambuddy
    docker compose up -d --build
    ```

Откройте в браузере `http://<your-host>:8000`, заменив `<your-host>` IP-адресом или именем компьютера с Bambuddy. В Linux при использовании сети хоста работает и `localhost`; в Docker Desktop (macOS/Windows) он **не работает** — см. раздел [macOS и Windows](#macos-and-windows-docker-desktop) ниже. :tada:

---

## :material-cog: Настройка {#configuration}

### docker-compose.yml {#docker-composeyml}

Стандартный `docker-compose.yml` работает без изменений:

```yaml
services:
  bambuddy:
    image: ghcr.io/maziggy/bambuddy:latest
    build: .
    # Usage:
    #   docker compose up -d          → pulls pre-built image
    #   docker compose up -d --build  → builds from source
    container_name: bambuddy
    network_mode: host  # Recommended for automatic printer discovery
    volumes:
      - bambuddy_data:/app/data
      - bambuddy_logs:/app/logs
    environment:
      - TZ=Europe/Berlin  # Your timezone
    restart: unless-stopped
    # Docker SIGKILLs after 10s by default. Bambuddy stops well inside that,
    # but the headroom means a slow teardown (several virtual printers on a Pi)
    # can still checkpoint the database instead of being killed mid-write.
    stop_grace_period: 30s

volumes:
  bambuddy_data:
  bambuddy_logs:
```

!!! info "Образ и сборка"
    Если указаны и `image`, и `build`:

    - `docker compose up -d` скачивает готовый образ с `ghcr.io`
    - `docker compose up -d --build` собирает образ локально из исходного кода

### Переменные окружения {#environment-variables}

| Переменная | По умолчанию | Описание |
|----------|---------|-------------|
| `TZ` | `UTC` | Ваш часовой пояс (например, `America/New_York`) |
| `PORT` | `8000` | Порт Bambuddy (при использовании сети хоста) |
| `DEBUG` | `false` | Включить отладочный журнал |
| `LOG_LEVEL` | `INFO` | Уровень журнала: `DEBUG`, `INFO`, `WARNING`, `ERROR` |
| `HA_URL` | _(нет)_ | URL Home Assistant для автоматической интеграции (например, `http://192.168.1.100:8123`) |
| `HA_TOKEN` | _(нет)_ | Долгосрочный токен доступа Home Assistant для автоматической интеграции |
| `TRUSTED_FRAME_ORIGINS` | _(нет)_ | Разделённые запятыми источники, которым разрешено встраивать Bambuddy через `<iframe>` (например, `http://homeassistant.local:8123`). Требуется для панели Webpage в HA. Для встраивания с **другого** источника параметр обязателен; оверлей трансляции `/overlay/{printer}` дополнительно можно встроить из собственного источника Bambuddy без него — так работает предпросмотр в настройках. |
| `DATABASE_URL` | _(нет)_ | Строка подключения внешнего PostgreSQL (например, `postgresql+asyncpg://user:pass@host:5432/bambuddy`). Если не задана, используется встроенный SQLite. |
| `USE_SYSTEM_TRUST_STORE` | _(нет)_ | Включает системное хранилище доверенных сертификатов для HTTPS-запросов |
| `BAMBUDDY_EXTERNAL_ROOTS` | _(нет)_ | Разделённые двоеточиями абсолютные пути, разрешённые как **внешние папки библиотеки** (см. [Файловый менеджер → Внешние папки](../features/file-manager.md#external-folders)). Пустое значение по умолчанию отключает функцию; собственные каталоги данных, журналов и статических файлов Bambuddy всегда отклоняются, даже при слишком широком списке. |

!!! info "Интеграция с Home Assistant"
    Когда заданы оба параметра `HA_URL` и `HA_TOKEN`, интеграция Home Assistant автоматически включается и настраивается. Поля URL и токена в интерфейсе становятся доступными только для чтения. Это в основном используется [дополнением Home Assistant](https://github.com/hobbypunk90/homeassistant-addon-bambuddy/) для настройки без ручных действий.

!!! tip "Поддержка самоподписанного сертификата CA"
    По умолчанию Bambuddy использует встроенное хранилище доверия библиотеки httpx для всех HTTPS-запросов. При использовании самостоятельного экземпляра Home Assistant с самоподписанным сертификатом CA доступ к нему будет отклонён.
    Чтобы добавить собственный сертификат CA, поместите его в каталог и добавьте в файл Docker Compose:

    ```yaml
    volume:
      - /path/to/your/ca-cert-dir:/usr/local/share/ca-certificates:ro
    environment:
      - USE_SYSTEM_TRUST_STORE=true
    ```

    При заданной переменной `USE_SYSTEM_TRUST_STORE` Bambuddy обновит сертификаты CA при запуске и включит использование системного хранилища доверия библиотекой httpx.
    USE_SYSTEM_TRUST_STORE действует только при запуске контейнера от root; если включаете флаг, удалите явно указанную директиву `user:`.

!!! tip "Встраивание Bambuddy в панель Webpage Home Assistant"
    По умолчанию Bambuddy отправляет строгие заголовки, блокирующие iframe (`X-Frame-Options: SAMEORIGIN` и CSP `frame-ancestors 'none'`), для защиты от кликджекинга при доступе из интернета. Это блокирует встраивание Bambuddy в панель Webpage Home Assistant даже в доверенной локальной сети: браузер считает HA на порту 8123 и Bambuddy на порту 8000 разными источниками.

    Чтобы разрешить встраивание из вашего экземпляра HA, задайте:

    ```yaml
    environment:
      - TRUSTED_FRAME_ORIGINS=http://homeassistant.local:8123
    ```

    Замените URL фактическим источником HA (только `scheme://host[:port]`, без путей и подстановочных символов). Несколько источников разделяются запятыми. При заданном параметре `X-Frame-Options` удаляется, а директива CSP `frame-ancestors` содержит `'self'` и указанные источники. В обычных установках Docker и на физическом сервере без этой переменной сохраняются строгие настройки по умолчанию.

    **Home Assistant через HTTPS (Nabu Casa, собственный домен или другой TLS-прокси):** браузеры запрещают встраивать HTTP iframe в страницу HTTPS из-за смешанного содержимого. Chrome и Firefox применяют это ограничение без возможности отключить его для отдельного сайта. Если HA работает по HTTPS, Bambuddy тоже должен быть доступен по HTTPS.

    Самый простой вариант для пользователей HA — **[дополнение Nginx Proxy Manager](https://github.com/hassio-addons/addon-nginx-proxy-manager)** из магазина дополнений HA. NPM использует проверку DNS-01 Let's Encrypt для вашего домена, поэтому перенаправлять порты и открывать Bambuddy в интернет не нужно:

    1. Установите и запустите дополнение Nginx Proxy Manager из магазина HA.
    2. В NPM добавьте **Proxy Host**: домен `bambuddy.<your-domain>`, схема `http`, целевой хост `<bambuddy-host-on-LAN>`, целевой порт `8000`.
    3. В разделе **SSL** запросите сертификат Let's Encrypt через **DNS Challenge** для `bambuddy.<your-domain>` — в NPM доступны Cloudflare, Hetzner, Route53 и многие другие провайдеры DNS.
    4. Установите в Bambuddy `TRUSTED_FRAME_ORIGINS=https://homeassistant.<your-domain>` (или фактический HTTPS-источник HA) и перезапустите его.
    5. Добавьте в HA панель Webpage с `url: https://bambuddy.<your-domain>`.

    Оба соединения используют HTTPS, сертификат является общедоверенным, а Bambuddy остаётся в локальной сети. DNS-01 требует только доступа к API провайдера DNS, но не входящих подключений. Если у вас нет домена, аналогично работают и другие обратные прокси (Caddy, Traefik, обычный nginx вне системы дополнений): Bambuddy должен находиться за доверенным HTTPS-сертификатом, соответствующим URL панели Webpage.

    **Обратные прокси с префиксом пути** (например, `https://example.com/bambuddy/` через Traefik, nginx `location /bambuddy/` или маршрутизацию Cloudflare Tunnel) поддерживаются с v0.2.4b2: ресурсы загружаются корректно под любым подпутём. Вызовы API по-прежнему идут в корень хоста, поэтому прокси должен направлять `/api/v1/*` в тот же Bambuddy, что и `/bambuddy/*`. Обычно Bambuddy размещают на отдельном поддомене либо открывают `/api/v1/` в корне хоста вместе с подпутём.

!!! warning "Встраивание через подпуть HA Ingress или дополнения не поддерживается"
    Система Ingress дополнений Home Assistant предоставляет дополнение по меняющемуся для каждой сессии подпути (`/api/hassio_ingress/<token>/`). Хотя описанное исправление путей позволяет SPA запуститься под этим префиксом, остальная часть SPA рассчитывает на стабильный источник: вызовы API, basename React Router, области действия манифеста PWA и service worker и подписки на push-уведомления привязаны к URL первоначальной установки SPA. Поддержка подпутей для всех этих компонентов потребовала бы переработки запуска SPA и создала бы новые проблемы при установке PWA и перезагрузке прямых ссылок.

    Поддерживается описанное выше встраивание через панель Webpage и `TRUSTED_FRAME_ORIGINS`. Завершение TLS внутри контейнера дополнения HA с самоподписанным сертификатом не является решением: современные Chrome и Edge блокируют такие сертификаты на IP-адресах локальной сети без возможности обхода вроде `thisisunsafe`. Если вы сопровождаете дополнение HA для Bambuddy, предложите пользователю разместить Bambuddy за Nginx Proxy Manager или другим прокси с общедоверенным сертификатом через DNS-01, а затем встроить HTTPS-адрес через Webpage.

!!! tip "Внешняя база данных PostgreSQL"
    По умолчанию Bambuddy использует встроенную базу SQLite без настройки. Для крупных установок или отдельного сервера базы данных задайте `DATABASE_URL` с адресом внешнего PostgreSQL:

    ```yaml
    environment:
      - DATABASE_URL=postgresql+asyncpg://bambuddy:yourpassword@db-host:5432/bambuddy
    ```

    Bambuddy автоматически создаст таблицы при первом запуске. Резервное копирование и восстановление используют `pg_dump`/`pg_restore` вместо копирования файла. Требуется PostgreSQL 13 или новее; см. [PostgreSQL](../features/postgresql.md).

### Внешние папки библиотеки (BAMBUDDY_EXTERNAL_ROOTS) {#external-library-folders-bambuddy_external_roots}

Функция **Файловый менеджер → Добавить внешнюю папку** позволяет подключать каталоги хоста (ресурсы NAS, USB-накопители, локальные каталоги печати) к библиотеке без копирования файлов. С v0.2.5b1 оператор должен **явно включить** эту функцию: задайте в `BAMBUDDY_EXTERNAL_ROOTS` разделённый двоеточиями список путей внутри контейнера, которые пользователям разрешено регистрировать. Пустое значение по умолчанию отключает функцию.

Оператор также должен выполнить bind-монтирование каталога хоста в контейнер по тому же пути, что указан в `BAMBUDDY_EXTERNAL_ROOTS`. Каталоги данных, журналов и статических файлов Bambuddy не могут быть внешними корнями даже при включении в список: они всегда отклоняются для защиты данных пользователей.

Пример: разрешить подключение одного ресурса NAS только для чтения:

```yaml
volumes:
  - /mnt/nas/3d-prints:/external/nas:ro
environment:
  - BAMBUDDY_EXTERNAL_ROOTS=/external/nas
```

Пример: разрешить два корня (NAS и локальные проекты): пути хоста разделены запятыми, а пути внутри контейнера — двоеточиями:

```yaml
volumes:
  - /mnt/nas/3d-prints:/external/nas:ro
  - /srv/library:/external/projects:ro
environment:
  - BAMBUDDY_EXTERNAL_ROOTS=/external/nas:/external/projects
```

Рекомендуется `:ro`, если пользователям не нужно загружать файлы обратно на ресурс хоста. Почему нужно явное разрешение оператора? См. [замечание о безопасности](../features/file-manager.md#external-folders-security-stance): до v0.2.5b1 использовался список запрещённых системных путей, а все остальные пути хоста неявно разрешались. Список разрешённых путей делает монтирование явным и проверяемым решением для каждой установки.

### Пользовательский порт {#custom-port}

=== ":material-linux: Linux (режим сети хоста)"

    При `network_mode: host` задайте переменную окружения PORT:

    ```bash
    PORT=8080 docker compose up -d
    ```

    Или добавьте её в `docker-compose.yml`:

    ```yaml
    environment:
      - TZ=Europe/Berlin
      - PORT=8080
    ```

=== ":material-apple: macOS / :material-microsoft-windows: Windows"

    При сопоставлении портов измените раздел ports:

    ```yaml
    ports:
      - "8080:8000"  # Access on port 8080
    ```

!!! note "Linux: отказано в доступе?"
    При ошибке «отказано в доступе» добавляйте `sudo` перед командами (например, `sudo docker compose up -d`) или [добавьте пользователя в группу docker](https://docs.docker.com/engine/install/linux-postinstall/).

---

## :material-database: Сохранение данных {#data-persistence}

Данные хранятся в трёх томах:

| Том | Назначение |
|--------|---------|
| `bambuddy.db` | База SQLite со всеми данными печати |
| `archive/` | Архивированные файлы 3MF и миниатюры |
| `logs/` | Журналы приложения |

!!! tip "Резервное копирование"
    Для резервного копирования просто скопируйте эти файлы и каталоги. О встроенной функции см. [Резервное копирование и восстановление](../features/backup.md).

---

## :material-update: Обновление {#updating}

!!! info "Обновление из приложения недоступно"
    Установки Docker нельзя обновить из приложения — используйте командную строку.

!!! warning "Сначала проверьте строку `image:`"
    Если в `docker-compose.yml` указан конкретный тег (например, `ghcr.io/maziggy/bambuddy:0.2.2.2`), команда `docker compose pull` снова скачает тот же тег. Перед загрузкой измените его на `:latest` или нужную версию (например, `:0.2.3`).

=== ":material-download: Готовый образ"

    ```bash
    docker compose pull
    docker compose up -d
    ```

=== ":material-source-branch: Сборка из исходного кода"

    ```bash
    cd bambuddy
    git pull
    docker compose build --pull
    docker compose up -d
    ```

    Флаг `--pull` обеспечивает получение последнего базового образа с обновлениями безопасности.

### Копирование команды из интерфейса {#copying-the-command-from-the-ui}

При наличии обновления раздел **Настройки → Обновления** показывает точную команду и кнопку копирования.

Обе подкоманды `docker compose` работают только из каталога с файлом Compose. Если Bambuddy знает этот каталог, показанная команда включает `cd`:

```bash
cd /opt/bambuddy && docker compose pull && docker compose up -d
```

Bambuddy может определить каталог самостоятельно, только если подключён его подкаталог: `./data:/app/data` позволяет это сделать, а именованные тома по умолчанию — нет. В остальных случаях заполните поле **Каталог Compose** на той же панели; оно сохраняется вместе с настройками. Если оставить его пустым, команда будет показана без `cd`.

Можно также задать его в самом файле Compose; это значение сохраняется после сброса настроек:

```yaml
environment:
  - BAMBUDDY_COMPOSE_DIR=${PWD}
```

Compose подставляет `${PWD}` из оболочки, в которой запущен `docker compose`, поэтому запускайте команду из каталога с файлом Compose. Сохранённый параметр **Каталог Compose** имеет приоритет над переменной.

!!! tip "Устаревший `docker-compose.yml`?"
    Начиная с 0.2.2 добавлены `cap_add: NET_BIND_SERVICE`, дополнительные порты виртуального принтера для bridge (2024–2026) и необязательный блок PostgreSQL. При более старом файле пользователи bridge (macOS/Windows) могут незаметно потерять прокси FTP и RTSP. Сравните свой файл с [текущим](https://github.com/maziggy/bambuddy/blob/main/docker-compose.yml) и внесите изменения вручную.

---

## :material-console: Полезные команды {#useful-commands}

### Просмотр журналов {#view-logs}

```bash
# Follow logs
docker compose logs -f

# Last 100 lines
docker compose logs --tail 100
```

### Остановка и запуск {#stopstart}

```bash
# Stop
docker compose down

# Start
docker compose up -d
```

### Пересборка после изменений {#rebuild-after-changes}

```bash
docker compose up -d --build
```

### Доступ к оболочке {#shell-access}

```bash
docker compose exec bambuddy /bin/bash
```

---

## :material-server: Дополнительные варианты настройки {#advanced-setups}

### Обратный прокси (Nginx / Caddy) {#reverse-proxy-nginx-caddy}

См. [Установка → Обратный прокси](installation.md#reverse-proxy): приведённые конфигурации Nginx и Caddy применимы и к Docker. Укажите контейнер Bambuddy как целевой сервер (`bambuddy:8000` в той же сети Compose или `localhost:8000`, если прокси запущен на хосте).

### Метки Traefik {#traefik-labels}

Если используется Traefik:

```yaml
services:
  bambuddy:
    build: .
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.bambuddy.rule=Host(`bambuddy.yourdomain.com`)"
      - "traefik.http.routers.bambuddy.entrypoints=websecure"
      - "traefik.http.routers.bambuddy.tls.certresolver=letsencrypt"
    # ... rest of config
```

### Сетевой режим host {#network-mode-host}

Для обнаружения принтеров и трансляции камеры **требуется** сетевой режим host, если контейнер не имеет собственного адреса в локальной сети через macvlan (или ipvlan в режиме L2):

```yaml
services:
  bambuddy:
    build: .
    network_mode: host
    # Note: ports mapping not needed with host mode
```

!!! warning "Необходимо для обнаружения принтеров"
    Стандартная сеть bridge Docker не принимает multicast-пакеты SSDP, необходимые для автоматического обнаружения. Используйте `network_mode: host` или сеть macvlan (также подходит ipvlan в режиме L2), которая предоставляет контейнеру собственный адрес в локальной сети.

!!! note "При использовании режима host"
    - Удалите раздел `ports:` (в режиме host он не нужен)
    - Bambuddy будет доступен напрямую на порту 8000
    - Остальные функции работают так же

!!! tip "Режим host недоступен? Используйте bridge"
    Режим host используется по умолчанию, поскольку обнаружение SSDP требует multicast уровня L2, но подходит не всем. На многосервисном хосте Linux (NAS, отдельная виртуальная машина Docker, Synology DSM, Unraid) нужный Bambuddy порт может быть уже занят другим контейнером. Полностью поддерживаемая альтернатива — bridge с добавлением принтеров по IP без автоматического обнаружения. Правила публикации портов см. в разделе **[Режим bridge](#bridge-mode-linux-and-docker-desktop)**. Настройка одинакова для многосервисных хостов Linux и Docker Desktop (macOS / Windows).

### Режим bridge (Linux и Docker Desktop) {#bridge-mode-linux-and-docker-desktop}

Режим bridge необходим во **всех** установках, где нельзя использовать `network_mode: host`. Одинаковая конфигурация применяется в двух случаях:

- **Многосервисные хосты Linux** (NAS, отдельная виртуальная машина Docker, Synology DSM, Unraid, общий домашний сервер), где режим host конфликтует с портами других контейнеров.
- **Docker Desktop в macOS / Windows**, где режим host подключается к виртуальной машине Linux, а не к сети компьютера ([подробнее ниже](#macos-and-windows-docker-desktop)).

В обоих случаях автоматическое обнаружение SSDP недоступно; добавляйте принтеры вручную по IP. Фрагмент Compose одинаков:

```yaml
services:
  bambuddy:
    image: ghcr.io/maziggy/bambuddy:latest
    container_name: bambuddy
    cap_add:
      - NET_BIND_SERVICE
    ports:
      - "${PORT:-8000}:8000"           # Web UI
      - "3000:3000"                    # Virtual printer bind/detect
      - "3002:3002"                    # Virtual printer bind/detect (alt port)
      - "990:990"                      # Virtual printer FTPS
      - "8883:8883"                    # Virtual printer MQTT
      - "6000:6000"                    # Virtual printer file transfer tunnel
      - "322:322"                      # Virtual printer RTSP camera (X1/H2/P2)
      - "2024-2026:2024-2026"          # Virtual printer proprietary ports (A1/P1S)
      - "50000-50029:50000-50029"      # Virtual printer FTP passive data — 3 VPs × 10-port slice
    volumes:
      - bambuddy_data:/app/data
      - bambuddy_logs:/app/logs
    environment:
      - TZ=Europe/Berlin
      # Required for virtual printer FTP passive mode behind Docker NAT:
      # Set to your Docker host's LAN IP
      #- VIRTUAL_PRINTER_PASV_ADDRESS=192.168.1.100
      # The address slicers use to reach Bambuddy. Set to the same LAN IP so
      # print uploads have a routable destination (see the note below).
      #- VIRTUAL_PRINTER_ADVERTISE_ADDRESS=192.168.1.100
    restart: unless-stopped
    # Docker SIGKILLs after 10s by default. Bambuddy stops well inside that,
    # but the headroom means a slow teardown (several virtual printers on a Pi)
    # can still checkpoint the database instead of being killed mid-write.
    stop_grace_period: 30s

volumes:
  bambuddy_data:
  bambuddy_logs:
```

!!! info "Пассивные порты FTP выделяются каждому виртуальному принтеру — публикуйте только необходимые"
    Каждому виртуальному принтеру выделяется собственный непересекающийся диапазон из 10 портов пассивного режима ([#1646](https://github.com/maziggy/bambuddy/issues/1646)): принтер 1 → `50000-50009`, принтер 2 → `50010-50019`, принтер 3 → `50020-50029` и т. д. **Публикуйте только используемый диапазон:**

    - **1 виртуальный принтер** → `50000-50009:50000-50009` (10 портов)
    - **3 виртуальных принтера** (по умолчанию выше) → `50000-50029:50000-50029` (30 портов)
    - **N виртуальных принтеров** → `50000-500{N-1}9:50000-500{N-1}9` (`N × 10` портов)
    - **Режим прокси** → для любого виртуального принтера по-прежнему требуется `50000-50100:50000-50100`: он прозрачно перенаправляет весь диапазон данных FTP реального принтера, а не часть Bambuddy.

    Почему важно публиковать минимум: при стандартном Docker `"userland-proxy": true` каждый порт создаёт процесс `docker-proxy` на хосте для каждого семейства адресов (IPv4 и IPv6). Диапазон из 1001 порта создаёт около 2000 процессов, занимающих примерно 3,5 ГБ ОЗУ хоста, которые не видны в `docker stats`. Приведённые 30 портов создают около 60 процессов и занимают примерно 210 МБ; установка с одним виртуальным принтером — около 20 процессов и 70 МБ.

    Если сузить диапазон нельзя, параметр `{ "userland-proxy": false }` в `/etc/docker/daemon.json` устраняет затраты `docker-proxy` для всех контейнеров хоста. Это глобальное изменение, поэтому разделение диапазона по принтерам выше имеет меньший эффект на остальную систему.

!!! tip "Адрес для слайсеров (только bridge)"
    Контейнер bridge не видит адрес, по которому слайсер подключается к нему, поэтому его нужно указать. Обе закомментированные выше переменные задаются равными IP-адресу хоста Docker в локальной сети:

    - **`VIRTUAL_PRINTER_ADVERTISE_ADDRESS`** — адрес в состоянии MQTT, откуда слайсер получает адрес загрузки. Без него слайсер получает частный IP контейнера (например, `172.17.0.2`) и загрузка не достигает назначения.
    - **`VIRTUAL_PRINTER_PASV_ADDRESS`** — адрес пассивного соединения данных FTP.

    В `network_mode: host` и macvlan эти переменные не нужны: контейнер уже имеет адрес, маршрутизируемый в локальной сети. Эти режимы остаются рекомендуемыми; ограничения bridge см. на странице [Виртуальный принтер](../features/virtual-printer.md).

### macOS и Windows (Docker Desktop) {#macos-and-windows-docker-desktop}

Docker Desktop в macOS и Windows запускает контейнеры внутри виртуальной машины Linux, поэтому `network_mode: host` подключается к сети виртуальной машины, а не компьютера. Это означает:

- **Порт 8000 недоступен** через localhost
- **Обнаружение принтеров не работает** (контейнер не видит локальную сеть)

**Решение:** используйте настройку **[bridge](#bridge-mode-linux-and-docker-desktop)** выше; фрагмент Compose, правила сопоставления портов и рекомендации по диапазонам пассивного FTP применяются аналогично.

!!! warning "Необходимо добавить принтер вручную"
    В macOS/Windows необходимо добавлять принтеры вручную по IP-адресу. Автоматическое обнаружение недоступно, поскольку Docker Desktop не имеет доступа к multicast-трафику локальной сети.

### Обнаружение принтеров в Docker {#printer-discovery-in-docker}

!!! note "Обнаружение виртуального принтера через SSDP"
    Обнаружение **виртуального принтера** через SSDP (когда слайсер ищет Bambuddy) также требует сети host или подключения к той же локальной сети. В режиме bridge Docker (macOS/Windows) виртуальный принтер нужно добавлять в слайсере вручную по IP.

При запуске Docker с `network_mode: host` Bambuddy использует для обнаружения принтеров **сканирование подсети** вместо multicast SSDP:

1. Нажмите **Добавить принтер** на странице принтеров
2. Bambuddy определит запуск в Docker и покажет поле подсети
3. Введите диапазон сети (например, `192.168.1.0/24`)
4. Нажмите **Сканировать сеть** — Bambuddy проверит каждый IP на порты принтера Bambu (8883, 990)
5. Найденные принтеры появятся в списке с названием и моделью

!!! tip "Как узнать подсеть"
    Обычно подсеть — это ваш IP-адрес, последнее число которого заменено на `0/24`. Например:

    - Если IP компьютера `192.168.1.50`, используйте `192.168.1.0/24`
    - Если IP компьютера `10.0.0.25`, используйте `10.0.0.0/24`

!!! info "Как это работает"
    Сканирование проверяет каждый IP-адрес диапазона на открытые порты 8883 (MQTT) и 990 (FTPS). Если оба открыты, отправляется запрос SSDP для получения названия, серийного номера и модели принтера.

---

## :material-help-circle: Устранение неполадок {#troubleshooting}

### Контейнер не запускается {#container-wont-start}

Проверьте журналы:

```bash
docker compose logs bambuddy
```

Распространённые проблемы:

- **Порт занят**: измените сопоставление портов
- **Отказано в доступе**: проверьте разрешения томов

### Не удаётся подключиться к принтеру {#cant-connect-to-printer}

Убедитесь, что сеть Docker может подключиться к принтеру:

```bash
# Test connectivity from inside container
docker compose exec bambuddy ping YOUR_PRINTER_IP
```

При проблемах в режиме сети `bridge` попробуйте `network_mode: host`.

### Проблемы с базой данных {#database-issues}

Если нужно сбросить базу данных:

```bash
docker compose down
rm bambuddy.db
docker compose up -d
```

!!! danger "Потеря данных"
    Это удалит всю историю печати и настройки!

---

## :checkered_flag: Следующие шаги {#next-steps}

<div class="quick-start" markdown>

[:material-printer-3d: **Добавить принтер**<br><small>Подключите первый принтер</small>](first-printer.md)

[:material-cellphone: **Мобильный доступ**<br><small>Работайте с телефона</small>](mobile.md)

[:material-help-circle: **Устранение неполадок**<br><small>Возникли проблемы?</small>](../reference/troubleshooting.md)

</div>
