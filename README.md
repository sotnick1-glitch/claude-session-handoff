# session-handoff

Ритуал сессии для Claude Code: открыться там, где остановились, и закрыться,
ничего не потеряв.

*(English below.)*

## Зачем

Контекст одной сессии конечен. Он кончается всегда — на середине правки, между
делом, без предупреждения. После этого начинается самое дорогое: следующая
сессия не знает ни что уже сделано, ни что сломано, ни почему полгода назад
решили именно так. Человек пересказывает это руками по двадцать минут, и с
каждым разом хуже, потому что сам забывает.

Цель ритуала: **обрыв на любом месте стоит не больше пяти минут.**

Никакой магии здесь нет. Это четыре текстовых файла, которые проект носит
в своём же git-репозитории.

## Из чего состоит

| Файл | Роль |
|---|---|
| `CLAUDE.md` в корне | грузится сам при старте сессии. Говорит, что прочитать, когда закрываться, и перечисляет 4–5 железных правил проекта |
| файл состояния (`STATUS.md`, `docs/handoff.md` — как принято у вас) | где остановились, что следующее, незакрытые хвосты, результат проверок. Перезаписывается целиком |
| `.claude/commands/handoff.md` | команда `/handoff` — закрытие сессии в пять шагов |
| `docs/session.md` и `docs/INDEX.md` | только большим проектам: процедура и оглавление «вопрос → файл» |

## Установка

В Claude Code:

```
/plugin marketplace add sotnick1-glitch/claude-session-handoff
/plugin install session-handoff@session-handoff
```

Дальше в любом проекте сказать «настрой ритуал сессии» (или `/session-handoff`).
Скилл сам посмотрит, что за проект, чем в нём прогоняются проверки, есть ли уже
файл состояния под другим именем, и разложит файлы по размеру проекта:
маленькому — три файла, большому — пять.

Плагин не обязателен: можно просто скопировать `skills/session-handoff/templates/`
себе в проект и заполнить `<...>` руками. Плагин отличается только тем, что
делает это за вас и не забывает подогнать размер.

## Пользоваться

- **Открыть сессию:** сказать «давай откроем сессию» в папке проекта.
- **Закрыть:** `/handoff`.

⚠️ `CLAUDE.md` подхватывается **только из той папки, в которой открыта сессия**.
Проект лежит в подпапке, а сессия открыта уровнем выше — файл не прочитается
и ритуал не сработает. Открывать сессию в самой папке проекта.

## Три решения, на которых всё держится

Их стоит перенести вместе с файлами, иначе система разваливается через месяц.

**Индекс — оглавление, а не пересказ.** Как только в нём заводится содержание,
он становится ещё одним документом, который надо читать целиком, и смысл
теряется. Новый факт кладётся в тематический файл, в индекс идёт строка-ссылка.

**Файл состояния — состояние, а не дневник.** Перезаписывается целиком.
История живёт в git, оттуда её всегда можно поднять. Дневник растёт, и через
двадцать сессий его не читает никто, включая Клода.

**Факт пишется сразу, а не «в конце сессии».** Конец может не наступить.
Незаписанный handoff при сжатии контекста теряется целиком — поэтому он
пишется до того, как доделана текущая правка.

## Чего здесь нет

Автоматики. Никаких хуков, демонов и фоновых записей: файлы пишет Клод по
команде, и вы видите каждое изменение в git-диффе. Так и задумано — система,
которая пишет сама, однажды запишет неправду, и вы узнаете об этом через месяц.

---

# session-handoff (English)

A session ritual for Claude Code: start where you stopped, close without losing
anything.

**The problem.** A context window always runs out — mid-edit, without warning.
The next session then knows nothing: what is done, what is broken, why a
decision was made that way six months ago. The goal of this ritual is that an
interrupt at any point costs no more than five minutes.

**What it is.** Four plain text files the project carries in its own git repo:
a `CLAUDE.md` that loads at session start and says what to read, a state file
that is overwritten whole every close, a `/handoff` command that closes the
session, and — for large projects — a procedure file and a "question → file"
index.

**Install:**

```
/plugin marketplace add sotnick1-glitch/claude-session-handoff
/plugin install session-handoff@session-handoff
```

Then say "set up the session ritual" in any project. Or skip the plugin and
copy `skills/session-handoff/templates/` by hand.

**Three rules that keep it alive:** the index is a table of contents, never a
summary; the state file is state, never a diary; a durable fact is written the
moment it is learned, never "at the end of the session" — the end may never
come.

**One gotcha:** `CLAUDE.md` is only picked up from the directory the session
was opened in. Open the session in the project folder itself.
