# План разработки — Debris

**Репозиторий:** https://github.com/lexusch/Debris

**Движок:** Unreal Engine **5.7.4** (основная разработка)  
**Миграция:** на **stable UE 5.8** после официального релиза — тот же проект `Debris`, без повторного «с нуля»

**IDE:** Visual Studio **2022** (проверенный toolchain; VS 2026 — не использовать до стабилизации)

**Проект:** `Debris` · модуль C++ `Debris` · API `DEBRIS_API` · префикс классов `Deb`

### Характер проекта

Весь план — **учебно-практический**: каждый модуль — освоение технологий UE и шаг к играбельному demo. Отдельных фаз «preview / production / после релиза» нет — один чеклист **0→18** на UE 5.7.4. Единственная метка scope — **`[Опционально]`**: пункт можно отложить без блокировки критерия готовности модуля.

---

## Видение

Иммерсивный, неторопливый, **грузный** FPS в **интерьере здания**. **1–4 NPC** охотятся на игрока — прятаться, выживать, искать расходники и полезные предметы, устранить угрозу и/или добраться до лифта. Перестрелки **редкие и ценные**. Минималистичный UI.

Продвинутая физика: пропы, разрушаемость, Control Rig Physics, hitscan с пробитием и рикошетом. Сложные физические объекты (прим. Навесной потолок, лампы, люстры, жалюзи, вращающиеся офисные кресла, офисные полки с выдвигающимися и отсреливаемыми ящиками). Продвинутая реакция на попадания пуль (объёмные отверстия от выстрелов, следы попаданий, множество адаптивных decals). Продвинутые материалы (вес, звук, следы разрушения, декаль, VFX), прим. Разрушающаяся плитка. Продвинутая процедурная анимация и совмещение физики и анимации для тел + использование новой ue системы physics control rig.

Игровой процесс: Игровой персонаж находится на процедурно-сгенерированном этаже здания, на котором его ищут 1-4 NPC. Задача игрока добраться до лифта. Лифт запускает экран загрузки; Генерируется новый PCG этаж; Цикл повторяется. Лифт нужно вызвать, дождаться пока он приедет, зайти в него, нажать внутри кнопку на панели, дождаться пока закроются двери. После этого начинается загрузка и генерация нового pcg уровня.
Стелс → вспышка боя → тишина.

**PCG** — неотъемлемая часть геймплея (процедурные layout этажей с интерьером и помещениями), но **после** основных механик → модуль 12.

**Направления обучения** (в рамках модулей): C++, Blueprint, Niagara, AI, Modeling, Animation, Sound, MetaSounds, UI, Chaos, Physics, Substrate (модуль 5), Control Rig (модуль 10), PCG (модуль 12), Metahuman.

---

## Маршрут разработки

Один проход модулей **0→18** на **UE 5.7.4**.

После каждого модуля: чекбоксы + запись в `[LEARNING.md](LEARNING.md)` · git-коммит `Debris: модуль N — …`

### Миграция на UE 5.8 (когда выйдет stable)

Справочный чеклист — не отдельная фаза плана:

1. Git-тег или ветка `pre-ue58-migration` на текущем состоянии 5.7.4.
2. Открыть копию проекта в установленном **stable UE 5.8**; выполнить conversion wizard движка.
3. Пересобрать C++ (Visual Studio 2022; `.sln` после регенерации проекта).
4. Smoke-тест: модули 0, 7–8, 11, 12, 17 (input, hitscan, AI, PCG, PSO).
5. Пересобрать PSO cache и NavMesh на PCG-карте.
6. Обновить ссылки в `LEARNING.md` с документации 5.7.4 на 5.8.
7. Зафиксировать грабли миграции в `LEARNING.md`; переносить **паттерны**, не слепой copy-paste кода с 5.7.4.

---

## Порядок модулей (0 → 18)


| #   | Модуль                                                  |
| --- | ------------------------------------------------------- |
| 0   | Фундамент: проект, input KB+геймпад, коллизии, GameMode |
| 1   | Перемещение, камера FPS, кривые стиков                  |
| 2   | Игровой каркас                                          |
| 3   | Здоровье и урон                                         |
| 4   | Взаимодействие                                          |
| 5   | Физматериалы, звук, Substrate                           |
| 6   | Физические пропы                                        |
| 7   | Оружие (базовый класс, конфиги, варианты)               |
| 8   | Hitscan и баллистика                                    |
| 9   | Разрушаемые объекты                                     |
| 10  | NPC: тело, Control Rig, ragdoll                         |
| 11  | NPC: perception, stealth, ИИ                            |
| 12  | PCG — процедурные интерьеры (часть геймплея)            |
| 13  | UI: HUD, меню, настройки, загрузка                      |
| 14  | Продвинутый звук (SFX)                                  |
| 15  | Динамический саундтрек                                  |
| 16  | Сохранения                                              |
| 17  | Производительность, PSO, отладка                        |
| 18  | Финальная полировка + Megascans (FAB)                   |


**Зависимости:** оружие (7) → hitscan (8) → destructibles (9) · тело NPC (10) → ИИ (11) · **основные механики (0–11) → PCG (12)** · polish (18) — **последний**.

---

## Graybox и production-уровни


| Этап   | Когда        | Карта                             | Наполнение                                                                                              |
| ------ | ------------ | --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **v0** | Модуль 0     | `L_Deb_Arena_v0`                  | Одна комната + коридор; `PlayerStart`; BSP/box; тест коллизий капсулы                                   |
| **v1** | Модуль 1–2   | `L_Deb_Building_Graybox`          | Несколько комнат, потолки под crouch/stand, укрытия разной высоты, 2–3 `PlayerStart` для быстрых тестов |
| **v2** | Модуль 2     | `L_Deb_Building_Graybox` (ручной) | Полный этаж вручную; `NavMeshBoundsVolume`; пустые spawn-точки NPC; **до PCG**                          |
| **v3** | Модуль 4, 6  | Graybox + зона `Props`            | Двери, кнопки, ящики; 5–10 физических пропов; зона grab/throw                                           |
| **v4** | Модуль 7–8   | `L_Deb_CombatLane`                | Полигон стрельбы; мишени с `IDebDamageable`; укрытия из пропов; тест pistol/SMG/shotgun                 |
| **v5** | Модуль 9     | CombatLane + destructible-секция  | Chaos GC стены/объекты; проверка hitscan → fracture                                                     |
| **v6** | Модуль 10–11 | Graybox (полный этаж)             | Patrol points, dark zones, choke points; 1–4 NPC; оружие на полу                                        |
| **v7** | Модуль 12    | `L_Deb_PCG_Demo`                  | PCG-layout как **основная demo-карта**; seed; NavMesh; spawner'ы из GameMode                            |
| **v8** | Модуль 18    | `L_Deb_Building_Final`            | **Quixel Megascans (FAB):** art pass поверх каркаса v7 или v6; Lumen; collision pass                    |


> Graybox-геометрию не выбрасывать — блокout остаётся под Megascans-слоями или как reference.

---

## Иерархия каталогов

### Content — `Content/Debris/`

```
Characters/
  Player/          BP_DebPlayer, AnimBP, mesh рук
  Enemies/         BP_DebEnemy, Physics Asset, Control Rig
Weapons/
  Base/            BP_DebWeaponBase, DA_DebWeaponConfig
  Pistol/          BP_DebWeapon_Pistol
  SMG/             BP_DebWeapon_SMG
  Shotgun/         BP_DebWeapon_Shotgun
AI/
  BehaviorTrees/   BT_DebHunter, BB_DebHunter
  Controllers/     BP_DebAIController
Combat/
  Hitscan/         DA_DebHitscanProfile_*
  Damage/          DA_DebDamageType_*
Interactables/     двери, кнопки, pickup
Props/             SM_Deb*, BP_DebPhysicalProp
Destructibles/     GC_Deb*, BP_DebDestructible
Maps/
  Graybox/         L_Deb_Arena_v0, L_Deb_Building_Graybox, L_Deb_CombatLane
  PCG/             L_Deb_PCG_Test, PCG_DebBuildingGraph
  Final/           L_Deb_Building_Final, L_Deb_MainMenu
Input/             IMC_Deb*, IA_Deb*
UI/                WBP_Deb*
Materials/         MI_Deb*, Substrate
PhysicalMaterials/ PM_Deb*
Megascans/         импорт FAB (модуль 18)
Audio/             SC_Deb*, SW_Deb*, MetaSounds
PCG/               PCG_Deb*, модульные SM_DebWall*
GameModes/         BP_DebGameMode
VFX/               NS_Deb*
```

### C++ — `Source/Debris/`

```
Debris.Build.cs
Debris.h / Debris.cpp

Public/
  Characters/
    DebPlayerCharacter.h
    DebEnemyCharacter.h
  Components/
    DebHealthComponent.h
    DebInteractionComponent.h
    DebPlayerVisibilityComponent.h
    DebNoiseEmitterComponent.h
    DebEnemyPhysicsRigComponent.h
  Interfaces/
    DebDamageable.h
    DebInteractable.h
    DebWeaponWielder.h
    DebFireable.h
  Weapons/
    DebWeaponBase.h
    DebWeaponConfig.h          // UDebWeaponConfig — Data Asset
    DebMagazineActor.h
    DebAmmoPickup.h
  Combat/
    DebHitscanSubsystem.h
    DebHitscanProfile.h
    DebHitscanTypes.h          // FDebHitscanRequest, FDebHitscanResult
    DebDamageTypes.h
  Interactables/
    DebInteractableActor.h
    DebPhysicalPropActor.h
    DebDestructibleActor.h
  AI/
    DebEnemyAIController.h
    DebTurretActor.h
    Tasks/                     // UBTTask_Deb* (при необходимости)
  UI/
    DebHUDWidget.h
    DebMainMenuWidget.h
    DebPauseMenuWidget.h
    DebSettingsWidget.h
    DebLoadingScreenWidget.h
  Audio/
    DebMusicSubsystem.h
  Game/
    DebGameMode.h
    DebGameInstance.h
    DebGameState.h
    DebPlayerController.h
    DebSaveGame.h
    DebWinCondition.h          // EDebWinCondition

Private/
  Characters/          … .cpp
  Components/          …
  Weapons/             …
  Combat/              …
  Interactables/       …
  AI/                  …
  UI/                  …
  Audio/               …
  Game/                …
```

**Правила:** один класс — один `.h` + `.cpp`; интерфейсы только в `Public/Interfaces/`; Data Asset конфиги — `Public/Weapons/` или `Public/Combat/`.

### Именование


| Тип        | Шаблон               | Пример                      |
| ---------- | -------------------- | --------------------------- |
| C++        | `A/U/F/E/I` + `Deb`  | `ADebWeaponBase`            |
| Blueprint  | `BP_Deb`             | `BP_DebGameMode`            |
| Widget     | `WBP_Deb`            | `WBP_DebHUD`                |
| Data Asset | `DA_Deb`             | `DA_DebWeaponConfig_Pistol` |
| Input      | `IMC_Deb` / `IA_Deb` | `IA_DebFire`                |
| Уровень    | `L_Deb_`             | `L_Deb_Building_Final`      |


### Чеклист нового класса

- Папка `Public/` + `Private/` по таблице выше
- Имя с префиксом `Deb`
- Зеркальный ассет в `Content/Debris/…`

---

## Коллизии (сквозная тема)


| Канал / preset                 | Назначение             | Модули  |
| ------------------------------ | ---------------------- | ------- |
| `ECC_Deb_Weapon` (trace)       | Hitscan, obstruction   | 0, 7, 8 |
| `ECC_Deb_Interactable` (trace) | Interaction line trace | 0, 4    |
| `ECC_Deb_Prop`                 | Физические пропы       | 0, 6    |
| `Deb_Pawn` (object)            | Игрок / NPC капсула    | 0, 1    |
| `Deb_Hitbox` (object)          | Bone hitboxes NPC      | 10      |
| `Deb_Destructible` (object)    | Chaos GC meshes        | 9       |
| `BlockAll` / `OverlapOnlyPawn` | Стены graybox          | 0       |


- [ ] **COL.0.** [C++/Редактор] Модуль 0: завести trace-каналы и collision presets в `DefaultEngine.ini` / Project Settings
- [ ] **COL.1.** [C++/Редактор] Модуль 0: таблица «кто с кем блокируется / overlap» → `LEARNING.md`
- [ ] **COL.2.** [C++] Модуль 4: interaction trace только на `ECC_Deb_Interactable`
- [ ] **COL.3.** [C++] Модуль 6: пропы — `ECC_Deb_Prop`, корректный `CollisionEnabled` при grab/sleep
- [ ] **COL.4.** [C++] Модуль 7–8: obstruction и hitscan — отдельные trace channels; ignore owner/wielder
- [ ] **COL.5.** [C++/Редактор] Модуль 10: Physics Asset NPC — hitbox bones block `ECC_Deb_Weapon`, не блокируют pawn movement
- [ ] **COL.6.** [C++/Редактор] Модуль 9: destructible — корректная collision после fracture; debris не блокирует pawn навсегда
- [ ] **COL.7.** [C++] Модуль 12: NavMesh + collision после PCG generation; rebuild при смене seed
- [ ] **COL.8.** [Редактор] Модуль 18: Megascans mesh — simplified collision; без дыр в стенах

---

## Архитектура (ключевые решения)


| Система                  | Решение                                                                                   |
| ------------------------ | ----------------------------------------------------------------------------------------- |
| Оружие                   | `ADebWeaponBase` + `UDebWeaponConfig` (Data Asset); дочерние BP: pistol / SMG / shotgun   |
| Hitscan                  | **После оружия:** `UDebHitscanSubsystem::ExecuteHitscan()`; профиль из `UDebWeaponConfig` |
| Shotgun                  | `FireMode = Shotgun`; `PelletCount` × spread → несколько `ExecuteHitscan` за один выстрел |
| Обоймы                   | `CurrentMagAmmo` / `ReserveAmmo`; `AMagazineActor` при reload                             |
| Perception               | Origin на `head`; trace к Head + Body (OR)                                                |
| NPC                      | **Сначала** тело + CR/ragdoll (10), **потом** BT и stealth (11)                           |
| Destructibles            | **После hitscan (8)** — модуль 9                                                          |
| PCG                      | **После механик (0–11)** — модуль 12; seed-driven demo                                    |
| Input                    | KB + **геймпад с модуля 0**; кривые стиков (1); aim assist (11/13)                        |
| PSO                      | Сбор в dev; runtime-прогрев на loading screen (13, 17)                                    |
| Perf (Основной ПК)       | ≥ 60 FPS @ **1440p** — RTX 5070, i5-14600KF, 32 GB DDR5; пресет `Deb_High1440p`           |
| Perf (Дополнительный ПК) | ≥ 60 FPS @ **1080p** — RTX 3060, i5-12400F, 32 GB DDR4; пресет `Deb_High1080p`            |
| Perf (Протативный ПК)    | Steam Deck, пресет `Deb_SteamDeck`; ≥ 30 FPS (цель 40+, в идеале 60–90)                   |
| Арт уровней              | Graybox → PCG demo → FAB Quixel Megascans (18)                                            |


---

## Модуль 0: Фундамент

**Цель:** проект Debris, персонаж, input KB+геймпад, коллизии, GameMode, graybox v0.

### Проект

- [x] **0.1.** [Редактор] C++ проект **Debris**, модуль `Debris` (При создании проекта: Blank проект, scalable графика, C++ пресет)
- [ ] **0.2.** Структура каталогов C++ и `Content/Debris/` (см. раздел выше)
- [ ] **0.3.** [C++] `Debris.Build.cs`: `EnhancedInput`, `InputCore`
- [x] **0.4.** [Git] `.gitignore` по расширенному списку (эталон: `Debris_UE_pre5_8/.gitignore` + дополнения ниже); проверить `git status` — в индексе только `Content/`, `Config/`, `Source/`, `*.uproject`, docs
  - **UE:** `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/`, `Build/*`
  - **IDE:** `.vs/`, `.vscode/`, `.idea/`
  - **Visual Studio:** `*.VC.db`, `*.VC.opendb`, `*.suo`, `*.opensdf`, `*.sdf`, `.vsconfig`
  - **Генерируемое:** `*.sln`, `*.slnx`, `*.xcodeproj`, `*.xcworkspace`, `Automation_*.slnx`, `shadertoolsconfig.json`
  - **Плагины/tools:** `Plugins/VisualStudioTools/`
  - **Прочее:** `*.log`, `*.dmp`, `*.tmp`
  - **Дополнительно:** `Saved/Autosaves/`, `Saved/Collections/`, `Saved/Config/Windows/EditorPerProjectUserSettings.ini`, `Content/Developers/`, `.DS_Store`, `Thumbs.db`

### Коллизии

- [ ] **0.5.** [C++/Редактор] Trace-каналы и object types (см. COL.0–COL.1)
- [ ] **0.6.** [Редактор] Collision presets: `Deb_Pawn`, `Deb_WorldStatic`

### Input — клавиатура + геймпад

- [ ] **0.7.** [C++] `ADebPlayerCharacter`: капсула, movement
- [ ] **0.8.** [Редактор] IA: `IA_DebMove`, `IA_DebLook`, `IA_DebJump` (Vector2D / Digital)
- [ ] **0.9.** [Редактор] `IMC_DebDefault`: **KB** — WASD, мышь, Space
- [ ] **0.10.** [Редактор] `IMC_DebDefault`: **геймпад** — Left Stick Move, Right Stick Look, A/Cross Jump, RT/R2 — задел Fire (модуль 7)
- [ ] **0.11.** [C++] `AddMappingContext`; `BindAction` Move / Look / Jump
- [ ] **0.12.** [C++] Обработка `Look`: и мышь, и правый стик (Enhanced Input Vector2D)
- [ ] **0.13.** [Blueprint] `BP_DebPlayer`

### GameMode

- [ ] **0.14.** [C++] `ADebGameMode`, `ADebPlayerController`
- [ ] **0.15.** [Blueprint] `BP_DebGameMode`
- [ ] **0.16.** [Редактор] `GlobalDefaultGameMode`; `GameDefaultMap = L_Deb_Arena_v0`
- [ ] **0.17.** [Редактор] PIE: spawn `BP_DebPlayer` через `PlayerStart`

### Graybox v0

- [ ] **0.18.** [Редактор] `L_Deb_Arena_v0`: комната + коридор; проверка коллизий и геймпада
- [ ] **0.19.** Отладка: `#define DEBUG_TRACES`
- [ ] **0.20.** [Редактор] PSO — задел к модулю 17
- [ ] **0.21.** `[LEARNING.md](LEARNING.md)` — первая запись

**Критерий готовности:** PIE на v0; WASD + мышь + геймпад (move/look/jump); коллизии работают.

---

## Модуль 1: Перемещение и камера FPS

- [ ] **1.1.** [C++/Редактор] Камера FPS; mesh рук, сокет `GripPoint`
- [ ] **1.2.** [Редактор] First Person Rendering
- [ ] **1.3.** [C++] `bUseControllerRotationYaw/Pitch`
- [ ] **1.4.** [C++] Спринт / присед + ceiling check
- [ ] **1.5.** [C++] «Грузное» движение: инерция, умеренная скорость
- [ ] **1.6.** [Редактор] **Кривые стиков:** `Curve_DebMoveResponse`, `Curve_DebLookResponse` — dead zone, чувствительность
- [ ] **1.7.** [C++] Применение curve к Look/Move для геймпада (масштаб vs raw input)
- [ ] **1.8.** [C++] Задел `WeaponObstructionTrace` → модуль 7
- [ ] **1.9.** [C++] `UDebPlayerVisibilityComponent`: Head, Body
- [ ] **1.10.** [Опционально] Покачивание камеры

**Graybox:** расширить до `L_Deb_Building_Graybox` — комнаты, потолки, укрытия.

**Критерий готовности:** sprint/crouch; геймпад с кривыми; visibility points.

---

## Модуль 2: Игровой каркас

- [ ] **2.1.** [C++] `ADebGameInstance`
- [ ] **2.2.** [C++] `ADebGameMode` → `GameStateClass = ADebGameState`
- [ ] **2.3.** [C++] `ADebGameState`: `bAlertActive`, `AliveHunterCount`, `EDebWinCondition`
- [ ] **2.4.** [C++] `ADebPlayerController`: чувствительность мыши **и геймпада**; `SetInputMode`
- [ ] **2.5.** [C++] `RestartPlayer` (задел)
- [ ] **2.6.** [Blueprint] `BP_DebGameMode` — параметры уровня

**Критерий готовности:** GameInstance → GameMode → GameState → Controller → Pawn.

---

## Модуль 3: Здоровье и урон

- [ ] **3.1.** [C++] `IDebDamageable`
- [ ] **3.2.** [C++] `UDebHealthComponent`
- [ ] **3.3.** [C++] `FDamageInfo`: урон, тип, bone multiplier
- [ ] **3.4.** [C++] `ApplyPointDamage` — единая точка входа
- [ ] **3.5.** [Опционально] Эффекты ранения

**Graybox:** тестовая мишень-актор на `L_Deb_CombatLane` (задел).

---

## Модуль 4: Взаимодействие

- [ ] **4.1.** [C++] `IDebInteractable`
- [ ] **4.2.** [C++] `UDebInteractionComponent` — trace `ECC_Deb_Interactable`
- [ ] **4.3.** [C++] `ADebInteractableActor`
- [ ] **4.4.** [C++/Blueprint] Дверь, кнопка, ящик
- [ ] **4.5.** [C++] Заготовка подсказки (UI — модуль 13)
- [ ] **4.6.** [Редактор] Input: **E / X (геймпад)** — Interact в `IMC_DebDefault`

**Graybox:** 2–3 interactables на Graybox.

---

## Модуль 5: Физматериалы, звук, Substrate

**Цель:** физматериалы, звук шагов, знакомство с Substrate.

### Физматериалы и звук

- [ ] **5.1.** [Редактор/C++] `UPhysicalMaterial`: металл, дерево, бетон, плоть, стекло
- [ ] **5.2.** [C++] Шаги: trace вниз → `PlayFootstepSound`; различать sprint / walk
- [ ] **5.3.** [Редактор] Sound Cue для каждой поверхности
- [ ] **5.4.** [C++] Звук приземления
- [ ] **5.5.** [Опционально] `UDataTable`: материал → звук, декаль, VFX

### Substrate

- [ ] **5.6.** [Редактор] Включить Substrate; 1–2 tutorial-материала
- [ ] **5.7.** [Редактор] Substrate-материал для graybox (бетон / штукатурка / металл)
- [ ] **5.8.** [Редактор] Substrate ↔ `UPhysicalMaterial` для шагов
- [ ] **5.9.** [Опционально] Production-библиотека материалов (или доработка в модуле 18)

**Graybox:** разметить зоны с разными physmat на Graybox для теста шагов.

**Критерий готовности:** шаги различаются по поверхности; 1–2 Substrate-материала.

---

## Модуль 6: Физические пропы

- [ ] **6.1.** [C++] `ADebPhysicalPropActor` — `ECC_Deb_Prop`
- [ ] **6.2.** [C++] `UPhysicsHandleComponent` — grab/drop/throw
- [ ] **6.3.** [C++] Physics sleep / wake; tick off в sleep; `OnComponentSleep` / `WakeAllRigidBodies`
- [ ] **6.4.** [C++] Звуки столкновений: громкость и pitch от импульса + physmat
- [ ] **6.5.** [Редактор] Input: grab — **ЛКМ / RT**; drop — **R / B**; throw — импульс по взгляду
- [ ] **6.6.** [C++] Коллизия при grab: kinematic hold vs simulate; не застревать в стенах
- [ ] **6.7.** [Опционально] Классы веса
- [ ] **6.8.** [Опционально] Штабелирование

**Graybox:** зона Props (5–10 объектов) на Graybox.

**Критерий готовности:** поднять, нести, бросить; сон/пробуждение; звук удара.

---

## Модуль 7: Оружие

**Цель:** базовый класс и **управляемые параметры** для дочерних экземпляров. Hitscan — **следующий модуль**.

### Базовый класс и wielder

- [ ] **7.1.** [C++] `IDebWeaponWielder`: `GetMuzzleSocket()`, `GetEjectSocket()`, `GetGripSocket()`, `CanUseWeapon()`
- [ ] **7.2.** [C++] `ADebWeaponBase`: attach, drop, reload — **без** line trace
- [ ] **7.3.** [C++] `AttachToWielder(IDebWeaponWielder*)` → сокет `GripPoint`

### UDebWeaponConfig (Data Asset)

- [ ] **7.4.** [C++] `UDebWeaponConfig`: `FireMode` (Single / Auto / Burst / **Shotgun**)
- [ ] **7.5.** [C++] Параметры стрельбы (для модуля 8): `BaseDamage`, `MaxRange`, `SpreadAngle`, `PelletCount`, `PelletSpread`, `RateOfFire`, `BurstCount`
- [ ] **7.6.** [C++] Параметры пробития: `PenetrationPower`, `MaxPenetrations`, `RicochetChance`, `MaxRicochets`
- [ ] **7.7.** [C++] `FAmmoConfig`: `MagazineSize`, `MaxReserveAmmo`, `ReloadDuration`
- [ ] **7.8.** [C++] VFX/SFX refs: muzzle flash, eject, fire sound, reload sound, dry fire
- [ ] **7.9.** [C++] `CurrentMagAmmo`, `ReserveAmmo`, `bIsReloading`; dry fire click

### Дочерние варианты (Blueprint)

- [ ] **7.10.** [Blueprint] `BP_DebWeapon_Pistol` — Single, низкий spread, `DA_DebWeaponConfig_Pistol`
- [ ] **7.11.** [Blueprint] `BP_DebWeapon_SMG` — Auto, высокий RoF, `DA_DebWeaponConfig_SMG`
- [ ] **7.12.** [Blueprint] `BP_DebWeapon_Shotgun` — Shotgun, `PelletCount` 8–12, широкий `PelletSpread`
- [ ] **7.13.** [Редактор] AnimMontage reload + AnimNotify `OnMagazineInserted`

### Обоймы, эффекты, инвентарь

- [ ] **7.14.** [C++] `AMagazineActor` — physics drop при reload; коллизия на полу
- [ ] **7.15.** [Редактор/C++] Niagara гильзы на `Eject` (анимация выстрела без урона)
- [ ] **7.16.** [C++] `FItemData`, `UInventoryComponent`; подбор / drop / swap; NPC — `CurrentWeapon`
- [ ] **7.17.** [C++] Input: Fire **ЛКМ / RT**; Reload **R / X**; swap **1-2-3 / D-Pad**
- [ ] **7.18.** [C++] Obstruction: связать 1.8 с mesh; AnimBP `ObstructionAlpha`
- [ ] **7.19.** [C++] Отдача и визуальный spread (без урона до модуля 8)
- [ ] **7.20.** [C++] `IFireable` / `UFireComponent` — задел для NPC (модуль 11)
- [ ] **7.21.** [Опционально][C++] ADS
- [ ] **7.22.** [Опционально][C++] Weapon sway
- [ ] **7.23.** [C++] Пул декалей (активируется в модуле 8)

**Graybox:** создать `L_Deb_CombatLane` — оружие, reload, gating, **без урона**.

**Критерий готовности:** pistol/SMG/shotgun configs; аним/VFX/ammo; hitscan не подключён.

---

## Модуль 8: Hitscan и баллистика

**Цель:** подключить к **готовому** `ADebWeaponBase` + `UDebWeaponConfig`. Тестировать на экземплярах из модуля 7.

### Подсистема hitscan

- [ ] **8.1.** [C++] `UDebHitscanProfile` и/или поля в `UDebWeaponConfig`
- [ ] **8.2.** [C++] `FDebHitscanRequest` / `FDebHitscanResult` — цепочка попаданий
- [ ] **8.3.** [C++] `UDebHitscanSubsystem::ExecuteHitscan()` — луч → `ApplyPointDamage` → пробитие → рикошет → повтор
- [ ] **8.4.** [C++] `UPhysicalMaterial`: `PenetrationCost`, `bCanRicochet`
- [ ] **8.5.** [C++] `ADebWeaponBase::Fire()` → config → subsystem
- [ ] **8.6.** [C++] **Shotgun:** `PelletCount` × trace со `SpreadAngle` / `PelletSpread`
- [ ] **8.7.** [C++] **Auto/Burst:** timer / gate по `RateOfFire`, `BurstCount`
- [ ] **8.8.** [C++] Эффекты попадания: Niagara, декаль по physmat
- [ ] **8.9.** [C++] `IDebFireable`; игрок и NPC — один путь
- [ ] **8.10.** [C++] Интеграция с `IDebDamageable` / `UDebHealthComponent` (модуль 3)
- [ ] **8.11.** [C++] Тестовые мишени + консольная команда `DebugFire`
- [ ] **8.12.** [C++] Debug: DrawDebugLine сегментов trace; hit sphere

**Graybox:** полный цикл на `L_Deb_CombatLane` — pistol, SMG, shotgun.

**Критерий готовности:** три типа оружия наносят урон; shotgun — несколько лучей; пробитие/рикошет.

---

## Модуль 9: Разрушаемые объекты

**Цель:** Chaos GC **после** рабочего hitscan (модуль 8).

- [ ] **9.1.** [C++/Редактор] `ADebDestructibleActor` (Chaos Geometry Collection)
- [ ] **9.2.** [C++] Пороги урона; накопление damage до fracture
- [ ] **9.3.** [C++] `ExecuteHitscan` → `ApplyPointDamage` → GC fracture
- [ ] **9.4.** [C++] Collision после fracture: debris `Deb_Destructible`; COL.6
- [ ] **9.5.** [C++] VFX/SFX разрушения по physmat (дым, осколки, звук)
- [ ] **9.6.** [Опционально] Опора: разрушение A → падение B
- [ ] **9.7.** [C++] Очистка / sleep обломков (оптимизация, модуль 17)

**Graybox:** destructible-секция на `L_Deb_CombatLane` (v5).

**Критерий готовности:** выстрел ломает объект; осколки падают; эффекты по материалу; hitscan триггерит fracture.

---

## Модуль 10: NPC — тело, Control Rig, ragdoll

**Цель:** физическое «тело» NPC **до** логики ИИ (модуль 11).

- [ ] **10.1.** [C++] `ADebEnemyCharacter` + `UDebHealthComponent` + `IDebDamageable`
- [ ] **10.2.** [Редактор/C++] Physics Asset: голова, торс, конечности → множители; **COL.5**
- [ ] **10.3.** [C++] `TakeDamage` / `ReceivePointDamage` по bone name
- [ ] **10.4.** [C++] `IDebWeaponWielder` — NPC держит `ADebWeaponBase`
- [ ] **10.5.** [Редактор] Control Rig: physics nodes
- [ ] **10.6.** [Редактор] Mass, damping, angular limits по сегментам
- [ ] **10.7.** [C++] `UDebEnemyPhysicsRigComponent`: `ApplyHitImpulse`, `ApplyRadialImpulse`, `EnterRagdoll`, `BlendToAnimated`
- [ ] **10.8.** [C++] Hit react / stagger → blend обратно в animated
- [ ] **10.9.** [C++] Столкновение с `ADebPhysicalPropActor` → импульс
- [ ] **10.10.** [C++] `EPhysicsBlendState`: Animated / HitReact / Stagger / Ragdoll
- [ ] **10.11.** [C++] Ragdoll on death; контекстный импульс от `FDamageInfo` (направление, кость)
- [ ] **10.12.** [Опционально] Death-poses в Control Rig
- [ ] **10.13.** [C++] Ragdoll sleep / фиксация pose через N сек
- [ ] **10.14.** [Опционально][C++] `UPhysicalAnimationComponent` fallback, если Control Rig Physics на 5.7.4 ограничен

**Graybox:** dummy NPC на CombatLane — стрельба → hit zones → ragdoll; **без BT**.

**Критерий готовности:** тело реагирует на попадания и пропы; ragdoll с limits или fallback.

---

## Модуль 11: NPC — perception, stealth, ИИ

**Цель:** поведение **после** готового тела (10).

### Сценарий «охота в здании»

- [ ] **11.0.** [C++/Редактор] Spawner / GameMode: 1–4 NPC; патруль, поиск
- [ ] **11.0b.** [C++] Blackboard: Idle / Search / Chase / Combat; акцент на **Search**

### Базовый ИИ

- [ ] **11.1.** [C++] `ADebEnemyAIController` + Behavior Tree
- [ ] **11.2.** [Редактор] BT-Tasks: MoveTo, Wait, ShootAtTarget
- [ ] **11.3.** [C++] NPC `Fire()` → `ADebWeaponBase` + hitscan (8)
- [ ] **11.4.** [C++/Blueprint] `ADebTurretActor` через `IFireable`
- [ ] **11.5.** [C++] Perception для `ADebWeaponBase` / `AMagazineActor` (tag `WeaponPickup`)
- [ ] **11.6.** [C++] Blackboard `SpottedWeapon`; BT Service сканирует оружие
- [ ] **11.7.** [Редактор] BT-Tasks: FindWeapon, MoveToWeapon, PickUpWeapon
- [ ] **11.8.** [C++] NPC без оружия → найти; низкий mag → reload / смена
- [ ] **11.9.** [C++] Блокировка BT во время Stagger (модуль 10)
- [ ] **11.10.** [Опционально] EQS укрытия
- [ ] **11.11.** [Опционально] Тревога между группой NPC

### Perception — зрение и слух

- [ ] **11.12.** [C++/Редактор] Sight origin на кости `head`; sync transform
- [ ] **11.13.** [C++] LineTrace к Head + Body; OR-логика
- [ ] **11.14.** [Редактор] Параметры sight: угол, дальность, lose-sight, stale time
- [ ] **11.15.** [C++] Интервал perception 0.1–0.2 с; stagger между NPC
- [ ] **11.16.** [C++] `UDebNoiseEmitterComponent`: Walk / Sprint / Land / Fire
- [ ] **11.17.** [C++] `UAISense_Hearing`: sprint громче walk
- [ ] **11.18.** [C++] Выстрел → noise + `bAlertActive` в `GameState`
- [ ] **11.19.** [C++] Crouch снижает noise; partial cover через props
- [ ] **11.20.** [Опционально] Зоны тьмы
- [ ] **11.21.** [C++] Debug: `showdebug ai`, cone, линии к точкам; подсветка оружия

### Геймпад

- [ ] **11.22.** [Опционально][C++] Aim assist (только геймпад): magnetism / slowdown
- [ ] **11.23.** [C++] Настройки aim assist → Settings (модуль 13)
- [ ] **11.24.** [C++] Playtest 15 мин → `LEARNING.md`

**Graybox:** полный этаж (v6) — 1–4 охотника, stealth, перестрелка.

**Критерий готовности:** NPC патрулирует, ищет, стреляет, подбирает оружие; stealth; win condition.

---

## Модуль 12: PCG — процедурные интерьеры

**Цель:** PCG — **неотъемлемая часть геймплея** (layout здания, replay seed), но **после** механик 0–11.

### Граф и контент

- [ ] **12.1.** [Редактор] PCG plugin в `Build.cs`; graph: grid / spline → пол + стены
- [ ] **12.2.** [Редактор] Модульные mesh: `SM_DebWall`, проём, угол, лестница
- [ ] **12.3.** [Редактор] Правила: мин. комната, связность коридоров, высота под crouch
- [ ] **12.4.** [Редактор] Collision-ready static meshes; **COL.7** NavMesh rebuild

### Интеграция с геймплеем

- [ ] **12.5.** [C++/Редактор] `L_Deb_PCG_Demo` — **основная demo-карта** после ручного Graybox
- [ ] **12.6.** [C++] Seed в `ADebGameMode` / `ADebGameState` — регенерация layout
- [ ] **12.7.** [Редактор] PCG spawns: `PlayerStart`, NPC spawn, weapon pickup, patrol spline points
- [ ] **12.8.** [C++] `ADebGameMode`: параметры сложности (число комнат, 1–4 NPC) → PCG graph
- [ ] **12.9.** [C++] После PCG generate — validate NavMesh, patrol, `AliveHunterCount`
- [ ] **12.10.** [Редактор] Сравнить PCG vs ручной Graybox; PCG — default для demo
- [ ] **12.11.** [Опционально][C++] Runtime regenerate по seed (dev menu)
- [ ] **12.12.** [Опционально] Production PCG-граф; процедурные варианты здания в shipping

**Критерий готовности:** seed-driven здание; NavMesh; 1–4 NPC работают; полный stealth-combat loop на PCG-карте.

---

## Модуль 13: UI — HUD, меню, настройки, загрузка

**Цель:** полный UI-слой: игра, пауза, главное меню, настройки KB+геймпад, экран загрузки.

### HUD (в игре)

- [ ] **13.1.** [C++] Подключить UMG в `Build.cs`
- [ ] **13.2.** [C++/Редактор] `UDebHUDWidget`: прицел, патроны (`30 / 120`), здоровье, подсказка взаимодействия, индикатор перезарядки
- [ ] **13.3.** [C++] Подписка на делегаты здоровья, инвентаря, взаимодействия
- [ ] **13.4.** [C++] Индикатор направления урона
- [ ] **13.5.** [Опционально] Маркер попадания
- [ ] **13.6.** [C++] Экран смерти: рестарт / загрузка

### Главное меню

- [ ] **13.7.** [C++/Редактор] `UDebMainMenuWidget`: Новая игра, Продолжить, Настройки, Выход
- [ ] **13.8.** [C++] Уровень `L_Deb_MainMenu`
- [ ] **13.9.** [C++] `ADebGameInstance`: переход MainMenu ↔ GameLevel / PCG demo

### Меню паузы

- [ ] **13.10.** [C++/Редактор] `UDebPauseMenuWidget`: Продолжить, Настройки, В главное меню, Выход
- [ ] **13.11.** [C++] `SetGamePaused`, `SetInputMode UIOnly`, курсор
- [ ] **13.12.** [C++] `IA_DebPause`: **Esc / Start (геймпад)**

### Настройки

- [ ] **13.13.** [C++/Редактор] `UDebSettingsWidget` с вкладками
- [ ] **13.14.** **Звук:** Master / SFX / Music / UI; Sound Mix
- [ ] **13.15.** **Графика:** Scalability; пресеты `**Deb_High1440p`** (reference PC) и `**Deb_SteamDeck**`; разрешение, VSync, FOV
- [ ] **13.16.** **Управление:** чувствительность **мыши и правого стика**; инверсия Y; выбор **кривых стиков** (`Curve_DebMove/Look`); aim assist on/off (модуль 11)
- [ ] **13.17.** [Опционально] Переназначение клавиш / кнопок геймпада
- [ ] **13.18.** [C++] Применить / Сброс / Назад → `GameInstance` и `SaveGame`

### Экран загрузки

- [ ] **13.19.** [C++/Редактор] `UDebLoadingScreenWidget`: прогресс-бар, статус («Загрузка…», «Компиляция шейдеров…»)
- [ ] **13.20.** [C++] Логика в `GameInstance`: виджет до завершения загрузки
- [ ] **13.21.** [C++] Асинхронная загрузка + `GetAsyncLoadPercentage()`
- [ ] **13.22.** [C++] Переходы: меню → уровень; A → B; загрузка save (16); PCG seed regen
- [ ] **13.23.** [C++] Минимальное время показа (anti-flicker)
- [ ] **13.24.** [C++] Прогресс `FShaderPipelineCache` (модуль 17)
- [ ] **13.25.** [Опционально] Советы / lore на экране загрузки

**Критерий готовности:** меню → игра через loading screen; Esc/Start — пауза; настройки KB+геймпад сохраняются.

---

## Модуль 14: Продвинутый звук (SFX)

**Цель:** от базовых шагов (модуль 5) к физически правдоподобному звуку.

- [ ] **14.1.** [Редактор] Sound Mix и Sound Class: Master, SFX, Music, UI, Voice
- [ ] **14.2.** [C++] Связь настроек (модуль 13) с Sound Mix
- [ ] **14.3.** [C++] Attenuation: falloff, spatial для выстрелов и взрывов
- [ ] **14.4.** [C++] Occlusion / obstruction: LineTrace + LPFilter за стенами
- [ ] **14.5.** [C++] Звук оружия послойно: near/far tail, reload, dry fire
- [ ] **14.6.** [C++] Процедурные звуки ударов пропов (улучшение 6.4)
- [ ] **14.7.** [Редактор] MetaSounds для попаданий по поверхностям
- [ ] **14.8.** [Blueprint/C++] `AAudioVolume` — реверберация и эмбиент
- [ ] **14.9.** [Опционально] Слух NPC (дублирует 11.17)

**Критерий готовности:** выстрелы с затуханием и occlusion; громкость из настроек.

---

## Модуль 15: Динамический саундтрек

**Цель:** музыка реагирует на состояние игры.

### Архитектура

- [ ] **15.1.** [C++] `UDebMusicSubsystem` — менеджер музыки
- [ ] **15.2.** [C++] `EDebMusicState`: MainMenu, Exploration, Tension, Combat, Death, Victory …
- [ ] **15.3.** [C++] Подписка на события: `OnEnemySpotted`, `OnCombatStarted`, `OnCombatEnded`, `GameState`

### Слои и переходы

- [ ] **15.4.** [Редактор] MetaSounds / слои на состояние (base + percussion)
- [ ] **15.5.** [C++] Логика слоёв (например, percussion при `EnemiesInCombat > 0`)
- [ ] **15.6.** [C++] Crossfade / `FInterpTo` при смене состояния
- [ ] **15.7.** [C++] Fade In / Fade Out при входе / паузе / смерти
- [ ] **15.8.** [C++] Приоритеты: Combat > Tension > Exploration
- [ ] **15.9.** [C++] Cooldown / hysteresis для Combat ends

### Контент и отладка

- [ ] **15.10.** [Редактор] Placeholder-музыка по состояниям
- [ ] **15.11.** [C++] Debug: on-screen `EDebMusicState` и активные слои
- [ ] **15.12.** [C++] Громкость Music из настроек (13)

**Критерий готовности:** музыка плавно реагирует на бой и паузу.

---

## Модуль 16: Сохранения

- [ ] **16.1.** [C++] `UDebSaveGame`: настройки (13) + прогресс
- [ ] **16.2.** [C++] Чекпоинты: позиция, здоровье, инвентарь, ключевые объекты, PCG seed
- [ ] **16.3.** [C++] «Продолжить» → `OpenLevelWithLoading()` (13)
- [ ] **16.4.** [Опционально] Автосохранение на триггерах
- [ ] **16.5.** [Опционально] Несколько слотов

**Критерий готовности:** настройки и прогресс между сессиями.

---

## Модуль 17: Производительность, PSO, отладка

**Цель:** стабильный FPS, отсутствие hitch'ей, инструменты контроля качества.

> Сквозные практики — с модуля 0. После каждого модуля: `stat unit` → baseline в `LEARNING.md`.

### Референсное железо и цели FPS


| Платформа        | Железо / режим                                                             | Цель                                                                |
| ---------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **Reference PC** | RTX 5070, i5-14600KF, 32 GB DDR5, **native 1440p**, пресет `Deb_High1440p` | **≥ 60 FPS** стабильно                                              |
| **Addition PC**  | RTX 3060, i5-12400F, 32 GB DDR4, **native 1080p**, пресет `Deb_High1080p`   | **≥ 60 FPS** стабильно                                              |
| **Steam Deck**   | пресет `**Deb_SteamDeck`** (отдельный Scalability + разрешение/FSR)        | **Играбельно:** ≥ 30 FPS (цель 40+) или ≥ 60 с агрессивным пресетом |


**Stress-сцена для замеров:** demo-здание (PCG или Graybox) — 20+ props, до **4 NPC**, активная стрельба, 2–3 ragdoll, destructibles.

- [ ] **17.0.** [Редактор/C++] Пресет `**Deb_High1440p`**: базовые Scalability для reference PC
- [ ] **17.0b.** [Редактор/C++] Пресет `**Deb_SteamDeck`**: сниженные shadows, Lumen quality, resolution scale / FSR
- [ ] **17.0c.** [C++] Автовыбор пресета при обнаружении Steam Deck (или ручной выбор в Settings)

### PSO Pipeline Cache

- [ ] **17.1.** [Редактор] `r.ShaderPipelineCache.Enabled=1`, `SaveUserCache=1` (задел — 0.20)
- [ ] **17.2.** [Редактор] Сбор `.upipelinecache` в разработке; shipping cache
- [ ] **17.3.** [Редактор] Чеклист ассетов для прогрева (материалы, Niagara, post-process, Quixel Megascans)
- [ ] **17.4.** [C++] Runtime-прогрев на loading screen (13.24)
- [ ] **17.5.** [C++] Не скрывать loading screen до PSO batch (или таймаут)
- [ ] **17.6.** [Редактор] Packaged build: нет hitch при первой стрельбе / VFX

### Практики производительности

- [ ] **17.7.** Tick off где не нужен; props в sleep — tick off (6)
- [ ] **17.8.** Object pooling: декали, Niagara, hitscan VFX (7–9)
- [ ] **17.9.** LineTrace budget: interaction, footstep, obstruction, AI — разные интервалы
- [ ] **17.10.** AI perception: 0.1–0.2 с; stagger; off для далёких NPC
- [ ] **17.11.** Лимит simulating props; ragdoll sleep (10); GC cleanup (9)
- [ ] **17.12.** [Опционально] Off-screen Control Rig off
- [ ] **17.13.** Niagara: max particles, GPU sim
- [ ] **17.14.** Nanite для статики (Quixel Megascans); разумное число shadow-casting lights
- [ ] **17.15.** Аудио: polyphony limit, attenuation cutoff
- [ ] **17.16.** Память: `obj list` / Unreal Insights — leaks
- [ ] **17.17.** Baseline `stat unit` (Game, Draw, GPU) после **каждого** модуля → `LEARNING.md`

### Debug по системам

- [ ] **17.18.** [C++] `bShowDebug` / console `ToggleDebug`
- [ ] **17.19.** Hitscan: DrawDebugLine сегментов (8)
- [ ] **17.19b.** Оружие: debug mag/reserve/reload; сокеты Muzzle/Eject (7)
- [ ] **17.20.** Interaction: trace + `CurrentTarget` (4)
- [ ] **17.21.** AI: cone от головы; линии Head/Body (11)
- [ ] **17.22.** Footsteps / obstruction (1, 5)
- [ ] **17.23.** Physics: счётчик active bodies; sleep/wake log
- [ ] **17.24.** [Опционально] Floating damage numbers
- [ ] **17.25.** Music state on-screen (15)
- [ ] **17.26.** [Опционально] Perf overlay UMG: FPS, frame time, physics count
- [ ] **17.27.** [C++] Console: `GodMode`, `SpawnEnemy`, `ToggleDebug`, `StatUnit`
- [ ] **17.28.** [Редактор] Unreal Insights — trace боевой сессии

### Профилирование и регрессия

- [ ] **17.29.** **Reference PC:** ≥ 60 FPS @ 1440p на stress-сцене; записать цифры в README
- [ ] **17.30.** **Steam Deck:** играбельный прогон demo end-to-end на `Deb_SteamDeck`; записать FPS и настройки
- [ ] **17.31.** [Опционально] Debug-меню: spawn, teleport, toggle systems
- [ ] **17.32.** [Опционально] Stress-карта: «fire 100 rounds», «spawn 10 props», «stress AI»

**Критерий готовности:** нет shader hitch; debug покрывает системы; FPS-цели на reference PC и Steam Deck задокументированы.

---

## Модуль 18: Финальная полировка + Quixel Megascans (FAB)

**Последний этап.** Gameplay-код не менять — только контент и presentaton.

- [ ] **18.1.** [Редактор] `L_Deb_Building_Final`: замена graybox на **Quixel Megascans (FAB)** — стены, пол, двери, props
- [ ] **18.2.** [Редактор] Collision pass после Quixel Megascans (COL.8)
- [ ] **18.3.** [Редактор] Освещение Lumen; локальный свет в коридорах
- [ ] **18.4.** [Редактор] Substrate / MI production на Quixel Megascans mesh
- [ ] **18.5.** [Редактор] Проверка FPS на reference PC (1440p) и Steam Deck после art pass (17.29–17.30)
- [ ] **18.6.** [Редактор] Постобработка — кинематографичность перестрелок
- [ ] **18.7.** [Опционально] Экранные эффекты; inspect weapon
- [ ] **18.8.** [Редактор] PSO final bake на финальных материалах/VFX
- [ ] **18.9.** README: управление KB+геймпад, сборка, FPS-цели, статус модулей

**Критерий готовности:** играбельный stealth-shooter demo в здании с Quixel Megascans-артом.

---

## Build.cs — roadmap

- Модуль 0: `EnhancedInput`, `InputCore`
- Модуль 6: `PhysicsCore`, `Chaos`
- Модуль 8–9: `Niagara` (если ещё не подключён); Chaos GC для destructibles
- Модуль 10–11: `AIModule`, `NavigationSystem`, `ControlRig`
- Модуль 12: plugin `PCG`
- Модуль 13: `UMG`, `Slate`, `SlateCore`

---

## Опциональные фичи

- 1.10 — camera bob
- 3.5 — wound FX
- 5.9 — production-библиотека материалов
- 6.7, 6.8 — prop weight, stacking
- 7.21, 7.22 — ADS, weapon sway
- 9.6 — destructible support chain
- 10.12 — Control Rig death poses
- 11.10, 11.11, 11.20 — EQS, group alert, dark zones
- 11.22 — aim assist tuning
- 12.11 — runtime PCG regen
- 13.5, 13.17, 13.25 — hit marker, rebind, loading tips
- 16.4, 16.5 — autosave, slots
- 17.12, 17.24, 17.26, 17.31, 17.32 — off-screen rig, damage numbers, perf overlay, debug menu, stress map
- 18.7 — screen FX, inspect weapon

---

## Потенциальные примеры Troubleshooting

**Персонаж не spawится** — GameMode Override, `DefaultPawnClass`, `PlayerStart`, PIE 1 player.

**Input не работает** — `AddMappingContext`; IMC в BP; для геймпада — `Enable Gamepad` в Project Settings.

**Геймпад: drift / резкий look** — dead zone и `Curve_DebLookResponse` (модуль 1.6).

**Hitscan не бьёт** — trace channel `ECC_Deb_Weapon`; ignore owner; collision на mesh, не только капсула.

**NPC не идёт** — NavMeshBoundsVolume; rebuild NavMesh после PCG/graybox.

**Quixel Megascans — проваливается / стрелять сквозь стены** — simplified collision, COL.8.

**Steam Deck — низкий FPS** — пресет `Deb_SteamDeck` (17.0b); снизить shadows, Lumen, resolution scale.

**Live Coding** — после нового UCLASS полная перекомпиляция.

**PSO hitch** — модули 17.1–17.6, 13.24, прогон VFX/Quixel Megascans до shipping.

**После миграции 5.7.4→5.8** — пересобрать C++ (VS 2022), прогнать conversion wizard; пересобрать PSO cache и NavMesh; проверить deprecated API в Control Rig, PCG, Substrate; smoke по разделу «Миграция на UE 5.8».

---

*Конспекты: `[LEARNING.md](LEARNING.md)`*