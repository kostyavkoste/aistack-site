# Подключение CLI-провайдеров к OpenClaw / AIStack

> Гайд для тех, кто выбрал в установщике Claude Code · Max или OpenAI Codex CLI вместо API-ключа.

---

## Claude Code · Max — пошагово

### Шаг 1 · Купить подписку Claude

Откройте [claude.ai/settings/plans](https://claude.ai/settings/plans). Логин под аккаунтом, на котором будете работать.

| План | Цена | Agent SDK credits |
|---|---|---|
| **Pro** | $20/мес | $20 в месяц (хватит на 1-2 дня команды) |
| **Max 5X** | $100/мес | $100 в месяц |
| **Max** | $200/мес | $200 в месяц |

**Что выбрать:**
- 1 агент, лёгкие задачи → Pro $20
- Команда 3-5 агентов, ежедневно → **Max 5X $100** (оптимально для большинства)
- Полная команда 8 агентов, тяжёлые workflow → Max $200

⚠ Если выберете Pro и команда из 6+ агентов будет работать активно — credits закончатся за день, агенты начнут отваливаться. Тогда либо включайте «extra usage billing» (платите по факту), либо апгрейд на Max 5X.

### Шаг 2 · Установить Claude Code CLI

```bash
# macOS / Linux
curl -fsSL https://claude.ai/install.sh | bash

# Windows (PowerShell)
iwr -useb https://claude.ai/install.ps1 | iex
```

После установки:
```bash
claude --version  # должна показать версию
```

### Шаг 3 · Авторизоваться

```bash
claude login
```

Откроется браузер, войдите под тем же аккаунтом claude.ai где у вас Max-подписка. Подтвердите доступ. Готово.

### Шаг 4 · Подключить к OpenClaw

```bash
openclaw config set providers.default claude-cli
openclaw config set providers.claude-cli.binary $(which claude)
```

Проверка:
```bash
openclaw doctor   # должно быть: "✓ provider claude-cli (Max plan)"
```

### Шаг 5 · Готово

Дальше работает обычный setup из bundle.zip. Команды агентам идут через `claude` CLI, который использует ваш Max-аккаунт. Расходуются Agent SDK credits согласно тарифу.

---

## OpenAI Codex CLI — пошагово

### Шаг 1 · Купить ChatGPT Pro $100/мес

Откройте [chatgpt.com/pricing](https://chatgpt.com/pricing). План **ChatGPT Pro** ($100/мес) даёт 5× больше Codex-запросов чем Plus за $20.

### Шаг 2 · Установить Codex CLI

```bash
npm install -g @openai/codex-cli
codex --version
```

### Шаг 3 · Авторизация через ChatGPT

```bash
codex login
```

Откроется браузер, войдите под аккаунтом ChatGPT Pro. Подтвердите доступ.

### Шаг 4 · Подключить к OpenClaw

```bash
openclaw config set providers.default codex-cli
openclaw config set providers.codex-cli.binary $(which codex)
```

---

## Что лучше — CLI или API-ключ?

### CLI лучше когда:
- ✓ У вас уже есть Claude Max или ChatGPT Pro для других задач
- ✓ Хотите фикс-цену без сюрпризов в счетах
- ✓ Не хотите следить за балансом

### API-ключ лучше когда:
- ✓ Используете команду нерегулярно (платите только когда работаете)
- ✓ Нужна полная гибкость без лимитов credits
- ✓ Через ProxyAPI для РФ — нет иностранной карты для подписки

### Анти-пример (что не работает):
- ❌ Claude Pro $20 + команда 6+ агентов в постоянной работе → credits кончатся за 1-2 дня
- ❌ Бесплатный API-ключ без депозита → 429 errors через 5 минут

---

## ProxyAPI для РФ — что доступно

[ProxyAPI](https://proxyapi.ru/) — российский прокси к зарубежным AI. Удобно когда нет иностранной карты.

**Что есть:**
- **OpenAI:** GPT-4o, GPT-5, DALL-E 3, Whisper
- **Anthropic:** Claude Sonnet, Claude Opus
- **Google:** Gemini 2.5 Pro
- **DeepSeek:** V3, R1
- **ProxyAPI Pro подписка:** Assistants API, Code Interpreter, Vector Store, работа с файлами

**Цены:**
- Все цены за 1 млн токенов в ₽
- НДС 5% включён
- Наценка ~×3 к официальным ценам Claude/GPT, ×5-6 для Gemini

**Оплата:** карта РФ (МИР, российские VISA/MC), СБП

**Конкуренты для сравнения (если хотите дешевле):**
- [AITunnel](https://aitunnel.ru) — похожий сервис, иногда дешевле
- [Polza AI](https://polza.ai) — фокус на DeepSeek и opensource моделях

**Партнёрская программа ProxyAPI** — на момент проверки публичной партнёрки на сайте не нашёл. Чтобы выяснить — напишите им напрямую в чат на сайте или на info@proxyapi.ru. У них есть удобный личный кабинет и автоматизированные продажи, реферальная программа им технически выгодна, велик шанс что согласятся на персональных условиях.

---

## Если хотите смешать (advanced)

В OpenClaw можно настроить **разные провайдеры для разных агентов**. Например:

```bash
# Координатор и копирайтер — на дорогой Claude Max (качество текстов важнее)
openclaw config set agents.coordinator.provider claude-cli
openclaw config set agents.copywriter.provider claude-cli

# Технические агенты — на дешёвом DeepSeek (код пишут одинаково)
openclaw config set agents.tech.provider deepseek
openclaw config set agents.tech.api_key sk-...

# Дизайнер — на ProxyAPI (нужны иногда тяжёлые модели по картинкам)
openclaw config set agents.designer.provider proxyapi
openclaw config set agents.designer.api_key sk-...
```

Это сильно снижает расходы — самые дорогие модели (Claude Max) только там где нужно качество, остальное на бюджетных провайдерах.

---

## Поддержка

Что-то не работает — @superwalletsru в Telegram. Приложите:
- Какой провайдер выбрали
- Что говорит `openclaw doctor`
- Скрин ошибки (без API-ключа в кадре!)

Sources:
- [Claude Max plans · Anthropic](https://www.anthropic.com/pricing)
- [Anthropic + OpenClaw · Agent SDK credits explained](https://venturebeat.com/technology/anthropic-reinstates-openclaw-and-third-party-agent-usage-on-claude-subscriptions-with-a-catch)
- [OpenAI ChatGPT Pro $100 vs Claude Max](https://thenextweb.com/news/openais-new-100-chatgpt-pro-plan-targets-claude-max-with-five-times-the-codex-access)
- [ProxyAPI · pricing](https://proxyapi.ru/pricing)
- [Сравнение ProxyAPI / AITunnel / Polza AI · Sostav](https://www.sostav.ru/blogs/289807/86785)
