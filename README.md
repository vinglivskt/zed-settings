# Zed Settings

Персональная конфигурация редактора `Zed` для Python/backend-разработки с упором на:

- привычные `JetBrains`-кейбинды
- аккуратный UI без лишнего шума
- форматирование и linting через `Ruff`
- навигацию и типизацию через `basedpyright`
- встроенную работу с AI-моделями
- удобный запуск задач и работу с терминалом

## Состав проекта

- `settings.json` — основные настройки редактора, UI, LSP, AI, терминала и языков
- `keymap.json` — кастомные горячие клавиши поверх `JetBrains` keymap
- `tasks.json` — шаблон конфигурации задач Zed

## Что настроено

### Editor / UI

Основные редакторские настройки:

- включён `vim`-режим
- восстановление последнего workspace при старте
- `autosave` при потере фокуса
- форматирование при сохранении
- курсор в виде `bar`
- подсветка текущей строки
- перенос по `preferred_line_length = 120`
- направляющая на 120 символах
- раскраска скобок и indent guides
- minimap отключён
- inlay hints отключены
- inline diagnostics включены справа от строки
- file icons / folder icons / git status отображаются в панели и вкладках

UI и внешний вид:

- тема: `One Dark Pycharm Matched`
- иконки: `Material Icon Theme`
- шрифт кода: `JetBrains Mono`
- шрифт UI: `.ZedSans`

### AI / Agent

Настроен встроенный AI-ассистент:

- профиль по умолчанию: `write`
- панель агента закреплена справа
- основная модель: `openai / gpt-5.4`
- inline assistant: `openai / gpt-5.4`
- альтернативы для inline:
  - `openrouter / qwen/qwen3-coder:free`
  - `openrouter / openai/gpt-oss-120b`
- отдельные модели для:
  - summary thread
  - commit message

Разрешения инструментов:

- редактирование файлов — с подтверждением
- удаление файлов — с подтверждением
- терминал — разрешён без подтверждения

Также включены edit predictions через `copilot` в режиме `subtle`.

### Python

Конфигурация заточена под Python-разработку:

- language servers:
  - `basedpyright`
  - `ruff`
- форматирование при сохранении включено
- formatter — `ruff`
- при форматировании:
  - `source.organizeImports.ruff`
  - `source.fixAll.ruff`
- размер отступа: 4 пробела
- `hard_tabs = false`

### LSP

#### Ruff

Используется для:

- форматирования
- linting
- автофиксов
- сортировки импортов

Настройки:

- `lineLength = 120`
- `configurationPreference = editorFirst`
- включены синтаксические ошибки
- выбраны правила:
  - `E4`
  - `E7`
  - `E9`
  - `F`
  - `I`
- игнорируется:
  - `E501`

#### basedpyright

Используется для:

- навигации по коду
- type checking
- автодополнения импортов
- анализа workspace

Настройки:

- `diagnosticMode = workspace`
- `typeCheckingMode = basic`
- `autoSearchPaths = true`
- `autoImportCompletions = true`
- `useLibraryCodeForTypes = true`

### JSON / TOML

Дополнительно настроены языки:

- `JSON`
  - форматирование через `Prettier`
  - parser: `json`
- `TOML`
  - форматирование при сохранении отключено
  - `Prettier` отключён

### Terminal

Настроено автоопределение виртуальных окружений Python в директориях:

- `.venv`
- `venv`
- `.env`
- `env`
- `.pypoetry`
- `.hatch`

Также задано:

- `EDITOR = zed --wait`

Это удобно, например, для `git commit`.

### Privacy / Telemetry

Настроены приватные файлы:

- `**/.env*`
- `**/*.pem`
- `**/*.key`
- `**/*.cert`
- `**/*.crt`
- `**/secrets.yml`

Telemetry отключена:

- `diagnostics = false`
- `metrics = false`

## Горячие клавиши

Базовая раскладка:

- `JetBrains`

Дополнительные кастомные бинды из `keymap.json`:

### Редактирование

- `Ctrl+Alt+L` / `Cmd+Alt+L` — форматировать файл
- `Ctrl+/` / `Cmd+/` — закомментировать/раскомментировать
- `Alt+Enter` — code actions
- `Shift+F6` — rename symbol

### Навигация

- `Ctrl+Enter` / `Cmd+Enter` — go to definition
- `Ctrl+Alt+B` / `Cmd+Alt+B` — go to definition in split
- `Ctrl+Shift+B` / `Cmd+Shift+B` — go to implementation
- `Ctrl+Alt+T` / `Cmd+Alt+T` — go to type definition
- `Alt+F7` — find all references

### Диагностика

- `F2` — next diagnostic
- `Shift+F2` — previous diagnostic
- `Alt+6` — открыть diagnostics/problems

### Поиск и навигация по проекту

- `Ctrl+F` / `Cmd+F` — поиск
- `Ctrl+Shift+N` — открыть file finder
- `Cmd+Shift+O` — открыть file finder
- `Ctrl+Alt+Shift+N` — symbols in project
- `Cmd+Alt+O` — symbols in project
- `Ctrl+F12` / `Cmd+F12` — outline файла
- `Ctrl+E` — recent projects

### Вкладки / файлы

- `Ctrl+S` / `Cmd+S` — сохранить
- `Ctrl+F4` / `Cmd+W` — закрыть вкладку
- `Alt+Right` — следующая вкладка
- `Alt+Left` — предыдущая вкладка
- `Ctrl+Alt+W` — закрыть остальные вкладки
- `Ctrl+Alt+Z` — назад по истории
- `Ctrl+Alt+Shift+Z` — вперёд по истории

### Терминал и задачи

- `Alt+F12` — открыть/скрыть terminal panel
- `Shift+F10` — запуск ближайшей задачи без перевода фокуса
- `Shift+F9` — запуск ближайшей задачи с показом вывода
- `F5` — повторный запуск task
- `Ctrl+F5` — rerun debugger

## Tasks

В `tasks.json` сейчас лежит шаблонный пример задачи Zed.

Там показаны параметры:

- `label`
- `command`
- `env`
- `cwd`
- `use_new_terminal`
- `allow_concurrent_runs`
- `reveal`
- `reveal_target`
- `hide`
- `shell`
- `show_summary`
- `show_command`
- `tags`

Этот файл можно использовать как основу под реальные project tasks.

## Для кого эта конфигурация

Подойдёт, если вы:

- работаете в `Zed`, но хотите UX ближе к `PyCharm` / `IntelliJ`
- пишете на Python
- используете `Ruff` + `basedpyright`
- хотите встроенный AI без перегруженного интерфейса
- предпочитаете компактный, быстрый и предсказуемый редактор

## Быстрый старт

1. Скопируйте файлы в каталог настроек `Zed`.
2. Перезапустите редактор или перезагрузите конфигурацию.
3. Убедитесь, что у вас доступны:
   - `Ruff`
   - `basedpyright`
   - нужные провайдеры AI (`OpenAI`, `OpenRouter`, `Copilot`)
4. При необходимости адаптируйте `keymap.json` под свою ОС и привычки.

## Примечание

Конфигурация ориентирована на личный workflow: Python/backend, JetBrains-style hotkeys, строгая работа с форматированием, удобная навигация и минимальный визуальный шум.
