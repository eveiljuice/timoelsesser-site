# timoelsesser.com

Персональный сайт-лендинг Timo Elsesser — портфолио, проекты и блог.

Сделано на [Astro](https://astro.build/) + [AstroPaper](https://github.com/satnaing/astro-paper) (MIT).
Статический сайт, пишется в Markdown, деплоится на Cloudflare Pages.

## Локальная разработка

```bash
bun install
bun run dev           # http://localhost:4321
bun run build         # сборка в dist/ + индекс поиска (pagefind)
bun run preview       # превью собранного сайта
```

## Структура

```
src/
├── config.ts                 — имя, домен, язык, часовой пояс
├── constants.ts              — список соцсетей
├── content.config.ts         — схемы коллекций blog и projects
├── data/
│   ├── blog/                 — посты (*.md с frontmatter)
│   └── projects/             — карточки проектов (*.md)
├── pages/
│   ├── index.astro           — главная: hero + проекты + последние посты
│   ├── about.md              — страница «Обо мне»
│   ├── posts/                — список и детали постов
│   ├── projects/             — список и детали проектов
│   ├── tags/, archives/, search, 404
├── components/               — ProjectCard, Card, Socials, Header, Footer…
└── layouts/                  — Layout, Main, AboutLayout, PostDetails
```

## Как добавить пост

Создайте файл `src/data/blog/моя-статья.md`:

```md
---
title: "Заголовок поста"
author: "Timo Elsesser"
pubDatetime: 2026-04-20T12:00:00.000Z
description: "Краткое описание для превью и SEO."
tags: ["заметки"]
featured: false
draft: false
---

Текст поста в markdown.
```

`featured: true` выносит пост в отдельную секцию на главной.
`draft: true` скрывает пост из списков и sitemap.

## Как добавить проект

Создайте файл `src/data/projects/мой-проект.md`:

```md
---
title: "Название проекта"
description: "Короткое описание — видно на карточке."
url: "https://example.com"            # опционально — ссылка на демо
repo: "https://github.com/..."        # опционально — ссылка на код
tech: ["Python", "Docker"]            # теги-технологии
featured: true                         # показывать на главной
order: 1                               # порядок сортировки (меньше — выше)
---

Полное описание проекта в markdown — отображается на странице /projects/мой-проект.
```

## Деплой на Cloudflare Pages

### 1. Публикация репозитория

```bash
gh repo create timoelsesser-site --private --source=. --remote=origin --push
```

(либо создать репозиторий вручную и сделать `git remote add origin ... && git push -u origin main`).

### 2. Подключить Cloudflare Pages

1. **Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git**
2. Выбрать репозиторий `timoelsesser-site`
3. Настройки сборки:
   - **Framework preset**: Astro
   - **Build command**: `bun run build`
   - **Build output directory**: `dist`
   - **Environment variables**: `NODE_VERSION=22`
4. **Save and Deploy** — результат появится на `<project>.pages.dev`

### 3. Привязать домен timoelsesser.com

1. В проекте Cloudflare Pages → **Custom domains** → **Set up a custom domain**
2. Ввести `timoelsesser.com` и `www.timoelsesser.com`
3. Если домен уже в Cloudflare — DNS-записи создадутся автоматически.
   Если у другого регистратора — Cloudflare покажет нужные CNAME/A
4. TLS-сертификат выпускается автоматически

После этого каждый `git push` в `main` триггерит пересборку и обновляет сайт.

## Смена темы, цветов, OG-картинки

- Цветовые схемы и токены: `src/styles/`
- Дефолтная OG-картинка: `public/astropaper-og.jpg` — замените на свою
- Динамические OG для постов собираются через `satori` (см. `src/pages/og.png.ts`)

## Лицензия шаблона

Astro Paper — MIT © Sat Naing. См. файл `LICENSE`.
Ваш контент (посты, проекты, личные данные) — ваш.
