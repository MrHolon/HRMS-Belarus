# HRMS Belarus

**HR-система для Беларуси** — кадровый учёт, приказы, сотрудники, отпуска, трудовые договоры.


Telegram: https://t.me/MonarchDev
Telegram chat https://t.me/+yIBcQ3ieWfszYjMy

---

## Описание

Веб-приложение для управления персоналом с учётом законодательства Республики Беларусь. Система построена на событийной модели: приказы являются источником истины, а состояние сотрудников (занятость, назначения, отсутствия) — проекция применённых пунктов приказов.

## Стек

| Компонент | Технология |
|-----------|------------|
| **Фронтенд** | Next.js 16, React 19, TypeScript, Tailwind CSS 4 |
| **UI** | Radix UI, Base UI, Lucide Icons, TipTap (редактор) |
| **State** | Zustand, React Context |
| **Бэкенд-логика** | n8n (workflow automation, webhooks) |
| **БД и Auth** | Supabase (self-hosted Docker) — PostgreSQL 15, GoTrue |
| **Инфраструктура** | Docker Compose, Kong API Gateway |

## Архитектура

```
┌─────────────┐     ┌─────────┐     ┌──────────────────┐
│  hrms-web   │────▶│   n8n   │────▶│    Supabase DB   │
│  (Next.js)  │     │ webhooks│     │   (PostgreSQL)   │
└─────────────┘     └─────────┘     └──────────────────┘
     :3000             :5678         :5432 (via Kong :8000)
```

- **Фронт → только n8n.** Все запросы идут через вебхуки n8n. Прямых вызовов Supabase с фронта нет (кроме Auth).
- **n8n** — бизнес-логика, CRUD, валидации, комплаенс.
- **Supabase** — PostgreSQL, Auth (JWT), Storage, RLS.

## Структура проекта

```
HRMS Belarus/
├── hrms-web/              # Фронтенд (Next.js)
│   ├── app/               # Маршруты (auth, dashboard)
│   ├── components/        # UI-компоненты
│   ├── features/          # Доменные модули (employees, documents, editor, ...)
│   ├── lib/               # n8n-клиент, auth, utils
│   └── .env.example       # Шаблон переменных окружения
├── docker/                # Docker Compose, Supabase override
├── scripts/               # Утилиты (backup-db.ps1)
├── backups/               # Резервные копии БД
└── docker-compose.yml     # n8n + hrms-web
```

## Быстрый старт

### Требования

- **Windows** 10/11 (x64)
- **Docker Desktop** 4.x (Linux containers)
- **Git** 2.x

### Порты

| Сервис | Порт |
|--------|------|
| hrms-web (фронтенд) | 3000 |
| n8n | 5678 |
| Supabase API (Kong) | 8000 |
| PostgreSQL (Supavisor) | 5432 |

## Документация

| Файл | Описание |
|------|----------|
| [`setup/README.md`](setup/README.md) | Развёртывание на новой машине |
| [`hrms-web/ARCHITECTURE.md`](hrms-web/ARCHITECTURE.md) | Архитектура фронтенда |
| [`docker/README.md`](docker/README.md) | Docker-инфраструктура |
| [`backups/README.md`](backups/README.md) | Резервное копирование |

## Скриншоты

### Карточка сотрудника
![Карточка сотрудника](screenshot/Screenshot_1.png)

### Сводные приказы
![Сводные приказы](screenshot/Screenshot_2.png)

### Редактор шаблонов
![Редактор шаблонов](screenshot/Screenshot_3.png)

### Печать приказа
![Печать приказа](screenshot/Screenshot_4.png)

## Лицензия

[MIT](LICENSE)


