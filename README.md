# Longevity Wellness MVP

Минимальный MVP на `Next.js + TypeScript + Tailwind CSS` для AI wellness / longevity assistant.

Flow:
- пользователь открывает landing page;
- заполняет анкету;
- backend валидирует данные и запускает safety pre-check;
- безопасные анкеты отправляются в OpenAI API за structured result;
- анкета и AI-результат сохраняются в Google Sheets;
- пользователь получает экран результата.

Важно:
- это не медицинская диагностика;
- это не постановка диагноза;
- это не назначение лечения;
- во всех результатах есть disclaimer.

## Stack

- `Next.js` App Router
- `TypeScript`
- `Tailwind CSS`
- `OpenAI API`
- `Google Sheets API`
- `Vercel`

## Local Run

1. Создайте `.env.local` по образцу `.env.example`.
2. Запустите:

```bash
npm install
npm run dev
```

3. Откройте `http://localhost:3000`.

Если `OPENAI_API_KEY` не задан, приложение использует локальный fallback-ответ, чтобы можно было проверить UI и flow без внешнего API.

## Environment Variables

```env
OPENAI_API_KEY=
OPENAI_MODEL=gpt-5.2
GOOGLE_CLIENT_EMAIL=
GOOGLE_PRIVATE_KEY=
GOOGLE_SHEET_ID=
```

## Google Sheets Setup

1. Создайте Google Sheet.
2. Передайте сервисному аккаунту доступ `Editor` к таблице.
3. Укажите `GOOGLE_SHEET_ID`.
4. При первой записи приложение автоматически создаст или обновит header row в `Sheet1`.

Ожидаемые колонки:
- `created_at`
- `name`
- `age`
- `sex`
- `height`
- `weight`
- `goal`
- `activity_level`
- `sleep_quality`
- `nutrition_quality`
- `stress_level`
- `bad_habits`
- `concerns`
- `notes`
- `ai_summary`
- `ai_focus_areas`
- `ai_recommendations`
- `ai_daily_plan`
- `ai_doctor_attention`

## OpenAI Notes

- Вызов модели идет только с сервера через `app/api/analyze/route.ts`.
- Structured output валидируется `zod`-схемой.
- Safety pre-check отсекает анкеты с red flags до вызова модели.

## Vercel Deploy

1. Импортируйте проект в Vercel.
2. Добавьте все переменные окружения из `.env.example` в Project Settings.
3. Убедитесь, что `GOOGLE_PRIVATE_KEY` вставлен с переносами строки в формате, который Vercel хранит как plain text.
4. Запустите deploy.

Команды проверки перед деплоем:

```bash
npm run lint
npm run build
```
