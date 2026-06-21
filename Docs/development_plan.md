# План разработки — Debris

**Репозиторий:** https://github.com/lexusch/Debris

**Движок:** Unreal Engine **5.8** (стабильный релиз, 17 июня 2026; последний крупный релиз линейки UE5 перед UE6)

**IDE:** Visual Studio **2022** (проверенный toolchain; VS 2026 — поддержка под UE 5.8 пока не подтверждена официально, не использовать как основную IDE)

**Проект:** `Debris` · модуль C++ `Debris` · API `DEBRIS_API` · префикс классов `Deb`

---

## СТАТУС ПРОЕКТА

> ⚠️ Этот блок обновляется вручную после каждого завершённого обязательного пункта.
> ИИ-наставник читает его **первым** при начале каждой сессии.

**Текущий модуль:** 0 — Фундамент  
**Последний завершённый обязательный пункт:** 0.1 (C++ проект Debris создан)
**Следующий шаг:** 0.2

**Быстрый статус модулей:**

| Модуль | Название | Статус |
|--------|----------|--------|
| 0 | Фундамент | ⬜ Не начат |
| 1 | Перемещение и камера FPS | ⬜ Не начат |
| 2 | Игровой каркас | ⬜ Не начат |
| 3 | Здоровье и урон | ⬜ Не начат |
| 4 | Взаимодействие | ⬜ Не начат |
| 5 | Физматериалы, звук, Substrate | ⬜ Не начат |
| 6 | Физические пропы | ⬜ Не начат |
| 7 | Оружие | ⬜ Не начат |
| 8 | Hitscan и баллистика | ⬜ Не начат |
| 9 | Разрушаемые объекты | ⬜ Не начат |
| 10 | NPC: тело, Control Rig, ragdoll | ⬜ Не начат |
| 11 | NPC: perception, stealth, ИИ | ⬜ Не начат |
| 12 | UI: HUD, меню, настройки | ⬜ Не начат |
| 13 | Продвинутый звук (SFX) | ⬜ Не начат |
| 14 | Динамический саундтрек | ⬜ Не начат |
| 15 | PCG — процедурные интерьеры | ⬜ Не начат |
| 16 | Сохранения | ⬜ Не начат |
| 17 | Производительность, PSO, отладка | ⬜ Не начат |
| 18 | Финальная полировка + Megascans | ⬜ Не начат |

_Статусы: ✅ Завершён · 🔄 В процессе · ⬜ Не начат_

---

## Характер проекта

Весь план — **учебно-практический**: каждый модуль — освоение технологий UE и шаг к играбельному demo. Отдельных фаз «preview / production / после релиза» нет — один чеклист **0→18** на UE 5.8. Единственная метка scope — **`[Опционально]`**: пункт можно отложить без блокировки критерия готовности модуля.

---

## Видение

Иммерсивный, неторопливый, **грузный** FPS в **интерьере здания**. **1–4 NPC** охотятся на игрока — прятаться, выживать, искать расходники и полезные предметы, устранить угрозу и/или добраться до лифта. Перестрелки **редкие и ценные**. Минималистичный UI.

Продвинутая физика: пропы, разрушаемость, Control Rig Physics, hitscan с пробитием и рикошетом. Сложные физические объекты (навесной потолок, лампы, люстры, жалюзи, вращающиеся офисные кресла, полки с выдвигающимися ящиками). Продвинутая реакция на попадания пуль (объёмные отверстия, следы, адаптивные decals). Продвинутые материалы (вес, звук, следы разрушения, декаль, VFX). Продвинутая процедурная анимация, совмещение физики и анимации для тел + новая система Physics Control Rig.

**Игровой процесс:** персонаж на процедурно-сгенерированном этаже здания, 1–4 NPC ищут игрока. Задача — добраться до лифта: вызвать, дождаться, зайти, нажать кнопку, дождаться закрытия дверей → генерация нового PCG-этажа. Стелс → вспышка боя → тишина.

**PCG** — неотъемлемая часть геймплея, но **после** основных механик → модуль 15.

**Направления обучения:** C++, Blueprint, Niagara, AI, Modeling, Animation, Sound, MetaSounds, UI, Chaos, Physics, Substrate (5), Control Rig (10), PCG (15), Metahuman.

---

## Маршрут разработки

Один проход модулей **0→18** на **UE 5.8**.

После каждого модуля: чекбоксы + запись в `LEARNING.md` · git-коммит `Debris: модуль N — …`

---

## Порядок модулей (0 → 18)

| #  | Модуль |
| --- | ------- |
| 0  | Фундамент: проект, input KB+геймпад, коллизии, GameMode |
| 1  | Перемещение, камера FPS, кривые стиков |
| 2  | Игровой каркас |
| 3  | Здоровье и урон |
| 4  | Взаимодействие |
| 5  | Физматериалы, звук, Substrate |
| 6  | Физические пропы |
| 7  | Оружие (базовый класс, конфиги, варианты) |
| 8  | Hitscan и баллистика |
| 9  | Разрушаемые объекты |
| 10 | NPC: тело, Control Rig, ragdoll |
| 11 | NPC: perception, stealth, ИИ |
| 12 | UI: HUD, меню, настройки, загрузка |
| 13 | Продвинутый звук (SFX) |
| 14 | Динамический саундтрек |
| 15 | PCG — процедурные интерьеры (часть геймплея) |
| 16 | Сохранения |
| 17 | Производительность, PSO, отладка |
| 18 | Финальная полировка + Megascans (FAB) |

**Жёсткие зависимости:** оружие (7) → hitscan (8) → destructibles (9) · тело NPC (10) → ИИ (11) · **механики (0–11) → PCG (15)** · UI/звук/музыка (12–14) тестируются на ручном graybox и переносятся на PCG-карту в модуле 15 · сохранения (16) идут после PCG, т.к. сохраняют PCG seed · polish (18) — **последний**.

---

## Graybox и production-уровни

| Этап | Когда | Карта | Наполнение |
| ------ | ------------ | --------------------------------- | ------- |
| **v0** | Модуль 0 | `L_Deb_Arena_v0` | Одна комната + коридор; `PlayerStart`; BSP/box; тест коллизий капсулы |
| **v1** | Модуль 1–2 | `L_Deb_Building_Graybox` | Несколько комнат, потолки под crouch/stand, укрытия разной высоты, 2–3 `PlayerStart` |
| **v2** | Модуль 2 | `L_Deb_Building_Graybox` (ручной) | Полный этаж вручную; `NavMeshBoundsVolume`; пустые spawn-точки NPC |
| **v3** | Модуль 4, 6 | Graybox + зона `Props` | Двери, кнопки, ящики; 5–10 физических пропов; зона grab/throw |
| **v4** | Модуль 7–8 | `L_Deb_CombatLane` | Полигон стрельбы; мишени с `IDebDamageable`; укрытия; тест pistol/SMG/shotgun |
| **v5** | Модуль 9 | CombatLane + destructible-секция | Chaos GC стены/объекты; проверка hitscan → fracture |
| **v6** | Модуль 10–11 | Graybox (полный этаж) | Patrol points, dark zones, choke points; 1–4 NPC; оружие на полу |
| **v7** | Модуль 15 | `L_Deb_PCG_Demo` | PCG-layout как **основная demo-карта**; seed; NavMesh; spawner'ы из GameMode |
| **v8** | Модуль 18 | `L_Deb_Building_Final` | **Quixel Megascans (FAB):** art pass поверх каркаса v7; Lumen; collision pass |

> Graybox-геометрию не выбрасывать — blockout остаётся под Megascans-слоями или как reference.

---

## Иерархия каталогов

**Это ориентир, не задание на упреждающее создание.** Каталоги (и `Content/`, и `Source/`) и C++ классы создаются **только через редактор Unreal** — Content Browser (Add New Folder) и `Tools → New C++ Class`, не через Проводник Windows и не через IDE напрямую. Структура ниже создаётся **по мере необходимости**, когда в конкретном пункте плана реально нужна новая папка или класс — не вся сразу в module 0. Точное соответствие приведённой иерархии не обязательно: это пример логичной раскладки, отклонения допустимы.

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

| Тип | Шаблон | Пример |
| ---------- | -------------------- | --------------------------- |
| C++ | `A/U/F/E/I` + `Deb` | `ADebWeaponBase` |
| Blueprint | `BP_Deb` | `BP_DebGameMode` |
| Widget | `WBP_Deb` | `WBP_DebHUD` |
| Data Asset | `DA_Deb` | `DA_DebWeaponConfig_Pistol` |
| Input | `IMC_Deb` / `IA_Deb` | `IA_DebFire` |
| Уровень | `L_Deb_` | `L_Deb_Building_Final` |

### Чеклист нового класса

- Папка `Public/` + `Private/` по таблице выше
- Имя с префиксом `Deb`
- Зеркальный ассет в `Content/Debris/…`

---

## Коллизии (сквозная тема)

| Канал / preset | Назначение | Модули |
| ------------------------------ | ---------------------- | ------- |
| `ECC_Deb_Weapon` (trace) | Hitscan, obstruction | 0, 7, 8 |
| `ECC_Deb_Interactable` (trace) | Interaction line trace | 0, 4 |
| `ECC_Deb_Prop` | Физические пропы | 0, 6 |
| `Deb_Pawn` (object) | Игрок / NPC капсула | 0, 1 |
| `Deb_Hitbox` (object) | Bone hitboxes NPC | 10 |
| `Deb_Destructible` (object) | Chaos GC meshes | 9 |
| `BlockAll` / `OverlapOnlyPawn` | Стены graybox | 0 |

- [ ] **COL.0.** [C++/Редактор] Модуль 0: завести trace-каналы и collision presets в `DefaultEngine.ini` / Project Settings
- [ ] **COL.1.** [C++/Редактор] Модуль 0: таблица «кто с кем блокируется / overlap» → `LEARNING.md`
- [ ] **COL.2.** [C++] Модуль 4: interaction trace только на `ECC_Deb_Interactable`
- [ ] **COL.3.** [C++] Модуль 6: пропы — `ECC_Deb_Prop`, корректный `CollisionEnabled` при grab/sleep
- [ ] **COL.4.** [C++] Модуль 7–8: obstruction и hitscan — отдельные trace channels; ignore owner/wielder
- [ ] **COL.5.** [C++/Редактор] Модуль 10: Physics Asset NPC — hitbox bones block `ECC_Deb_Weapon`, не блокируют pawn movement
- [ ] **COL.6.** [C++/Редактор] Модуль 9: destructible — корректная collision после fracture; debris не блокирует pawn навсегда
- [ ] **COL.7.** [C++] Модуль 15: NavMesh + collision после PCG generation; rebuild при смене seed
- [ ] **COL.8.** [Редактор] Модуль 18: Megascans mesh — simplified collision; без дыр в стенах

---

## Архитектура (ключевые решения)

| Система | Решение |
| ------------------------ | ------- |
| Оружие | `ADebWeaponBase` + `UDebWeaponConfig` (Data Asset); дочерние BP: pistol / SMG / shotgun |
| Hitscan | **После оружия:** `UDebHitscanSubsystem::ExecuteHitscan()`; профиль из `UDebWeaponConfig` |
| Shotgun | `FireMode = Shotgun`; `PelletCount` × spread → несколько `ExecuteHitscan` за один выстрел |
| Обоймы | `CurrentMagAmmo` / `ReserveAmmo`; `AMagazineActor` при reload |
| Perception | Origin на `head`; trace к Head + Body (OR) |
| NPC | **Сначала** тело + CR/ragdoll (10), **потом** BT и stealth (11) |
| Destructibles | **После hitscan (8)** — модуль 9 |
| PCG | **После механик (0–11) и UI/звука/музыки (12–14)** — модуль 15; seed-driven demo |
| Input | KB + **геймпад с модуля 0**; кривые стиков (1); aim assist (11/12) |
| PSO | Сбор в dev; runtime-прогрев на loading screen (12, 17) |
| Perf (Основной ПК) | ≥ 60 FPS @ **1440p** — RTX 5070, i5-14600KF, 32 GB DDR5; пресет `Deb_High1440p` |
| Perf (Дополнительный ПК) | ≥ 60 FPS @ **1080p** — RTX 3060, i5-12400F, 32 GB DDR4; пресет `Deb_High1080p` |
| Perf (Портативный ПК) | Steam Deck, пресет `Deb_SteamDeck`; ≥ 30 FPS (цель 40+) |
| Арт уровней | Graybox → PCG demo → FAB Quixel Megascans (18) |

---

## Модуль 0: Фундамент

**Цель:** проект Debris, персонаж, input KB+геймпад, коллизии, GameMode, graybox v0.

**Результат в PIE:** запускается `L_Deb_Arena_v0`; персонаж спавнится через `PlayerStart`; WASD + мышь + геймпад двигают персонажа; нет провала сквозь пол.

**Новые концепты:** `UCharacterMovementComponent`, Enhanced Input System, collision channels/presets, `GameMode` / `DefaultPawnClass`, `PlayerStart`.

**Входные условия:** нет (первый модуль).

**Блокирует:** все остальные модули.

---

### Проект

- [x] **0.1.** [Редактор] C++ проект **Debris**, модуль `Debris` (Blank проект, scalable графика, C++ пресет)
- [ ] **0.2.** Проверить корневые `Content/Debris/` и `Source/Debris/` (создаются автоматически визардом проекта); детальную структуру (см. «Иерархия каталогов» выше) создавать по мере необходимости через редактор, не заранее
- [ ] **0.3.** [C++] `Debris.Build.cs`: `EnhancedInput`, `InputCore`
- [ ] **0.4.** [Git] `.gitignore` по расширенному списку; проверить `git status` — в индексе только `Content/`, `Config/`, `Source/`, `*.uproject`, docs
  * **UE:** `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/`, `Build/*`
  * **IDE:** `.vs/`, `.vscode/`, `.idea/`
  * **Visual Studio:** `*.VC.db`, `*.VC.opendb`, `*.suo`, `*.opensdf`, `*.sdf`, `.vsconfig`
  * **Генерируемое:** `*.sln`, `*.slnx`, `*.xcodeproj`, `*.xcworkspace`, `Automation_*.slnx`, `shadertoolsconfig.json`
  * **Плагины/tools:** `Plugins/VisualStudioTools/`
  * **Прочее:** `*.log`, `*.dmp`, `*.tmp`
  * **Дополнительно:** `Saved/Autosaves/`, `Saved/Collections/`, `Content/Developers/`, `.DS_Store`, `Thumbs.db`

### Коллизии

- [ ] **0.5.** [C++/Редактор] Trace-каналы и object types (см. COL.0–COL.1)
- [ ] **0.6.** [Редактор] Collision presets: `Deb_Pawn`, `Deb_WorldStatic`

### Input — клавиатура + геймпад

- [ ] **0.7.** [C++] `ADebPlayerCharacter` — наследник `ACharacter`; капсула и movement уже в базовом классе; не добавлять камеру и input здесь (это модули 1 и 0.11).
  > ИИ: объяснить иерархию ACharacter → APawn → AActor; зачем наследоваться от ACharacter, а не AActor.
- [ ] **0.8.** [Редактор] Input Actions: `IA_DebMove` (Vector2D), `IA_DebLook` (Vector2D), `IA_DebJump` (Digital)
  > ИИ: объяснить разницу между старой Input System и Enhanced Input; что такое Input Action и зачем Vector2D для Move/Look.
- [ ] **0.9.** [Редактор] `IMC_DebDefault`: **KB** — WASD, мышь, Space
- [ ] **0.10.** [Редактор] `IMC_DebDefault`: **геймпад** — Left Stick Move, Right Stick Look, A/Cross Jump, RT/R2 — задел Fire (модуль 7)
- [ ] **0.11.** [C++] `AddMappingContext`; `BindAction` Move / Look / Jump
  > ИИ: объяснить где вызывать AddMappingContext (BeginPlay через PlayerController или Character); почему важно передавать Priority.
- [ ] **0.12.** [C++] Обработка `Look`: и мышь, и правый стик (Enhanced Input Vector2D)
- [ ] **0.13.** [Blueprint] `BP_DebPlayer` — наследник `ADebPlayerCharacter`; назначить `DefaultPawnClass` в GameMode

### GameMode

- [ ] **0.14.** [C++] `ADebGameMode`, `ADebPlayerController`
  > ИИ: объяснить зачем отдельный PlayerController уже на этом этапе (задел для модуля 2 и 13); Gameplay Framework в целом.
- [ ] **0.15.** [Blueprint] `BP_DebGameMode`
- [ ] **0.16.** [Редактор] `GlobalDefaultGameMode = BP_DebGameMode`; `GameDefaultMap = L_Deb_Arena_v0`
- [ ] **0.17.** [Редактор] PIE: spawn `BP_DebPlayer` через `PlayerStart`

### Graybox v0

- [ ] **0.18.** [Редактор] `L_Deb_Arena_v0`: одна комната + коридор; проверка коллизий капсулы и геймпада
- [ ] **0.19.** [C++] Отладка: `#define DEBUG_TRACES` — задел для отладочных визуализаций
- [ ] **0.20.** [Редактор] PSO — задел к модулю 17: включить `r.ShaderPipelineCache.Enabled=1`
- [ ] **0.21.** `LEARNING.md` — первая запись: что изучено, какие проблемы возникли

**Критерий готовности:** PIE на v0; WASD + мышь + геймпад (move/look/jump); коллизии работают; персонаж не проваливается.

---

## Модуль 1: Перемещение и камера FPS

**Цель:** полноценное FPS-перемещение с «грузным» ощущением, камера, кривые стиков.

**Результат в PIE:** персонаж ходит с инерцией; спринт/присед работают; камера прикреплена от первого лица; правый стик геймпада управляет взглядом с dead zone.

**Новые концепты:** `UCameraComponent`, `USpringArmComponent`, `UCurveFloat`, `bUseControllerRotationYaw`, First Person Rendering, `UCharacterMovementComponent` параметры, camera offset lean, `FInterpTo`.

**Входные условия (обязательны [x]):** 0.7, 0.11, 0.13, 0.14, 0.18.

**Блокирует:** 7.18 (obstruction trace), 11.12 (visibility points), 11.20 (тёмные зоны + фонарик).

---

- [ ] **1.1.** [C++/Редактор] Камера FPS: `UCameraComponent` на `head` socket; mesh рук; сокет `GripPoint`
  > ИИ: объяснить почему First Person камера крепится к mesh, а не к капсуле; что такое сокеты.
- [ ] **1.2.** [Редактор] First Person Rendering: настройка visibility mesh рук (только для владельца)
- [ ] **1.3.** [C++] `bUseControllerRotationYaw = true`; `bUseControllerRotationPitch = false`; camera pitch через контроллер
- [ ] **1.4.** [C++] Спринт (`IA_DebSprint`) и присед (`IA_DebCrouch`) + ceiling check перед вставанием
  > ИИ: объяснить ceiling check через SweepSingleByChannel; почему без него персонаж застрянет в потолке.
- [ ] **1.5.** [C++] «Грузное» движение: настройка `MaxWalkSpeed`, `MaxAcceleration`, `BrakingDecelerationWalking`, `GroundFriction`
- [ ] **1.5b.** [C++/Редактор] Наклоны (Lean): `IA_DebLeanLeft` / `IA_DebLeanRight` в `IMC_DebDefault` — **Q/E + LB/RB (геймпад)**, тип Digital (Hold); смещение `UCameraComponent` по Y (макс. ~50 см) + camera roll (~12°); `SweepSingleByChannel` вбок — нельзя наклониться в стену; плавный `FInterpTo` для возврата камеры
  > ИИ: объяснить почему lean реализуется через camera offset, а не поворот капсулы; как SweepSingleByChannel отличается от LineTrace; связь с weapon obstruction (1.8) — при наклоне проверять obstruction со стороны наклона.
- [ ] **1.6.** [Редактор] **Кривые стиков:** `Curve_DebMoveResponse`, `Curve_DebLookResponse` — dead zone, чувствительность (UCurveFloat в редакторе кривых)
  > ИИ: объяснить что такое dead zone и почему без него стик «дрейфует»; показать как читать UCurveFloat в C++.
- [ ] **1.7.** [C++] Применение curve к Look/Move для геймпада через `FRichCurve::Eval()`
- [ ] **1.8.** [C++] Задел `WeaponObstructionTrace` — LineTrace вперёд от камеры → модуль 7
- [ ] **1.9.** [C++] `UDebPlayerVisibilityComponent`: точки `Head`, `Body` — используются NPC perception в модуле 11; lean state смещает точку `Head` вбок — при наклоне игрок выглядывает лишь частично
- [ ] **1.10.** [Опционально] Покачивание камеры при ходьбе (camera bob через Timeline или Curve)
- [ ] **1.11.** [Опционально][C++/Редактор] Фонарик: `USpotLightComponent` на `head` socket; `IA_DebFlashlight` — **F / D-Pad Down (геймпад)**, тип Digital (Toggle); настройки: `AttenuationRadius`, `InnerConeAngle`, `OuterConeAngle`; задел для модуля 11: включённый фонарик увеличивает `SightRadius` NPC в тёмных зонах (см. 11.20)

**Graybox:** расширить до `L_Deb_Building_Graybox` — несколько комнат с потолками под crouch/stand, укрытия.

**Критерий готовности:** sprint/crouch/lean (Q/E + LB/RB); геймпад с кривыми и dead zone; visibility points с учётом lean; фонарик переключается.

---

## Модуль 2: Игровой каркас

**Цель:** связать GameInstance → GameMode → GameState → PlayerController → Pawn. Каркас для всех будущих игровых состояний.

**Результат в PIE:** GameInstance переживает смену уровней; GameState хранит состояние матча; PlayerController принимает input.

**Новые концепты:** `UGameInstance` (живёт всю сессию), `AGameState` (реплицируется всем), `APlayerController` vs `APawn`, разделение ответственности в Gameplay Framework.

**Входные условия (обязательны [x]):** 0.14, 0.15, 0.16, 0.17.

**Блокирует:** 11.0 (GameState для AlertActive), 15.6 (seed в GameMode), 12.9 (переходы через GameInstance), 16.1 (SaveGame через GameInstance).

---

- [ ] **2.1.** [C++] `ADebGameInstance` — наследник `UGameInstance`; живёт всю сессию приложения
  > ИИ: объяснить разницу GameInstance / GameMode / GameState: кто живёт сколько и кто доступен кому.
- [ ] **2.2.** [C++] `ADebGameMode` → `GameStateClass = ADebGameState`
- [ ] **2.3.** [C++] `ADebGameState`: поля `bAlertActive`, `AliveHunterCount`, `EDebWinCondition`
- [ ] **2.4.** [C++] `ADebPlayerController`: чувствительность мыши **и геймпада**; `SetInputMode`
- [ ] **2.5.** [C++] `RestartPlayer` — задел для смерти/перезапуска (модуль 3)
- [ ] **2.6.** [Blueprint] `BP_DebGameMode` — параметры уровня (кол-во NPC, сложность)

**Критерий готовности:** цепочка GameInstance → GameMode → GameState → Controller → Pawn работает; GameState читается из любого Actor.

---

## Модуль 3: Здоровье и урон

**Цель:** универсальная система урона через интерфейс — работает для игрока, NPC и разрушаемых объектов.

**Результат в PIE:** тестовая мишень теряет здоровье от прямого вызова `ApplyPointDamage`; смерть триггерит событие.

**Новые концепты:** `IInterface` в UE5, `UActorComponent`, `DECLARE_DYNAMIC_MULTICAST_DELEGATE`, `ApplyPointDamage` / `ApplyDamage` UE framework, `FPointDamageEvent`.

**Входные условия (обязательны [x]):** 0.7, 0.14.

**Блокирует:** 8.10 (hitscan → damage), 10.1 (NPC health), 11.9 (BT блокировка при stagger).

---

- [ ] **3.1.** [C++] `IDebDamageable` — интерфейс; методы `ApplyDamage(FDamageInfo)`, `GetCurrentHealth()`, `IsDead()`
  > ИИ: объяснить UE-интерфейсы: почему два класса (UDebDamageable + IDebDamageable); как вызывать через `Cast<IDebDamageable>` vs `TScriptInterface`.
- [ ] **3.2.** [C++] `UDebHealthComponent` — компонент; `CurrentHealth`, `MaxHealth`; делегаты `OnDamaged`, `OnDeath`
- [ ] **3.3.** [C++] `FDamageInfo`: `float Damage`, `EDamageType Type`, `FName BoneName`, `float BoneMultiplier`, `FVector HitDirection`
- [ ] **3.4.** [C++] `ApplyPointDamage` — единая точка входа; вызывает `UDebHealthComponent::TakeDamage`
- [ ] **3.5.** [Опционально] Визуальные/звуковые эффекты ранения (кровь, вспышка) — лучше в модуле 5

**Graybox:** тестовый актор-мишень на `L_Deb_Building_Graybox` с `UDebHealthComponent`; консольная команда для нанесения урона.

**Критерий готовности:** `IDebDamageable` реализован; `UDebHealthComponent` работает; делегат смерти срабатывает.

---

## Модуль 4: Взаимодействие

**Цель:** система взаимодействия через line trace — двери, кнопки, подбор предметов.

**Результат в PIE:** персонаж смотрит на дверь, нажимает E / X — дверь открывается; подсказка появляется в точке прицела.

**Новые концепты:** `IInterface` для интерактивных объектов, `LineTraceSingleByChannel`, `ECC_Deb_Interactable`, trace debugging.

**Входные условия (обязательны [x]):** 0.5 (COL.2 готов), 0.7, 0.11.

**Блокирует:** 6.2 (grab через interaction), 7.16 (подбор оружия), 11.5 (NPC видит оружие), 15.9b (лифт → PCG regenerate).

---

- [ ] **4.1.** [C++] `IDebInteractable` — методы `Interact(AActor* Instigator)`, `GetInteractPrompt()`, `CanInteract()`
- [ ] **4.2.** [C++] `UDebInteractionComponent` — trace `ECC_Deb_Interactable`; интервал 0.1 с; хранит `CurrentTarget`
  > ИИ: объяснить почему trace на отдельном канале, а не на всём World Static; как настраивать TraceComplex.
- [ ] **4.3.** [C++] `ADebInteractableActor` — базовый актор; реализует `IDebInteractable`
- [ ] **4.4.** [C++/Blueprint] Дверь (timeline открытие), кнопка (одноразовая), ящик (toggle)
- [ ] **4.5.** [C++] Заготовка подсказки взаимодействия — строка для HUD (UI → модуль 13)
- [ ] **4.6.** [Редактор] Input: `IA_DebInteract` — **E / X (геймпад)** в `IMC_DebDefault`

- [ ] **4.7.** [C++/Blueprint] `ADebElevatorActor`: кнопка вызова → анимация дверей (`Timeline`) → entry trigger (`UBoxComponent` overlap); кнопка этажа внутри → `EDebWinCondition::Escaped` в `ADebGameMode`; задел интеграции с PCG — `RegenerateLevel(NewSeed)` в модуле 15
  > ИИ: объяснить почему лифт — один Actor с несколькими Interactable-компонентами, а не несколько отдельных акторов; как Timeline управляет анимацией дверей без AnimBP.

**Graybox:** 2–3 interactable объекта + лифт на `L_Deb_Building_Graybox`.

**Критерий готовности:** trace находит объект; взаимодействие срабатывает по нажатию; лифт вызывается и открывает двери; подсказка готова к передаче в HUD.

---

## Модуль 5: Физматериалы, звук, Substrate

**Цель:** физматериалы связывают поверхность с поведением (звук, декаль, VFX); знакомство с Substrate.

**Результат в PIE:** шаг по металлу звучит иначе чем по бетону; sprint громче walk; для каждого базового материала готовы звук шага, звук/декаль попадания и заготовка реакции на удар; 1–2 Substrate-материала на graybox.

**Новые концепты:** `UPhysicalMaterial`, foot trace pattern, `USoundCue`, `UDataTable` для маппинга материал→звук, Substrate шейдерная модель.

**Входные условия (обязательны [x]):** 0.5, 0.6, 1.4 (sprint/crouch для разных звуков).

**Блокирует:** 6.4 (звук столкновений пропов от physmat + массы), 8.8 (эффекты попаданий по physmat), 9.5 (VFX разрушений по physmat), 13.3 (spatial audio).

---

### Базовые физматериалы

> Каждый материал — это `UPhysicalMaterial` + звук шага + звук/декаль попадания пули (используется позже в 8.8) + заготовка под пропы (модуль 6). Один физматериал = одна цельная единица данных, не разрозненные ассеты.

- [ ] **5.1.** [Редактор/C++] `PM_DebStone` (камень/бетон): создать `UPhysicalMaterial`, назначить тестовой поверхности graybox
  > ИИ: объяснить как физматериал назначается меш-компоненту; разница между surface type и физическим материалом.
- [ ] **5.2.** [Редактор/C++] `PM_DebMetal` (металл): `UPhysicalMaterial` + тестовая поверхность
- [ ] **5.3.** [Редактор/C++] `PM_DebWood` (дерево): `UPhysicalMaterial` + тестовая поверхность
- [ ] **5.4.** [Редактор/C++] `PM_DebPlastic` (пластик): `UPhysicalMaterial` + тестовая поверхность
- [ ] **5.5.** [Редактор/C++] `PM_DebFlesh` (плоть — тела NPC/игрока, заготовка для модуля 10): `UPhysicalMaterial`
- [ ] **5.6.** [Редактор/C++] `PM_DebGlass` (стекло — заготовка для разрушаемых объектов модуля 9): `UPhysicalMaterial`

### Звук шагов

- [ ] **5.7.** [C++] Шаги: SweepSingleByChannel вниз от капсулы → `GetPhysicalMaterial()` → `PlayFootstepSound()`; sprint vs walk (pitch/volume)
- [ ] **5.8.** [Редактор] Sound Cue / MetaSound шага для каждого из 6 базовых материалов (random из набора 2–3 звука на материал)
- [ ] **5.9.** [C++] Звук приземления после прыжка — также по physmat

### Реакция поверхностей на попадания (заготовка под 8.8/9.5)

- [ ] **5.10.** [C++] `UDataTable`: `FPhysMatResponseRow` (физматериал → звук попадания + декаль + VFX-тег); строка на каждый из 6 материалов; переиспользуется в модулях 8, 9, 13
- [ ] **5.11.** [Редактор] Декаль попадания для каждого материала (Камень — пыль/скол, Металл — искра/вмятина, Дерево — щепки, Пластик — трещина, Плоть — заготовка под модуль 10, Стекло — заготовка под модуль 9)

### Substrate

- [ ] **5.12.** [Редактор] Включить Substrate в Project Settings; 1–2 tutorial-материала по официальной доке
  > ИИ: объяснить чем Substrate отличается от стандартного Material Graph; ссылка на официальную документацию UE 5.8.
- [ ] **5.13.** [Редактор] Substrate-материал для graybox (камень / штукатурка / металл)
- [ ] **5.14.** [Редактор] Связать Substrate-материал с `UPhysicalMaterial` для корректных шагов
- [ ] **5.15.** [Опционально] Production-библиотека Substrate-материалов (доработка в модуле 18)

**Graybox:** зоны всех 6 базовых physmat на `L_Deb_Building_Graybox` для теста шагов и попаданий.

**Критерий готовности:** шаги различаются по всем 6 материалам; декаль/звук попадания готовы для каждого через `FPhysMatResponseRow`; 1–2 Substrate-материала работают; Substrate подключён к physmat.

---

## Модуль 6: Физические пропы

**Цель:** интерактивные физические объекты — поднять, нести, бросить; вес влияет на возможность поднятия и на звук; корректный сон/пробуждение.

**Результат в PIE:** ящик поднимается, переносится, бросается с импульсом; тяжёлый предмет либо не поднимается (только толкается), либо несдвигаем; при ударе о стену звучит звук, который зависит и от силы удара, и от массы; пропы засыпают через 3 сек после остановки.

**Новые концепты:** `UPhysicsHandleComponent`, kinematic vs simulating physics, `BodyInstance` / `Density` физматериала и автоматический расчёт массы, `bOverrideMass` / `MassInKg`, `OnComponentSleep` / `WakeAllRigidBodies`, tick management.

**Входные условия (обязательны [x]):** 0.5 (COL.3 готов), 4.1 (Interactable задел), 5.1–5.6 (physmat для звуков и Density).

**Блокирует:** 9.1 (destructible пропы), 10.9 (NPC толкает пропы), 17.7 (tick off в sleep).

---

- [ ] **6.1.** [C++] `ADebPhysicalPropActor` — `UStaticMeshComponent` с `ECC_Deb_Prop`; `UPhysicalMaterial` на mesh (один из 6 базовых из модуля 5)
- [ ] **6.2.** [C++] Масса пропа: проверить авторасчёт массы движком (`Объём меша × Density` физматериала); `bOverrideMass`/`MassInKg` для точечной коррекции там, где авторасчёт даёт нелогичный результат
  > ИИ: объяснить как UE считает массу `BodyInstance` по объёму и Density физматериала; зачем нужен override, а не всегда доверять авторасчёту.
- [ ] **6.3.** [C++] `UPhysicsHandleComponent` — grab/drop/throw; крепление через `GrabComponentAtLocationWithRotation`
  > ИИ: объяснить разницу между кинематическим удержанием (PhysicsHandle) и заморозкой physics; почему PhysicsHandle лучше для grab.
- [ ] **6.4.** [C++] Гейтинг по массе: `CanGrab()` сравнивает `GetMass()` пропа с порогами из Data Asset/DataTable (`GrabMassThreshold`, `PushOnlyMassThreshold`); ниже порога — поднимается, между порогами — только толкается (физика остаётся симулируемой, PhysicsHandle не хватает), выше верхнего порога — несдвигаем (`Static`/очень большая масса)
  > ИИ: объяснить почему пороги массы лучше держать в Data Asset, а не хардкодить в C++ (паттерн «логика → C++, параметры → Data Asset»).
- [ ] **6.5.** [C++] Physics sleep / wake: `OnComponentSleep` → `SetComponentTickEnabled(false)`; `WakeAllRigidBodies` при grab
- [ ] **6.6.** [C++] Звуки столкновений: `OnComponentHit` → амплитуда от `NormalImpulse` **и** от `GetMass()` (второй множитель — тяжёлый предмет звучит глуше при том же импульсе); pitch/volume через `FPhysMatResponseRow` (5.10)
- [ ] **6.7.** [Редактор] Input: `IA_DebGrab` — **ЛКМ / RT**; drop — **R / B**; throw — импульс по взгляду
- [ ] **6.8.** [C++] Коллизия при grab: переключать между `SetSimulatePhysics(false)` для удержания и `true` при броске; не застревать в стенах (offset от камеры)
- [ ] **6.9.** [Опционально] Штабелирование объектов (stack stability)

**Graybox:** зона Props (5–10 объектов разных размеров и материалов) на `L_Deb_Building_Graybox`; среди них минимум один объект за верхним порогом массы (несдвигаемый) и один между порогами (только толкается).

**Критерий готовности:** поднять, нести, бросить — для пропов ниже порога массы; тяжёлые пропы корректно толкаются или не двигаются; сон/пробуждение; звук удара зависит от поверхности и массы.

---

## Модуль 7: Оружие

**Цель:** базовый класс и **управляемые параметры** для дочерних экземпляров. Hitscan — **следующий модуль**.

**Результат в PIE:** pistol/SMG/shotgun экипируется, показывает анимацию стрельбы и перезарядки, расходует патроны, сухой щелчок при пустом магазине; урона нет.

**Новые концепты:** `UPrimaryDataAsset`, `FireMode` enum, `FAmmoConfig`, `IInterface` для wielder, `AnimMontage` + `AnimNotify`, `UInventoryComponent`.

**Входные условия (обязательны [x]):** 0.7, 0.8, 1.1 (GripPoint сокет), 1.8 (obstruction trace), 3.2 (HealthComponent задел).

**Блокирует:** 8.5 (Fire → hitscan), 11.3 (NPC стреляет), 11.5 (NPC подбирает оружие).

---

### Базовый класс и wielder

- [ ] **7.1.** [C++] `IDebWeaponWielder`: `GetMuzzleSocket()`, `GetEjectSocket()`, `GetGripSocket()`, `CanUseWeapon()`
- [ ] **7.2.** [C++] `ADebWeaponBase`: attach, drop, reload — **без** line trace (урон — модуль 8)
  > ИИ: объяснить почему оружие — Actor, а не Component; паттерн AttachToActor vs AttachToComponent.
- [ ] **7.3.** [C++] `AttachToWielder(IDebWeaponWielder*)` → сокет `GripPoint`

### UDebWeaponConfig (Data Asset)

- [ ] **7.4.** [C++] `UDebWeaponConfig : UPrimaryDataAsset`: `FireMode` (Single / Auto / Burst / Shotgun)
  > ИИ: объяснить UPrimaryDataAsset vs UDataAsset; зачем Data Asset вместо хардкода параметров в Blueprint; как загружать DA в C++.
- [ ] **7.5.** [C++] Параметры стрельбы: `BaseDamage`, `MaxRange`, `SpreadAngle`, `PelletCount`, `PelletSpread`, `RateOfFire`, `BurstCount`
- [ ] **7.6.** [C++] Параметры пробития: `PenetrationPower`, `MaxPenetrations`, `RicochetChance`, `MaxRicochets`
- [ ] **7.7.** [C++] `FAmmoConfig`: `MagazineSize`, `MaxReserveAmmo`, `ReloadDuration`
- [ ] **7.8.** [C++] VFX/SFX refs: muzzle flash NS, eject NS, fire sound, reload sound, dry fire
- [ ] **7.9.** [C++] `CurrentMagAmmo`, `ReserveAmmo`, `bIsReloading`; dry fire click при пустом магазине

### Дочерние варианты (Blueprint)

- [ ] **7.10.** [Blueprint] `BP_DebWeapon_Pistol` — Single, низкий spread, `DA_DebWeaponConfig_Pistol`
- [ ] **7.11.** [Blueprint] `BP_DebWeapon_SMG` — Auto, высокий RoF, `DA_DebWeaponConfig_SMG`
- [ ] **7.12.** [Blueprint] `BP_DebWeapon_Shotgun` — Shotgun, `PelletCount` 8–12, широкий `PelletSpread`
- [ ] **7.13.** [Редактор] AnimMontage reload + `AnimNotify` `OnMagazineInserted` для смены mag

### Обоймы, эффекты, инвентарь

- [ ] **7.14.** [C++] `AMagazineActor` — physics drop при reload; collision на полу (подбирать нельзя — мусор)
- [ ] **7.15.** [Редактор/C++] Niagara гильзы на `Eject` сокет (анимация выстрела без урона)
- [ ] **7.16.** [C++] `FItemData`, `UInventoryComponent`; подбор / drop / swap; у NPC — `CurrentWeapon`
- [ ] **7.17.** [C++] Input: Fire **ЛКМ / RT**; Reload **R / X**; swap **1-2-3 / D-Pad**
- [ ] **7.18.** [C++] Obstruction: связать 1.8 с mesh оружия; AnimBP `ObstructionAlpha` — опускать оружие у стены
- [ ] **7.19.** [C++] Визуальная отдача (camera pitch impulse) и visual spread (случайный spread без урона до модуля 8)
- [ ] **7.20.** [C++] `IFireable` / `UFireComponent` — задел для NPC (модуль 11): единый интерфейс стрельбы
- [ ] **7.21.** [Опционально][C++] ADS (прицеливание) — FOV + camera offset
- [ ] **7.22.** [Опционально][C++] Weapon sway — покачивание при движении
- [ ] **7.23.** [C++] Пул декалей (object pool): создать пул, активировать в модуле 8

**Graybox:** создать `L_Deb_CombatLane` — полигон стрельбы; оружие на полу, reload, gating, **без урона**.

**Критерий готовности:** pistol/SMG/shotgun configs; аним/VFX/ammo; Fire() вызывается но hitscan не подключён.

---

## Модуль 8: Hitscan и баллистика

**Цель:** подключить физику урона к **готовому** `ADebWeaponBase`. Тестировать на экземплярах из модуля 7.

**Результат в PIE:** выстрел из pistol/SMG/shotgun наносит урон мишени; shotgun — несколько лучей; пробитие пробивает тонкую стену; рикошет отскакивает; декаль на поверхности.

**Новые концепты:** `UGameInstanceSubsystem`, `FHitResult`, `WorldType::SweepMultiByChannel`, пробитие через множественные traces, пул декалей, `DrawDebugLine`.

**Входные условия (обязательны [x]):** 7.2 (WeaponBase), 7.4–7.6 (Config с параметрами), 7.9 (ammo), 3.1–3.4 (DamageSystem), 5.1–5.6 (physmat для декалей), 5.10 (DataTable реакций).

**Блокирует:** 9.3 (hitscan → fracture), 11.3 (NPC Fire()), 17.19 (debug hitscan).

---

- [ ] **8.1.** [C++] `UDebHitscanProfile` и/или поля в `UDebWeaponConfig` для профиля трассировки
- [ ] **8.2.** [C++] `FDebHitscanRequest` (откуда, куда, кто стрелял) / `FDebHitscanResult` (цепочка попаданий)
  > ИИ: объяснить зачем отдельные struct для запроса и результата; immutable request pattern.
- [ ] **8.3.** [C++] `UDebHitscanSubsystem::ExecuteHitscan()` — луч → `ApplyPointDamage` → пробитие → рикошет → повтор
- [ ] **8.4.** [C++] `UPhysicalMaterial`: добавить `PenetrationCost`, `bCanRicochet` — расширение модуля 5
- [ ] **8.5.** [C++] `ADebWeaponBase::Fire()` → читает `UDebWeaponConfig` → передаёт в `UDebHitscanSubsystem`
- [ ] **8.6.** [C++] **Shotgun:** `PelletCount` × trace со случайным `SpreadAngle` / `PelletSpread`
- [ ] **8.7.** [C++] **Auto/Burst:** timer / gate по `RateOfFire`, `BurstCount`
- [ ] **8.8.** [C++] Эффекты попадания: Niagara из пула (7.23), декаль по physmat из DataTable (5.10)
- [ ] **8.9.** [C++] `IDebFireable` реализован и игроком, и NPC — один путь стрельбы
- [ ] **8.10.** [C++] Интеграция с `IDebDamageable` / `UDebHealthComponent` (модуль 3)
- [ ] **8.11.** [C++] Тестовые мишени + консольная команда `DebugFire [weapon_type] [count]`
- [ ] **8.12.** [C++] Debug: `DrawDebugLine` всех сегментов trace (hitscan, пробитие, рикошет); hit sphere

**Graybox:** полный боевой цикл на `L_Deb_CombatLane` — pistol, SMG, shotgun наносят урон мишеням.

**Критерий готовности:** три типа оружия наносят урон; shotgun — несколько лучей; пробитие/рикошет работают; debug визуализация.

---

## Модуль 9: Разрушаемые объекты

**Цель:** Chaos Geometry Collection **после** рабочего hitscan.

**Результат в PIE:** стекло разбивается от выстрела; стена крошится при накоплении урона; обломки падают с физикой; засыпают через несколько секунд.

**Новые концепты:** `UGeometryCollectionComponent`, Chaos Physics, `AGeometryCollectionActor`, fracture threshold, Chaos Sleep/Wake.

**Входные условия (обязательны [x]):** 8.3 (ExecuteHitscan), 8.10 (ApplyPointDamage), 5.6 (PM_DebGlass), 5.10 (DataTable для VFX).

**Блокирует:** 17.11 (cleanup обломков).

---

- [ ] **9.1.** [C++/Редактор] `ADebDestructibleActor` обёртывает `UGeometryCollectionComponent`; реализует `IDebDamageable`
  > ИИ: объяснить pipeline Chaos GC: Static Mesh → Fracture Mode → Geometry Collection; основные настройки кластеров.
- [ ] **9.2.** [C++] Пороги урона: `DamageThreshold` → накопление → fracture по кластерам
- [ ] **9.3.** [C++] `ExecuteHitscan` → `ApplyPointDamage` на `ADebDestructibleActor` → GC fracture
- [ ] **9.4.** [C++] Collision после fracture: debris получает `Deb_Destructible`; COL.6 — не блокирует pawn навсегда
- [ ] **9.5.** [C++] VFX/SFX разрушения: Niagara (дым, осколки) + звук по physmat из DataTable (5.10)
- [ ] **9.6.** [Опционально] Опора: разрушение объекта A → структурный коллапс B
- [ ] **9.7.** [C++] Очистка / Chaos Sleep обломков через N секунд → `SetSimulatePhysics(false)` или destroy

**Graybox:** destructible-секция на `L_Deb_CombatLane` (v5): стекло, тонкая стена, ящик.

**Критерий готовности:** выстрел ломает объект; осколки падают; эффекты по материалу; засыпают через N сек.

---

## Модуль 10: NPC — тело, Control Rig, ragdoll

**Цель:** физическое «тело» NPC **до** логики ИИ — hit reactions, ragdoll, Control Rig Physics.

**Результат в PIE:** NPC стоит на карте; выстрел в голову даёт больше урона; попадание в корпус вызывает hit react; смерть → ragdoll с правильным импульсом от пули.

**Новые концепты:** Physics Asset, `UPhysicalAnimationComponent`, Control Rig Physics nodes, `BlendPhysics`, `EPhysicsBlendState`, bone-based damage multipliers.

**Входные условия (обязательны [x]):** 3.1–3.4 (DamageSystem), 8.3 (hitscan с FDamageInfo), 6.3 (physics sleep задел).

**Блокирует:** 11.1 (AIController на готовом NPC), 11.9 (BT блокировка при Stagger).

---

- [ ] **10.1.** [C++] `ADebEnemyCharacter : ACharacter` + `UDebHealthComponent` + `IDebDamageable`
- [ ] **10.2.** [Редактор/C++] Physics Asset: настройка тела (голова, торс, конечности); damage multipliers по bone; **COL.5**
  > ИИ: объяснить Physics Asset: constraints, angular limits, collision между костями; почему капсула персонажа отдельна от hitbox'ов.
- [ ] **10.3.** [C++] `TakeDamage` / `ReceivePointDamage` → lookup по `BoneName` → умноженный урон
- [ ] **10.4.** [C++] `IDebWeaponWielder` на NPC — держит `ADebWeaponBase` в `GripPoint`
- [ ] **10.5.** [Редактор] Control Rig: Physics nodes — физическое поведение частей тела поверх анимации
- [ ] **10.6.** [Редактор] Mass, damping, angular limits по сегментам (голова легче торса)
- [ ] **10.7.** [C++] `UDebEnemyPhysicsRigComponent`: методы `ApplyHitImpulse(FName Bone, FVector Impulse)`, `ApplyRadialImpulse`, `EnterRagdoll(FVector DeathImpulse)`, `BlendToAnimated(float BlendTime)`
- [ ] **10.8.** [C++] Hit react: `BlendPhysics` на поражённый сегмент → постепенный возврат в анимацию (`FTimerHandle`)
- [ ] **10.9.** [C++] Столкновение NPC с `ADebPhysicalPropActor` → `ApplyRadialImpulse` на проп
- [ ] **10.10.** [C++] `EPhysicsBlendState`: `Animated / HitReact / Stagger / Ragdoll` — FSM для тела
- [ ] **10.11.** [C++] Ragdoll on death: `EnterRagdoll` + контекстный импульс от `FDamageInfo.HitDirection` на кость смерти
- [ ] **10.12.** [Опционально] Death-poses через Control Rig (поза трупа по ситуации)
- [ ] **10.13.** [C++] Ragdoll sleep через N сек: фиксировать pose, `SetSimulatePhysics(false)`, tick off
- [ ] **10.14.** [Опционально][C++] `UPhysicalAnimationComponent` fallback если Control Rig Physics (Beta в UE 5.8) даёт нестабильное поведение в твоих сценах
- [ ] **10.15.** [Опционально][Редактор/C++] Foot IK для NPC: Control Rig → Two Bone IK на ногах; ступни ставятся на поверхность при движении по неровному полу; `FFootIKData` (позиция, нормаль) обновляется через LineTrace вниз от каждой кости стопы раз в такт
- [ ] **10.16.** [Опционально][Редактор] Motion Matching для NPC: включить плагин `Pose Search`; собрать `UPoseSearchDatabase` с анимациями (Idle, Walk, Run, Alert, Crouch) с аннотациями траектории; заменить Blend Spaces на ноду `Motion Matching` в AnimGraph; `FPoseSearchTrajectoryData` — прогноз траектории из `UCharacterMovementComponent`
  > ИИ: объяснить чем Motion Matching отличается от Blend Space: вместо ручных переходов система сама ищет наиболее подходящую позу по траектории и скорости; Pose Search plugin доступен с UE 5.4+ как production-ready.

**Graybox:** dummy NPC на `L_Deb_CombatLane` — тест: стрельба → hit zones → ragdoll; **без Behavior Tree**.

**Критерий готовности:** тело реагирует на попадания по зонам; ragdoll с импульсом; physics sleep; blend обратно в анимацию.

---

## Модуль 11: NPC — perception, stealth, ИИ

**Цель:** полноценное поведение охотника **после** готового тела (10).

**Результат в PIE:** 1–4 NPC патрулируют этаж; замечают игрока по зрению или звуку; преследуют; стреляют; подбирают оружие с пола; stealth через crouch + darkness снижает обнаружение.

**Новые концепты:** `UAIPerceptionComponent`, `UAISense_Sight`, `UAISense_Hearing`, Behavior Tree, Blackboard, `UBTTask`, `UBTService`, `UBTDecorator`, EQS.

**Входные условия (обязательны [x]):** 10.1–10.13 (тело NPC), 8.9 (IFireable), 7.16 (Inventory + weapon pickup), 1.9 (VisibilityComponent на игроке).

**Блокирует:** 15.7 (PCG spawns для NPC), 17.10 (оптимизация AI perception).

---

### Сценарий «охота в здании»

- [ ] **11.0.** [C++/Редактор] Spawner / GameMode: 1–4 NPC; начальные patrol points
- [ ] **11.0b.** [C++] Blackboard ключи: `PlayerLocation`, `LastKnownLocation`, `AIState` (Idle/Search/Chase/Combat), `SpottedWeapon`

### Базовый ИИ

- [ ] **11.1.** [C++] `ADebEnemyAIController : AAIController` + Behavior Tree `BT_DebHunter`
  > ИИ: объяснить архитектуру BT: Selector / Sequence / Task / Decorator / Service; Blackboard как shared memory.
- [ ] **11.2.** [Редактор] BT-Tasks: `MoveTo`, `Wait`, `ShootAtTarget`, `Patrol`
- [ ] **11.3.** [C++] NPC `Fire()` через `ADebWeaponBase` + hitscan (8) — тот же путь что и у игрока
- [ ] **11.4.** [C++/Blueprint] `ADebTurretActor` через `IFireable` — неподвижный стрелок
- [ ] **11.5.** [C++] Perception для `ADebWeaponBase` / `AMagazineActor` (tag `WeaponPickup`)
- [ ] **11.6.** [C++] Blackboard `SpottedWeapon`; BT Service `BTService_ScanForWeapons`
- [ ] **11.7.** [Редактор] BT-Tasks: `FindWeapon`, `MoveToWeapon`, `PickUpWeapon`
- [ ] **11.8.** [C++] NPC без оружия → найти ближайшее; низкий mag → reload или смена на поднятое
- [ ] **11.9.** [C++] Блокировка BT во время `Stagger` (из модуля 10) через Decorator или BT Service
- [ ] **11.10.** [Опционально] EQS укрытия: `EnvQueryContext_Cover`, поиск позиции за укрытием
- [ ] **11.11.** [Опционально] Тревога между NPC: `UGameplayMessageSubsystem` или делегат в `GameState.bAlertActive`

### Perception — зрение и слух

- [ ] **11.12.** [C++/Редактор] Sight origin на кости `head`; `UAIPerceptionComponent` + `UAISenseConfig_Sight`
- [ ] **11.13.** [C++] LineTrace к `Head` + `Body` игрока; OR-логика (виден если любая точка)
- [ ] **11.14.** [Редактор] Параметры sight: `SightRadius`, `LoseSightRadius`, угол, stale time
- [ ] **11.15.** [C++] Perception interval 0.1–0.2 с; stagger между NPC (не все обновляются в один тик)
- [ ] **11.16.** [C++] `UDebNoiseEmitterComponent`: Walk / Sprint / Land / Fire — разные `Loudness`
- [ ] **11.17.** [C++] `UAISense_Hearing`: `MakeNoise`; sprint громче walk; crouch тихо
- [ ] **11.18.** [C++] Выстрел → noise на весь этаж + `GameState.bAlertActive = true`
- [ ] **11.19.** [C++] Crouch снижает noise signature; физические пропы как частичное укрытие
- [ ] **11.20.** [Опционально] Зоны тьмы: `ADebDarkZoneVolume` снижает sight radius внутри
- [ ] **11.21.** [C++] Debug: `showdebug AI`; cone зрения; линии к `Head`/`Body` игрока; подсветка спотченного оружия

### Геймпад

- [ ] **11.22.** [Опционально][C++] Aim assist (только геймпад): magnetism / slowdown при прицеливании
- [ ] **11.23.** [C++] Настройки aim assist → `UDebSettingsWidget` (модуль 13)
- [ ] **11.24.** [C++] Playtest 15 мин → подробная запись в `LEARNING.md`: что работает, что нет, что нужно поправить

**Graybox:** полный этаж v6 — 1–4 охотника, stealth, перестрелка, win condition.

**Критерий готовности:** NPC патрулирует, ищет, стреляет, подбирает оружие; stealth через crouch/noise; `EDebWinCondition` срабатывает.

---

## Модуль 12: UI — HUD, меню, настройки, загрузка

**Цель:** полный UI-слой: в игре, пауза, главное меню, настройки KB+геймпад, loading screen.

**Результат в PIE:** меню → New Game → loading screen → игра; Esc/Start — пауза; настройки сохраняются между сессиями.

**Новые концепты:** UMG (`UUserWidget`), `UWidgetComponent`, Widget Blueprints, `FSlateColor`, `SoundMix` / `SoundClass`, `FShaderPipelineCache`, async level loading.

**Входные условия (обязательны [x]):** 2.1 (GameInstance для переходов), 2.4 (PlayerController), 3.2 (HealthComponent делегаты).

**Блокирует:** 13.2 (Sound Mix settings), 14.12 (Music volume settings), 16.3 (Load game → OpenLevel), 17.4 (PSO на loading screen).

---

### HUD (в игре)

- [ ] **12.1.** [C++] Подключить `UMG`, `Slate`, `SlateCore` в `Build.cs`
- [ ] **12.2.** [C++/Редактор] `UDebHUDWidget`: прицел, патроны (`30 / 120`), здоровье, подсказка взаимодействия, индикатор перезарядки
- [ ] **12.3.** [C++] Подписка на делегаты: `HealthComponent.OnDamaged`, `InventoryComponent.OnAmmoChanged`, `InteractionComponent.OnTargetChanged`
- [ ] **12.4.** [C++] Индикатор направления урона (hit marker по yaw угла атаки)
- [ ] **12.5.** [Опционально] Маркер попадания (crosshair flash при hit)
- [ ] **12.6.** [C++] Экран смерти: рестарт / загрузка главного меню

### Главное меню

- [ ] **12.7.** [C++/Редактор] `UDebMainMenuWidget`: Новая игра, Продолжить, Настройки, Выход
- [ ] **12.8.** [C++] Уровень `L_Deb_MainMenu` (пустой уровень, только UI + ambient)
- [ ] **12.9.** [C++] `ADebGameInstance`: `GoToMainMenu()`, `StartNewGame()`, `LoadGame()`

### Меню паузы

- [ ] **12.10.** [C++/Редактор] `UDebPauseMenuWidget`: Продолжить, Настройки, В главное меню, Выход
- [ ] **12.11.** [C++] `SetGamePaused(true)`, `SetInputMode UIOnly`, показать курсор
- [ ] **12.12.** [C++] `IA_DebPause`: **Esc / Start (геймпад)**

### Настройки

- [ ] **12.13.** [C++/Редактор] `UDebSettingsWidget` с вкладками: Звук, Графика, Управление
- [ ] **12.14.** **Звук:** Master / SFX / Music / UI; `USoundMix` + `USoundClass`
- [ ] **12.15.** **Графика:** Scalability; пресеты `Deb_High1440p`, `Deb_High1080p`, `Deb_SteamDeck`; разрешение, VSync, FOV
- [ ] **12.16.** **Управление:** чувствительность мыши и правого стика; инверсия Y; выбор кривых стиков; aim assist on/off
- [ ] **12.17.** [Опционально] Переназначение клавиш / кнопок геймпада (`IMC` rebinding API)
- [ ] **12.18.** [C++] Применить / Сброс / Назад → сохранить в `GameInstance` и `SaveGame`

### Экран загрузки

- [ ] **12.19.** [C++/Редактор] `UDebLoadingScreenWidget`: прогресс-бар, статус («Загрузка…», «Компиляция шейдеров…»)
- [ ] **12.20.** [C++] Логика в `GameInstance`: виджет до завершения загрузки + PSO batch
- [ ] **12.21.** [C++] Асинхронная загрузка уровня + `GetAsyncLoadPercentage()`
- [ ] **12.22.** [C++] Переходы: меню → уровень; A → B; загрузка save (16); PCG seed regen (15)
- [ ] **12.23.** [C++] Минимальное время показа (anti-flicker, мин. 1 сек)
- [ ] **12.24.** [C++] Прогресс `FShaderPipelineCache` → модуль 17
- [ ] **12.25.** [Опционально] Советы / lore на экране загрузки

**Критерий готовности:** меню → игра через loading screen; пауза; настройки KB+геймпад сохраняются между сессиями.

---

## Модуль 13: Продвинутый звук (SFX)

**Цель:** от базовых шагов (модуль 5) к физически правдоподобному пространственному звуку.

**Результат в PIE:** выстрел за стеной звучит приглушённо; выстрел в открытом пространстве — гулко; звук оружия слоистый (near/far); настройки громкости работают.

**Новые концепты:** `USoundAttenuation`, `USoundConcurrency`, occlusion через LineTrace + LPF, `UAudioVolume`, MetaSounds Graph для процедурного звука.

**Входные условия (обязательны [x]):** 5.1–5.4 (physmat + base звук), 7.8 (SFX refs в WeaponConfig), 12.14 (Sound Mix + Sound Class).

**Блокирует:** 14.1 (Music Subsystem → Sound Mix).

---

- [ ] **13.1.** [Редактор] Sound Mix и Sound Class иерархия: Master → SFX / Music / UI / Voice
- [ ] **13.2.** [C++] Связь `UDebSettingsWidget` (12.14) с `SetSoundMixClassOverride`
- [ ] **13.3.** [C++] Attenuation: `USoundAttenuation` для выстрелов — falloff curve, reverb, air absorption
  > ИИ: объяснить разницу Attenuation Asset vs Attenuation Settings override; spatial blend 2D vs 3D.
- [ ] **13.4.** [C++] Occlusion / obstruction: LineTrace между источником и слушателем → `SetLowPassFilterFrequency` за стеной
- [ ] **13.5.** [C++] Звук оружия послойно (MetaSound): near crack, far tail, reload, dry fire
- [ ] **13.6.** [C++] Процедурные звуки ударов пропов — улучшение 6.4: pitch + volume от скорости удара
- [ ] **13.7.** [Редактор] MetaSounds для попаданий по поверхностям: random из набора + pitch рандом
- [ ] **13.8.** [Blueprint/C++] `AAudioVolume`: реверберация в комнате, эмбиент в коридоре
- [ ] **13.9.** [Опционально] Слух NPC как отдельная реализация — уточнение от 11.17

**Критерий готовности:** выстрелы с атенюацией и occlusion; громкость из настроек; MetaSound для попаданий.

---

## Модуль 14: Динамический саундтрек

**Цель:** музыка реагирует на состояние игры — exploration/tension/combat.

**Результат в PIE:** при виде NPC музыка переходит в tension; при бое — combat слой с перкуссией; после боя — fade out в exploration.

**Новые концепты:** `UGameInstanceSubsystem` для музыки, слоёная музыка MetaSound, crossfade через `FInterpTo`, state machine для музыки.

**Входные условия (обязательны [x]):** 12.14 (Sound Class для музыки), 13.1 (Sound Mix), 11.18 (bAlertActive в GameState).

**Блокирует:** нет (финальная система).

---

- [ ] **14.1.** [C++] `UDebMusicSubsystem : UGameInstanceSubsystem` — менеджер музыки; живёт как GameInstance
- [ ] **14.2.** [C++] `EDebMusicState`: `MainMenu / Exploration / Tension / Combat / Death / Victory`
- [ ] **14.3.** [C++] Подписка на события: `OnEnemySpotted`, `OnCombatStarted`, `OnCombatEnded`, `GameState.bAlertActive`
- [ ] **14.4.** [Редактор] MetaSounds / слои: base ambient + tension layer + combat percussion
- [ ] **14.5.** [C++] Логика слоёв: percussion включается при `EnemiesInCombat > 0`; tension при `bAlertActive`
- [ ] **14.6.** [C++] Crossfade при смене состояния через `FInterpTo` на `VolumeMultiplier`
- [ ] **14.7.** [C++] Fade in / Fade out при входе в уровень / паузе / смерти
- [ ] **14.8.** [C++] Приоритеты: Combat > Tension > Exploration; cooldown после боя
- [ ] **14.9.** [C++] Hysteresis для "Combat ends": не переключать мгновенно, выждать 5 сек тишины
- [ ] **14.10.** [Редактор] Placeholder-музыка по состояниям (royalty-free или generated)
- [ ] **14.11.** [C++] Debug: on-screen overlay `EDebMusicState` + активные слои (модуль 17.25)
- [ ] **14.12.** [C++] Громкость Music из настроек через Sound Class (12.14)

**Критерий готовности:** музыка плавно реагирует на Combat/Exploration/Pause; fade без щелчков.

---

## Модуль 15: PCG — процедурные интерьеры

**Цель:** PCG как **неотъемлемая часть геймплея** — seed-driven layout здания.

**Результат в PIE:** смена seed в GameMode генерирует другой этаж; NavMesh пересчитывается; NPC работают на PCG-уровне; полный stealth-combat loop.

**Новые концепты:** PCG Graph, `UPCGComponent`, PCG Grid, модульные mesh-сеты, seed-driven generation, NavMesh rebuild после PCG.

**Входные условия (обязательны [x]):** все обязательные пункты модулей 0–11.

**Блокирует:** 17.5 (NavMesh rebuild), 18.1 (art pass поверх PCG).

---

### Граф и контент

- [ ] **15.1.** [Редактор] PCG plugin в `Build.cs`; PCG Graph: grid / spline → пол + стены + потолок
  > ИИ: объяснить ноды PCG Graph: Point Sampler, Spawn Static Mesh, Filter; связь с Spline Actor для коридоров.
- [ ] **15.2.** [Редактор] Модульные mesh: `SM_DebWall_4x3`, `SM_DebWall_Door`, `SM_DebWall_Corner`, `SM_DebStairs`
- [ ] **15.3.** [Редактор] Правила генерации: мин. размер комнаты, связность коридоров, высота под crouch
- [ ] **15.4.** [Редактор] Collision-ready static meshes (Nanite + simplified collision); **COL.7** NavMesh rebuild после generate

### Интеграция с геймплеем

- [ ] **15.5.** [C++/Редактор] `L_Deb_PCG_Demo` — **основная demo-карта** (заменяет ручной Graybox)
- [ ] **15.6.** [C++] Seed в `ADebGameMode` / `ADebGameState` — `RegenerateLevel(int32 Seed)`
- [ ] **15.7.** [Редактор] PCG spawns: `PlayerStart`, NPC spawn points, weapon pickup, patrol spline points
- [ ] **15.8.** [C++] `ADebGameMode`: параметры сложности (кол-во комнат, 1–4 NPC) передаются в PCG graph через параметры
- [ ] **15.9.** [C++] После PCG generate: validate NavMesh, инициализировать patrol points, установить `AliveHunterCount`
- [ ] **15.9b.** [C++] Интеграция лифта с PCG: `ADebElevatorActor::OnPlayerEscaped` → `ADebGameMode::RegenerateLevel(NewSeed)`; новый seed → пересборка PCG-графа → новый этаж → spawn игрока у нового `PlayerStart`
- [ ] **15.10.** [Редактор] Сравнить PCG vs ручной Graybox по gameplay; PCG — default для demo
- [ ] **15.11.** [Опционально][C++] Runtime regenerate по seed через dev menu (консольная команда `DebSeed [N]`)
- [ ] **15.12.** [Опционально] Production PCG-граф: процедурные варианты здания (офис, склад, больница)
- [ ] **15.13.** [Редактор] Регресс-чек: HUD/меню/настройки (12), occlusion и атенюация звука (13), смена музыкальных состояний (14) корректно работают на PCG-карте `L_Deb_PCG_Demo`, не только на ручном graybox

**Критерий готовности:** seed-driven здание; NavMesh; 1–4 NPC работают; полный stealth-combat loop на PCG-карте; UI/звук/музыка регресс-проверены на PCG-карте.

---

## Модуль 16: Сохранения

**Цель:** настройки и прогресс сохраняются между сессиями.

**Результат в PIE:** настройки графики/звука применяются после перезапуска; «Продолжить» возобновляет игру с последнего чекпоинта.

**Новые концепты:** `USaveGame`, `UGameplayStatics::SaveGameToSlot`, сериализация игрового состояния, async save.

**Входные условия (обязательны [x]):** 12.18 (Settings → SaveGame), 2.1 (GameInstance), 15.6 (PCG seed).

**Блокирует:** нет (поздний модуль).

---

- [ ] **16.1.** [C++] `UDebSaveGame : USaveGame`: настройки (12) + прогресс игрока
- [ ] **16.2.** [C++] Чекпоинты: позиция игрока, здоровье, инвентарь, ключевые объекты (двери), PCG seed
  > ИИ: объяснить что сохранять, а что пересоздавать: PCG regenerate по seed лучше чем сохранять весь уровень.
- [ ] **16.3.** [C++] «Продолжить» → `UGameplayStatics::LoadGameFromSlot` → `GameInstance.OpenLevelWithLoading()`
- [ ] **16.4.** [Опционально] Автосохранение на триггерах (вход в лифт, смена этажа)
- [ ] **16.5.** [Опционально] Несколько слотов сохранения

**Критерий готовности:** настройки и прогресс сохраняются; «Продолжить» работает.

---

## Модуль 17: Производительность, PSO, отладка

**Цель:** стабильный FPS, отсутствие shader hitch'ей, покрытие всех систем debug-инструментами.

> Сквозные практики — с модуля 0. После каждого модуля: `stat unit` → baseline в `LEARNING.md`.

**Результат:** ≥ 60 FPS @ 1440p на Reference PC на stress-сцене; нет shader hitch при первой стрельбе; debug команды покрывают все системы.

**Новые концепты:** `FShaderPipelineCache`, PSO precompile, Nanite, Lumen scalability, Unreal Insights, `stat unit` / `stat gpu`, `obj list`.

**Входные условия (обязательны [x]):** все модули 0–16 завершены.

**Блокирует:** 18.5 (финальный FPS-тест).

---

### Референсное железо и цели FPS

| Платформа | Железо / режим | Цель |
| ---------------- | ------------- | ---- |
| **Reference PC** | RTX 5070, i5-14600KF, 32 GB DDR5, **native 1440p**, пресет `Deb_High1440p` | **≥ 60 FPS** стабильно |
| **Addition PC** | RTX 3060, i5-12400F, 32 GB DDR4, **native 1080p**, пресет `Deb_High1080p` | **≥ 60 FPS** стабильно |
| **Steam Deck** | пресет `Deb_SteamDeck` (отдельный Scalability + FSR) | **≥ 30 FPS** (цель 40+) |

**Stress-сцена:** demo-здание (PCG или Graybox) — 20+ props, до 4 NPC, активная стрельба, 2–3 ragdoll, destructibles.

- [ ] **17.0.** [Редактор/C++] Пресет `Deb_High1440p`: Scalability для Reference PC
- [ ] **17.0b.** [Редактор/C++] Пресет `Deb_SteamDeck`: сниженные shadows, Lumen quality, resolution scale / FSR
- [ ] **17.0c.** [C++] Автовыбор пресета при обнаружении Steam Deck (или ручной выбор в Settings)

### PSO Pipeline Cache

- [ ] **17.1.** [Редактор] `r.ShaderPipelineCache.Enabled=1`, `SaveUserCache=1` (задел — 0.20)
- [ ] **17.2.** [Редактор] Сбор `.upipelinecache` в dev: пройти все системы (стрельба, VFX, Niagara, Quixel)
- [ ] **17.3.** [Редактор] Чеклист ассетов для прогрева: материалы, Niagara, post-process, Quixel Megascans
- [ ] **17.4.** [C++] Runtime-прогрев на loading screen (13.24): `FShaderPipelineCache::OpenPipelineFileCache`
- [ ] **17.5.** [C++] Не скрывать loading screen до PSO batch (таймаут 10 сек max)
- [ ] **17.6.** [Редактор] Packaged build: нет hitch при первой стрельбе / первом VFX

### Практики производительности

- [ ] **17.7.** Tick off где не нужен; пропы в sleep — tick off (6.3)
- [ ] **17.8.** Object pooling: декали, Niagara emitters, hitscan VFX (7.23 + 8.8)
- [ ] **17.9.** LineTrace budget: interaction (0.1 с), footstep (каждый шаг), obstruction (0.05 с), AI (0.1–0.2 с) — разные интервалы
- [ ] **17.10.** AI perception: stagger между NPC; `SetActorTickInterval` для далёких NPC
- [ ] **17.11.** Лимит simulating props (макс 20); ragdoll sleep (10.13); Chaos GC cleanup (9.7)
- [ ] **17.12.** [Опционально] Off-screen Control Rig: отключить для NPC вне frustum
- [ ] **17.13.** Niagara: `MaxParticleCount`, GPU sim для большого числа частиц
- [ ] **17.14.** Nanite для static meshes (Quixel Megascans); ограничить shadow-casting dynamic lights
- [ ] **17.15.** Аудио: polyphony limit через `USoundConcurrency`; attenuation cutoff
- [ ] **17.16.** Память: `obj list class=Texture2D` в консоли; Unreal Insights memory track
- [ ] **17.17.** Baseline `stat unit` (Game, Draw, GPU) после **каждого** модуля → `LEARNING.md`

### Debug по системам

- [ ] **17.18.** [C++] `bShowDebug` / console `ToggleDebug [system]`
- [ ] **17.19.** Hitscan: `DrawDebugLine` всех сегментов (8.12)
- [ ] **17.19b.** Оружие: debug mag/reserve/reload state; Muzzle/Eject сокеты (7)
- [ ] **17.20.** Interaction: trace line + `CurrentTarget` текст (4)
- [ ] **17.21.** AI: cone зрения от головы; линии к Head/Body; текущий `AIState` BB ключ (11)
- [ ] **17.22.** Footsteps / obstruction: physmat под ногами (1, 5)
- [ ] **17.23.** Physics: счётчик `ActiveSimulatingBodies`; sleep/wake events log
- [ ] **17.24.** [Опционально] Floating damage numbers над хитбоксами
- [ ] **17.25.** Music state on-screen + активные слои (15.11)
- [ ] **17.26.** [Опционально] Perf overlay UMG: FPS, frame time, physics count, AI count
- [ ] **17.27.** [C++] Console commands: `GodMode`, `SpawnEnemy [count]`, `ToggleDebug`, `StatUnit`, `DebSeed [N]`
- [ ] **17.28.** [Редактор] Unreal Insights: trace полной боевой сессии; найти top 3 CPU bottleneck

### Профилирование и регрессия

- [ ] **17.29.** **Reference PC:** ≥ 60 FPS @ 1440p на stress-сцене; записать Game/Draw/GPU в README
- [ ] **17.30.** **Steam Deck:** играбельный прогон demo end-to-end на `Deb_SteamDeck`; записать FPS и настройки
- [ ] **17.31.** [Опционально] Debug-меню: spawn, teleport, toggle systems (UMG поверх игры)
- [ ] **17.32.** [Опционально] Stress-карта: автоматический тест «fire 100 rounds», «spawn 10 props», «4 NPC active»

**Критерий готовности:** нет shader hitch; debug покрывает все системы; FPS-цели на Reference PC и Steam Deck задокументированы в README.

---

## Модуль 18: Финальная полировка + Quixel Megascans (FAB)

**Цель:** art pass — заменить graybox на Quixel Megascans. Gameplay-код не менять.

**Результат в PIE:** играбельный stealth-shooter demo в здании с Quixel Megascans-артом при ≥ 60 FPS.

**Новые концепты:** Quixel Bridge / FAB импорт, Nanite Megascans, Lumen финальные настройки, PSO final bake, Substrate + Megascans.

**Входные условия (обязательны [x]):** 17.29 (FPS-цели достигнуты), все модули 0–17.

**Блокирует:** ничего (последний модуль).

---

- [ ] **18.1.** [Редактор] `L_Deb_Building_Final`: замена graybox на **Quixel Megascans (FAB)** — стены, пол, двери, мебель
  > ИИ: объяснить workflow FAB → Bridge → UE5 импорт; Nanite auto-enable; LOD стратегия для interior.
- [ ] **18.2.** [Редактор] Collision pass после Megascans (COL.8): simplified collision, нет дыр в стенах
- [ ] **18.3.** [Редактор] Освещение Lumen: локальный свет в коридорах, ambient occlusion, sky light для атриума
- [ ] **18.4.** [Редактор] Substrate / MI production на Quixel Megascans mesh: слои грязи, потёртостей
- [ ] **18.5.** [Редактор] Проверка FPS на Reference PC (1440p) и Steam Deck после art pass (17.29–17.30)
- [ ] **18.6.** [Редактор] Постобработка: кинематографичность перестрелок (film grain, vignette, lens flare)
- [ ] **18.7.** [Опционально] Экранные эффекты при ранении; inspect weapon анимация
- [ ] **18.8.** [Редактор] PSO final bake на финальных материалах, VFX, Megascans освещении
- [ ] **18.9.** README: управление KB+геймпад, сборка проекта, FPS-цели, статус всех модулей

**Критерий готовности:** играбельный stealth-shooter demo в здании с Megascans-артом при целевом FPS.

---

## Build.cs — roadmap

| Модуль | Добавить |
|--------|----------|
| 0 | `EnhancedInput`, `InputCore` |
| 6 | `PhysicsCore`, `Chaos` |
| 8–9 | `Niagara`; `GeometryCollectionEngine` для Chaos GC |
| 10–11 | `AIModule`, `NavigationSystem`, `ControlRig`, `AnimGraphRuntime` |
| 12 | `UMG`, `Slate`, `SlateCore` |
| 15 | plugin `PCG` |

---

## Опциональные фичи

| Пункт | Описание |
|-------|----------|
| 1.10 | Camera bob при ходьбе |
| 1.11 | Фонарик (USpotLightComponent, toggle) |
| 3.5 | Визуальные эффекты ранения |
| 5.15 | Production-библиотека Substrate-материалов |
| 6.9 | Штабелирование пропов (stack stability) |
| 7.21, 7.22 | ADS, weapon sway |
| 9.6 | Структурный коллапс при разрушении |
| 10.12 | Control Rig death poses |
| 10.14 | UPhysicalAnimationComponent fallback |
| 10.15 | Foot IK для NPC (Two Bone IK, LineTrace) |
| 10.16 | Motion Matching для NPC (Pose Search plugin) |
| 11.10, 11.11, 11.20 | EQS укрытия, групповая тревога, зоны тьмы |
| 11.22 | Aim assist tuning |
| 12.5, 12.17, 12.25 | Hit marker, rebind клавиш, loading tips |
| 15.11, 15.12 | Runtime PCG regen, production граф |
| 16.4, 16.5 | Автосохранение, несколько слотов |
| 17.12, 17.24, 17.26, 17.31, 17.32 | Off-screen rig, damage numbers, perf overlay, debug menu, stress map |
| 18.7 | Screen FX, inspect weapon |

---

## Troubleshooting

**Персонаж не спавнится** — `DefaultPawnClass` в GameMode, `PlayerStart` на уровне, `GameMode Override` в World Settings, PIE 1 player.

**Input не работает** — `AddMappingContext` вызван, IMC назначен в BP, для геймпада — `Enable Gamepad` в Project Settings.

**Геймпад: drift / резкий look** — dead zone и `Curve_DebLookResponse` (модуль 1.6).

**Hitscan не попадает** — trace channel `ECC_Deb_Weapon`; ignore owner/wielder; collision на mesh, не только на капсуле.

**NPC не идёт** — `NavMeshBoundsVolume` покрывает уровень; rebuild NavMesh после graybox/PCG.

**Quixel Megascans — проваливаться / стрелять сквозь стены** — simplified collision, COL.8.

**Steam Deck — низкий FPS** — пресет `Deb_SteamDeck` (17.0b); снизить shadows, Lumen quality, resolution scale, FSR.

**Live Coding + новый UCLASS** — после добавления нового `UCLASS` / `USTRUCT` нужна **полная** перекомпиляция, не hot reload.

**PSO hitch при первой стрельбе** — модули 17.1–17.6; прогон всех VFX/материалов до shipping build.

**MSBuild SetEnv ошибка (UE 5.5+)** — `$(IncludePath)` превышает лимит переменных среды Windows; `Ctrl+Shift+B` падает с `MSB4018: SetEnv`. Решение: в `Microsoft.Cpp.Current.targets` найти блок `<SetEnv>` и установить `Value=""`. Обновления VS могут сбросить патч — проверять после каждого обновления IDE. Live Coding и сборки из редактора UE используют UBT напрямую и обходят MSBuild — работают без патча.

---

*Конспекты: `LEARNING.md`*