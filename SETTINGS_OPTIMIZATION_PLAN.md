# План оптимизации настроек Zed для backend-разработки на Python

## Обзор

Этот документ содержит детальный разбор текущих настроек Zed, рекомендации по их улучшению и новые фишки, специально подобранные для backend-разработки на Python.

---

## 1. Основные настройки редактора

### Текущие настройки:
```json
{
  "accessible_mode": true,
  "restore_on_startup": "last_workspace",
  "close_on_file_delete": true,
  "autosave": "on_focus_change",
  "format_on_save": "on",
  "prettier": {
    "allowed": true
  },
  "load_direnv": "direct"
}
```

### Что уже хорошо:
- `accessible_mode: true` — включает режим доступности, улучшает поддержку скринридеров
- `restore_on_startup: "last_workspace"` — восстанавливает последнее рабочее пространство
- `close_on_file_delete: true` — автоматически закрывает вкладки удалённых файлов
- `autosave: "on_focus_change"` — экономит ресурсы, сохраняя при смене фокуса
- `format_on_save: "on"` — автоматическое форматирование при сохранении
- `load_direnv: "direct"` — загрузка переменных окружения через direnv

### Рекомендации:

#### 1.1 `accessible_mode`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Режим доступности полезен даже без скринридеров — улучшает навигацию с клавиатуры.

#### 1.2 `restore_on_startup`
**Текущее:** `"last_workspace"`
**Рекомендуется:** `"last_session"`
**Обоснование:** `last_session` восстанавливает все окна и вкладки, а не только последнее workspace. Это удобнее для backend-разработки, где часто работают с несколькими проектами одновременно.

```json
"restore_on_startup": "last_session"
```

#### 1.3 `close_on_file_delete`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Удобно для временных файлов и скриптов.

#### 1.4 `autosave`
**Текущее:** `"on_focus_change"`
**Рекомендуется:** `"after_delay"` с 1000ms
**Обоснование:** `on_focus_change` может сохранять файлы в нежелательный момент (например, при переключении на терминал). `after_delay` даёт больше контроля:

```json
"autosave": {
  "after_delay": {
    "milliseconds": 1000
  }
}
```

#### 1.5 `format_on_save`
**Текущее:** `"on"`
**Рекомендуется:** `"modifications_if_available"`
**Обоснование:** Форматировать только изменённые строки — экономит время и избегает конфликтов в больших файлах:

```json
"format_on_save": "modifications_if_available"
```

#### 1.6 `prettier`
**Текущее:**
```json
"prettier": {
  "allowed": true
}
```
**Рекомендуется:** добавить настройки для JSON и JS/TS:
```json
"prettier": {
  "allowed": true,
  "prettier": {
    "singleQuote": true,
    "trailingComma": "all",
    "printWidth": 100,
    "tabWidth": 2
  }
}
```

#### 1.7 `load_direnv`
**Текущее:** `"direct"`
**Рекомендуется:** `"shell_hook"`
**Обоснование:** `shell_hook` лучше интегрируется с окружением, поддерживая все возможности direnv:

```json
"load_direnv": "shell_hook"
```

---

## 2. Настройки toolbar

### Текущие настройки:
```json
"toolbar": {
  "breadcrumbs": true,
  "quick_actions": true,
  "selections_menu": false,
  "agent_review": false,
  "code_actions": false
}
```

### Рекомендации:

#### 2.1 `breadcrumbs`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Х хорош для навигации по коду.

#### 2.2 `quick_actions`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Быстрый доступ к часто используемым действиям.

#### 2.3 `selections_menu`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Удобен для работы с несколькими выделениями:

```json
"selections_menu": true
```

#### 2.4 `agent_review`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Показывает inline diff для AI-правок:

```json
"agent_review": true
```

#### 2.5 `code_actions`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Быстрый доступ к быстрым исправлениям и рефакторингу:

```json
"code_actions": true
```

---

## 3. Настройки AI-агента

### Текущие настройки:
```json
"agent": {
  "sidebar_side": "right",
  "tool_permissions": {
    "default": "confirm",
    "tools": {
      "edit_file": { "default": "confirm" },
      "delete_path": { "default": "confirm" },
      "terminal": { "default": "allow" }
    }
  },
  "default_profile": "write",
  "dock": "right",
  "default_model": {
    "enable_thinking": true,
    "provider": "openrouter",
    "model": "poolside/laguna-s-2.1:free"
  },
  "inline_assistant_model": {
    "enable_thinking": false,
    "provider": "openai",
    "model": "gpt-5.4"
  },
  "inline_alternatives": [...],
  "thread_summary_model": {...},
  "commit_message_model": {...},
  "favorite_models": [...]
}
```

### Рекомендации:

#### 3.1 `sidebar_side` и `dock`
**Текущее:** `"right"`
**Рекомендуется:** оставить `"right"`
**Обоснование:** Правая сторона удобна для backend-разработки, не мешая основному коду.

#### 3.2 `tool_permissions`
**Текущее:** `terminal: "allow"`
**Рекомендуется:** добавить более строгие правила:
```json
"tool_permissions": {
  "default": "confirm",
  "tools": {
    "edit_file": { "default": "confirm" },
    "delete_path": { "default": "confirm" },
    "terminal": {
      "default": "allow",
      "always_allow": [
        {
          "pattern": "^(python|pytest|pip|uv|poetry|docker|docker-compose)\\s+.*"
        },
        {
          "pattern": "^(git\\s+(add|commit|diff|log|status|push|pull|branch))"
        }
      ],
      "always_deny": [
        {
          "pattern": "^(sudo|su|doas)\\s"
        },
        {
          "pattern": "^rm\\s+-[rf]\\s+/"
        }
      ]
    }
  }
}
```

#### 3.3 `default_model`
**Текущее:** `poolside/laguna-s-2.1:free` через OpenRouter
**Рекомендуется:** добавить Claude 3.5 Sonnet для сложных задач:
```json
"default_model": {
  "enable_thinking": true,
  "provider": "anthropic",
  "model": "claude-3-5-sonnet-20241022"
}
```

#### 3.4 `inline_assistant_model`
**Текущее:** `gpt-5.4`
**Рекомендуется:** использовать более быструю модель для inline:
```json
"inline_assistant_model": {
  "enable_thinking": false,
  "provider": "openrouter",
  "model": "google/gemini-2.0-flash-thinking"
}
```

#### 3.5 Новые настройки для AI:

##### Автоматическая компактизация потоков:
```json
"auto_compact": {
  "enabled": true,
  "threshold": "85%"
}
```

##### Модель для компактизации:
```json
"compaction_model": {
  "provider": "openrouter",
  "model": "google/gemini-2.0-flash"
}
```

##### Инструкции для commit message:
```json
"commit_message_instructions": "Use Conventional Commits format: <type>(<scope>): <description>. Types: feat, fix, docs, style, refactor, test, chore. Scope: api, db, auth, config, etc."
```

##### Профили агента:
```json
"profiles": {
  "write": {
    "name": "Write Code",
    "tools": {
      "edit_file": true,
      "create_file": true,
      "delete_file": true,
      "rename_file": true,
      "find_file": true,
      "terminal": true,
      "search": true,
      "fetch": true,
      "run_command": true,
      "copy_path": true,
      "move_path": true,
      "open_file": true,
      "go_to_file": true,
      "set_context": true,
      "set_files": true,
      "spawn_agent": false,
      "create_task": false
    },
    "enable_all_context_servers": true
  },
  "review": {
    "name": "Code Review",
    "tools": {
      "edit_file": false,
      "create_file": false,
      "delete_file": false,
      "rename_file": false,
      "find_file": true,
      "terminal": false,
      "search": true,
      "fetch": true,
      "run_command": false,
      "copy_path": true,
      "move_path": false,
      "open_file": true,
      "go_to_file": true,
      "set_context": true,
      "set_files": true,
      "spawn_agent": false,
      "create_task": false
    },
    "enable_all_context_servers": true,
    "default_model": {
      "provider": "anthropic",
      "model": "claude-3-5-sonnet-20241022"
    }
  },
  "debug": {
    "name": "Debug",
    "tools": {
      "edit_file": true,
      "create_file": false,
      "delete_file": false,
      "rename_file": false,
      "find_file": true,
      "terminal": true,
      "search": true,
      "fetch": true,
      "run_command": true,
      "copy_path": true,
      "move_path": false,
      "open_file": true,
      "go_to_file": true,
      "set_context": true,
      "set_files": true,
      "spawn_agent": true,
      "create_task": false
    },
    "enable_all_context_servers": false,
    "default_model": {
      "provider": "openrouter",
      "model": "google/gemini-2.0-flash-thinking"
    }
  }
}
```

---

## 4. Настройки языковых моделей

### Текущие настройки:
```json
"language_models": {
  "openai_compatible": {
    "RouterAI": {
      "api_url": "https://routerai.ru/api/v1",
      "available_models": [...]
    }
  }
}
```

### Рекомендации:

#### 4.1 Добавить Ollama для локальных моделей:
```json
"language_models": {
  "openai_compatible": {
    "RouterAI": {
      "api_url": "https://routerai.ru/api/v1",
      "available_models": [...]
    }
  },
  "ollama": {
    "api_url": "http://localhost:11434"
  },
  "openai": {
    "api_url": "https://api.openai.com/v1"
  },
  "anthropic": {
    "api_url": "https://api.anthropic.com"
  }
}
```

#### 4.2 Добавить Bedrock для AWS-моделей:
```json
"bedrock": {
  "available_models": [
    {
      "name": "anthropic.claude-3-5-sonnet-20241022-v2:0",
      "display_name": "Claude 3.5 Sonnet (Bedrock)"
    }
  ]
}
```

---

## 5. Настройки предсказаний редактирования

### Текущие настройки:
```json
"edit_predictions": {
  "provider": "copilot",
  "mode": "subtle",
  "disabled_globs": [...],
  "copilot": {
    "enable_next_edit_suggestions": false
  }
}
```

### Рекомендации:

#### 5.1 `provider`
**Текущее:** `"copilot"`
**Рекомендуется:** `"zed"`
**Обоснование:** Встроенные предсказания Zed работают быстрее и не требуют подписки:

```json
"edit_predictions": {
  "provider": "zed",
  "mode": "subtle",
  "disabled_globs": [
    "**/.env*",
    "**/*.pem",
    "**/*.key",
    "**/*.cert",
    "**/*.crt",
    "**/.dev.vars",
    "**/secrets.yml",
    "**/.zed/settings.json",
    "**/.zed/keymap.json"
  ]
}
```

#### 5.2 Добавить настройки для Copilot:
```json
"copilot": {
  "enable_next_edit_suggestions": true,
  "debounce_ms": 250
}
```

---

## 6. Настройки клавиатурных комбинаций

### Текущие настройки:
```json
"base_keymap": "JetBrains",
"cursor_shape": "bar",
"current_line_highlight": "line",
"auto_signature_help": true,
"show_signature_help_after_edits": true,
"scroll_beyond_last_line": "off",
"text_rendering_mode": "subpixel"
```

### Рекомендации:

#### 6.1 `base_keymap`
**Текущее:** `"JetBrains"`
**Рекомендуется:** `"VSCode"`
**Обоснование:** VSCode-ключмап более привычен для большинства разработчиков и легче кастомизируется:

```json
"base_keymap": "VSCode"
```

#### 6.2 `cursor_shape`
**Текущее:** `"bar"`
**Рекомендуется:** `"block"`
**Обоснование:** Блочный курсор легче видеть в Vim-режиме:

```json
"cursor_shape": "block"
```

#### 6.3 `current_line_highlight`
**Текущее:** `"line"`
**Рекомендуется:** `"all"`
**Обоснование:** Подсветка всей строки лучше видна:

```json
"current_line_highlight": "all"
```

#### 6.4 `auto_signature_help`
**Текущее:** `true`
**Рекомендуется:** `false`
**Обоснование:** Автоматические подсказки могут отвлекать. Лучше включать по необходимости:

```json
"auto_signature_help": false
```

#### 6.5 `show_signature_help_after_edits`
**Текущее:** `true`
**Рекомендуется:** `false`
**Обоснование:** Отключить для уменьшения нагрузки на LSP:

```json
"show_signature_help_after_edits": false
```

#### 6.6 `scroll_beyond_last_line`
**Текущее:** `"off"`
**Рекомендуется:** `"one_page"`
**Обоснование:** Прокрутка за последнюю строку удобна для навигации:

```json
"scroll_beyond_last_line": "one_page"
```

#### 6.7 `text_rendering_mode`
**Текущее:** `"subpixel"`
**Рекомендуется:** оставить `"subpixel"`
**Обоснование:** Улучшает читаемость на LCD-экранах.

---

## 7. Настройки автодополнения

### Текущие настройки:
```json
"completions": {
  "words": "fallback",
  "words_min_length": 2,
  "lsp_insert_mode": "replace_suffix"
}
```

### Рекомендации:

#### 7.1 `words`
**Текущее:** `"fallback"`
**Рекомендуется:** `"enabled"`
**Обоснование:** Всегда показывать слова из файла для автодополнения:

```json
"completions": {
  "words": "enabled",
  "words_min_length": 2,
  "lsp_insert_mode": "replace_suffix"
}
```

#### 7.2 `words_min_length`
**Текущее:** `2`
**Рекомендуется:** `1`
**Обоснование:** Быстрее начинать автодополнение:

```json
"words_min_length": 1
```

#### 7.3 `lsp_insert_mode`
**Текущее:** `"replace_suffix"`
**Рекомендуется:** `"replace"`
**Обоснование:** Точное замещение текста:

```json
"lsp_insert_mode": "replace"
```

---

## 8. Настройки отображения текста

### Текущие настройки:
```json
"show_whitespaces": "selection",
"preferred_line_length": 120,
"wrap_guides": [120],
"colorize_brackets": true,
"indent_guides": {
  "background_coloring": "disabled",
  "coloring": "indent_aware"
},
"use_on_type_format": false
```

### Рекомендации:

#### 8.1 `show_whitespaces`
**Текущее:** `"selection"`
**Рекомендуется:** `"boundary"`
**Обоснование:** Показывать только пробелы в начале и конце строк:

```json
"show_whitespaces": "boundary"
```

#### 8.2 `preferred_line_length`
**Текущее:** `120`
**Рекомендуется:** оставить `120`
**Обоснование:** Стандартная длина строки для Python-кода.

#### 8.3 `wrap_guides`
**Текущее:** `[120]`
**Рекомендуется:** `[88, 120]`
**Обоснование:** 88 — для Black, 120 — для общего кода:

```json
"wrap_guides": [88, 120]
```

#### 8.4 `colorize_brackets`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** "Радужные скобки" улучшают читаемость вложенных структур.

#### 8.5 `indent_guides`
**Текущее:**
```json
"indent_guides": {
  "background_coloring": "disabled",
  "coloring": "indent_aware"
}
```
**Рекомендуется:** включить фоновую подсветку:
```json
"indent_guides": {
  "background_coloring": "indent_aware",
  "coloring": "indent_aware"
}
```

#### 8.6 `use_on_type_format`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Автоматическое форматирование при вводе улучшает скорость разработки:

```json
"use_on_type_format": true
```

---

## 9. Настройки панели проекта

### Текущие настройки:
```json
"project_panel": {
  "starts_open": true,
  "auto_reveal_entries": true,
  "hide_gitignore": false,
  "auto_open": {
    "on_paste": true,
    "on_create": true
  },
  "hide_root": false,
  "dock": "left",
  "hide_hidden": false,
  "bold_folder_labels": false,
  "diagnostic_badges": true,
  "git_status_indicator": true,
  "auto_fold_dirs": false,
  "folder_icons": true,
  "file_icons": true,
  "entry_spacing": "standard"
}
```

### Рекомендации:

#### 9.1 `starts_open`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Панель проекта всегда открыта — удобно для навигации.

#### 9.2 `auto_reveal_entries`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Автоматически раскрывает текущий файл в панели проекта.

#### 9.3 `hide_gitignore`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Скрывать файлы из .gitignore для чистоты интерфейса:

```json
"hide_gitignore": true
```

#### 9.4 `auto_open`
**Текущее:**
```json
"auto_open": {
  "on_paste": true,
  "on_create": true
}
```
**Рекомендуется:** добавить `on_drop`:
```json
"auto_open": {
  "on_paste": true,
  "on_create": true,
  "on_drop": true
}
```

#### 9.5 `hide_root`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Скрывает корневую папку, показывая только содержимое:

```json
"hide_root": true
```

#### 9.6 `dock`
**Текущее:** `"left"`
**Рекомендуется:** оставить `"left"`
**Обоснование:** Левая панель — стандартное расположение.

#### 9.7 `hide_hidden`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Скрывать скрытые файлы для чистоты:

```json
"hide_hidden": true
```

#### 9.8 `bold_folder_labels`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Жирные папки легче различать:

```json
"bold_folder_labels": true
```

#### 9.9 `diagnostic_badges`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Показ ошибок в панели проекта.

#### 9.10 `git_status_indicator`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Индикатор статуса Git.

#### 9.11 `auto_fold_dirs`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Автоматически складывать директории с одной вложенной папкой:

```json
"auto_fold_dirs": true
```

#### 9.12 `entry_spacing`
**Текущее:** `"standard"`
**Рекомендуется:** `"comfortable"`
**Обоснование:** Более комфортный отступ между элементами:

```json
"entry_spacing": "comfortable"
```

---

## 10. Настройки вкладок

### Текущие настройки:
```json
"tabs": {
  "git_status": true,
  "file_icons": true,
  "show_diagnostics": "errors",
  "show_close_button": "always"
}
```

### Рекомендации:

#### 10.1 `git_status`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Индикатор статуса Git на вкладках.

#### 10.2 `file_icons`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Иконки файлов улучшают визуальное восприятие.

#### 10.3 `show_diagnostics`
**Текущее:** `"errors"`
**Рекомендуется:** `"all"`
**Обоснование:** Показывать все диагностические сообщения:

```json
"show_diagnostics": "all"
```

#### 10.4 `show_close_button`
**Текущее:** `"always"`
**Рекомендуется:** `"hover"`
**Обоснование:** Кнопка закрытия только при наведении экономит место:

```json
"show_close_button": "hover"
```

---

## 11. Настройки превью-вкладок

### Текущие настройки:
```json
"preview_tabs": {
  "enable_preview_from_file_finder": true
}
```

### Рекомендации:

#### 11.1 Добавить дополнительные настройки:
```json
"preview_tabs": {
  "enable_preview_from_file_finder": true,
  "enable_preview_from_project_panel": true,
  "enable_preview_from_multibuffer": true,
  "enable_preview_file_from_code_navigation": true,
  "enable_keep_preview_on_code_navigation": false
}
```

---

## 12. Настройки скроллбара

### Текущие настройки:
```json
"scrollbar": {
  "show": "auto",
  "diagnostics": "information"
}
```

### Рекомендации:

#### 12.1 `show`
**Текущее:** `"auto"`
**Рекомендуется:** оставить `"auto"`
**Обоснование:** Автоматическое отображение скроллбара.

#### 12.2 `diagnostics`
**Текущее:** `"information"`
**Рекомендуется:** `"all"`
**Обоснование:** Показывать все диагностические маркеры:

```json
"scrollbar": {
  "show": "auto",
  "diagnostics": "all",
  "cursors": true,
  "git_diff": true,
  "search_results": true,
  "selected_text": true,
  "selected_symbol": true
}
```

---

## 13. Настройки минимапы

### Текущие настройки:
```json
"minimap": {
  "show": "never"
}
```

### Рекомендации:

#### 13.1 `show`
**Текущее:** `"never"`
**Рекомендуется:** `"auto"`
**Обоснование:** Минимап полезен для навигации по большим файлам:

```json
"minimap": {
  "show": "auto",
  "thumb": "always",
  "thumb_border": "left_open",
  "current_line_highlight": "line"
}
```

---

## 14. Настройки поиска

### Текущие настройки:
```json
"go_to_definition_fallback": "find_all_references",
"use_smartcase_search": true,
"seed_search_query_from_cursor": "selection",
"search": {
  "center_on_match": true
}
```

### Рекомендации:

#### 14.1 `go_to_definition_fallback`
**Текущее:** `"find_all_references"`
**Рекомендуется:** оставить `"find_all_references"`
**Обоснование:** Удобный fallback при отсутствии определения.

#### 14.2 `use_smartcase_search`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Умный поиск с учётом регистра.

#### 14.3 `seed_search_query_from_cursor`
**Текущее:** `"selection"`
**Рекомендуется:** `"always"`
**Обоснование:** Всегда использовать слово под курсором:

```json
"seed_search_query_from_cursor": "always"
```

#### 14.4 `search.center_on_match`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Центрирование результатов поиска.

---

## 15. Настройки семантических токенов и символов

### Текущие настройки:
```json
"semantic_tokens": "combined",
"document_symbols": "on",
"document_folding_ranges": "on",
"diagnostics_max_severity": null
```

### Рекомендации:

#### 15.1 `semantic_tokens`
**Текущее:** `"combined"`
**Рекомендуется:** оставить `"combined"`
**Обоснование:** Комбинация tree-sitter и LSP для лучшей подсветки.

#### 15.2 `document_symbols`
**Текущее:** `"on"`
**Рекомендуется:** оставить `"on"`
**Обоснование:** Использование LSP для символов документа.

#### 15.3 `document_folding_ranges`
**Текущее:** `"on"`
**Рекомендуется:** оставить `"on"`
**Обоснование:** Сворачивание кода через LSP.

#### 15.4 `diagnostics_max_severity`
**Текущее:** `null`
**Рекомендуется:** `"hint"`
**Обоснование:** Показывать все диагностические сообщения:

```json
"diagnostics_max_severity": "hint"
```

---

## 16. Настройки диагностики

### Текущие настройки:
```json
"diagnostics": {
  "inline": {
    "enabled": true,
    "max_severity": "warning",
    "min_column": 120
  }
}
```

### Рекомендации:

#### 16.1 `inline.enabled`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Inline-диагностика полезна для быстрого просмотра ошибок.

#### 16.2 `inline.max_severity`
**Текущее:** `"warning"`
**Рекомендуется:** `"error"`
**Обоснование:** Показывать только ошибки в inline:

```json
"diagnostics": {
  "inline": {
    "enabled": true,
    "max_severity": "error",
    "min_column": 80,
    "padding": 4,
    "update_debounce_ms": 150
  }
}
```

#### 16.3 `inline.min_column`
**Текущее:** `120`
**Рекомендуется:** `80`
**Обоснование:** Начинать отображение после 80 символов (стандартная длина строки):

```json
"min_column": 80
```

---

## 17. Настройки статус-бара

### Текущие настройки:
```json
"status_bar": {
  "line_endings_button": true
}
```

### Рекомендации:

#### 17.1 Добавить больше информации в статус-бар:
```json
"status_bar": {
  "line_endings_button": true,
  "active_language_button": true,
  "cursor_position_button": true
}
```

---

## 18. Настройки inlay hints

### Текущие настройки:
```json
"inlay_hints": {
  "enabled": false,
  "toggle_on_modifiers_press": {
    "alt": true
  }
}
```

### Рекомендации:

#### 18.1 `enabled`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Inlay hints полезны для Python-разработки (показывают типы параметров и возвращаемых значений):

```json
"inlay_hints": {
  "enabled": true,
  "show_type_hints": true,
  "show_parameter_hints": true,
  "show_other_hints": true,
  "show_background": false,
  "edit_debounce_ms": 700,
  "scroll_debounce_ms": 50,
  "toggle_on_modifiers_press": {
    "alt": true
  }
}
```

---

## 19. Настройки языков

### Текущие настройки для JSON:
```json
"JSON": {
  "prettier": {
    "allowed": true,
    "parser": "json"
  },
  "always_treat_brackets_as_autoclosed": true,
  "indent_guides": {
    "background_coloring": "disabled"
  },
  "use_on_type_format": true
}
```

### Рекомендации:

#### 19.1 JSON
**Рекомендуется:** добавить настройки отступов:
```json
"JSON": {
  "prettier": {
    "allowed": true,
    "parser": "json"
  },
  "always_treat_brackets_as_autoclosed": true,
  "indent_guides": {
    "background_coloring": "disabled"
  },
  "use_on_type_format": true,
  "tab_size": 2,
  "hard_tabs": false
}
```

### Текущие настройки для TOML:
```json
"TOML": {
  "format_on_save": "off",
  "prettier": {
    "allowed": false
  }
}
```

### Рекомендации:

#### 19.2 TOML
**Рекомендуется:** включить форматирование:
```json
"TOML": {
  "format_on_save": "on",
  "prettier": {
    "allowed": false
  }
}
```

### Текущие настройки для Python:
```json
"Python": {
  "semantic_tokens": "combined",
  "language_servers": ["basedpyright", "ruff"],
  "format_on_save": "on",
  "code_actions_on_format": {
    "source.organizeImports.ruff": true,
    "source.fixAll.ruff": true
  },
  "formatter": {
    "language_server": {
      "name": "ruff"
    }
  },
  "tab_size": 4,
  "hard_tabs": false
}
```

### Рекомендации:

#### 19.3 Python
**Рекомендуется:** добавить дополнительные настройки:
```json
"Python": {
  "semantic_tokens": "combined",
  "language_servers": ["basedpyright", "ruff"],
  "format_on_save": "on",
  "code_actions_on_format": {
    "source.organizeImports.ruff": true,
    "source.fixAll.ruff": true
  },
  "formatter": {
    "language_server": {
      "name": "ruff"
    }
  },
  "tab_size": 4,
  "hard_tabs": false,
  "preferred_line_length": 88,
  "soft_wrap": "bounded",
  "show_whitespaces": "boundary",
  "indent_guides": {
    "background_coloring": "indent_aware",
    "coloring": "indent_aware"
  },
  "inlay_hints": {
    "enabled": true,
    "show_type_hints": true,
    "show_parameter_hints": true,
    "show_other_hints": true
  }
}
```

---

## 20. Настройки LSP

### Текущие настройки:
```json
"lsp": {
  "codebook": {
    "initialization_options": {
      "diagnosticSeverity": "hint"
    }
  },
  "ruff": {
    "initialization_options": {
      "settings": {
        "configurationPreference": "editorFirst",
        "showSyntaxErrors": true,
        "lineLength": 120,
        "lint": {
          "select": ["E4", "E7", "E9", "F", "I"],
          "ignore": ["E501"]
        }
      }
    }
  },
  "basedpyright": {
    "settings": {
      "python.analysis": {
        "diagnosticMode": "workspace",
        "typeCheckingMode": "basic",
        "autoSearchPaths": true,
        "autoImportCompletions": true,
        "useLibraryCodeForTypes": true
      }
    }
  }
}
```

### Рекомендации:

#### 20.1 `codebook`
**Рекомендуется:** добавить настройки для Python:
```json
"codebook": {
  "initialization_options": {
    "diagnosticSeverity": "hint",
    "python": {
      "analysis": {
        "autoSearchPaths": true,
        "useLibraryCodeForTypes": true
      }
    }
  }
}
```

#### 20.2 `ruff`
**Рекомендуется:** расширить правила линтера:
```json
"ruff": {
  "initialization_options": {
    "settings": {
      "configurationPreference": "editorFirst",
      "showSyntaxErrors": true,
      "lineLength": 88,
      "lint": {
        "select": [
          "E", "W", "F", "I", "B", "C4", "SIM", "UP", "N", "ANN", "D"
        ],
        "ignore": [
          "E501", "D103", "D104", "D107", "ANN101", "ANN102", "ANN401"
        ]
      },
      "format": {
        "quote-style": "double",
        "indent-style": "space",
        "line-length": 88
      }
    }
  }
}
```

#### 20.3 `basedpyright`
**Рекомендуется:** добавить больше настроек:
```json
"basedpyright": {
  "settings": {
    "python.analysis": {
      "diagnosticMode": "workspace",
      "typeCheckingMode": "standard",
      "autoSearchPaths": true,
      "autoImportCompletions": true,
      "useLibraryCodeForTypes": true,
      "inlayHints": {
        "callArgumentNames": true,
        "variableTypes": true,
        "parameterTypes": true,
        "functionReturnTypes": true,
        "classVariableTypes": true
      },
      "reportMissingTypeStubs": false,
      "reportUnusedImport": true,
      "reportUnusedVariable": true,
      "reportGeneralTypeIssues": true,
      "reportOptionalMemberAccess": true,
      "reportOptionalCall": true,
      "reportOptionalSubscript": true,
      "reportOptionalIterable": true,
      "reportOptionalContextManager": true,
      "reportOptionalOperand": true
    }
  }
}
```

#### 20.4 Добавить настройки для других LSP:

##### Pyright (fallback):
```json
"pyright": {
  "settings": {
    "python.analysis": {
      "diagnosticMode": "workspace",
      "typeCheckingMode": "standard",
      "autoSearchPaths": true,
      "useLibraryCodeForTypes": true
    }
  }
}
```

##### PyLSP:
```json
"pylsp": {
  "settings": {
    "pylsp": {
      "plugins": {
        "pycodestyle": {
          "enabled": true,
          "maxLineLength": 88
        },
        "pyflakes": {
          "enabled": true
        },
        "autopep8": {
          "enabled": false
        },
        "yapf": {
          "enabled": false
        },
        "mccabe": {
          "enabled": true,
          "threshold": 10
        }
      }
    }
  }
}
```

---

## 21. Настройки терминала

### Текущие настройки:
```json
"terminal": {
  "show_count_badge": true,
  "detect_venv": {
    "on": {
      "directories": [".venv", "venv", ".env", "env", ".pypoetry", ".hatch"],
      "activate_script": "default"
    }
  },
  "env": {
    "EDITOR": "zed --wait"
  }
}
```

### Рекомендации:

#### 21.1 `show_count_badge`
**Текущее:** `true`
**Рекомендуется:** оставить `true`
**Обоснование:** Индикатор количества терминалов.

#### 21.2 `detect_venv`
**Текущее:**
```json
"directories": [".venv", "venv", ".env", "env", ".pypoetry", ".hatch"]
```
**Рекомендуется:** добавить больше вариантов:
```json
"detect_venv": {
  "on": {
    "directories": [
      ".venv", "venv", ".env", "env", ".pypoetry", ".hatch",
      ".tox", ".nox", "build", "dist"
    ],
    "activate_script": "default"
  }
}
```

#### 21.3 `env`
**Рекомендуется:** добавить больше переменных:
```json
"env": {
  "EDITOR": "zed --wait",
  "VISUAL": "zed --wait",
  "PYTHONUNBUFFERED": "1",
  "PIP_DISABLE_PIP_VERSION_CHECK": "1"
}
```

#### 21.4 Добавить новые настройки терминала:
```json
"terminal": {
  "show_count_badge": true,
  "detect_venv": {
    "on": {
      "directories": [
        ".venv", "venv", ".env", "env", ".pypoetry", ".hatch",
        ".tox", ".nox", "build", "dist"
      ],
      "activate_script": "default"
    }
  },
  "env": {
    "EDITOR": "zed --wait",
    "VISUAL": "zed --wait",
    "PYTHONUNBUFFERED": "1",
    "PIP_DISABLE_PIP_VERSION_CHECK": "1"
  },
  "font_size": 14,
  "font_family": "Maple Mono NF",
  "blinking": "terminal_controlled",
  "copy_on_select": false,
  "keep_selection_on_copy": true,
  "open_links_in_mouse_mode": true,
  "dock": "bottom",
  "scroll_multiplier": 3.0,
  "working_directory": "current_project_directory",
  "shell": {
    "program": "zsh",
    "args": ["--login"]
  }
}
```

---

## 22. Настройки приватных файлов

### Текущие настройки:
```json
"private_files": ["**/.env*", "**/*.pem", "**/*.key", "**/*.cert", "**/*.crt", "**/secrets.yml"],
"redact_private_values": false
```

### Рекомендации:

#### 22.1 `private_files`
**Рекомендуется:** расширить список:
```json
"private_files": [
  "**/.env*",
  "**/*.pem",
  "**/*.key",
  "**/*.cert",
  "**/*.crt",
  "**/secrets.yml",
  "**/.aws/**",
  "**/.ssh/**",
  "**/credentials.json",
  "**/service-account*.json"
]
```

#### 22.2 `redact_private_values`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Скрывать значения переменных в приватных файлах:

```json
"redact_private_values": true
```

---

## 23. Настройки отладчика

### Текущие настройки:
```json
"debugger": {
  "dock": "bottom",
  "stepping_granularity": "line"
}
```

### Рекомендации:

#### 23.1 `dock`
**Текущее:** `"bottom"`
**Рекомендуется:** оставить `"bottom"`
**Обоснование:** Нижняя панель удобна для отладки.

#### 23.2 `stepping_granularity`
**Текущее:** `"line"`
**Рекомендуется:** оставить `"line"`
**Обоснование:** Стандартная гранулярность шагов.

#### 23.3 Добавить новые настройки:
```json
"debugger": {
  "dock": "bottom",
  "stepping_granularity": "line",
  "save_breakpoints": true,
  "button": true
}
```

---

## 24. Настройки автообновления расширений

### Текущие настройки:
```json
"auto_update_extensions": {
  "colored-zed-icons-theme": true
}
```

### Рекомендации:

#### 24.1 Добавить больше расширений:
```json
"auto_update_extensions": {
  "colored-zed-icons-theme": true,
  "biome": true,
  "ruff": true,
  "python": true,
  "docker": true,
  "yaml": true,
  "toml": true,
  "markdown": true
}
```

---

## 25. Настройки телеметрии

### Текущие настройки:
```json
"telemetry": {
  "diagnostics": false,
  "metrics": false
}
```

### Рекомендации:

#### 25.1 Оставить как есть
**Обоснование:** Отключение телеметрии — хорошая практика для конфиденциальности.

---

## 26. Настройки Git

### Текущие настройки:
```json
"git": {
  "inline_blame": {
    "show_commit_summary": true
  },
  "git_gutter": "tracked_files"
}
```

### Рекомендации:

#### 26.1 `inline_blame`
**Рекомендуется:** добавить больше настроек:
```json
"git": {
  "inline_blame": {
    "enabled": true,
    "show_commit_summary": true,
    "location": "inline",
    "delay_ms": 500,
    "min_column": 80,
    "padding": 10
  },
  "git_gutter": "tracked_files",
  "gutter_debounce": 100,
  "hunk_style": "staged_hollow",
  "branch_picker": {
    "show_author_name": true
  }
}
```

---

## 27. Настройки панели Git

### Текущие настройки:
```json
"git_panel": {
  "sort_by": "name",
  "group_by": "status",
  "dock": "left",
  "show_count_badge": true,
  "file_icons": true,
  "tree_view": false,
  "collapse_untracked_diff": false,
  "sort_by_path": false
}
```

### Рекомендации:

#### 27.1 `sort_by`
**Текущее:** `"name"`
**Рекомендуется:** `"path"`
**Обоснование:** Сортировка по пути удобнее для больших проектов:

```json
"sort_by": "path"
```

#### 27.2 `tree_view`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Древовидный вид удобен для навигации:

```json
"tree_view": true
```

#### 27.3 `collapse_untracked_diff`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Сворачивать незатрекенные файлы:

```json
"collapse_untracked_diff": true
```

#### 27.4 `sort_by_path`
**Текущее:** `false`
**Рекомендуется:** `true`
**Обоснование:** Сортировать по пути:

```json
"sort_by_path": true
```

#### 27.5 Добавить новые настройки:
```json
"git_panel": {
  "sort_by": "path",
  "group_by": "status",
  "dock": "left",
  "show_count_badge": true,
  "file_icons": true,
  "tree_view": true,
  "collapse_untracked_diff": true,
  "sort_by_path": true,
  "button": true,
  "default_width": 360,
  "status_style": "icon",
  "fallback_branch_name": "main",
  "starts_open": false
}
```

---

## 28. Настройки стиля диффа

### Текущие настройки:
```json
"diff_view_style": "split"
```

### Рекомендации:

#### 28.1 `diff_view_style`
**Текущее:** `"split"`
**Рекомендуется:** оставить `"split"`
**Обоснование:** Разделённый вид удобен для сравнения изменений.

---

## 29. Настройки which_key

### Текущие настройки:
```json
"which_key": {
  "enabled": true
}
```

### Рекомендации:

#### 29.1 Добавить задержку:
```json
"which_key": {
  "enabled": true,
  "delay_ms": 500
}
```

---

## 30. Настройки сессии

### Текущие настройки:
```json
"session": {
  "trust_all_worktrees": true
}
```

### Рекомендации:

#### 30.1 Добавить настройку восстановления:
```json
"session": {
  "trust_all_worktrees": true,
  "restore_unsaved_buffers": true
}
```

---

## 31. Настройки темы и шрифтов

### Текущие настройки:
```json
"icon_theme": "Material Icon Theme",
"theme": "One Dark PyCharm Comfort",
"ui_font_family": ".ZedSans",
"ui_font_size": 15,
"buffer_font_size": 13,
"buffer_line_height": {
  "custom": 2.0
}
```

### Рекомендации:

#### 31.1 `icon_theme`
**Текущее:** `"Material Icon Theme"`
**Рекомендуется:** оставить
**Обоснование:** Хорошая тема иконок.

#### 31.2 `theme`
**Текущее:** `"One Dark PyCharm Comfort"`
**Рекомендуется:** использовать объект для поддержки светлой/тёмной темы:
```json
"theme": {
  "mode": "dark",
  "dark": "One Dark",
  "light": "One Light"
}
```

#### 31.3 `ui_font_family`
**Текущее:** `".ZedSans"`
**Рекомендуется:** добавить fallback:
```json
"ui_font_family": ".ZedSans",
"ui_font_fallbacks": ["SF Pro", "Helvetica Neue", "Arial"]
```

#### 31.4 `ui_font_size`
**Текущее:** `15`
**Рекомендуется:** `16`
**Обоснование:** Стандартный размер UI:

```json
"ui_font_size": 16
```

#### 31.5 `buffer_font_size`
**Текущее:** `13`
**Рекомендуется:** `14`
**Обоснование:** Более читаемый размер шрифта кода:

```json
"buffer_font_size": 14
```

#### 31.6 `buffer_line_height`
**Текущее:** `{ "custom": 2.0 }`
**Рекомендуется:** `{ "custom": 1.6 }`
**Обоснование:** Более плотный межстрочный интервал:

```json
"buffer_line_height": {
  "custom": 1.6
}
```

#### 31.7 Добавить новые настройки шрифтов:
```json
"buffer_font_family": "Maple Mono NF",
"buffer_font_fallbacks": [
  "Maple Mono NF",
  "JetBrainsMono Nerd Font Mono",
  "Fira Code",
  "Menlo",
  "Monaco",
  "Courier New"
],
"buffer_font_features": {
  "calt": true,
  "liga": true,
  "ss01": true
},
"buffer_font_weight": 400,
"ui_font_features": {
  "calt": false
},
"ui_font_weight": 400
```

---

## 32. Новые настройки для backend-разработки на Python

### 32.1 Настройки для работы с Docker:
```json
"languages": {
  "Dockerfile": {
    "format_on_save": "on",
    "tab_size": 4,
    "hard_tabs": false
  },
  "YAML": {
    "format_on_save": "on",
    "tab_size": 2,
    "hard_tabs": false
  }
}
```

### 32.2 Настройки для SQL:
```json
"languages": {
  "SQL": {
    "format_on_save": "on",
    "tab_size": 4,
    "hard_tabs": false,
    "preferred_line_length": 120
  }
}
```

### 32.3 Настройки для Markdown:
```json
"languages": {
  "Markdown": {
    "format_on_save": "off",
    "preferred_line_length": 80,
    "soft_wrap": "bounded"
  }
}
```

### 32.4 Настройки для Shell Script:
```json
"languages": {
  "Shell Script": {
    "format_on_save": "on",
    "tab_size": 2,
    "hard_tabs": false
  }
}
```

### 32.5 Настройки для INI:
```json
"languages": {
  "INI": {
    "format_on_save": "on",
    "tab_size": 4,
    "hard_tabs": false
  }
}
```

### 32.6 Глобальные настройки LSP:
```json
"global_lsp_settings": {
  "button": true,
  "request_timeout": 120,
  "max_buffer_line_length": 20000,
  "notifications": {
    "dismiss_timeout_ms": 5000
  }
}
```

### 32.7 Настройки для файловых сканеров:
```json
"file_scan_exclusions": [
  "**/.git",
  "**/.svn",
  "**/.hg",
  "**/.jj",
  "**/CVS",
  "**/.DS_Store",
  "**/.classpath",
  "**/.settings",
  "**/out",
  "**/dist",
  "**/.husky",
  "**/.turbo",
  "**/.vscode-test",
  "**/.vscode",
  "**/.next",
  "**/.storybook",
  "**/.tap",
  "**/.nyc_output",
  "**/report",
  "**/node_modules",
  "**/.venv",
  "**/__pycache__",
  "**/*.pyc",
  "**/.mypy_cache",
  "**/.pytest_cache",
  "**/.ruff_cache",
  "**/.coverage",
  "**/.tox",
  "**/.nox"
]
```

### 32.8 Настройки для типов файлов:
```json
"file_types": {
  "Dockerfile": ["Dockerfile", "Dockerfile.*"],
  "JSON": ["json", "jsonc", "*.code-snippets"],
  "YAML": ["yml", "yaml"],
  "Shell Script": ["sh", "bash", "zsh", "fish"],
  "TOML": ["toml"],
  "INI": ["ini", "cfg", "conf"]
}
```

### 32.9 Настройки для REPL:
```json
"repl": {
  "max_columns": 128,
  "max_lines": 32
}
```

### 32.10 Настройки для профилей:
```json
"profiles": {
  "Presentation": {
    "settings": {
      "buffer_font_size": 20,
      "ui_font_size": 18,
      "theme": "One Dark",
      "tab_bar": { "show": false },
      "toolbar": { "breadcrumbs": false }
    }
  },
  "Writing": {
    "settings": {
      "buffer_font_size": 15,
      "ui_font_size": 15,
      "theme": "One Light",
      "tab_bar": { "show": false },
      "toolbar": { "breadcrumbs": false }
    }
  }
}
```

---

## 33. Итоговый оптимизированный конфиг

Ниже приведён итоговый оптимизированный `settings.json` с учётом всех рекомендаций:

```json
{
  // === Основные настройки ===
  "accessible_mode": true,
  "restore_on_startup": "last_session",
  "close_on_file_delete": true,
  "autosave": {
    "after_delay": {
      "milliseconds": 1000
    }
  },
  "format_on_save": "modifications_if_available",
  "prettier": {
    "allowed": true,
    "prettier": {
      "singleQuote": true,
      "trailingComma": "all",
      "printWidth": 100,
      "tabWidth": 2
    }
  },
  "load_direnv": "shell_hook",
  "ensure_final_newline_on_save": true,
  "remove_trailing_whitespace_on_save": true,
  "line_ending": "detect",
  "confirm_quit": true,
  "use_system_window_tabs": true,

  // === Toolbar ===
  "toolbar": {
    "breadcrumbs": true,
    "quick_actions": true,
    "selections_menu": true,
    "agent_review": true,
    "code_actions": true
  },

  // === AI-агент ===
  "agent": {
    "sidebar_side": "right",
    "tool_permissions": {
      "default": "confirm",
      "tools": {
        "edit_file": { "default": "confirm" },
        "delete_path": { "default": "confirm" },
        "terminal": {
          "default": "allow",
          "always_allow": [
            {
              "pattern": "^(python|pytest|pip|uv|poetry|docker|docker-compose)\\s+.*"
            },
            {
              "pattern": "^(git\\s+(add|commit|diff|log|status|push|pull|branch))"
            }
          ],
          "always_deny": [
            {
              "pattern": "^(sudo|su|doas)\\s"
            },
            {
              "pattern": "^rm\\s+-[rf]\\s+/"
            }
          ]
        }
      }
    },
    "default_profile": "write",
    "dock": "right",
    "default_model": {
      "enable_thinking": true,
      "provider": "anthropic",
      "model": "claude-3-5-sonnet-20241022"
    },
    "inline_assistant_model": {
      "enable_thinking": false,
      "provider": "openrouter",
      "model": "google/gemini-2.0-flash-thinking"
    },
    "inline_alternatives": [
      {
        "enable_thinking": false,
        "provider": "openrouter",
        "model": "qwen/qwen3-coder:free"
      },
      {
        "enable_thinking": false,
        "provider": "openrouter",
        "model": "openai/gpt-oss-120b"
      }
    ],
    "thread_summary_model": {
      "enable_thinking": false,
      "provider": "openrouter",
      "model": "openai/gpt-oss-120b"
    },
    "commit_message_model": {
      "enable_thinking": false,
      "provider": "openrouter",
      "model": "openai/gpt-oss-120b"
    },
    "commit_message_instructions": "Use Conventional Commits format: <type>(<scope>): <description>. Types: feat, fix, docs, style, refactor, test, chore. Scope: api, db, auth, config, etc.",
    "auto_compact": {
      "enabled": true,
      "threshold": "85%"
    },
    "compaction_model": {
      "provider": "openrouter",
      "model": "google/gemini-2.0-flash"
    },
    "notify_when_agent_waiting": "primary_screen",
    "play_sound_when_agent_done": "never",
    "single_file_review": true,
    "agent_follow": true,
    "model_parameters": [
      {
        "provider": "anthropic",
        "temperature": 0.3
      },
      {
        "provider": "openrouter",
        "temperature": 0.4
      }
    ],
    "profiles": {
      "write": {
        "name": "Write Code",
        "tools": {
          "edit_file": true,
          "create_file": true,
          "delete_file": true,
          "rename_file": true,
          "find_file": true,
          "terminal": true,
          "search": true,
          "fetch": true,
          "run_command": true,
          "copy_path": true,
          "move_path": true,
          "open_file": true,
          "go_to_file": true,
          "set_context": true,
          "set_files": true,
          "spawn_agent": false,
          "create_task": false
        },
        "enable_all_context_servers": true
      },
      "review": {
        "name": "Code Review",
        "tools": {
          "edit_file": false,
          "create_file": false,
          "delete_file": false,
          "rename_file": false,
          "find_file": true,
          "terminal": false,
          "search": true,
          "fetch": true,
          "run_command": false,
          "copy_path": true,
          "move_path": false,
          "open_file": true,
          "go_to_file": true,
          "set_context": true,
          "set_files": true,
          "spawn_agent": false,
          "create_task": false
        },
        "enable_all_context_servers": true,
        "default_model": {
          "provider": "anthropic",
          "model": "claude-3-5-sonnet-20241022"
        }
      },
      "debug": {
        "name": "Debug",
        "tools": {
          "edit_file": true,
          "create_file": false,
          "delete_file": false,
          "rename_file": false,
          "find_file": true,
          "terminal": true,
          "search": true,
          "fetch": true,
          "run_command": true,
          "copy_path": true,
          "move_path": false,
          "open_file": true,
          "go_to_file": true,
          "set_context": true,
          "set_files": true,
          "spawn_agent": true,
          "create_task": false
        },
        "enable_all_context_servers": false,
        "default_model": {
          "provider": "openrouter",
          "model": "google/gemini-2.0-flash-thinking"
        }
      }
    }
  },

  // === Языковые модели ===
  "language_models": {
    "openai_compatible": {
      "RouterAI": {
        "api_url": "https://routerai.ru/api/v1",
        "available_models": [
          {
            "name": "anthropic/claude-sonnet-4.6",
            "display_name": "Claude Sonnet 4.6 (RouterAI)",
            "max_tokens": 200000,
            "max_output_tokens": 32000,
            "max_completion_tokens": 200000,
            "capabilities": {
              "tools": true,
              "images": true,
              "parallel_tool_calls": false,
              "prompt_cache_key": false
            }
          }
        ]
      }
    },
    "ollama": {
      "api_url": "http://localhost:11434"
    },
    "openai": {
      "api_url": "https://api.openai.com/v1"
    },
    "anthropic": {
      "api_url": "https://api.anthropic.com"
    }
  },

  // === Предсказания редактирования ===
  "edit_predictions": {
    "provider": "zed",
    "mode": "subtle",
    "disabled_globs": [
      "**/.env*",
      "**/*.pem",
      "**/*.key",
      "**/*.cert",
      "**/*.crt",
      "**/.dev.vars",
      "**/secrets.yml",
      "**/.zed/settings.json",
      "**/.zed/keymap.json"
    ]
  },

  // === Клавиатурные комбинации ===
  "base_keymap": "VSCode",
  "cursor_shape": "block",
  "current_line_highlight": "all",
  "auto_signature_help": false,
  "show_signature_help_after_edits": false,
  "scroll_beyond_last_line": "one_page",
  "text_rendering_mode": "subpixel",
  "vertical_scroll_margin": 4,
  "horizontal_scroll_margin": 8,

  // === Автодополнение ===
  "completions": {
    "words": "enabled",
    "words_min_length": 1,
    "lsp_insert_mode": "replace"
  },

  // === Отображение текста ===
  "show_whitespaces": "boundary",
  "preferred_line_length": 120,
  "wrap_guides": [88, 120],
  "colorize_brackets": true,
  "indent_guides": {
    "background_coloring": "indent_aware",
    "coloring": "indent_aware"
  },
  "use_on_type_format": true,
  "use_auto_surround": true,
  "use_autoclose": true,

  // === Панель проекта ===
  "project_panel": {
    "starts_open": true,
    "auto_reveal_entries": true,
    "hide_gitignore": true,
    "auto_open": {
      "on_paste": true,
      "on_create": true,
      "on_drop": true
    },
    "hide_root": true,
    "dock": "left",
    "hide_hidden": true,
    "bold_folder_labels": true,
    "diagnostic_badges": true,
    "git_status_indicator": true,
    "auto_fold_dirs": true,
    "folder_icons": true,
    "file_icons": true,
    "entry_spacing": "comfortable",
    "default_width": 240,
    "sort_mode": "directories_first",
    "sort_order": "default",
    "indent_size": 20,
    "sticky_scroll": true,
    "show_diagnostics": "all",
    "indent_guides": {
      "show": "always"
    }
  },

  // === Вкладки ===
  "tabs": {
    "git_status": true,
    "file_icons": true,
    "show_diagnostics": "all",
    "show_close_button": "hover",
    "close_position": "right",
    "activate_on_close": "history"
  },

  // === Превью-вкладки ===
  "preview_tabs": {
    "enable_preview_from_file_finder": true,
    "enable_preview_from_project_panel": true,
    "enable_preview_from_multibuffer": true,
    "enable_preview_file_from_code_navigation": true,
    "enable_keep_preview_on_code_navigation": false
  },

  // === Скроллбар ===
  "scrollbar": {
    "show": "auto",
    "diagnostics": "all",
    "cursors": true,
    "git_diff": true,
    "search_results": true,
    "selected_text": true,
    "selected_symbol": true,
    "axes": {
      "horizontal": true,
      "vertical": true
    }
  },

  // === Минимап ===
  "minimap": {
    "show": "auto",
    "thumb": "always",
    "thumb_border": "left_open",
    "current_line_highlight": "line"
  },

  // === Поиск ===
  "go_to_definition_fallback": "find_all_references",
  "use_smartcase_search": true,
  "seed_search_query_from_cursor": "always",
  "search": {
    "center_on_match": true,
    "button": true,
    "whole_word": false,
    "case_sensitive": false,
    "include_ignored": false,
    "regex": false
  },
  "search_wrap": true,

  // === Семантические токены ===
  "semantic_tokens": "combined",
  "document_symbols": "on",
  "document_folding_ranges": "on",
  "diagnostics_max_severity": "hint",

  // === Диагностика ===
  "diagnostics": {
    "button": true,
    "include_warnings": true,
    "inline": {
      "enabled": true,
      "update_debounce_ms": 150,
      "padding": 4,
      "min_column": 80,
      "max_severity": "error"
    }
  },

  // === Статус-бар ===
  "status_bar": {
    "line_endings_button": true,
    "active_language_button": true,
    "cursor_position_button": true
  },

  // === Inlay hints ===
  "inlay_hints": {
    "enabled": true,
    "show_type_hints": true,
    "show_parameter_hints": true,
    "show_other_hints": true,
    "show_background": false,
    "edit_debounce_ms": 700,
    "scroll_debounce_ms": 50,
    "toggle_on_modifiers_press": {
      "alt": true
    }
  },

  // === Языки ===
  "languages": {
    "JSON": {
      "prettier": {
        "allowed": true,
        "parser": "json"
      },
      "always_treat_brackets_as_autoclosed": true,
      "indent_guides": {
        "background_coloring": "disabled"
      },
      "use_on_type_format": true,
      "tab_size": 2,
      "hard_tabs": false
    },
    "TOML": {
      "format_on_save": "on",
      "prettier": {
        "allowed": false
      }
    },
    "Python": {
      "semantic_tokens": "combined",
      "language_servers": ["basedpyright", "ruff"],
      "format_on_save": "on",
      "code_actions_on_format": {
        "source.organizeImports.ruff": true,
        "source.fixAll.ruff": true
      },
      "formatter": {
        "language_server": {
          "name": "ruff"
        }
      },
      "tab_size": 4,
      "hard_tabs": false,
      "preferred_line_length": 88,
      "soft_wrap": "bounded",
      "show_whitespaces": "boundary",
      "indent_guides": {
        "background_coloring": "indent_aware",
        "coloring": "indent_aware"
      },
      "inlay_hints": {
        "enabled": true,
        "show_type_hints": true,
        "show_parameter_hints": true,
        "show_other_hints": true
      }
    },
    "Dockerfile": {
      "format_on_save": "on",
      "tab_size": 4,
      "hard_tabs": false
    },
    "YAML": {
      "format_on_save": "on",
      "tab_size": 2,
      "hard_tabs": false
    },
    "SQL": {
      "format_on_save": "on",
      "tab_size": 4,
      "hard_tabs": false,
      "preferred_line_length": 120
    },
    "Markdown": {
      "format_on_save": "off",
      "preferred_line_length": 80,
      "soft_wrap": "bounded"
    },
    "Shell Script": {
      "format_on_save": "on",
      "tab_size": 2,
      "hard_tabs": false
    },
    "INI": {
      "format_on_save": "on",
      "tab_size": 4,
      "hard_tabs": false
    }
  },

  // === LSP ===
  "global_lsp_settings": {
    "button": true,
    "request_timeout": 120,
    "max_buffer_line_length": 20000,
    "notifications": {
      "dismiss_timeout_ms": 5000
    }
  },
  "lsp": {
    "codebook": {
      "initialization_options": {
        "diagnosticSeverity": "hint"
      }
    },
    "ruff": {
      "initialization_options": {
        "settings": {
          "configurationPreference": "editorFirst",
          "showSyntaxErrors": true,
          "lineLength": 88,
          "lint": {
            "select": ["E", "W", "F", "I", "B", "C4", "SIM", "UP", "N", "ANN", "D"],
            "ignore": ["E501", "D103", "D104", "D107", "ANN101", "ANN102", "ANN401"]
          },
          "format": {
            "quote-style": "double",
            "indent-style": "space",
            "line-length": 88
          }
        }
      }
    },
    "basedpyright": {
      "settings": {
        "python.analysis": {
          "diagnosticMode": "workspace",
          "typeCheckingMode": "standard",
          "autoSearchPaths": true,
          "autoImportCompletions": true,
          "useLibraryCodeForTypes": true,
          "inlayHints": {
            "callArgumentNames": true,
            "variableTypes": true,
            "parameterTypes": true,
            "functionReturnTypes": true,
            "classVariableTypes": true
          },
          "reportMissingTypeStubs": false,
          "reportUnusedImport": true,
          "reportUnusedVariable": true,
          "reportGeneralTypeIssues": true,
          "reportOptionalMemberAccess": true,
          "reportOptionalCall": true,
          "reportOptionalSubscript": true,
          "reportOptionalIterable": true,
          "reportOptionalContextManager": true,
          "reportOptionalOperand": true
        }
      }
    }
  },

  // === Терминал ===
  "terminal": {
    "show_count_badge": true,
    "detect_venv": {
      "on": {
        "directories": [
          ".venv", "venv", ".env", "env", ".pypoetry", ".hatch",
          ".tox", ".nox", "build", "dist"
        ],
        "activate_script": "default"
      }
    },
    "env": {
      "EDITOR": "zed --wait",
      "VISUAL": "zed --wait",
      "PYTHONUNBUFFERED": "1",
      "PIP_DISABLE_PIP_VERSION_CHECK": "1"
    },
    "font_size": 14,
    "font_family": "Maple Mono NF",
    "blinking": "terminal_controlled",
    "copy_on_select": false,
    "keep_selection_on_copy": true,
    "open_links_in_mouse_mode": true,
    "dock": "bottom",
    "scroll_multiplier": 3.0,
    "working_directory": "current_project_directory",
    "shell": {
      "program": "zsh",
      "args": ["--login"]
    }
  },

  // === Приватные файлы ===
  "private_files": [
    "**/.env*",
    "**/*.pem",
    "**/*.key",
    "**/*.cert",
    "**/*.crt",
    "**/secrets.yml",
    "**/.aws/**",
    "**/.ssh/**",
    "**/credentials.json",
    "**/service-account*.json"
  ],
  "redact_private_values": true,

  // === Отладчик ===
  "debugger": {
    "dock": "bottom",
    "stepping_granularity": "line",
    "save_breakpoints": true,
    "button": true
  },

  // === Автообновление расширений ===
  "auto_update_extensions": {
    "colored-zed-icons-theme": true,
    "biome": true,
    "ruff": true,
    "python": true,
    "docker": true,
    "yaml": true,
    "toml": true,
    "markdown": true
  },

  // === Телеметрия ===
  "telemetry": {
    "diagnostics": false,
    "metrics": false
  },

  // === Git ===
  "git": {
    "inline_blame": {
      "enabled": true,
      "show_commit_summary": true,
      "location": "inline",
      "delay_ms": 500,
      "min_column": 80,
      "padding": 10
    },
    "git_gutter": "tracked_files",
    "gutter_debounce": 100,
    "hunk_style": "staged_hollow",
    "branch_picker": {
      "show_author_name": true
    }
  },

  // === Панель Git ===
  "git_panel": {
    "sort_by": "path",
    "group_by": "status",
    "dock": "left",
    "show_count_badge": true,
    "file_icons": true,
    "tree_view": true,
    "collapse_untracked_diff": true,
    "sort_by_path": true,
    "button": true,
    "default_width": 360,
    "status_style": "icon",
    "fallback_branch_name": "main",
    "starts_open": false
  },

  // === Стили диффов ===
  "diff_view_style": "split",

  // === Which Key ===
  "which_key": {
    "enabled": true,
    "delay_ms": 500
  },

  // === Сессия ===
  "session": {
    "trust_all_worktrees": true,
    "restore_unsaved_buffers": true
  },

  // === Тема и шрифты ===
  "icon_theme": "Material Icon Theme",
  "theme": {
    "mode": "dark",
    "dark": "One Dark",
    "light": "One Light"
  },
  "ui_font_family": ".ZedSans",
  "ui_font_fallbacks": ["SF Pro", "Helvetica Neue", "Arial"],
  "ui_font_size": 16,
  "ui_font_features": {
    "calt": false
  },
  "ui_font_weight": 400,
  "buffer_font_size": 14,
  "buffer_font_family": "Maple Mono NF",
  "buffer_font_fallbacks": [
    "Maple Mono NF",
    "JetBrainsMono Nerd Font Mono",
    "Fira Code",
    "Menlo",
    "Monaco",
    "Courier New"
  ],
  "buffer_font_features": {
    "calt": true,
    "liga": true,
    "ss01": true
  },
  "buffer_font_weight": 400,
  "buffer_line_height": {
    "custom": 1.6
  },

  // === Дополнительные настройки ===
  "file_scan_exclusions": [
    "**/.git",
    "**/.svn",
    "**/.hg",
    "**/.jj",
    "**/CVS",
    "**/.DS_Store",
    "**/.classpath",
    "**/.settings",
    "**/out",
    "**/dist",
    "**/.husky",
    "**/.turbo",
    "**/.vscode-test",
    "**/.vscode",
    "**/.next",
    "**/.storybook",
    "**/.tap",
    "**/.nyc_output",
    "**/report",
    "**/node_modules",
    "**/.venv",
    "**/__pycache__",
    "**/*.pyc",
    "**/.mypy_cache",
    "**/.pytest_cache",
    "**/.ruff_cache",
    "**/.coverage",
    "**/.tox",
    "**/.nox"
  ],
  "file_types": {
    "Dockerfile": ["Dockerfile", "Dockerfile.*"],
    "JSON": ["json", "jsonc", "*.code-snippets"],
    "YAML": ["yml", "yaml"],
    "Shell Script": ["sh", "bash", "zsh", "fish"],
    "TOML": ["toml"],
    "INI": ["ini", "cfg", "conf"]
  },
  "repl": {
    "max_columns": 128,
    "max_lines": 32
  },
  "profiles": {
    "Presentation": {
      "settings": {
        "buffer_font_size": 20,
        "ui_font_size": 18,
        "theme": "One Dark",
        "tab_bar": { "show": false },
        "toolbar": { "breadcrumbs": false }
      }
    },
    "Writing": {
      "settings": {
        "buffer_font_size": 15,
        "ui_font_size": 15,
        "theme": "One Light",
        "tab_bar": { "show": false },
        "toolbar": { "breadcrumbs": false }
      }
    }
  }
}
```

---

## 34. Краткое резюме изменений

### Что изменено:
1. **Autosave** — переключено на `after_delay` с 1000ms
2. **Format on save** — переключено на `modifications_if_available`
3. **Load direnv** — переключено на `shell_hook`
4. **Toolbar** — включены `selections_menu`, `agent_review`, `code_actions`
5. **AI модели** — добавлен Claude 3.5 Sonnet в качестве основной модели
6. **Tool permissions** — добавлены правила для Python и Git команд
7. **Edit predictions** — переключено на встроенный Zed provider
8. **Base keymap** — переключено на VSCode
9. **Cursor shape** — изменено на `block`
10. **Current line highlight** — изменено на `all`
11. **Auto signature help** — отключено
12. **Scroll beyond last line** — изменено на `one_page`
13. **Completions** — включены word completions
14. **Show whitespaces** — изменено на `boundary`
15. **Wrap guides** — добавлена граница 88
16. **Indent guides** — включено background_coloring
17. **Use on type format** — включено
18. **Project panel** — множество улучшений (hide_gitignore, hide_root, bold_folder_labels, auto_fold_dirs, entry_spacing)
19. **Tabs** — изменены настройки отображения
20. **Scrollbar** — включены все индикаторы
21. **Minimap** — включен в режиме `auto`
22. **Diagnostics** — добавлены новые настройки
23. **Inlay hints** — включены
24. **Python LSP** — расширены настройки basedpyright и ruff
25. **Terminal** — добавлены новые настройки
26. **Private files** — расширен список
27. **Git** — добавлены новые настройки
28. **Git panel** — изменены настройки сортировки и отображения
29. **Theme** — изменена на объект с поддержкой светлой/тёмной темы
30. **Fonts** — оптимизированы размеры и настройки

### Что добавлено нового:
1. **Auto compact** для AI-агента
2. **Compaction model** для AI-агента
3. **Commit message instructions** для AI-агента
4. **Model parameters** с настройкой temperature
5. **Agent profiles** (write, review, debug)
6. **Tool permissions** с always_allow и always_deny
7. **Global LSP settings**
8. **Настройки для Docker, YAML, SQL, Shell Script, INI**
9. **Настройки для REPL**
10. **Настройки для профилей (Presentation, Writing)**
11. **Расширенные настройки терминала**
12. **Расширенные настройки Git (branch_picker, hunk_style)**
13. **Настройки для файловых сканеров и типов файлов**
14. **Настройки для отладчика (save_breakpoints, button)**
15. **Расширенные настройки basedpyright (inlayHints, report*)**
16. **Настройки для Pyright и PyLSP**

---

## 35. Рекомендации по использованию

### Ежедневная работа:
1. Используйте `space a c` для открытия AI-агента
2. Используйте `space a i` для inline-ассистента
3. Используйте `space f f` для быстрого поиска файлов
4. Используйте `space g s` для открытия панели Git
5. Используйте `space s w` для поиска слова под курсором
6. Используйте `space x x` для просмотра всех диагностик

### Python-разработка:
1. Используйте `space c f` для форматирования
2. Используйте `space c r` для переименования символов
3. Используйте `space c a` для действий с кодом
4. Используйте `g d` для перехода к определению
5. Используйте `g r` для поиска ссылок
6. Используйте `] d` и `[ d` для навигации по диагностике

### AI-ассистент:
1. Используйте профиль "write" для написания кода
2. Используйте профиль "review" для код-ревью
3. Используйте профиль "debug" для отладки
4. Используйте `space a n` для создания нового потока
5. Используйте `space a m` для смены модели

---

## 36. Полезные ссылки

- [Официальная документация Zed](https://zed.dev/docs/)
- [Настройка Python в Zed](https://zed.dev/docs/languages/python)
- [Настройка AI в Zed](https://zed.dev/docs/ai/agent-settings)
- [Настройка языковых серверов](https://zed.dev/docs/configuring-languages)
- [Все настройки Zed](https://zed.dev/docs/reference/all-settings)
- [Репозиторий zed-101-setup](https://github.com/jellydn/zed-101-setup)
- [Документация Ruff](https://docs.astral.sh/ruff/)
- [Документация BasedPyright](https://github.com/detailyang/basedpyright)
- [Документация Ollama](https://ollama.com/)
