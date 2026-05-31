# LEARNING.md — конспект проекта Debris

> Заполняй после каждого модуля: 5–10 строк «что понял», 1–2 ссылки на docs UE 5.7, грабли если были.
> Этот файл — главный переносимый актив: при миграции на stable UE 5.8 обнови ссылки на документацию и зафиксируй грабли конверсии.

---

## Шаблон записи (копируй блок на каждый модуль)

### Модуль N — [название]

**Дата:**

**Что понял:**

- …

**Docs UE 5.7:**

- [Название](https://docs.unrealengine.com/5.7/…)

**Грабли:**

- …

---

## Модуль 0 — Фундамент

**Дата:**

**Что понял:**

- …

**Docs UE 5.7:**

- [Enhanced Input](https://docs.unrealengine.com/5.7/en-US/enhanced-input-in-unreal-engine/)
- [Game Mode and Game State](https://docs.unrealengine.com/5.7/en-US/game-mode-and-game-state-in-unreal-engine/)

**Грабли:**

- …

---

## Грабли (накопительный FAQ)

| Симптом | Решение |
|---------|---------|
| Персонаж не spawится | GameMode Override, DefaultPawnClass, PlayerStart |
| IMC не работает | AddMappingContext в BeginPlay, LocalPlayer subsystem |
| Первый выстрел фризит | PSO-кеш — см. development_plan.md модули 17, 13.24 |
| NPC не идёт | NavMeshBoundsVolume, Recast |
| Live Coding ломает UCLASS | Полная перекомпиляция |
| После миграции 5.7.4→5.8 | Conversion wizard, rebuild C++, PSO, NavMesh — см. development_plan.md «Миграция на UE 5.8» |

---

## Baseline производительности (`stat unit`)

| Модуль | Game | Draw | GPU | Заметки |
|--------|------|------|-----|---------|
| 0 | | | | |
| 1 | | | | |
| … | | | | |

---

## Архитектурные решения

- Проект: **Debris** · модуль `Debris` · префикс `Deb` · движок **UE 5.7.4** · IDE **VS 2022**
- Порядок: оружие (7) → hitscan (8) → destructibles (9) → NPC тело (10) → ИИ (11) → PCG (12)
- `UDebWeaponConfig` + `UDebHitscanSubsystem`; shotgun = `PelletCount` × spread
- Input: KB + геймпад с модуля 0; кривые стиков (1); aim assist (11/13)
- Perf: ≥ 60 FPS @ native 1440p (RTX 5070 / i5-14600KF / 32 GB DDR5); ≥ 60 FPS @ native 1080p (RTX 3060 / i5-12400F / 32 GB DDR4); Steam Deck preset `Deb_SteamDeck`
- Уровни: graybox → PCG demo (12) → Megascans FAB (18)

---

*См. полный чеклист: [`development_plan.md`](development_plan.md)*
