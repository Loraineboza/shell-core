# shell-core

`shell-core` — это полноценное ядро командной оболочки. Функционируя в нативной среде UNIX-подобные системы.Проект воплощает в себе низкоуровневую архитектуру и ключевые механизмы работы интерактивного терминала, обеспечивая полный цикл выполнения пользовательских команд

<img width="700" height="550" alt="demo" src="shell-demo.png" />

**Prompt**

Формат приглашения:

```text
[username@hostname cwd]$ 
```

## Сборка

### 1) Без `readline` (гарантированно работает)

```bash
cc shell.c -o shell
```

### 2) С `readline` (рекомендуется для полноценного интерактива)

```bash
cc -DUSE_READLINE shell.c -lreadline -o shell
```

Если `readline` установлен через Homebrew, используйте явные пути:

```bash
cc -DUSE_READLINE shell.c \
  -I"$(brew --prefix readline)/include" \
  -L"$(brew --prefix readline)/lib" \
  -lreadline -o shell
```

## Запуск

```bash
./shell
```

## Что поддерживает shell-core

- Внутренние команды:
  - `cd`, `pwd`, `export`, `unset`, `echo`, `exit`
  - `alias`, `history`, `type`, `which`
  - `kill`, `jobs`, `fg`, `bg`
  - `true`, `false`
- Внешние команды через `fork/exec` + поиск по `PATH`
- Операторы:
  - `;`, `&`, `&&`, `||`, `|`
- Перенаправления:
  - `>`, `>>`, `<`, `2>`, `2>>`, `2>&1`, `&>`, `>&`, `<<` (heredoc)
- Подстановки:
  - переменные: `$VAR`, `${VAR}`, `$?`, `$$`, `$!`
  - command substitution: `$(...)` и обратные кавычки `` `...` ``
- Кавычки и экранирование
- Globbing:
  - `*`, `?`, `[]`
- Job control:
  - фоновые задачи (`&`), `jobs`, `fg`, `bg`
- Сигналы:
  - `Ctrl+C`, `Ctrl+Z`, `Ctrl+D`
- История команд:
  - в режиме `USE_READLINE`: стрелки, `Ctrl+R`, Tab completion
  - без `USE_READLINE`: история хранится и доступна через builtin `history`
- Однострочные и многострочные команды

## Примеры

```bash
ls | grep ".c"
```

```bash
echo hello > out.txt
cat < out.txt
```

```bash
sleep 30 &
jobs
fg %1
```

```bash
cat << EOF
line 1
line 2
EOF
```

## История

По умолчанию файл истории:

```text
~/.myshell_history
```

## Ограничения

`shell-core` — учебно-практический shell. Он покрывает большой набор возможностей, но не является 100% клоном `bash/zsh` по всем POSIX-деталям.

## Файлы проекта

- `shell.c` — весь shell в одном C-файле
- `shell` - исполняемый файл
- `shell.png` - изображение демонстрации работы shell
- `README.md` — документация

