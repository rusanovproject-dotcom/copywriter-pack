# Copywriter knowledge — INDEX

> Карта базы знаний копирайтера. Обновляется при апдейте пака.

| Файл | Что внутри | Когда грузить |
|------|-----------|---------------|
| [craft.md](craft.md) | Универсальная механика текста: awareness-диагностика Schwartz, 6 типов лидов Great Leads, LF8, 3 правила Harry Dry, хук-паттерны, ритм/связность, reason-why, self-check | Перед DRAFT — всегда |
| [anti-slop-playbook.md](anti-slop-playbook.md) | 3-слойный анти-слоп RU: структурные паттерны ИИ · RU-канцелярит и русские AI-тики · критерий Willison «след кураторского усилия» | Перед чисткой и в CRITIQUE; база скилла `anti-slop` |
| [craft-rubric.md](craft-rubric.md) | A-F критик-гейт: 12 пунктов в 6 измерениях (голос · смысл · хук · ритм · слоп · платформа-fit), формула флаги→буква, evidence-grounding (флаг без цитаты недействителен) | Перед CRITIQUE — каждый текст; порог отдачи ≥ B |
| [leads-matrix.md](leads-matrix.md) | Матрица «5 вариантов»: оси лид-тип × LF8 × awareness, правило «вариант = другая ось», цикл evaluator-optimizer | Внутри `post-refine` — на VARIANTS |
| [voice-learning-method.md](voice-learning-method.md) | Методология обучения голосу: корпус → дистиллированный voiceprint (не голые few-shot), примеры без AI-полировки, режимы «свой автор»/«клиент» | Внутри `voice-learning` — при сборке/обновлении голоса |
| [platforms.md](platforms.md) | Цифры и механики платформ: Threads (ER, реплаи, экстраполяция — RU-данных нет), Telegram (лимиты, длина по прогретости, превью), лонгриды (дочитываемость, CTA, timing per-platform) | Жанровые скиллы `tg-post`/`threads`/`article`; вопрос «как лучше по платформе» |
| [arsenal.md](arsenal.md) | Индекс арсенала: 6 своих скиллов + опциональные MCP + внешние скиллы офиса (если установлены) | По запросу «под это есть приём»; подбор инструмента |
| [capabilities-map.md](capabilities-map.md) | Карта возможностей + онбординг «что умеешь» + fallback-матрица деградации без MCP | Первый контакт; нет MCP/голоса |
| [styles/INDEX.md](styles/INDEX.md) | Витрина 4 стилей-пресетов (universal · expert · provocative · storyteller) + механика дефолта и онбординга; файлы стилей рядом, грузить ПОСЛЕ выбора | Голос-резолюция упёрлась в «нет ни дефолта, ни voice.md»; «покажи стили», «смени стиль», пункт 6 развилки |
| [longread-architecture.md](longread-architecture.md) | Архитектура длинного текста 5-15К знаков: контракт с читателем, 6 блоков, reset-маркеры, callback, самопроверка | Внутри `article` — статьи/лонгриды/presell |

## Не в этой папке, но канон

- **Голос автора** — `knowledge/voice.md` (собирается скиллом `voice-learning`
  из 5-20 текстов автора; при первой установке файла нет — каркас для ручной
  сборки лежит в `../templates/voice.md.template`).
- **Память** — `../memory.md`, `../failures.md`, `../wins.md` (append-only;
  grep failures по теме перед задачей).
- **Скиллы** — `.claude/skills/` (6 штук: tg-post, threads, article,
  post-refine, anti-slop, voice-learning). Роутинг-триггеры — в `../core.md`.
