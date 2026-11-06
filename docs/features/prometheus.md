# Метрики Prometheus {#prometheus-metrics}

Bambuddy может предоставлять телеметрию принтеров в формате Prometheus для интеграции с **Grafana**, **Prometheus** и другими системами мониторинга.

## Настройка {#configuration}

Чтобы включить конечную точку, откройте **Настройки → Сеть → Метрики Prometheus**.

### Настройки {#settings}

| Настройка | Описание | По умолчанию |
|---------|-------------|---------|
| **Включить конечную точку метрик** | Включить или выключить конечную точку | Выключено |
| **Bearer Token** | Необязательный токен аутентификации | Пусто (без аутентификации) |

!!! note "Сохранённый токен"
    После сохранения токен больше не показывается: вместо него в поле отображается `********`. Оставьте поле как есть, чтобы сохранить значение, очистите его для удаления или введите новый токен. Сохраните копию для конфигурации Prometheus.

## Конечная точка {#endpoint}

```
GET /api/v1/metrics
```

Возвращает метрики в [текстовом формате Prometheus](https://prometheus.io/docs/instrumenting/exposition_formats/).

### Аутентификация {#authentication}

Если настроен bearer-токен, запросы должны содержать заголовок `Authorization`:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" http://bambuddy:8000/api/v1/metrics
```

Если токен не настроен, включённая конечная точка доступна всем.

---

## Доступные метрики {#available-metrics}

### Подключение принтера {#printer-connection}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_printer_connected` | gauge | Состояние подключения (1 — подключён, 0 — отключён) |
| `bambuddy_printer_state` | gauge | Состояние принтера (0 — неизвестно, 1 — ожидание, 2 — работает, 3 — пауза, 4 — завершено, 5 — ошибка) |

### Ход печати {#print-progress}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_print_progress` | gauge | Ход текущей печати (0–100%) |
| `bambuddy_print_remaining_seconds` | gauge | Расчётное оставшееся время в секундах |
| `bambuddy_print_layer_current` | gauge | Номер текущего слоя |
| `bambuddy_print_layer_total` | gauge | Общее количество слоёв текущей печати |

### Температуры {#temperatures}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_bed_temp_celsius` | gauge | Текущая температура стола |
| `bambuddy_bed_target_celsius` | gauge | Целевая температура стола |
| `bambuddy_nozzle_temp_celsius` | gauge | Текущая температура сопла (метка: nozzle=0/1) |
| `bambuddy_nozzle_target_celsius` | gauge | Целевая температура сопла |
| `bambuddy_chamber_temp_celsius` | gauge | Температура камеры (только серии X1/H2/P2) |

### Вентиляторы {#fans}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_fan_speed_percent` | gauge | Скорость вентилятора (метка: fan=part/aux/chamber) |

### Сеть {#network}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_wifi_signal_dbm` | gauge | Уровень сигнала Wi-Fi в дБм |

### Статистика (счётчики) {#statistics-counters}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_prints_total` | counter | Число печатей по результату (метка: result=completed/failed/и т. д.) |
| `bambuddy_printer_prints_total` | counter | Общее число печатей на принтер |
| `bambuddy_filament_used_grams` | counter | Общий расход филамента в граммах |
| `bambuddy_print_time_seconds` | counter | Общее время печати в секундах |

### Очередь {#queue}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_queue_pending` | gauge | Число ожидающих заданий в очереди |
| `bambuddy_queue_printing` | gauge | Число заданий очереди, которые сейчас печатаются |

### Система {#system}

| Метрика | Тип | Описание |
|--------|------|-------------|
| `bambuddy_printers_connected` | gauge | Число подключённых принтеров |
| `bambuddy_printers_total` | gauge | Число настроенных принтеров |

---

## Метки {#labels}

Большинство метрик содержат метки для фильтрации:

| Метка | Описание |
|-------|-------------|
| `printer_id` | Числовой ID принтера |
| `printer_name` | Понятное название принтера |
| `serial` | Серийный номер принтера |
| `model` | Модель принтера (X1C, H2D и т. д.) |
| `nozzle` | Индекс сопла (0 или 1) для принтеров с двумя соплами |
| `fan` | Тип вентилятора (part, aux, chamber) |
| `result` | Результат печати (completed, failed, archived и т. д.) |

---

## Настройка Prometheus {#prometheus-configuration}

Добавьте Bambuddy в `prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'bambuddy'
    scrape_interval: 15s
    metrics_path: '/api/v1/metrics'
    static_configs:
      - targets: ['bambuddy-host:8000']
    # If using bearer token:
    bearer_token: 'YOUR_TOKEN'
```

В сетях Docker используйте имя контейнера или `host.docker.internal`:

```yaml
static_configs:
  - targets: ['host.docker.internal:8000']
```

---

## Панель Grafana {#grafana-dashboard}

### Примеры запросов {#example-queries}

**Температуры принтера:**
```promql
bambuddy_bed_temp_celsius{printer_name="X1C-1"}
bambuddy_nozzle_temp_celsius{printer_name="X1C-1", nozzle="0"}
```

**Ход печати:**
```promql
bambuddy_print_progress
```

**Доля успешных печатей (за последние 24 часа):**
```promql
increase(bambuddy_prints_total{result="completed"}[24h]) /
(increase(bambuddy_prints_total{result="completed"}[24h]) + increase(bambuddy_prints_total{result="failed"}[24h]))
```

**Скорость расхода филамента:**
```promql
rate(bambuddy_filament_used_grams[1h]) * 3600
```

### Примеры панелей {#sample-dashboard-panels}

1. **Stat Panel**: подключённые принтеры (`bambuddy_printers_connected`)
2. **Gauge Panel**: ход печати каждого принтера
3. **Time Series**: графики температуры (стол, сопло, камера)
4. **Time Series**: изменение уровня сигнала Wi-Fi
5. **Bar Chart**: печати по результату (`bambuddy_prints_total`)
6. **Table**: обзор состояния принтеров

---

## Пример вывода {#example-output}

```
# HELP bambuddy_printer_connected Printer connection status (1=connected, 0=disconnected)
# TYPE bambuddy_printer_connected gauge
bambuddy_printer_connected{printer_id="1",printer_name="X1C-1",serial="00M09C411500579",model="X1C"} 1

# HELP bambuddy_printer_state Printer state
# TYPE bambuddy_printer_state gauge
bambuddy_printer_state{printer_id="1",printer_name="X1C-1",serial="00M09C411500579"} 2

# HELP bambuddy_bed_temp_celsius Current bed temperature
# TYPE bambuddy_bed_temp_celsius gauge
bambuddy_bed_temp_celsius{printer_id="1",printer_name="X1C-1",serial="00M09C411500579"} 60.0

# HELP bambuddy_nozzle_temp_celsius Current nozzle temperature
# TYPE bambuddy_nozzle_temp_celsius gauge
bambuddy_nozzle_temp_celsius{printer_id="1",printer_name="X1C-1",serial="00M09C411500579",nozzle="0"} 220.0

# HELP bambuddy_prints_total Total number of prints by result
# TYPE bambuddy_prints_total counter
bambuddy_prints_total{result="completed"} 342
bambuddy_prints_total{result="failed"} 18

# HELP bambuddy_filament_used_grams Total filament used in grams
# TYPE bambuddy_filament_used_grams counter
bambuddy_filament_used_grams 2042.0

# HELP bambuddy_printers_connected Number of connected printers
# TYPE bambuddy_printers_connected gauge
bambuddy_printers_connected 2
```

---

## Устранение неполадок {#troubleshooting}

### Конечная точка Returns 404 {#endpoint-returns-404}

Метрики Prometheus отключены. Включите их в **Настройки → Сеть → Метрики Prometheus**.

### Конечная точка Returns 401 {#endpoint-returns-401}

Настроен bearer-токен. Добавьте заголовок `Authorization`:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" http://bambuddy:8000/api/v1/metrics
```

### Нет метрик принтера {#no-printer-metrics}

Если метрики отдельных принтеров отсутствуют:

- Убедитесь, что принтеры подключены (проверьте панель Bambuddy)
- Убедитесь, что принтеры отмечены как активные

### Prometheus показывает "Down" {#prometheus-shows-down}

- Убедитесь, что Bambuddy доступен из Prometheus
- Проверьте, разрешают ли правила межсетевого экрана порт 8000
- Проверьте совпадение bearer-токена, если он настроен
