---
title: Возможности
description: Изучите возможности Bambuddy
---

# Возможности {#features}

Bambuddy предлагает множество возможностей для управления процессом 3D-печати. Ознакомьтесь с ними ниже.

---

## :material-printer-3d-nozzle-outline: НОВИНКА: встроенная нарезка {#new-integrated-slicing}

<div class="feature-card highlight" markdown>

### [:material-cursor-pointer: Нарезка на сервере](slicer-api.md) {#server-side-slicing}

**Нарезайте STL и 3MF без настольного слайсера.** Необязательный стек Compose `slicer-api/` запускает OrcaSlicer или Bambu Studio без графического интерфейса внутри Docker. Кнопка **Нарезать** в файловом менеджере, архиве и MakerWorld создаёт готовый к печати `.gcode.3mf` в той же папке.

- Нарезка одним нажатием &mdash; результат с встроенной миниатюрой появляется в библиотеке и готов к отправке
- Для 3MF с несколькими пластинами сначала открывается выбор пластины; для многоцветной пластины отображается список филамента для каждого используемого слота AMS с автоматическим подбором импортированных, облачных Orca и Bambu или стандартных пресетов по типу и цвету
- Порядок поиска пресетов при нарезке: **импортированные &rarr; Orca Cloud &rarr; Bambu Cloud &rarr; стандартные**. Локально импортированный пресет имеет приоритет перед одноимённым облачным
- Всплывающий индикатор отслеживает задание от постановки в очередь до завершения на разных страницах, поэтому длительная нарезка не удерживает окно открытым
- Работа без графического интерфейса &mdash; запускается на NAS, мини-ПК или RPi 5 без установки настольного слайсера

[Руководство по настройке :material-arrow-right:](slicer-api.md){ .md-button .md-button--primary }

</div>

---

## :material-printer-3d: Принтеры и мониторинг {#printers-monitoring}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-monitor-dashboard: Мониторинг в реальном времени](monitoring.md) {#real-time-monitoring}
Текущее состояние принтера, температуры, ход печати и отслеживание ошибок HMS через WebSocket.
</div>

<div class="feature-card" markdown>
### [:material-camera: Трансляция с камеры](camera.md) {#camera-streaming}
Трансляция MJPEG и снимки со встроенной камеры принтера.
</div>

<div class="feature-card" markdown>
### [:material-eye-check: Обнаружение ошибок с помощью ИИ](failure-detection.md) {#ai-failure-detection}
Анализ снимков камеры с помощью самостоятельно размещённого Obico или облачного OctoEverywhere. Настройте уведомления, приостановку и список отслеживаемых принтеров.
</div>

<div class="feature-card" markdown>
### [:material-water-percent: AMS и влажность](ams.md) {#ams-humidity}
Отслеживание состояния слотов AMS, влажности и температуры. Удалённая сушка вручную, по расписанию, автоматическая сушка очереди, сушка при температуре окружающей среды и продолжение сушки во время печати на поддерживаемых устройствах (H2D / H2C / H2S / P2S / X2D / X1C / A2L / H2D Pro), настраиваемые пресеты и пороги влажности для каждого филамента (например, 20% для Nylon и 60% для PLA).
</div>

<div class="feature-card" markdown>
### [:material-tune-vertical: Управление принтером](printer-control.md) {#printer-control}
Запуск печати с карточки кнопкой или перетаскиванием, пресеты скорости, температура камеры, управление светом, состояние вентиляторов, модули обнаружения с ИИ и **режим обслуживания** для временного вывода принтера из работы без удаления.
</div>

</div>

![Страница принтеров](../assets/printers.png){ .screenshot }

---

## :material-archive: Архив печати {#print-archive}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-archive-outline: Архивация печати](archiving.md) {#print-archiving}
Автоматическая архивация 3MF с извлечением метаданных, предпросмотром 3D и обнаружением дубликатов.
</div>

<div class="feature-card" markdown>
### [:material-clipboard-list: Журнал печати](print-log.md) {#print-log}
Хронологическая таблица всех печатей с фильтрацией, поиском и отслеживанием состояния.
</div>

<div class="feature-card" markdown>
### [:material-magnify: Полнотекстовый поиск](search.md) {#full-text-search}
Быстрый поиск FTS5 по названиям печатей и файлов, тегам, заметкам, дизайнерам и филаменту.
</div>

<div class="feature-card" markdown>
### [:material-folder-multiple: Проекты](projects.md) {#projects}
Объединение связанных печатей в проекты с отслеживанием прогресса и цветными метками.
</div>

<div class="feature-card" markdown>
### [:material-compare: Сравнение архивов](comparison.md) {#archive-comparison}
Сравнение 2–5 архивов рядом с выделением различий в настройках.
</div>

</div>

![Страница архива](../assets/archives.png){ .screenshot }

---

## :material-chart-line: Аналитика {#analytics}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-view-dashboard: Панель статистики](statistics.md) {#statistics-dashboard}
Настраиваемые виджеты с перетаскиванием для доли успешных печатей, расхода филамента, затрат и других показателей.
</div>

<div class="feature-card" markdown>
### [:material-alert-circle: Анализ неудач](failure-analysis.md) {#failure-analysis}
Сопоставление неудач с условиями для выявления закономерностей и повышения надёжности.
</div>

<div class="feature-card" markdown>
### [:material-lightning-bolt: Учёт энергии](energy.md) {#energy-tracking}
Учёт энергопотребления по печатям и суммарно с расчётом стоимости.
</div>

<div class="feature-card" markdown>
### [:material-file-export: Экспорт](export.md) {#export}
Экспорт архивов и статистики в CSV или Excel с поддержкой фильтров.
</div>

</div>

![Страница статистики](../assets/statistics.png){ .screenshot }

---

## :material-robot: Автоматизация {#automation}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-printer-3d-nozzle: Очередь печати](print-queue.md) {#print-queue}
Очередь печати с изменением порядка перетаскиванием, количеством для каждой пластины, пакетными заказами с отслеживанием остатка, временной шкалой, запуском по расписанию, настройкой параметров по умолчанию и автоматизацией.
</div>

<div class="feature-card" markdown>
### [:material-power-plug: Умные розетки](smart-plugs.md) {#smart-plugs}
Интеграция с Tasmota, Home Assistant, REST/Webhook и MQTT для автоматического включения перед печатью, выключения после охлаждения и учёта энергии.
</div>

<div class="feature-card" markdown>
### [:material-printer-3d: Виртуальный принтер](virtual-printer.md) {#virtual-printer}
Эмуляция принтера Bambu в сети для отправки печати прямо из слайсера.
</div>

<div class="feature-card" markdown>
### [:material-bell-ring: Уведомления](notifications.md) {#notifications}
Уведомления через WhatsApp, Telegram, Discord, электронную почту и другие сервисы.
</div>

</div>

![Страница очереди](../assets/print-queue.png){ .screenshot }

---

## :material-puzzle: Интеграции {#integrations}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-cylinder: Учёт филамента](inventory.md) {#spool-inventory}
Встроенный учёт катушек с назначением слотов AMS, автоматическим отслеживанием расхода и управлением оставшимся весом.
</div>

<div class="feature-card" markdown>
### [:material-map-marker: Места хранения](storage-locations.md) {#storage-locations}
Каталог полок, ящиков и сухих боксов: назначайте катушкам места хранения и фильтруйте филамент по ним.
</div>

<div class="feature-card" markdown>
### [:material-database-sync-outline: Spoolman](spoolman.md) {#spoolman}
Синхронизация учёта филамента со Spoolman для полного отслеживания катушек.
</div>

<div class="feature-card" markdown>
### [:material-cloud: Облачные профили](cloud-profiles.md) {#cloud-profiles}
Управление пресетами слайсера Bambu Cloud и сравнение различий шаблонов.
</div>

<div class="feature-card" markdown>
### [:material-harddisk: Локальные профили](local-profiles.md) {#local-profiles}
Импорт пресетов OrcaSlicer без Bambu Cloud. Поддерживаются файлы .orca_filament, .bbscfg, .bbsflmt, .zip и .json.
</div>

<div class="feature-card" markdown>
### [:material-speedometer: K-профили](k-profiles.md) {#k-profiles}
Управление настройками опережения давления для оптимального качества печати.
</div>

<div class="feature-card" markdown>
### [:material-key: Ключи API и вебхуки](api-keys.md) {#api-keys-webhooks}
REST API с детальными разрешениями для внешних интеграций.
</div>

<div class="feature-card" markdown>
### [:material-page-layout-sidebar-left: Настройка боковой панели](external-links.md) {#sidebar-customization}
Отображение и скрытие страниц Bambuddy, изменение порядка навигации и добавление ссылок на внешние инструменты.
</div>

<div class="feature-card" markdown>
### [:material-wifi: Публикация MQTT](mqtt.md) {#mqtt-publishing}
Публикация событий во внешние брокеры MQTT для Home Assistant и Node-RED.
</div>

<div class="feature-card" markdown>
### [:material-chart-line: Метрики Prometheus](prometheus.md) {#prometheus-metrics}
Экспорт телеметрии принтеров для панелей Grafana и систем мониторинга.
</div>

</div>

---

## :material-wrench: Обслуживание и безопасность {#maintenance-security}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-tools: Учёт обслуживания](maintenance.md) {#maintenance-tracker}
Планирование и учёт обслуживания с напоминаниями об интервалах.
</div>

<div class="feature-card" markdown>
### [:material-folder: Файловый менеджер](file-manager.md) {#file-manager}
Просмотр и управление файлами во внутренней памяти принтера.
</div>

<div class="feature-card" markdown>
### [:material-backup-restore: Резервное копирование и восстановление](backup.md) {#backup-restore}
Полное резервное копирование и восстановление базы данных. Резервное копирование на GitHub для автоматической синхронизации облачных профилей.
</div>

<div class="feature-card" markdown>
### [:material-elephant: Поддержка PostgreSQL](postgresql.md) {#postgresql-support}
Необязательная внешняя база PostgreSQL для крупных установок. SQLite остаётся вариантом по умолчанию, не требующим настройки.
</div>

<div class="feature-card" markdown>
### [:material-information: Информация о системе](system-info.md) {#system-info}
Просмотр статистики базы данных и системных ресурсов, создание пакетов поддержки.
</div>

<div class="feature-card" markdown>
### [:material-lock: Аутентификация](authentication.md) {#authentication}
Необязательная аутентификация с ролевым доступом. Расширенная аутентификация по электронной почте для самостоятельного сброса пароля, интеграция LDAP / Active Directory, **двухфакторная аутентификация** (TOTP и одноразовый код по почте с резервными кодами) и **единый вход (OIDC / SSO)** через PocketID, Authentik, Keycloak и других поставщиков, соответствующих стандарту.
</div>

</div>

---

## :material-lightbulb: Полезные советы {#pro-tips}

!!! tip "Сочетания клавиш"
    Нажмите ++question++ на любой странице, чтобы увидеть сочетания клавиш. Переходите между страницами цифровыми клавишами от ++1++ до ++5++.

!!! tip "Контекстные меню"
    Щёлкните карточку правой кнопкой мыши или нажмите и удерживайте на мобильном устройстве для быстрого доступа к повторной печати, сравнению и удалению.

!!! tip "Настраиваемые темы"
    Bambuddy позволяет подробно настроить тему отдельно для светлого и тёмного режимов:

    - **Стиль**: Classic (чёткие тени), Glow (свечение акцентного цвета), Vibrant (выразительные тени)
    - **Фон**: Neutral, Warm, Cool и варианты только для тёмной темы (OLED, Slate, Forest)
    - **Акцентные цвета**: зелёный, бирюзовый, синий, оранжевый, фиолетовый, красный

    Выберите любое сочетание в разделе Настройки → Внешний вид.
