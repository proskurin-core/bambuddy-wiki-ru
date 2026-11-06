---
title: Дополнения сообщества
description: Сторонние аппаратные и программные дополнения сообщества Bambuddy
---

# Дополнения сообщества {#community-add-ons}

Сторонние проекты, расширяющие Bambuddy, созданные сообществом для сообщества. Эти независимые проекты сопровождаются вне основного репозитория Bambuddy и взаимодействуют с ним через публичный API.

!!! info "Хотите создать дополнение?"
    Bambuddy предоставляет документированный [REST API](../reference/api.md) и поддерживает [ключи API](../features/api-keys.md) для интеграций без графического интерфейса. Если вы что-то создали, откройте PR в [репозитории Wiki](https://github.com/maziggy/bambuddy-wiki), чтобы добавить проект сюда.

!!! warning "Независимые проекты"
    Дополнения сопровождаются их авторами, а не командой Bambuddy. Сообщения об ошибках и предложения функций направляйте в соответствующий репозиторий дополнения.

---

## :material-gesture-tap-button: Оборудование {#hardware}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-radiobox-marked: Bambutton](bambutton.md) {#bambutton}
Беспроводная кнопка на ESP32-C3 позволяет одним нажатием отметить пластину очищенной у принтера, чтобы следующее задание из очереди отправилось автоматически. Светодиодное кольцо сигнализирует о необходимости очистки.

**Автор:** [Edward Chamberlain](https://github.com/EdwardChamberlain) &middot; [Репозиторий](https://github.com/EdwardChamberlain/bambutton)
</div>

<div class="feature-card" markdown>
### [:material-nfc: ESPHome Bambu Spool Reader](https://github.com/bemble/esphome-bambuddy-reader) {#esphome-bambu-spool-reader}
Прошивка ESPHome для Waveshare ESP32-S3 с NFC-считывателем PN532 и сенсорным экраном 2,8 дюйма. Считывает метки катушек Bambu Lab и синхронизирует их с Bambuddy: находит известные катушки по UID метки, показывает оставшийся вес и позволяет регистрировать новые катушки прямо с устройства.

**Автор:** [bemble](https://github.com/bemble) &middot; [Репозиторий](https://github.com/bemble/esphome-bambuddy-reader)
</div>

<div class="feature-card" markdown>
### [:material-chip: ESP Virtual Printer](https://github.com/mengxyz/ESP_VP) {#esp-virtual-printer}
Прошивка ESP-IDF для ESP32-S3 с PSRAM представляет в локальной сети виртуальный принтер Bambu только для архивации. Слайсеры обнаруживают его как настоящий принтер и загружают файлы `.3mf` по неявному FTPS; ESP передаёт данные непосредственно в Bambuddy частями через HTTP, без SD-карты и локального хранилища. Сейчас работает с прилагаемым дополнительным сервисом `buddy_recv.py`.

**Автор:** [mengxyz](https://github.com/mengxyz) &middot; [Репозиторий](https://github.com/mengxyz/ESP_VP)
</div>

<div class="feature-card" markdown>
### [:material-nfc-search-variant: ESPoolBuddy](https://github.com/CSchlipp/espoolbuddy) {#espoolbuddy}
Прошивка ESPHome превращает недорогие платы ESP32-S3 в NFC-станцию катушек в стиле [SpoolBuddy](../spoolbuddy/index.md), без Raspberry Pi. **Консоль** (WT32-SC01 Plus, сенсорный экран 3,5 дюйма и PN532) регистрируется в Bambuddy через HTTP API, отправляет сигналы состояния, опрашивает AMS и принтер и передаёт результаты NFC-сканирования. Если отсканировать метку и загрузить катушку в AMS в течение 60 секунд, она назначается автоматически. *Запись* метки запускается с панели Bambuddy. Необязательные **весы** (любой ESP32-S3, датчик HX711 и PN532) не связываются с Bambuddy напрямую: они находят консоль через mDNS и передают вес через неё, поэтому всё поступает под одним ID устройства. Поддерживаются глубокий сон и питание от батареи.

**Автор:** [CSchlipp](https://github.com/CSchlipp) &middot; [Репозиторий](https://github.com/CSchlipp/espoolbuddy)
</div>

<div class="feature-card" markdown>
### [:material-barcode-scan: NFC Handheld Case](https://makerworld.com/en/models/3043887) {#nfc-handheld-case}
Печатный корпус в форме сканера штрихкодов превращает консоль SpoolBuddy, ESPoolBuddy или SpoolEase в **ручной** NFC-сканер: управление одной рукой, все элементы под большим пальцем, поэтому можно обходить полки и сканировать катушки на месте. Два варианта сборки: ввод кабеля для постоянного USB-C-подключения либо отсек батареи и контакты pogo-pin для беспроводной работы с магнитной зарядкой (Adafruit PowerBoost 1000C + 18650). Руководство по сборке описывает термовставки, PN532 на задней крышке и дополнительный динамик.

**Дизайнер:** Chris ([CSchlipp](https://github.com/CSchlipp)) &middot; [Модель MakerWorld](https://makerworld.com/en/models/3043887)
</div>

<div class="feature-card" markdown>
### [:material-keyboard-outline: Bambuddy Keypad](https://github.com/Bridawg/bambuddy-keypad) {#bambuddy-keypad}
Контроллер MicroPython без экрана на Raspberry Pi Pico W с Pimoroni Pico Display Pack появился после поломки LCD: интерфейс состоит из четырёх кнопок и одного RGB-светодиода. Цвет и движение показывают состояние: зелёный постоянно — ожидание, синий пульсирует — печать, жёлтый постоянно — пауза, жёлтый **мигает** — пластина готова и задание ожидает, красный — ошибка HMS, тусклый белый сигнал — офлайн. Кнопки позволяют приостановить и продолжить печать, подтвердить очистку пластины и отправить следующее задание, сбросить ошибку HMS и повторить последнюю архивную печать. Для двух необратимых действий требуется удержание 2 секунды с нарастающим сигналом светодиода. Устройство опрашивает REST API Bambuddy напрямую по Wi-Fi примерно каждые 3 секунды, без промежуточного ПК; действия записи используют ключ API.

**Автор:** [Bridawg](https://github.com/Bridawg) &middot; [Репозиторий](https://github.com/Bridawg/bambuddy-keypad)
</div>

</div>

---

## :material-cellphone: Мобильные приложения {#mobile-apps}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-android: BuddyDash](https://github.com/ChronosWing/BuddyDash) {#buddydash}
Неофициальное приложение Android для Bambuddy: мобильная панель быстрого просмотра состояния нескольких принтеров, архива и управления умными розетками. Быстрые NFC-действия позволяют приложить метку на принтере для подтверждения очистки пластины, переключения питания с проверкой простоя или запуска процедуры завершения без навигации по приложению. Создано на Jetpack Compose с компоновкой для телефонов, складных устройств и планшетов. Подключается к существующему серверу Bambuddy через REST API и дополняет его.

!!! warning "Бета-версия"
    Автор указывает, что BuddyDash находится в бета-версии: возможны недоработки и несовместимые изменения. Следите за стабильными выпусками в репозитории.

**Автор:** [ChronosWing](https://github.com/ChronosWing) &middot; [Репозиторий](https://github.com/ChronosWing/BuddyDash)
</div>

<div class="feature-card" markdown>
### [:material-watch: Bambuddy Mobile](https://github.com/DoYouHost/bambuddy-mobile) {#bambuddy-mobile}
Приложение Flutter для **телефонов Android и часов Wear OS**: состояние, прогресс, температуры и время окончания в реальном времени; пауза, продолжение, остановка и управление очередью; учёт филамента со сканированием QR; напоминания об обслуживании, оповещения оборудования и виджеты главного экрана. Приложение часов работает самостоятельно или через телефон, если сервер напрямую недоступен. Связывается только с вашим Bambuddy через REST и WebSocket API, без облака Bambu; деморежим позволяет изучить интерфейс без сервера. Лицензия AGPL-3.0, как у Bambuddy.

!!! warning "Бета-версия"
    Опубликовано в [Google Play](https://play.google.com/store/apps/details?id=page.codeberg.morganmlgman.bambuddy_mobile). Приложение всё ещё в **бета-версии**; APK для телефона и часов также доступны на GitHub. Возможны недоработки.

**Автор:** [DoYouHost](https://github.com/DoYouHost) &middot; [Репозиторий](https://github.com/DoYouHost/bambuddy-mobile)
</div>

</div>

---

## :material-monitor: Настольные приложения {#desktop}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-apple: BambuddyTray](https://github.com/bcsutar/BambuddyTray) {#bambuddytray}
Нативное приложение macOS в **строке меню**. Показывает значок состояния (&check; исправно, ! требуется внимание, &times; ошибка), текущий прогресс и оставшееся время; при изменении состояния печати или недоступности сервера может отправлять уведомления macOS. За печатью можно следить без вкладки браузера, Bambu Handy и облачной учётной записи. Подключается к Bambuddy с ключом API, включает интерфейс настроек и LaunchAgent для запуска при входе, распространяется как подписанный и нотариально заверенный `.dmg`.

**Автор:** [bcsutar](https://github.com/bcsutar) &middot; [Репозиторий](https://github.com/bcsutar/BambuddyTray)
</div>

</div>

---

## :material-monitor-dashboard: Панели и дисплеи {#dashboards-displays}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-mirror: MMM-Bambuddy](https://github.com/fbthpg/MMM-Bambuddy) {#mmm-bambuddy}
Модуль [MagicMirror&sup2;](https://magicmirror.builders/) показывает принтеры на зеркале: название, состояние (онлайн / печать / офлайн / ошибка), текущий прогресс и при ошибке HMS — сообщение и снимок камеры. Запросы к Bambuddy выполняются серверным `node_helper.js`, поэтому ключ API не попадает в браузер; снимки проксируются через собственный маршрут модуля после получения краткосрочного токена камеры, поскольку конечные точки снимков Bambuddy предназначены для `<img>`, не передающих заголовки. Одного ключа с разрешением **Чтение состояния** достаточно для опроса и токена. `npm install` не требуется: используется встроенный `fetch` Node. Работает с локальными и облачными принтерами, обращаясь только к Bambuddy.

**Автор:** [fbthpg](https://github.com/fbthpg) &middot; [Репозиторий](https://github.com/fbthpg/MMM-Bambuddy)
</div>

</div>

---

## :material-google-chrome: Расширения браузера {#browser-extensions}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-google-chrome: MakerWorld Import Extension](https://github.com/wolfrage76/Bambuddy-Extension) {#makerworld-import-extension}
Расширение Chrome (MV3) отправляет любой профиль печати MakerWorld в Bambuddy одним нажатием, как встроенная функция *Импорт из MakerWorld*. Определяет модели через соответствующую конечную точку Bambuddy (фрагмент URL заранее выбирает профиль при `#profileId-XXXXX`), отмечает уже имеющиеся профили значком ✓ и получает миниатюры через прокси Bambuddy, чтобы ваш IP не обращался напрямую к CDN MakerWorld. Укажите базовый URL Bambuddy и ключ API с разрешениями **Управление библиотекой** и **Разрешить доступ к облаку**; проверка соединения вызывает `/health`, `/system/info` и `/makerworld/status` для выявления ошибок до сохранения.

**Автор:** [wolfrage76](https://github.com/wolfrage76) &middot; [Репозиторий](https://github.com/wolfrage76/Bambuddy-Extension)
</div>

</div>

---

## :material-home-automation: Домашняя автоматизация {#home-automation}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-toggle-switch-outline: Hubitat Driver](https://github.com/jc21/hubitat-bambuddy) {#hubitat-driver}
Драйвер Hubitat Elevation взаимодействует с Bambuddy через REST и при необходимости MQTT. Позволяет использовать состояние принтеров Bambu Lab и команды очистки пластины, включения и выключения подсветки, паузы, продолжения и остановки в правилах домашней автоматизации и физических кнопках.

**Автор:** [jc21](https://github.com/jc21) &middot; [Репозиторий](https://github.com/jc21/hubitat-bambuddy)
</div>

<div class="feature-card" markdown>
### [:material-home-assistant: Home Assistant Sidecar Add-ons](https://github.com/tzimpel/homeassistant-bambuddy-sidecar-apps) {#home-assistant-sidecar-add-ons}
Репозиторий дополнений Home Assistant, упаковывающий необязательные дополнительные контейнеры Bambuddy для установки одним нажатием. Включает сервис OrcaSlicer API для нарезки на сервере: добавьте URL репозитория в магазин дополнений Home Assistant и установите его без написания Docker Compose.

**Автор:** [Jan-Tobias Zimpel](https://github.com/tzimpel) &middot; [Репозиторий](https://github.com/tzimpel/homeassistant-bambuddy-sidecar-apps)
</div>

<div class="feature-card" markdown>
### [:material-home-assistant: Home Assistant App (Bambuddy)](https://github.com/Spegeli/homeassistant-app-bambuddy) {#home-assistant-app-bambuddy}
Репозиторий дополнений Home Assistant, предоставляющий сам Bambuddy как приложение HA для установок Supervised и HA OS. Доступны стабильная, бета- и ежедневная версии с ежечасной проверкой обновлений, поэтому новые выпуски Bambuddy появляются прямо в магазине приложений HA. Данные сохраняются в `addon_configs` при обновлениях. Примечание: HA Ingress не поддерживается из-за ограничений SPA и service worker.

**Автор:** [Spegeli](https://github.com/Spegeli) &middot; [Репозиторий](https://github.com/Spegeli/homeassistant-app-bambuddy)
</div>

<div class="feature-card" markdown>
### [:material-puzzle-outline: HACS Integration](https://github.com/Spegeli/hacs_bambuddy) {#hacs-integration}
Пользовательская интеграция HACS подключает Bambuddy к Home Assistant. Каждый принтер представлен устройством с датчиками состояния, прогресса, слоя, температур, скоростей вентиляторов, HMS и диагностики, камерой, обложкой задания, кнопками паузы, продолжения, остановки и очистки пластины, переключателем подсветки камеры и выбором скорости печати. Также доступны общие показатели экземпляра: количество печатей, расход филамента и использование диска.

!!! warning "Активная разработка"
    Автор предупреждает, что интеграция ещё не готова к рабочему применению: возможны несовместимые изменения и незавершённые функции. Следите за стабильными выпусками в репозитории.

**Автор:** [Spegeli](https://github.com/Spegeli) &middot; [Репозиторий](https://github.com/Spegeli/hacs_bambuddy)
</div>

</div>

---

## :material-package-variant-closed: Филамент и его учёт {#filament-inventory}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-puzzle-outline: FilaMan Bambuddy Plugin](https://github.com/Fire-Devils/filaman-bambuddy-plugin) {#filaman-bambuddy-plugin}
Плагин драйвера подключает [FilaMan](https://www.filaman.app) к Bambuddy. Получает данные слотов AMS в реальном времени через WebSocket, поддерживает ручное и автоматическое назначение катушек и при необходимости синхронизирует учёт катушек FilaMan с Bambuddy.

**Автор:** [FilaMan / Fire-Devils](https://github.com/Fire-Devils) &middot; [Репозиторий](https://github.com/Fire-Devils/filaman-bambuddy-plugin)
</div>

<div class="feature-card" markdown>
### [:material-database-import-outline: CSV Spool Import](https://github.com/bsaunder/bambuddy_spoolimport) {#csv-spool-import}
Набор инструментов Python для управления филаментом Bambuddy из командной строки. Массово импортирует катушки из CSV, сопоставляя существующие ID катушек с ID каталога Bambuddy, выводит список катушек и создаёт PDF-этикетки с QR-кодами для печати. Использует публичный REST API Bambuddy с ключом API и полезен при переходе с других систем учёта.

**Автор:** [bsaunder](https://github.com/bsaunder) &middot; [Репозиторий](https://github.com/bsaunder/bambuddy_spoolimport)
</div>

<div class="feature-card" markdown>
### [:material-cash-sync: Spoolman Cost Sync](https://github.com/ojimpo/bambuddy-spoolman-cost-sync) {#spoolman-cost-sync}
Дополнительный сервис Python копирует цены катушек из Spoolman в локальное поле `cost_per_kg` Bambuddy, чтобы расчёт стоимости печати учитывал фактическую цену. Доступ к Spoolman только для чтения; в Bambuddy изменяется только `cost_per_kg`. Катушки с RFID сопоставляются по `tray_uuid` / `tag_uid`, синхронизация выполняется с настраиваемым интервалом (по умолчанию 10 минут). Используется только стандартная библиотека; поставляется как дополнительный контейнер Docker.

**Автор:** [ojimpo](https://github.com/ojimpo) &middot; [Репозиторий](https://github.com/ojimpo/bambuddy-spoolman-cost-sync)
</div>

<div class="feature-card" markdown>
### [:material-cellphone-nfc: Spool NFC PWA](https://github.com/Poltavtcev/spool-nfc-pwa) {#spool-nfc-pwa}
Мобильное PWA для чтения и записи NFC-меток OpenTag3D (NTAG213/215/216) на сторонних катушках прямо с телефона. Позволяет сканировать метку для поиска или привязки катушки, просматривать филамент с фильтрами материалов и индикаторами остатка, назначать катушки слотам AMS и внешним слотам и обновлять вес через REST API Bambuddy с ключом API. Устанавливается как PWA на Android и в настольном Chrome; учётные данные хранятся только в браузере. Для записи NFC нужен Chrome на Android с Web NFC; на iOS PWA устанавливается, но NFC недоступен.

**Автор:** [Poltavtcev](https://github.com/Poltavtcev) &middot; [Репозиторий](https://github.com/Poltavtcev/spool-nfc-pwa)
</div>

<div class="feature-card" markdown>
### [:material-nfc-variant: BambuMan](https://github.com/bambuman/BambuMan.App) {#bambuman}
Приложение для Android (встроенный NFC) и Windows (считыватель PCSC, например ACR122U) читает NFC-метки катушек Bambu Lab и импортирует их непосредственно в филамент Bambuddy. Каждая метка сопоставляется со SpoolmanDB для автоматического заполнения производителя, материала, цвета, веса и соответствующего пресета слайсера Bambuddy; последующие сканирования обновляют катушку по `tray_uuid` вместо создания дубликата. В настройках выбирается сервер Bambuddy или Spoolman, поэтому катушка, передаваемая AMS через MQTT, соответствует импортированной BambuMan. Приложение доступно в Google Play и F-Droid; проект также ведёт публичную библиотеку NFC-меток [bambuman.ee](https://bambuman.ee).

**Автор:** [bambuman](https://github.com/bambuman) &middot; [Репозиторий](https://github.com/bambuman/BambuMan.App)
</div>

<div class="feature-card" markdown>
### [:material-nfc-tap: SpoolTap](https://github.com/dmuth23/spooltap) {#spooltap}
Учёт филамента через NFC с автоматизацией Home Assistant. Приклейте метку NTAG215 к каждому слоту AMS и каждой катушке, сканируйте приложением HA Companion на Android, и SpoolTap отправит соответствующий пользовательский профиль филамента на принтер через Bambuddy. Профиль берётся из Spoolman, применяется к отсканированному слоту через API Bambuddy (коды `setting_id`), а `ha-bambulab` передаёт сведения о расходе обратно в Spoolman. Вся связующая логика работает в автоматизациях HA без отдельного сервера.

**Автор:** [dmuth23](https://github.com/dmuth23) &middot; [Репозиторий](https://github.com/dmuth23/spooltap)
</div>

<div class="feature-card" markdown>
### [:material-barcode-scan: Filament Box Scanner](https://github.com/hibikipr/filament_to_bambuddy) {#filament-box-scanner}
Мобильное веб-приложение (PWA) добавляет сторонние катушки в учёт Bambuddy по штрихкоду коробки или фотографии этикетки. Поиск выполняется последовательно: локальный кэш подтверждённых штрихкодов → [Open Filament Database](https://openfilamentdatabase.org), каталог производителя, материала, цвета, веса и температур по штрихкоду → пустая форма с автозаполнением по вставленному названию. Распознавание этикетки выполняется на устройстве и работает через обычный HTTP, в отличие от камеры в реальном времени, требующей HTTPS. Подтверждённые данные запоминаются для каждого штрихкода и автоматически заполняются при следующем сканировании. API филамента Bambuddy используется с ключом и разрешением **Управление филаментом**. Доступен многоархитектурный образ Docker на GHCR и установка PWA на Android и iOS.

**Автор:** [Victor Manuel / hibikipr](https://github.com/hibikipr) &middot; [Репозиторий](https://github.com/hibikipr/filament_to_bambuddy)
</div>

<div class="feature-card" markdown>
### [:material-console-line: bambuddy-cli](https://github.com/mailletf/bambuddy-cli) {#bambuddy-cli}
Терминальная утилита для назначения слотов AMS и изменения метаданных архива без веб-интерфейса. Решает ограничение AMS Lite, не читающей RFID и показывающей сторонние катушки как занятые, но неизвестные: CLI обращается к API назначения Bambuddy и связывает любую катушку с любым блоком и слотом. Также изменяет стоимость, заметки, теги, состояние, причину неудачи и внешний URL архивных печатей одноразовыми командами или через интерактивный выбор. Использует REST API Bambuddy; хост и ID принтера берутся из `.env` или флагов. Для обхода особенности IPv6-сокетов macOS вызывает `curl`.

**Автор:** [mailletf](https://github.com/mailletf) &middot; [Репозиторий](https://github.com/mailletf/bambuddy-cli)
</div>

</div>

---

## :material-folder-multiple-outline: Библиотеки моделей {#model-libraries}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-folder-sync-outline: Manyfold Sync](https://github.com/hibikipr/bambuddy_to_manyfold) {#manyfold-sync}
Инструмент Python синхронизирует **архивы печати и библиотеку файлового менеджера** Bambuddy с [Manyfold](https://manyfold.app), локальным менеджером 3D-моделей. Иерархия папок Bambuddy воссоздаётся как вложенные коллекции Manyfold, метаданные MakerWorld (описание, теги, обложка) дополняют модель, а несколько профилей одного дизайна можно сгруппировать. Загрузка использует возобновляемый API Manyfold (Tus), требующий приложение OAuth с **client_credentials** и разрешением `upload`: личные токены доступа его не поддерживают, поэтому скрипт проверяет разрешение при запуске и при отсутствии выводит инструкцию. Локальный файл состояния пропускает уже синхронизированное при повторном запуске, а `--force` восстанавливает элементы с неудачной фоновой загрузкой. Есть CLI, настольный GUI Tkinter и веб-интерфейс; `--cleanup-empty` удаляет модели Manyfold без файлов после неудачных загрузок (также требует `delete`).

**Автор:** [Victor Manuel / hibikipr](https://github.com/hibikipr) &middot; [Репозиторий](https://github.com/hibikipr/bambuddy_to_manyfold)
</div>

</div>

---

## :material-message-text-outline: Боты и уведомления {#bots-notifications}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-discord: Discord Print Queue Bot](https://github.com/CrazyClone55/bambuddy-discord-bot) {#material-discord-discord-print-queue-bot}
Бот Discord превращает канал форума в очередь печати. Участники отправляют файлы `.gcode.3mf` командой `!print` в ветке; задания суперпользователей сразу попадают в очередь, остальные требуют реакции назначенного утверждающего. Бот уведомляет отправителя о начале и завершении печати.

**Автор:** [CrazyClone55](https://github.com/CrazyClone55) &middot; [Репозиторий](https://github.com/CrazyClone55/bambuddy-discord-bot)
</div>

<div class="feature-card" markdown>
### [:material-message-processing-outline: Signal Print Queue Bot](https://github.com/phieb/bambu-bot) {#signal-print-queue-bot}
Бот Signal превращает ссылки MakerWorld в задания Bambuddy. Отправьте ссылку в личном сообщении: бот создаст постоянную группу Signal и предложит выбрать профиль, пластины и слоты AMS для каждого филамента нумерованными ответами, повторно нарежет модель для выбранного принтера и поставит пластины в очередь через REST API Bambuddy. Команды группы показывают прогресс со снимком камеры (`!progress`), текущую очередь (`!queue`) и отменяют последнее ожидающее задание (`!cancel`); бот сообщает группе об успешной и неудачной печати. Сообщения поступают от внешнего диспетчера ([signal-router](https://github.com/phieb/signal-router) автора или другого посредника, вызывающего `/claims` и `/receive`).

**Автор:** [phieb](https://github.com/phieb) &middot; [Репозиторий](https://github.com/phieb/bambu-bot)
</div>

<div class="feature-card" markdown>
### [:material-printer-pos: PrinterPrinter](https://github.com/FirstBuild/PrinterPrinter) {#printerprinter}
Локальный демон Python отслеживает начало печати в Bambuddy и автоматически печатает этикетку задания на Brother QL-820NWB. Опрашивает REST API с настраиваемым интервалом (по умолчанию 5 секунд), обнаруживает новые задания, сохраняет события в SQLite и создаёт этикетку DK1202 (62&times;100 мм) с массой филамента и при необходимости стоимостью (цена за грамм &times; округлённые граммы). Административные конечные точки: `/health`, `/admin/printers`, `/admin/events` и повторная печать (`POST /admin/print-event/{id}`). Включает интерактивный установщик Raspberry Pi для постоянной работы.

**Автор:** [FirstBuild](https://github.com/FirstBuild) &middot; [Репозиторий](https://github.com/FirstBuild/PrinterPrinter)
</div>

<div class="feature-card" markdown>
### [:material-bell-ring-outline: Bambark](https://github.com/rogernolan/Bambark) {#bambark}
Небольшая служба Go получает [уведомления вебхуков](../features/notifications.md) Bambuddy и пересылает их в [Bark](https://bark.day.app), приложение push-уведомлений iOS с открытым кодом. События завершения и неудачи печати поступают на iPhone без учётной записи. Принимает вебхук как POST JSON или параметры GET, аутентифицирует отправителей bearer-токеном, предоставляет `/healthz` и включает службы systemd для запуска с root и без него. Предназначена для работы в локальной сети за вашим обратным прокси: сама TLS не завершает.

**Автор:** [Roger Nolan](https://github.com/rogernolan) &middot; [Репозиторий](https://github.com/rogernolan/Bambark)
</div>

</div>

---

## :material-server-network: Развёртывание и автоматизация {#deployment-automation}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-script-text-outline: Ansible Collection](https://github.com/nils-ost/ansible-collection-bambuddy) {#ansible-collection}
Коллекция Ansible для масштабной установки и настройки Bambuddy. Включает модули первоначальной настройки, входа и получения токена, параметров, управления принтерами и виртуального принтера; все вызываются через `delegate_to: localhost`.

**Автор:** [nils-ost](https://github.com/nils-ost) &middot; [Репозиторий](https://github.com/nils-ost/ansible-collection-bambuddy)
</div>

<div class="feature-card" markdown>
### [:material-kubernetes: Helm Chart](https://github.com/npelikan/bambuddy-helm) {#helm-chart}
Helm chart для запуска Bambuddy в Kubernetes. Фиксирует образ `ghcr.io/maziggy/bambuddy` через `appVersion`, по умолчанию использует SQLite на постоянном томе с возможностью внешнего PostgreSQL через `database.externalDatabase`, сохраняет `/app/data` и `/app/logs` на отдельных PVC. Поддерживает `hostNetwork` для обнаружения SSDP и предоставляет `NET_BIND_SERVICE`, чтобы пользователь без root мог привязать порты виртуального принтера (322, 990 и пассивный FTP). Предназначен для одного экземпляра: стратегия `Recreate` корректно освобождает том RWO между развёртываниями.

**Автор:** [npelikan](https://github.com/npelikan) &middot; [Репозиторий](https://github.com/npelikan/bambuddy-helm)
</div>

</div>

---

## :material-robot-outline: ИИ и интеграции {#ai-integrations}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-cog-outline: Bambuddy MCP Server](bambuddy-mcp.md) {#bambuddy-mcp-server}
Сервер Model Context Protocol предоставляет полный REST API Bambuddy в виде инструментов ИИ-ассистентам, таким как Claude Desktop и Claude Code. Общайтесь с принтерами обычным языком: проверяйте состояние, ставьте задания в очередь, управляйте подсветкой и смотрите снимки камеры.

**Автор:** [MrMebelMan](https://github.com/MrMebelMan) &middot; [Репозиторий](https://github.com/MrMebelMan/bambuddy-mcp)
</div>

<div class="feature-card" markdown>
### [:material-shield-key-outline: Print Concierge](https://github.com/dpdev69/print-concierge) {#print-concierge}
Защитный слой Python MCP / CLI между агентными клиентами (Claude, Codex, Hermes, OpenClaw) и сервером Bambuddy. Предоставляет отобранные инструменты для поиска в архиве, поиска моделей через 3DSEARCH, импорта из MakerWorld / Printables / Thingiverse, нарезки с явными пресетами, проверки хешей результатов и составления детерминированных планов печати; прямые команды очереди, запуска, паузы и отмены намеренно **не предоставляются**. Единственное чувствительное действие — ограниченный `queue_print_request(request_id)`, отправляющий заранее утверждённый запрос, связанный с хешем плана; ведётся журнал аудита, а по умолчанию используется ручной запуск Bambuddy. Связь с Bambuddy идёт через REST API и ключ с минимальными правами (`BAMBUDDY_BASE_URL` + `BAMBUDDY_API_KEY`). Для демонстрации без оборудования доступен режим песочницы.

**Автор:** [dpdev69](https://github.com/dpdev69) &middot; [Репозиторий](https://github.com/dpdev69/print-concierge)
</div>

</div>

---

## Добавление дополнения {#contributing-an-add-on}

Если вы создали интеграцию с Bambuddy — устройство, расширение браузера, мобильную команду, скрипт, панель или что-то ещё — её можно добавить сюда.

Чтобы предложить проект:

1. Убедитесь, что у проекта есть понятный README и лицензия.
2. Откройте PR в [репозитории Wiki](https://github.com/maziggy/bambuddy-wiki), добавив страницу в `docs/community/` и карточку на эту страницу.
3. Обеспечьте совместимость: дополнения должны использовать публичный API Bambuddy, а не внутренние конечные точки.

Остались вопросы? Заходите в [Discord](https://discord.gg/aFS3ZfScHM).
