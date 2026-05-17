<div align="center">

# TechZone

**Полнофункциональная e-commerce платформа для электроники — каталог, сравнение, экспертные обзоры, ценовые алерты и гарантии в одной экосистеме.**

<br/>

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.x-000000?style=for-the-badge&logo=flask&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-8-005571?style=for-the-badge&logo=elasticsearch&logoColor=white)
![Celery](https://img.shields.io/badge/Celery-5-37814A?style=for-the-badge&logo=celery&logoColor=white)
![Docker](https://img.shields.io/badge/Docker_Compose-3.9-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-1.25-009639?style=for-the-badge&logo=nginx&logoColor=white)
![JWT](https://img.shields.io/badge/Auth-JWT-000000?style=for-the-badge)
![Stripe](https://img.shields.io/badge/Payments-Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white)

</div>

---

## Содержание

1. [О проекте](#1-о-проекте)
2. [Ключевые возможности](#2-ключевые-возможности)
3. [Технологический стек](#3-технологический-стек)
4. [Структура репозитория](#4-структура-репозитория)
5. [Архитектура и как это работает](#5-архитектура-и-как-это-работает)
6. [Доменная модель (крупными блоками)](#6-доменная-модель-крупными-блоками)
7. [Сервисы в Docker Compose](#7-сервисы-в-docker-compose)
8. [Быстрый старт (локально, Docker)](#8-быстрый-старт-локально-docker)
9. [Основные команды эксплуатации](#9-основные-команды-эксплуатации)
10. [Ручной запуск frontend и backend](#10-ручной-запуск-frontend-и-backend)
11. [Конфигурация и переменные окружения](#11-конфигурация-и-переменные-окружения)
12. [API, очереди и интеграции](#12-api-очереди-и-интеграции)
13. [Мониторинг и эксплуатация](#13-мониторинг-и-эксплуатация)
14. [CI/CD](#14-cicd)
15. [Безопасность и хранение данных](#15-безопасность-и-хранение-данных)
16. [Роли компонентов в продакшене](#16-роли-компонентов-в-продакшене)
17. [Лицензия](#17-лицензия)
18. [Поддержка](#18-поддержка)

---

## 1. О проекте

---

**TechZone** — production-ready платформа интернет-магазина электроники. Она объединяет полноценный каталог с детальными характеристиками, side-by-side сравнение до четырёх товаров, редакционные и пользовательские обзоры, историю цен с умными уведомлениями, оформление заказов и постпродажное управление гарантиями.

Платформа рассчитана на три аудитории:

| Аудитория | Сценарий |
|-----------|----------|
| **Покупатели** | Поиск, фильтрация, сравнение, корзина, заказы, отзывы, price alerts, гарантии |
| **Администраторы** | Управление каталогом, заказами, пользователями, редакционными обзорами |
| **Интеграторы** | REST API с JWT-аутентификацией для внешних клиентов и автоматизации |

### Что это за тип системы

TechZone — **многосервисная распределённая платформа**, а не монолитный скрипт. Слои чётко разделены: SPA-фронтенд, API-сервер, фоновые воркеры, поисковый движок и reverse proxy.

| Аспект | Описание |
|--------|----------|
| **Продукт** | B2C e-commerce для электроники с сравнением, обзорами, price tracking и warranty lifecycle |
| **Архитектура** | Flask API + React SPA + Celery workers + Elasticsearch + Nginx |
| **Хранилище** | PostgreSQL (метаданные и транзакции) + Redis (кэш и брокер Celery) + Elasticsearch (полнотекстовый поиск) + локальное файловое хранилище (изображения) |

---

## 2. Ключевые возможности

---

### Каталог и товары

- CRUD товаров с богатыми метаданными: бренд, категория, галерея, спецификации
- Иерархические категории (например, *Ноутбуки → Игровые ноутбуки*)
- Таблицы характеристик по категориям: CPU, RAM, накопитель, дисплей и др.
- Пагинация, сортировка, фасетная фильтрация

### Сравнение товаров

- До **4 товаров** в одном сравнении
- Автовыравнивание строк спецификаций
- Сохранение и шаринг списков сравнения
- Подсветка отличий между моделями

### Обзоры

- **Редакционные** обзоры с многомерной оценкой (производительность, цена/качество, дизайн и т.д.)
- **Пользовательские** отзывы с бейджем «подтверждённая покупка»
- Списки «за» и «против», итоговый вердикт
- Голосование за полезность отзыва

### Ценовые алерты

- Целевая цена на товар — уведомление при достижении
- История цен с временными метками
- Визуализация тренда (Recharts)
- Email и фоновые задачи через Celery Beat (интервал проверки — 6 часов)

### Гарантии

- Регистрация гарантии после покупки
- Отслеживание срока действия
- Подача и статус warranty claim
- Напоминания за 30 дней до истечения

### Заказы и оплата

- Корзина с персистентным состоянием (Redux)
- Многошаговый checkout
- Жизненный цикл заказа: Pending → Paid → Processing → Shipped → Delivered / Cancelled
- Интеграция **Stripe** для платежей

### Поиск и discovery

- Полнотекстовый поиск через **Elasticsearch 8**
- Фасеты: бренд, цена, характеристики
- Навигация по категориям, featured и trending секции

---

## 3. Технологический стек

---

| Слой | Технологии |
|------|------------|
| **Backend** | Python 3.11+, Flask 3, Flask-RESTful, SQLAlchemy 2, Alembic |
| **Frontend** | React 18, Redux Toolkit, React Router 6, Axios, Recharts |
| **БД** | PostgreSQL 15 |
| **Кэш / брокер** | Redis 7 |
| **Поиск** | Elasticsearch 8.11 |
| **Очереди** | Celery 5 + Celery Beat |
| **Сериализация** | Marshmallow, Flask-Marshmallow |
| **Auth** | JWT (Flask-JWT-Extended), bcrypt |
| **Платежи** | Stripe |
| **Почта** | Flask-Mail (SMTP) |
| **Прокси** | Nginx 1.25 (rate limiting, gzip, upstream) |
| **Контейнеризация** | Docker, Docker Compose 3.9 |
| **WSGI** | Gunicorn (4 workers в compose) |
| **Тесты** | pytest (backend) |

---

## 4. Структура репозитория

---

```
TechZone/
├── docker-compose.yml          # Оркестрация всех сервисов
├── .env.example                # Шаблон переменных окружения
├── README.md
├── nginx/
│   └── nginx.conf              # Reverse proxy, rate limit, gzip
├── backend/
│   ├── requirements.txt
│   ├── run.py                  # Точка входа Flask
│   ├── celery_worker.py        # Celery app
│   ├── Dockerfile
│   ├── tests/
│   └── app/
│       ├── __init__.py         # Application factory
│       ├── config.py           # Dev / Test / Prod конфиги
│       ├── extensions.py
│       ├── models/             # User, Product, Order, Review, …
│       ├── api/                # REST blueprints
│       ├── services/           # Бизнес-логика
│       ├── tasks/              # Celery: цены, email, уведомления
│       ├── schemas/            # Marshmallow DTO
│       ├── middleware/         # Rate limit, request logger
│       ├── utils/
│       └── migrations/
└── frontend/
    ├── package.json
    ├── public/
    └── src/
        ├── api/                # axiosClient, authApi, productApi, …
        ├── components/         # UI: Navbar, ProductCard, Comparison, …
        ├── pages/              # Login, Register, ProductList, Detail, …
        ├── store/              # Redux slices: auth, cart, product
        ├── hooks/
        └── styles/
```

---

## 5. Архитектура и как это работает

---

```
                    ┌─────────────────────┐
                    │   Nginx Reverse     │
                    │       Proxy         │
                    └──────────┬──────────┘
                               │
              ┌────────────────┴────────────────┐
              │                                 │
     ┌────────▼────────┐             ┌─────────▼─────────┐
     │  React Frontend │             │   Flask Backend    │
     │   (port 3000)   │             │   (port 5000)      │
     └─────────────────┘             └─────┬───┬───┬─────┘
                                           │   │   │
                        ┌──────────────────┘   │   └──────────────────┐
                        │                      │                      │
               ┌────────▼────────┐    ┌────────▼────────┐   ┌─────────▼──────────┐
               │   PostgreSQL    │    │      Redis       │   │  Elasticsearch   │
               │   (metadata)    │    │ cache + broker   │   │  (full-text)     │
               └─────────────────┘    └────────┬─────────┘   └────────────────────┘
                                               │
                                    ┌──────────▼──────────┐
                                    │  Celery Worker      │
                                    │  + Celery Beat      │
                                    └─────────────────────┘
```

### Поток запроса

1. Клиент обращается к **Nginx** (`:80`) — статика и SPA маршрутизируются на frontend, `/api/*` — на backend.
2. **Flask API** выполняет бизнес-логику, читает/пишет **PostgreSQL**, кэширует горячие данные в **Redis**.
3. **Elasticsearch** индексирует товары для поиска и фасетной фильтрации.
4. **Celery Worker** обрабатывает фоновые задачи из Redis: проверка цен, email, warranty reminders.
5. **Celery Beat** по расписанию запускает периодические задачи (price check каждые 6 ч).
6. **React SPA** общается с API через Axios; состояние — Redux Toolkit.

---

## 6. Доменная модель (крупными блоками)

---

| Сущность | Назначение |
|----------|------------|
| **User** | Учётная запись, роли (Guest / Customer / Admin), JWT-сессии |
| **Product** | Товар, цена, спецификации, изображения, связь с категорией и брендом |
| **Order** | Заказ, позиции, статус lifecycle, платёж |
| **Review** | Пользовательский и редакционный обзор, рейтинги, pros/cons |
| **Comparison** | Список сравнения (до 4 product ID), шаринг |
| **PriceAlert** | Целевая цена пользователя, флаг срабатывания |
| **PriceHistory** | История изменения цены для графиков |
| **Warranty** | Регистрация гарантии, claim, срок истечения |

### Роли и права

| Роль | Возможности |
|------|-------------|
| **Guest** | Каталог, поиск, просмотр обзоров и сравнений |
| **Customer** | Заказы, отзывы, price alerts, гарантии, сохранённые сравнения |
| **Admin** | CRUD каталога, tech reviews, управление заказами и пользователями, dashboard |

---

## 7. Сервисы в Docker Compose

---

| Сервис | Образ / сборка | Порт | Назначение |
|--------|----------------|------|------------|
| `db` | postgres:15-alpine | 5432 | Основная БД, healthcheck `pg_isready` |
| `redis` | redis:7-alpine | 6379 | Кэш, Celery broker/result backend |
| `elasticsearch` | ES 8.11 (single-node) | 9200 | Поиск и индексация товаров |
| `backend` | `./backend` (Gunicorn ×4) | 5000 | REST API |
| `celery_worker` | `./backend` | — | Фоновые задачи |
| `celery_beat` | `./backend` | — | Планировщик (price checks, reminders) |
| `frontend` | `./frontend` | 3000 | React dev/build сервер |
| `nginx` | nginx:1.25-alpine | 80, 443 | Единая точка входа |

**Volumes:** `postgres_data`, `redis_data`, `es_data` — персистентность данных между перезапусками.

---

## 8. Быстрый старт (локально, Docker)

---

### Требования

- [Docker](https://docs.docker.com/get-docker/) и Docker Compose
- Git

### Запуск за 4 шага

```bash
# 1. Клонирование
git clone https://github.com/NodirOdilov/TechZone.git
cd TechZone

# 2. Окружение
cp .env.example .env
# Отредактируйте SECRET_KEY, JWT_SECRET_KEY, SMTP и Stripe при необходимости

# 3. Сборка и запуск
docker compose up --build -d

# 4. Миграции БД (первый запуск)
docker compose exec backend flask db upgrade
```

### Точки доступа

| Сервис | URL |
|--------|-----|
| **Веб-приложение** | http://localhost |
| **API** | http://localhost/api |
| **Backend напрямую** | http://localhost:5000 |
| **Frontend напрямую** | http://localhost:3000 |
| **Elasticsearch** | http://localhost:9200 |

---

## 9. Основные команды эксплуатации

---

```bash
# Поднять / остановить стек
docker compose up -d
docker compose down

# Логи (все сервисы или один)
docker compose logs -f
docker compose logs -f backend celery_worker

# Пересборка после изменений
docker compose up --build -d

# Миграции
docker compose exec backend flask db upgrade
docker compose exec backend flask db migrate -m "описание"

# Celery вручную (если не через compose)
docker compose exec celery_worker celery -A celery_worker.celery inspect active

# Тесты backend
docker compose exec backend pytest -v
```

---

## 10. Ручной запуск frontend и backend

---

### Backend (без Docker)

```bash
cd backend
python -m venv venv

# Windows
venv\Scripts\activate
# Linux / macOS
source venv/bin/activate

pip install -r requirements.txt
export FLASK_APP=run.py
export FLASK_ENV=development
flask db upgrade
python run.py
```

### Celery (отдельный терминал)

```bash
cd backend
celery -A celery_worker.celery worker --loglevel=info
celery -A celery_worker.celery beat --loglevel=info
```

### Frontend

```bash
cd frontend
npm install
npm start
# Production build
npm run build
```

> Убедитесь, что PostgreSQL, Redis и Elasticsearch доступны локально (или через `docker compose up db redis elasticsearch`).

---

## 11. Конфигурация и переменные окружения

---

Скопируйте `.env.example` → `.env`. Ключевые группы:

| Группа | Переменные | Назначение |
|--------|------------|------------|
| **Общие** | `FLASK_ENV`, `SECRET_KEY`, `JWT_SECRET_KEY` | Режим и криптографические ключи |
| **PostgreSQL** | `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB` | Учётные данные БД |
| **Redis** | `REDIS_PASSWORD` | Пароль Redis (broker + cache) |
| **Поиск** | `ELASTICSEARCH_URL` | URL кластера ES |
| **Почта** | `MAIL_SERVER`, `MAIL_PORT`, `MAIL_USERNAME`, `MAIL_PASSWORD` | SMTP для алертов и гарантий |
| **Платежи** | `STRIPE_PUBLIC_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET` | Stripe Checkout / webhooks |
| **Загрузки** | `MAX_CONTENT_LENGTH`, `UPLOAD_FOLDER` | Лимит 16 MB, папка изображений |
| **Frontend** | `REACT_APP_API_URL` | Базовый URL API для SPA |

Конфигурационные профили в `backend/app/config.py`: `development`, `testing`, `production`.

---

## 12. API, очереди и интеграции

---

### REST API (префикс `/api`)

| Модуль | Endpoints | Auth |
|--------|-----------|------|
| **Auth** | `POST /auth/register`, `/login`, `/refresh`; `GET|PUT /auth/me` | JWT |
| **Products** | CRUD, `/search`, `/specs`, категории | Admin для записи |
| **Orders** | `POST /orders`, список, детали, отмена | JWT |
| **Reviews** | Пользовательские и tech reviews | JWT / Admin |
| **Comparisons** | Создание, список, добавление товаров | JWT |
| **Price Alerts** | CRUD алертов, `/price-history` | JWT |
| **Warranties** | Регистрация, claims | JWT |
| **Admin** | Dashboard, users, orders | Admin |

### Фоновые задачи (Celery)

| Задача | Триггер | Действие |
|--------|---------|----------|
| Price check | Beat, каждые 6 ч | Сравнение цены с target → email |
| Price history | При обновлении цены | Запись в историю для графиков |
| Warranty reminder | Beat | Email за 30 дней до expiry |
| Email dispatch | События заказа / алерта | SMTP через Flask-Mail |

### Внешние интеграции

- **Stripe** — оплата заказов
- **Elasticsearch** — индексация и поиск товаров
- **SMTP** — транзакционные письма

---

## 13. Мониторинг и эксплуатация

---

| Область | Рекомендация |
|---------|--------------|
| **Логи Nginx** | `access.log`, `error.log` — latency (`$request_time`), 4xx/5xx |
| **Gunicorn** | 4 workers в compose; масштабировать по CPU при нагрузке |
| **Healthchecks** | `db`, `redis`, `elasticsearch` — встроены в `docker-compose.yml` |
| **Redis** | Мониторинг памяти broker DB 1/2 vs cache DB 0 |
| **Elasticsearch** | `_cluster/health`, индекс product — yellow/green |
| **Celery** | `celery inspect active`, `celery inspect scheduled` |
| **Rate limiting** | Nginx: `30r/s` API, `5r/m` auth endpoints |

Middleware backend: `rate_limiter`, `request_logger` — аудит и защита от злоупотреблений.

---

## 14. CI/CD

---

Рекомендуемый pipeline (GitHub Actions / GitLab CI):

```yaml
# Пример этапов
- lint + pytest (backend)
- npm test + build (frontend)
- docker build & push (backend, frontend)
- deploy: docker compose pull && up -d на staging/prod
- flask db upgrade (миграции перед трафиком)
```

| Этап | Действие |
|------|----------|
| **PR** | Unit-тесты, линтеры, preview build |
| **main** | Сборка образов, деплой на staging |
| **release tag** | Продакшен-деплой, smoke-тесты `/api/health` |

> В репозитории CI-конфигурацию можно добавить в `.github/workflows/ci.yml` — структура проекта к этому готова (`tests/`, `Dockerfile`).

---

## 15. Безопасность и хранение данных

---

- **JWT** — access (1 ч prod / 24 ч dev) + refresh (30 дней), только заголовок `Authorization: Bearer`
- **Пароли** — bcrypt-хеширование
- **Секреты** — только через `.env`, никогда в git (`SECRET_KEY`, `JWT_SECRET_KEY`, Stripe, SMTP)
- **CORS** — настраивается через `CORS_ORIGINS`
- **Nginx** — rate limiting на API и auth, `client_max_body_size 16M`
- **Загрузки** — whitelist расширений: `png`, `jpg`, `jpeg`, `webp`, `gif`
- **PostgreSQL** — транзакционные данные; бэкапы volume `postgres_data`
- **Redis** — пароль в production (`requirepass`)
- **Elasticsearch** — в dev отключён xpack security; в prod включить TLS и auth

---

## 16. Роли компонентов в продакшене

---

| Компонент | Роль |
|-----------|------|
| **Nginx** | TLS termination, static/cache, балансировка, WAF-уровень rate limit |
| **React (build)** | Статика после `npm run build` или CDN |
| **Gunicorn** | WSGI, горизонтальное масштабирование backend replicas |
| **PostgreSQL** | Source of truth для заказов, пользователей, каталога |
| **Redis** | Сессионный кэш, Celery broker — отдельные logical DB |
| **Elasticsearch** | Поисковый кластер (реплики в HA-сценарии) |
| **Celery** | Изоляция тяжёлой работы от HTTP-потока |
| **Stripe** | PCI scope минимизируется — карты не хранятся на сервере |

---

## 17. Лицензия

---

Проприетарное программное обеспечение. Все права защищены.

Для коммерческого использования и лицензирования обращайтесь к владельцу репозитория.

---

## 18. Поддержка

---

| Канал | Действие |
|-------|----------|
| **Issues** | [GitHub Issues](https://github.com/NodirOdilov/TechZone/issues) — баги и feature requests |
| **Документация** | Этот README + inline-код в `backend/app/` и `frontend/src/` |
| **Roadmap** | Wishlist, Q&A, loyalty, i18n, AI-рекомендации, marketplace, React Native |

---

<div align="center">

**TechZone** — покупайте электронику осознанно: сравнивайте, читайте обзоры, ловите лучшую цену.

<br/>

Сделано с вниманием к деталям · Flask · React · Docker

</div>
