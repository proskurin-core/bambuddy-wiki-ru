---
title: SpoolBuddy
description: Дополнительное устройство на Raspberry Pi для идентификации и учёта катушек с помощью NFC
---

# SpoolBuddy {#spoolbuddy}

SpoolBuddy — дополнительное устройство для Bambuddy, позволяющее управлять катушками с помощью NFC. Используя Raspberry Pi и NFC-считыватель, можно снабдить катушки филамента NFC-наклейками и мгновенно распознавать их прикосновением.

SpoolBuddy подключает Raspberry Pi с NFC-считывателем к вашему экземпляру Bambuddy и предоставляет следующие возможности:

- **Мгновенная идентификация катушки** &mdash; приложите катушку, чтобы открыть сведения о ней.
- **Запись NFC-меток** &mdash; записывайте данные катушки на NFC-наклейки для удобства учёта.
- **Синхронизация филамента** &mdash; данные катушек автоматически синхронизируются с учётом филамента Bambuddy.
- **Отслеживание веса катушки** &mdash; текущий вес поступает с подключённых весов.
- **Быстрое меню** &mdash; проведите вниз от верхнего края, чтобы включать и выключать принтер через умные розетки и управлять устройством (перезапуск, перезагрузка, выключение).
- **Полностью локальная работа** &mdash; всё работает в вашей сети, облако не требуется.

---

## :material-rocket-launch: Начало работы {#getting-started}

<div class="feature-grid" markdown>

<div class="feature-card" markdown>
### [:material-clipboard-list: Требования к оборудованию](hardware.md) {#hardware-requirements}
Что понадобится для сборки актуальной версии SpoolBuddy: Raspberry Pi, сенсорный экран, NFC-считыватель PN5180, модуль весов и провода.
</div>

<div class="feature-card" markdown>
### [:material-format-list-checks: Необходимые материалы](materials.md) {#materials-required}
Практический список компонентов для полной сборки SpoolBuddy.
</div>

<div class="feature-card" markdown>
### [:material-vector-polyline: Схемы подключения](wiring-diagrams.md) {#wiring-diagrams}
Распиновка, текстовые схемы и наглядная схема в стиле Fritzing для подключения NFC-считывателя PN5180 и весов NAU7802 к разъёму GPIO Raspberry Pi.
</div>

<div class="feature-card" markdown>
### [:material-tools: Сборка](assembly.md) {#assembly}
Пошаговое руководство по сборке напечатанного на 3D-принтере корпуса и установке электроники.
</div>

<div class="feature-card" markdown>
### [:material-download: Установка](installation.md) {#installation}
Установите стек SpoolBuddy из репозитория Bambuddy и включите автоматический запуск киоска при загрузке.
</div>

<div class="feature-card" markdown>
### [:material-cog: Настройка](configuration.md) {#configuration}
Подключитесь к Bambuddy, настройте доступ к API и работу экрана и весов, проверьте работу NFC и меток.
</div>

<div class="feature-card" markdown>
### [:material-lan-connect: Режимы развёртывания](deployment-modes.md) {#deployment-modes}
Выберите между установкой всего на одном Pi (Bambuddy + SpoolBuddy) и режимом только SpoolBuddy.
</div>

<div class="feature-card" markdown>
### [:material-raspberry-pi: Поддерживаемые Raspberry Pi](supported-pis.md) {#supported-pis}
Таблица совместимости и практические рекомендации по моделям Raspberry Pi.
</div>

</div>
