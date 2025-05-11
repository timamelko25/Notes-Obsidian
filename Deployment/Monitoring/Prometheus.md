- [Monitoring System Types](#Monitoring%20System%20Types)
- [Monitoring vs Observability](#Monitoring%20vs%20Observability)
- [Prometheus Monitoring Agents](#Prometheus%20Monitoring%20Agents)
- [Prometheus Metric Types](#Prometheus%20Metric%20Types)
- [Prometheus Monitoring Infrastructure](#Prometheus%20Monitoring%20Infrastructure)
- [Prometheus Alert Manager](#Prometheus%20Alert%20Manager)
- [Запросы в Prometheus](#%D0%97%D0%B0%D0%BF%D1%80%D0%BE%D1%81%D1%8B%20%D0%B2%20Prometheus)

Prometheus - сбор метрик (бд) написанный на GO
Система мониторинга с открытым исходным кодом с размерной моделью данных, гибким языком запросов, эффективной базой данных временных рядов и современным подходом к оповещению.

Собирает и хранит метрики в виде данных Time Series в TSDB (Time Series Data Base), то есть информация и метриках сохраняется с отметкой времени, в которой она была записана

Monitoring - информация о части системы определенное время
- CPU Utilization
- Disk Space
- Количество файлов
- Количество ошибок
- Количество запросов

Для чего нужен мониторинг
- debug 
- состояние системы
- видеть тренд проблемы
- автоматизация (при нагрузке 100% -> перезагрузка)

# Monitoring System Types

- Push
	- на клиент серверах установлен Monitoring Agent
	- агент собирает данные о системе
	- посылает данные на Monitoring Server (удаленный)
- Pull
	- на клиент серверах установлен Monitoring Agent
	- агент собирает данные о системе
	- никуда не посылает, делает данные доступными из места доступа
	- Monitoring Server собирает данные из мест доступа

# Monitoring vs Observability

Monitoring - процесс сбора и создания дашбордов по различным показателям, определяющим работоспособность системы

Observability - исследовательский процесс. Изучает взаимодействие компонентов распределенной системы и данные собранные в ходе мониторинга, чтобы найти основную причину проблем (root cause). 

# Prometheus Monitoring Agents

В Prometheus называется Exporter. Каждый Exporter собирает свои метрики и на каждый сервис нужно ставить свой Exporter

https://prometheus.io/docs/instrumenting/exporters/

Все метрики хранятся на ручке `/metrics` и доступны на веб страничке под портом

# Prometheus Metric Types

- Counter
	- инкрементируемая
	- обнуляется после перезапуска
- Gauge (шкала)
	- инкрементируемая и декрементируемая
- Histogram
	- показывает гистограмму - распределение величин метрики по группам
- Summary
	- показывает перцентиль / квантиль из гистограммы

# Prometheus Monitoring Infrastructure
[[Prometheus Infrastructure.canvas|Prometheus Infrastructure]]
![[Pasted image 20250509150741.png]]

# Prometheus Alert Manager

Официальный менеджер для отправки алертов в различные клиенты (telegram, discord, email, etc)

Prometheus Main Files and Folders
```
data/ (db)
prometheus (bin)
prometheus.yml (config)
```

По умолчанию бд имеет тип TSDB:
- `path` - `:./data`
- TSDB Retention: 15 дней (удаление старых данных)

# Запросы в Prometheus

Для запросов используется PromQL
![[667c1611-4e99-4c59-b050-77fbae3a67b2.pdf]]