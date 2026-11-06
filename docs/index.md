---
title: Главная
description: Bambuddy — локальная система архивации и управления печатью для 3D-принтеров Bambu Lab
hide:
  - navigation
  - toc
---

<style>
.md-typeset h1 { display: none; }
</style>

<div class="hero" markdown>

<div markdown>

# Ваши принтеры.<br>Без облака.<br>По вашим правилам. {#your-printersno-cloudyour-rules}

**Bambuddy** — локальный центр управления 3D-принтерами Bambu Lab: от одного A1 до целой фермы. Следите за принтерами в реальном времени, автоматически архивируйте каждую печать, нарезайте модели и ставьте задания в очередь локально и управляйте процессом 3D-печати.

<div class="stats-row" markdown>
  <span class="stat-badge" markdown>:material-printer-3d: Несколько принтеров</span>
  <span class="stat-badge" markdown>:material-cloud-off-outline: Работа без интернета</span>
  <span class="stat-badge" markdown>:material-open-source-initiative: Открытый исходный код</span>
</div>

[:material-rocket-launch: Попробовать демоверсию](https://demo.bambuddy.cool){ .btn .btn-primary target=_blank }
[Начать работу :material-arrow-right:](getting-started/index.md){ .btn .btn-secondary }
[Посмотреть на GitHub :material-github:](https://github.com/maziggy/bambuddy){ .btn .btn-secondary }

!!! tip "Попробуйте Bambuddy в браузере"
    Полностью работающий экземпляр Bambuddy создаётся примерно за 10 секунд на **[demo.bambuddy.cool](https://demo.bambuddy.cool)**: установка и регистрация не нужны, сеанс длится 30 минут. В нём уже есть симулированные принтеры, архивы, задания очереди и филамент, чтобы вы могли изучить все страницы.

</div>

<div markdown>
![Панель Bambuddy](assets/hero-screenshot.png){ .screenshot }
</div>

</div>

---

## :material-nfc-variant: НОВИНКА: SpoolBuddy — управление катушками через NFC {#new-spoolbuddy-nfc-spool-management}

<div class="spoolbuddy-announce" markdown>
<div class="spoolbuddy-announce-content" markdown>

**Приложите. Определите. Отслеживайте.** SpoolBuddy — отдельное устройство для Bambuddy: киоск на Raspberry Pi с сенсорным экраном, NFC-считывателем и весовым датчиком, который мгновенно определяет катушки филамента.

- :material-tag-multiple: **Работает с любыми катушками** — используйте метки и отслеживайте филамент любых брендов, не только Bambu Lab
- :material-nfc: **Записывайте собственные NFC-метки** — привязывайте любую катушку к метке прямо на сенсорном экране
- :material-scale: **Встроенные весы** — отслеживание веса катушки в реальном времени с помощью весового датчика
- :material-monitor: **7-дюймовый сенсорный киоск** — отдельный постоянно работающий экран для вашей станции печати

[Собрать самостоятельно :material-arrow-right:](spoolbuddy/index.md){ .btn .btn-primary }

</div>
<div class="spoolbuddy-announce-image">
<img src="assets/spoolbuddy-preview.png" alt="Предпросмотр SpoolBuddy">
</div>
</div>

---

## :material-printer-3d-nozzle-outline: НОВИНКА: встроенная нарезка {#new-integrated-slicing}

<div class="feature-card highlight" markdown>

**Нарезайте файлы STL и 3MF на сервере &mdash; настольный слайсер не нужен.** Необязательный дополнительный контейнер slicer-api запускает OrcaSlicer или Bambu Studio без графического интерфейса внутри Docker. Кнопка **Нарезать** в файловом менеджере, архиве и на странице MakerWorld создаёт готовый к печати `.gcode.3mf` в той же папке &mdash; остаётся отправить его на печать.

- :material-cursor-pointer: **Нарезка одним нажатием** &mdash; из файлового менеджера, архива, импортов MakerWorld и очереди печати
- :material-layers-triple: **Выбор филамента для каждого слота AMS** &mdash; для многоцветных пластин отображается выбор для каждого используемого слота с автоматическим подбором из импортированных, облачных Orca и Bambu или стандартных пресетов
- :material-sort: **Порядок поиска пресетов** &mdash; импортированные &rarr; Orca Cloud &rarr; Bambu Cloud &rarr; стандартные: локально импортированный пресет имеет приоритет перед одноимённым облачным
- :material-cellphone: **Работа без графического интерфейса** &mdash; запускается на NAS, мини-ПК или RPi; установка настольного слайсера для печати одним нажатием не нужна
- :material-flash: **Использование существующего механизма отправки** &mdash; результат нарезки попадает в библиотеку, откуда доступны выбор пластины, сопоставление AMS и очередь

Благодаря этому для быстрой повторной печати больше не нужен настольный компьютер только ради нарезки.

[Руководство по настройке :material-arrow-right:](features/slicer-api.md){ .md-button .md-button--primary }

</div>

---

## :globe_with_meridians: Удалённая печать в режиме прокси {#remote-printing-with-proxy-mode}

<div class="feature-card highlight" markdown>

**Печатайте из любой точки мира!** Режим прокси Bambuddy служит защищённым посредником между слайсером и принтером.

- :lock: **Сквозное шифрование TLS** — данные печати шифруются на пути от слайсера к принтеру
- :earth_americas: **Без зависимости от облака** — прямое соединение через ваш сервер Bambuddy
- :key: **Используется код доступа принтера** — дополнительные учётные данные не нужны
- :zap: **Печать на полной скорости** — протоколы FTP и MQTT прозрачно проксируются

Подходит для удалённых ферм печати, работы в поездках или доступа к домашнему принтеру с работы.

[Руководство по настройке :material-arrow-right:](features/virtual-printer.md#proxy-mode-remote-printing){ .md-button .md-button--primary }

</div>

---

## :rocket: Быстрый старт {#quick-start}

<div class="quick-start" markdown>

[:material-download: **Установка**<br><small>Начните работу за несколько минут</small>](getting-started/installation.md)

[:material-docker: **Docker**<br><small>Развёртывание одной командой</small>](getting-started/docker.md)

[:material-printer-3d: **Добавить принтер**<br><small>Подключите первый принтер</small>](getting-started/first-printer.md)

[:material-cellphone: **Мобильный доступ**<br><small>Доступ откуда угодно</small>](getting-started/mobile.md)

</div>

---

## :sparkles: Возможности {#features}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### :material-archive: Архив печати {#print-archive}
Автоматическая архивация 3MF с извлечением метаданных, предпросмотром 3D-моделей, обнаружением дубликатов и полнотекстовым поиском. Файлы печати больше не потеряются.
</div>

<div class="feature-card" markdown>
### :material-monitor-dashboard: Мониторинг в реальном времени {#real-time-monitoring}
Текущее состояние принтера через WebSocket, трансляция MJPEG с камеры, отслеживание ошибок HMS, а также влажности и температуры AMS с графиками истории.
</div>

<div class="feature-card" markdown>
### :material-chart-line: Статистика и аналитика {#statistics-analytics}
Настраиваемая панель с перетаскиванием виджетов, показателями успешности, расходом филамента, учётом затрат, анализом точности времени и взаимосвязей неудач.
</div>

<div class="feature-card" markdown>
### :material-clock-outline: Планирование и автоматизация {#scheduling-automation}
Очередь печати с изменением порядка перетаскиванием, печать по расписанию, интеграция с умными розетками (Tasmota, Home Assistant), автоматическое включение и выключение и учёт энергопотребления.
</div>

<div class="feature-card" markdown>
### :material-bell-ring: Push-уведомления {#push-notifications}
Оповещения через WhatsApp, Telegram, Discord, Email, Pushover, ntfy и пользовательские вебхуки. Поддерживаются тихие часы и ежедневная сводка.
</div>

<div class="feature-card" markdown>
### :material-puzzle: Интеграции {#integrations}
Синхронизация филамента со Spoolman, профили Bambu Cloud, K-профили (pressure advance), внешние ссылки и REST API с аутентификацией по ключу API.
</div>

</div>

[Все возможности :material-arrow-right:](features/index.md){ .md-button }

---

## :printer: Поддерживаемые принтеры {#supported-printers}

| Серия | Модели |
|--------|--------|
| **Серия X1** | X1, X1 Carbon, X1E |
| **Серия H2** | H2D, H2D Pro, H2C, H2S |
| **Серия P1** | P1P, P1S |
| **Серия P2** | P2S |
| **Серия A1** | A1, A1 Mini |
| **Серия A2** | A2L |

---

## :wrench: Технологии {#tech-stack}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### Бэкенд {#backend}
:material-language-python: Python
:material-api: FastAPI
:material-database: SQLAlchemy + SQLite
</div>

<div class="feature-card" markdown>
### Фронтенд {#frontend}
:material-react: React
:material-language-typescript: TypeScript
:material-tailwind: Tailwind CSS
</div>

<div class="feature-card" markdown>
### Связь {#communication}
:material-transit-connection-variant: MQTT через TLS
:material-folder-network: FTPS
:material-web: WebSocket
</div>

</div>

---

## :people_holding_hands: Сообщество {#community}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### :material-chat: Присоединяйтесь к Discord {#join-discord}
[discord.gg/aFS3ZfScHM](https://discord.gg/aFS3ZfScHM) &mdash; чат сообщества, вопросы и ответы, руководства
</div>

<div class="feature-card" markdown>
### :material-bug: Нашли ошибку? {#found-a-bug}
[Создать обращение](https://github.com/maziggy/bambuddy/issues/new)
</div>

<div class="feature-card" markdown>
### :material-lightbulb: Есть предложение? {#feature-request}
[Начать обсуждение](https://github.com/maziggy/bambuddy/discussions)
</div>

<div class="feature-card" markdown>
### :material-code-tags: Хотите внести вклад? {#want-to-contribute}
[Руководство для участников](https://github.com/maziggy/bambuddy/blob/main/CONTRIBUTING.md)
</div>

<div class="feature-card" markdown>
### :material-file-document-edit: Хотите помочь с документацией или форумом? {#help-with-docs-or-the-forum}
См. раздел [Хотите внести вклад?](#want-to-contribute) или напишите на [martin@bambuddy.cool](mailto:martin@bambuddy.cool)
</div>

</div>

---

<div style="text-align: center; margin-top: 3rem;" markdown>
<span style="opacity: 0.6;">Сделано с :heart: для сообщества 3D-печати</span>
<br><br>
![Посетители Wiki](https://api.visitorbadge.io/api/visitors?path=http%3A%2F%2Fwiki.bambuddy.cool&labelColor=%23555555&countColor=%2379C83D&label=visitors&style=flat-square)
</div>
