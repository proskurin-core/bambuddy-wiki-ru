---
title: Установка
description: Установка SpoolBuddy на Raspberry Pi
---

# Установка {#installation}

---

## :material-check-all: Перед началом работы {#prerequisites}

Перед запуском установщика подготовьте:

- Raspberry Pi с Raspberry Pi OS 64-Bit с Pi Desktop (на базе Debian), доступом к интернету и `sudo`.
- Подключённое или готовое к подключению оборудование: NFC-считыватель PN5180, весы NAU7802 и дисплей.
- URL существующего сервера Bambuddy и ключ API (для режима только SpoolBuddy) либо план запуска полной установки на Pi.

---

## :material-rocket: Рекомендуемый способ установки {#recommended-install-method}

Используйте скрипт установки из репозитория Bambuddy:

```bash
curl -fsSL https://raw.githubusercontent.com/maziggy/bambuddy/refs/heads/dev/spoolbuddy/install/install.sh -o install.sh
chmod +x install.sh
sudo ./install.sh
```

Установщик настраивает:

- SPI, I2C и необходимые загрузочные оверлеи
- Системные пакеты и виртуальное окружение Python
- Службу демона SpoolBuddy (`spoolbuddy.service`)
- Среду сенсорного киоска (labwc, автозапуск Chromium и заставку)
- При необходимости локальную службу Bambuddy (полный режим)

---

## :material-form-select: Режимы установки {#installation-modes}

Подробности выбора режима и их особенности см. на странице [Режимы развёртывания](deployment-modes.md).

### Примеры автоматической установки {#unattended-examples}

```bash
# SpoolBuddy-only mode
sudo ./install.sh --mode spoolbuddy --bambuddy-url http://192.168.1.100:8000 --api-key bb_xxx --yes

# Full local mode
sudo ./install.sh --mode full --port 8000 --yes
```

---

## :material-restart: Первая загрузка после установки {#first-boot-after-install}

После установки необходима перезагрузка, чтобы применить:

- Изменения SPI/I2C и загрузочной конфигурации
- Автоматический вход и запуск сеанса киоска
- Изменения заставки и initramfs

```bash
sudo reboot
```

---

## :material-flask-outline: Проверка оборудования {#hardware-validation}

После перезагрузки убедитесь, что оборудование подключено и определяется правильно:

```bash
# SPI devices present
ls /dev/spidev0.*

# I2C bus 1 present
ls /dev/i2c-1

# NAU7802 visible on bus 1 (0x2A)
sudo i2cdetect -y 1
```

Затем запустите диагностику SpoolBuddy:

```bash
sudo /opt/bambuddy/spoolbuddy/venv/bin/python /opt/bambuddy/spoolbuddy/scripts/pn5180_diag.py
sudo /opt/bambuddy/spoolbuddy/venv/bin/python /opt/bambuddy/spoolbuddy/scripts/scale_diag.py
```

---

При установке в полном режиме создайте ключ API в **Bambuddy → Настройки → [Ключи API](../features/api-keys.md)**, добавьте `SPOOLBUDDY_API_KEY=bb_xxx` в `/opt/bambuddy/spoolbuddy/.env` и перезапустите службу (`sudo systemctl restart spoolbuddy`).

---

## :material-application-cog: Управление службами {#service-management}

```bash
# SpoolBuddy service status
sudo systemctl status spoolbuddy

# Follow SpoolBuddy logs
sudo journalctl -u spoolbuddy -f
```

При установке в полном режиме:

```bash
sudo systemctl status bambuddy
sudo journalctl -u bambuddy -f
```

---

## :material-clipboard-check: Проверка установки {#verify-the-setup}

1. Откройте Bambuddy в браузере.
2. Откройте путь киоска на Pi: `/spoolbuddy`.
3. Убедитесь, что устройство отображается в SpoolBuddy как подключённое.
4. Приложите известную метку к считывателю и убедитесь, что она обнаружена.
5. Проверьте, изменяются ли текущие показания весов.

Если оборудование отображается как отключённое, выполните описанную выше [диагностику оборудования](#hardware-validation).

---

## :material-alert-circle: Частые проблемы {#common-pitfalls}

- Неверный URL исходного скрипта (используйте `.../bambuddy/main/install/install.sh`)
- Отсутствует ключ API в режиме только SpoolBuddy
- Не включена шина I2C 0 (`dtparam=i2c_vc=on`)
- Отсутствует оверлей SPI CS (`dtoverlay=spi0-0cs`)
- Разрешения для NFC или весов не применились, поскольку перезагрузка была пропущена

!!! note "На киоске отображается «Can't reach Bambuddy. Retrying…»"
    Это сообщение появляется, когда киоск не может проверить свой ключ API, потому что Bambuddy или его база данных не отвечает, например при исчерпании соединений с базой или переключении на резервный узел. Ключ сохраняется, а киоск продолжает попытки: первая выполняется через 2 секунды, затем интервал увеличивается до минуты. Как только Bambuddy отвечает, SpoolBuddy снова появляется без перезагрузки. На страницу входа киоск переходит только после ответа Bambuddy о том, что ключ недействителен или срок его действия истёк либо его владелец деактивирован. В последнем случае ключ сохраняется: повторная активация владельца и перезагрузка киоска восстановят работу.
