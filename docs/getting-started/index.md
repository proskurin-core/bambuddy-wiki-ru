---
title: Начало работы
description: Начните работать с Bambuddy за несколько минут
---

# Начало работы {#getting-started}

> Одна команда. Две минуты. Каждая печать автоматически сохраняется в архиве.

---

## :rocket: Быстрая установка {#quick-install}

=== ":material-docker: Docker (Linux / macOS)"

    Интерактивный скрипт выполнит все действия за вас. Требуется оболочка, совместимая с bash (Linux, macOS или WSL в Windows):

    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/docker-install.sh -o docker-install.sh && chmod +x docker-install.sh && ./docker-install.sh
    ```

    Откройте в браузере `http://<your-host>:8000`, заменив `<your-host>` IP-адресом или именем компьютера, на котором работает Bambuddy. В Linux при использовании сети хоста можно указать `localhost`; в Docker Desktop (macOS/Windows) через localhost контейнер **недоступен** — используйте IP-адрес компьютера в локальной сети.

    [:material-arrow-right: Полное руководство по Docker](docker.md)

=== ":material-microsoft-windows: Docker (Windows)"

    Интерактивный скрипт установки работает только в bash; в Windows PowerShell нет команды `chmod`. Вместо него выполните установку через Compose вручную:

    ```powershell
    mkdir bambuddy
    cd bambuddy
    Invoke-WebRequest -Uri https://raw.githubusercontent.com/maziggy/bambuddy/main/docker-compose.yml -OutFile docker-compose.yml
    docker compose up -d
    ```

    Откройте в браузере `http://<your-host>:8000`, заменив `<your-host>` IP-адресом Windows-компьютера с Docker Desktop в локальной сети. **`localhost` не сработает**: Docker Desktop запускает контейнер внутри виртуальной машины Linux с `network_mode: host`, поэтому контейнер находится в сети виртуальной машины, а не на loopback-интерфейсе Windows.

    [:material-arrow-right: Полное руководство по Docker](docker.md)

=== ":material-microsoft-windows: Нативная установка (Windows)"

    Установщик Windows устанавливает Bambuddy непосредственно в Windows, без Docker. Запустите PowerShell от имени администратора и выполните команду:

    ```powershell
    powershell -ExecutionPolicy Bypass -Command "iwr -useb https://raw.githubusercontent.com/maziggy/bambuddy/main/install/windows-installer.ps1 -OutFile windows-installer.ps1; .\windows-installer.ps1"
    ```

    Откройте в браузере `http://localhost:8000` или `http://<your-host>:8000`, заменив `<your-host>` IP-адресом Windows-компьютера, на котором работает Bambuddy, в локальной сети.

    [:material-arrow-right: Полное руководство по нативной установке в Windows](windows-installer.md)

=== ":material-language-python: Нативная установка (Linux / macOS)"

    Интерактивный скрипт устанавливает виртуальное окружение Python, собирает фронтенд и настраивает службу systemd/launchd:

    ```bash
    curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/main/install/install.sh -o install.sh && chmod +x install.sh && ./install.sh
    ```

    Откройте в браузере `http://<your-host>:8000`, заменив `<your-host>` IP-адресом или именем компьютера, на котором работает Bambuddy. На этом же компьютере можно использовать `localhost`: при нативной установке приложение работает непосредственно в сети хоста, без виртуальной машины Docker.

    [:material-arrow-right: Полное руководство по установке](installation.md)

---

## :footprints: Следующие шаги {#next-steps}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### :material-numeric-1-circle: Включите режим разработчика {#enable-developer-mode}
Включите режим разработчика на принтере и запишите код доступа.

[:material-arrow-right: Инструкция](#enabling-developer-mode)
</div>

<div class="feature-card" markdown>
### :material-numeric-2-circle: Добавьте принтер {#add-your-printer}
Введите IP-адрес, код доступа и серийный номер принтера.

[:material-arrow-right: Добавить первый принтер](first-printer.md)
</div>

<div class="feature-card" markdown>
### :material-numeric-3-circle: Начните печать! {#start-printing}
Bambuddy автоматически архивирует каждую печать.

[:material-arrow-right: Изучить возможности](../features/index.md)
</div>

</div>

---

## Включение режима разработчика {#enabling-developer-mode}

Bambuddy подключается к принтеру через **режим разработчика** — локальное соединение, обеспечивающее полное управление без интернета.

!!! info "Зачем нужен режим разработчика?"
    Режим разработчика обеспечивает прямую связь Bambuddy с принтером по локальной сети. Это означает:

    - :material-check: **Работа без интернета** — подключение к интернету не требуется
    - :material-check: **Полное управление** — запуск и остановка печати, загрузка файлов, управление подсветкой
    - :material-check: **Данные остаются у вас** — зависимости от облака нет

!!! warning "Режим разработчика и режим «Только локальная сеть»"
    Начиная с обновления прошивки за январь 2025 года обычный режим «Только локальная сеть» без режима разработчика предоставляет доступ **только для чтения**. Вы можете наблюдать за принтером, но не управлять им. Для полной функциональности Bambuddy **необходим режим разработчика**.

### Шаг 1. Включите режим «Только локальная сеть» {#step-1-enable-lan-only-mode}

1. На сенсорном экране принтера откройте **Настройки**
2. Перейдите в **Сеть** или **WLAN**
3. Включите режим **«Только локальная сеть»**

### Шаг 2. Включите режим разработчика {#step-2-enable-developer-mode}

1. После включения режима «Только локальная сеть» появится пункт **«Режим разработчика»**
2. Включите **режим разработчика**
3. Запишите отображаемый **код доступа** из 8 символов

!!! warning "Изменение кода доступа"
    Код доступа меняется при каждом выключении и повторном включении этих режимов. Если вы снова включили режим разработчика, обновите код доступа в Bambuddy.

### Шаг 3. Вставьте SD-карту {#step-3-insert-sd-card}

!!! warning "Необходима SD-карта"
    Для правильной работы Bambuddy в принтер должна быть вставлена SD-карта. Она необходима для:

    - :material-check: Передачи файлов на принтер и с него
    - :material-check: Запуска печати из Bambuddy
    - :material-check: Архивации завершённых печатей

    Без SD-карты Bambuddy не сможет передавать файлы или запускать печать на принтере.

### Шаг 4. Включите сохранение отправленных файлов на внешнем накопителе {#step-4-enable-store-sent-files-on-external-storage}

Чтобы получать миниатюры и метаданные слайсера для архива, Bambuddy нужна копия файла `.gcode.3mf`, сохранённая слайсером на SD-карте принтера. В зависимости от сочетания слайсера и прошивки настройка находится в одном из двух мест:

=== "В слайсере (старые версии Bambu Studio / OrcaSlicer)"

    1. Откройте **Bambu Studio** или **OrcaSlicer**
    2. Перейдите на вкладку **Устройство** вашего принтера
    3. Включите **«Сохранять отправленные файлы на внешнем накопителе»**

=== "На принтере (новые версии прошивки, например P2S 01.02.00.00 + Bambu Studio 2.6+)"

    В последних версиях прошивки и слайсера переключатель перенесли из слайсера на принтер. Если на вкладке устройства Bambu Studio такой настройки нет, установите её на принтере:

    1. На принтере откройте **Настройки :material-arrow-right: Настройки печати** (или **Параметры печати**, в зависимости от прошивки)
    2. Включите соответствующий параметр **«Сохранять отправленные файлы на внешнем накопителе»**

    Об этом сообщили для P2S в [#1170](https://github.com/maziggy/bambuddy/issues/1170).

!!! info "Зачем это нужно?"
    Bambuddy извлекает миниатюры и предпросмотр 3D-моделей из файлов 3MF на SD-карте. Без этой настройки каждая печать будет архивироваться без 3MF: без миниатюры, исходного 3MF и метаданных слайсера.

### Шаг 5. Подготовьте сведения о принтере {#step-5-gather-printer-information}

Чтобы добавить принтер, понадобятся следующие данные:

| Данные | Где найти |
|------------|---------------|
| **IP-адрес** | Настройки :material-arrow-right: Сеть |
| **Серийный номер** | Настройки :material-arrow-right: Информация об устройстве |
| **Код доступа** | Отображается при включении режима разработчика |

---

## :checkered_flag: Что дальше? {#whats-next}

<div class="quick-start" markdown>

[:material-printer-3d: **Добавить принтер**<br><small>Подключите свой первый принтер</small>](first-printer.md)

[:material-archive: **Архивация печати**<br><small>Как работает автоматическая архивация</small>](../features/archiving.md)

[:material-bell-ring: **Уведомления**<br><small>Получайте оповещения на телефон</small>](../features/notifications.md)

[:material-help-circle: **Устранение неполадок**<br><small>Возникли проблемы?</small>](../reference/troubleshooting.md)

</div>
