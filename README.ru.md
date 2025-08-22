# 🤖 MyTONProvider Bot

[![TON](https://img.shields.io/badge/TON-grey?logo=TON\&logoColor=40AEF0)](https://ton.org)
[![Telegram Bot](https://img.shields.io/badge/Bot-grey?logo=telegram)](https://core.telegram.org/bots)
[![Python](https://img.shields.io/badge/Python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/)
[![License](https://img.shields.io/github/license/nessshon/mytonprovider-bot)](https://github.com/nessshon/mytonprovider-bot/blob/main/LICENSE)
[![Redis](https://img.shields.io/badge/Redis-Yes?logo=redis\&color=white)](https://redis.io/)
[![Docker](https://img.shields.io/badge/Docker-blue?logo=docker\&logoColor=white)](https://www.docker.com/)

**[English version](README.md)**

Telegram-бот для мониторинга **TON Storage провайдеров** в сети TON с поддержкой телеметрии, уведомлений и аналитики.

## Описание

Проект представляет собой систему мониторинга, которая собирает данные о провайдерах
с [mytonprovider.org](https://mytonprovider.org), сохраняет их в базе данных и предоставляет пользователям удобный
интерфейс в Telegram для просмотра, подписки и получения уведомлений.

## Возможности

* Просмотр списка всех провайдеров
* Поиск по публичному ключу
* Подписка на провайдеров и управление подписками
* Включение и отключение уведомлений
* Настройка типов уведомлений (CPU, RAM, диск, перезапуск сервисов и др.)
* Многоязычный интерфейс (RU, EN, ZH-TW)
* Автоматический мониторинг финансовых показателей
* Формирование отчетов по трафику и заработку

## Установка и запуск

### Требования

* Python 3.10+
* Redis
* Docker (опционально)

### Настройка окружения

1. Скопируйте `.env.example` в `.env`:

   ```bash
   cp .env.example .env
   ```

2. Отредактируйте `.env` и заполните необходимые переменные окружения:

| Переменная              | Описание                                 | Пример значения                               |
|-------------------------|------------------------------------------|-----------------------------------------------|
| `BOT_TOKEN`             | Токен Telegram-бота от @BotFather        | `1234567890:AAE...`                           |
| `TONCENTER_API_KEY`     | Ключ доступа к TONCenter API             | `abcd1234efgh5678...`                         |
| `MYTONPROVIDER_API_KEY` | Ключ доступа к MyTONProvider API         | `abcd1234efgh5678...`                         |
| `DB_URL`                | Строка подключения к базе данных         | `sqlite+aiosqlite:///./data/database.sqlite3` |
| `REDIS_URL`             | Адрес Redis для хранения состояния       | `redis://localhost:6379/0`                    |
| `ADMIN_PASSWORD`        | Пароль администратора для панели/доступа | `supersecret`                                 |

### Запуск

#### Локально

```bash
# Установка зависимостей
pip install -r requirements.txt

# Применение миграций базы данных
alembic upgrade head

# Запуск бота
python -m app
```

#### Через Docker

```bash
# Запуск в фоне
docker-compose up -d

# Просмотр логов
docker-compose logs -f
```

## Разработка

### Структура проекта

```
    .
    ├── app/                               # Основное приложение
    │   ├── bot/                           # Telegram-бот
    │   │   ├── commands.py                # Команды бота
    │   │   ├── dialogs/                   # Диалоги и состояния
    │   │   ├── handlers/                  # Обработчики событий
    │   │   ├── middlewares/               # Middleware
    │   │   └── utils/                     # Утилиты
    │   ├── database/                      # База данных
    │   │   ├── models/                    # SQLAlchemy модели
    │   │   ├── repository.py              # Репозиторий
    │   │   └── unitofwork.py              # Паттерн Unit of Work
    │   ├── scheduler/                     # Планировщик задач
    │   │   └── jobs/                      # Фоновые задачи
    │   │       ├── monitor_providers.py   # Синхронизация и обновление провайдеров
    │   │       ├── monitor_balances.py    # Учет балансов и заработка
    │   │       ├── monitor_traffics.py    # Сбор статистики по трафику
    │   │       └── monthly_reports.py     # Генерация ежемесячных отчетов
    │   └── utils/                         # Общие утилиты
    │       ├── alerts/                    # Система уведомлений
    │       ├── i18n/                      # Интернационализация
    │       ├── mtpapi/                    # Обертка для MyTONProvider API
    │       └── toncenter/                 # Обертка для TONCenter API
    ├── alembic/                           # Миграции БД
    ├── locales/                           # Файлы локализации
    ├── data/                              # База данных и служебные файлы
    └── docker-compose.yml                 # Конфигурация Docker
```

### Интеграции

* **MyTONProvider API** — данные о провайдерах и телеметрия
* **TONCenter API** — транзакции, балансы и доходность провайдеров

### Модели данных

* **Provider** — информация о провайдере
* **ProviderTelemetry** — телеметрия и метрики
* **ProviderWalletHistory** — история баланса и заработка
* **ProviderTrafficHistory** — статистика трафика
* **Telemetry** — расширенная телеметрия и метрики
* **User** — данные пользователя
* **UserSubscription** — подписки пользователей
* **UserAlertSetting** — индивидуальные настройки уведомлений

### Планировщик задач

* **monitor_providers** — синхронизация и обновление провайдеров
* **monitor_balances** — учет балансов и заработка
* **monitor_traffics** — сбор статистики по трафику
* **monthly_reports** — генерация ежемесячных отчетов

## Лицензия

Проект распространяется под лицензией [Apache-2.0](LICENSE).
