# Copywriter — install manifest

Машиночитаемые метаданные для установки пака через скилл `/install-agent copywriter`.

---

## Metadata

```yaml
agent_id: copywriter
agent_name_human: Копирайтер
agent_name_in_chat: Копирайтер
short_role: |
  Пишет посты, треды, лендинги, лонгриды, рассылки и рекламу
  в голосе автора (не в шаблонном AI-тоне). Перед первым текстом
  собирает голос через /voice-dna из 5-15 текстов автора. Без
  voice.md писать отказывается — иначе слоп.
trigger_keywords: [
  копирайтер, copywriter, напиши пост, пост в тг, пост телеграм,
  текст для канала, напиши текст, продающий текст, креатив,
  реклама, рекламный текст, баннер, таргет, лендинг, посадочная,
  посадочная страница, лонгрид, статья, presell, прогрев,
  продающая статья, рассылка, email, тред, threads, X пост,
  цепочка постов, контент-план, батч, N постов, фабрика контента,
  проверь текст, проверь виральность, ревью лонгрида, докрути
  лонгрид, обложка, визуал к тексту, voice dna, голос, собери мой
  голос, распакуй стиль, переберём голос, обнови голос
]
version: 1.0.4
requires: []
recommends: [alex-marketer]   # для маркетинговых текстов (audience analysis, core-offer, customer quotes из hub/marketing/)
provides_pipeline:
  - voice-dna       # сборка живого голоса автора (база перед любыми текстами)
  - stop-slop       # чистка AI-паттернов из черновика после написания
  - style-switch    # переписка в другом регистре (сканирует voice-*.md, предлагает 2-3, может несколько сразу)
  - smart-rewrite   # докрутка по 5 линзам (аватар / жирность / глубина / обогащение / сегментация)
  - angle-shift     # пересборка с нуля под одну ключевую идею (переворот / новый механизм / убеждение)
```

---

## Files to copy

Источник — `_agent-packs/copywriter/`, цель — `office/agents/copywriter/` и `.claude/skills/`.

```yaml
files:
  # Корневой файл агента — точка сборки (@core.md @overrides.md)
  - src: CLAUDE.md
    dest: office/agents/copywriter/CLAUDE.md
  # Ядро агента — роутинг, шаги, развилка после текста (обновляется при апдейте пакета)
  - src: core.md
    dest: office/agents/copywriter/core.md

  # Universal knowledge core — обновляется при апдейте пакета
  - src: knowledge/INDEX.md
    dest: office/agents/copywriter/knowledge/INDEX.md
  - src: knowledge/craft.md
    dest: office/agents/copywriter/knowledge/craft.md
  - src: knowledge/anti-patterns.md
    dest: office/agents/copywriter/knowledge/anti-patterns.md
  - src: knowledge/voice.md.template
    dest: office/agents/copywriter/knowledge/voice.md.template

  # Стартовые регистры-надстройки — копируются при первой установке,
  # при апдейте сохраняем правки ученика (preserve_if_exists)
  - src: knowledge/voice-universal.md
    dest: office/agents/copywriter/knowledge/voice-universal.md
    preserve_if_exists: true
  - src: knowledge/voice-provocative.md
    dest: office/agents/copywriter/knowledge/voice-provocative.md
    preserve_if_exists: true
  - src: knowledge/voice-expert.md
    dest: office/agents/copywriter/knowledge/voice-expert.md
    preserve_if_exists: true
  - src: knowledge/voice-longread.md
    dest: office/agents/copywriter/knowledge/voice-longread.md
    preserve_if_exists: true
  - src: knowledge/voice-storyteller.md
    dest: office/agents/copywriter/knowledge/voice-storyteller.md
    preserve_if_exists: true

  # Шаблоны памяти / правил — копируются один раз, при апдейте сохраняются
  - src: memory.md
    dest: office/agents/copywriter/memory.md
    preserve_if_exists: true
  - src: failures.md
    dest: office/agents/copywriter/failures.md
    preserve_if_exists: true
  - src: overrides.md
    dest: office/agents/copywriter/overrides.md
    preserve_if_exists: true
  - src: audience-map.md.template
    dest: office/agents/copywriter/audience-map.md
    preserve_if_exists: true   # карта ЦА: пустая при первой установке, копирайтер заполнит сам при первом маркетинговом запросе

  # Скиллы — ставятся в .claude/skills/ офиса
  - src: skills/
    dest: .claude/skills/
    recursive: true

# voice.md (личный голос автора) НЕ копируется из пакета —
# собирается через /voice-dna из 5-15 текстов автора при первом запуске.
```

---

## Updates to existing files

```yaml
updates:
  - file: CLAUDE.md
    section: "## Обязательный layered include при старте"
    add_line: "@office/agents/copywriter/core.md"

  - file: office/AGENTS.md
    section: "## Активная команда"
    add_row: |
      | **Копирайтер** | Пишет посты, треды, лендинги, лонгриды, рассылки, рекламу в голосе автора. Перед первым текстом собирает голос через `/voice-dna` из 5-15 текстов. Без собранного голоса — писать отказывается. После каждой итерации — развилка 1-5: почистить от слопа / докрутить по смыслам / сменить угол / сменить регистр / сохранить. | *«напиши пост», «лендинг», «лонгрид», «креатив», «собери мой голос»* |

  - file: office/agents/director/core.md
    section: "## Роутинг"
    add_rows: |
      | соберём голос, voice dna, распакуй стиль, переберём голос, обнови голос | **Копирайтер** → `/voice-dna` (точка входа: первый запуск или пересборка голоса) |
      | напиши пост, лендинг, лонгрид, креатив, рассылка, контент, серия постов, пост-история | **Копирайтер** (пишет напрямую по контексту voice.md + craft.md + anti-patterns.md + один из 5 регистров `voice-*.md`. Если voice.md ещё не собран — направь на `/voice-dna` сначала. После черновика — `/stop-slop` для чистки AI-паттернов) |
```

---

## Post-install message to client

```
✅ Копирайтер в команде.

Прежде чем он напишет первый текст — нужно собрать **твой голос**.
Без него любой пост будет звучать «нейтральным AI-стилем», что = слоп.

Скажи **«собери мой голос»** или **«voice dna»** — копирайтер попросит
5-15 твоих текстов (посты, длинные сообщения, расшифровки голосовых,
кейсы), за один заход соберёт камертон в `voice.md` и зафиксирует
твой стиль навсегда.

После голоса — пиши что нужно:
- **«напиши пост про X»** — Telegram-пост
- **«сделай лендинг для Y»** — текст посадочной
- **«напиши лонгрид-прогрев»** — продающая статья
- **«придумай 3 креатива для рекламы»** — рекламные тексты
- **«серию из 10 постов про Z»** — батч с проверкой
- **«пост-история про моего клиента»** — pre-sell в стиле сторител

Копирайтер пишет напрямую — по твоему `voice.md` + универсальной
механике текста + одному из 5 регистров под задачу. Никаких write-* скиллов
не нужно — база покрывает все типы.

**В пакете 5 скиллов:**
- `/voice-dna` — собрать твой голос из 5-15 текстов (запускается первым)
- `/stop-slop` — почистить черновик от AI-слопа
- `/style-switch` — переписать в другом регистре (или нескольких сразу для сравнения)
- `/smart-rewrite` — докрутить по 5 линзам (аватар / жирность / глубина / обогащение / сегментация)
- `/angle-shift` — пересобрать с нуля под другую ключевую идею

После каждого текста копирайтер сам предложит развилку 1-5:
- **1** — `/stop-slop` (почистить)
- **2** — `/smart-rewrite` (докрутить)
- **3** — `/angle-shift` (другой угол)
- **4** — `/style-switch` (другой регистр)
- **5** — сохранить как готовое

С порога у тебя **5 стартовых регистров** — общая упаковка тона,
поверх твоего личного голоса:

- **Универсальный** — практик в окопах (по умолчанию)
- **Разговорный, без цензуры** — сторителл-эссе на больших темах
- **Прожжённый эксперт** — разоблачение мифов, жёсткая позиция
- **Экспертный лонгрид** — pre-sell статьи, методички 5-15К знаков
- **Экспертный сторител** — pre-sell посты-истории про героя-зеркало

Свой регистр под нишу можно будет собрать в версии 1.1.0 (см. CHANGELOG → Roadmap). Пока хватит стартовых пяти.

Если работаешь с маркетинговыми текстами — **копирайтер один раз
просканирует офис** на файлы ЦА / цитат / оффера, покажет тебе что
нашёл, попросит подтвердить и **запомнит пути в `audience-map.md`**.
Дальше каждый раз молча читает оттуда — не спрашивает «где ЦА» по
второму кругу. Если поменялось — скажи «пересобери карту аудитории»,
пересканирует.

Сейчас — собирай голос. Это база для всего остального.
```

---

## Manual install (если в офисе нет команды `/install-agent`)

Если ты не используешь `client-office-template` или скилл `/install-agent` не установлен — пакет можно поставить руками. Терминал, из корня твоего AI-офиса:

```bash
# 1. Клонировать пак (если ещё не клонировал)
git clone https://github.com/rusanovproject-dotcom/copywriter-pack.git _agent-packs/copywriter

# 2. Создать папку агента
mkdir -p office/agents/copywriter/knowledge
mkdir -p .claude/skills

# 3. Скопировать файлы агента
cp _agent-packs/copywriter/CLAUDE.md         office/agents/copywriter/CLAUDE.md
cp _agent-packs/copywriter/core.md           office/agents/copywriter/core.md
cp _agent-packs/copywriter/knowledge/INDEX.md           office/agents/copywriter/knowledge/INDEX.md
cp _agent-packs/copywriter/knowledge/craft.md           office/agents/copywriter/knowledge/craft.md
cp _agent-packs/copywriter/knowledge/anti-patterns.md   office/agents/copywriter/knowledge/anti-patterns.md
cp _agent-packs/copywriter/knowledge/voice.md.template  office/agents/copywriter/knowledge/voice.md.template

# 4. Стартовые регистры (5 файлов) — копировать только если их ещё нет
for f in voice-universal voice-provocative voice-expert voice-longread voice-storyteller; do
  [ -f "office/agents/copywriter/knowledge/$f.md" ] || \
    cp "_agent-packs/copywriter/knowledge/$f.md" "office/agents/copywriter/knowledge/$f.md"
done

# 5. Шаблоны памяти — копировать только если их ещё нет
for f in memory failures overrides; do
  [ -f "office/agents/copywriter/$f.md" ] || \
    cp "_agent-packs/copywriter/$f.md" "office/agents/copywriter/$f.md"
done

# 5b. Карта ЦА (пустая при первой установке, копирайтер заполнит сам)
[ -f "office/agents/copywriter/audience-map.md" ] || \
  cp "_agent-packs/copywriter/audience-map.md.template" "office/agents/copywriter/audience-map.md"

# 6. Скиллы (рекурсивно)
cp -R _agent-packs/copywriter/skills/* .claude/skills/
```

После копирования — добавь в корневой `CLAUDE.md` своего офиса строку:

```
@office/agents/copywriter/core.md
```

В секцию обязательных layered include (где уже подключены остальные агенты).

И в `office/AGENTS.md` руками добавь строку про Копирайтера в активной команде (см. секцию `updates → office/AGENTS.md` в YAML выше).

После — `/voice-dna` для сборки голоса. Дальше пиши.
