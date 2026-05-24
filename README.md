# AIStack · витрина и установщик

Публичная статика проекта AIStack. Хостится на GitHub Pages.

## Структура

- `index.html` — лендинг витрины
- `installer.html` — закрытый установщик (требует пароль + Telegram-привязку)
- `dashboard.html` — дашборд команды (требует sessionStorage от установщика)
- `cli-setup-guide.md` — гайд по подключению Claude Code / Codex CLI

## Knowledge-репо

Контент (skills, knowledge, presets, workspace-templates) лежит отдельно в приватном репо `aistack-knowledge`. Установщик дёргает его через GitHub raw API с токеном (зашит в installer.html).

## Поддержка

@superwalletsru
