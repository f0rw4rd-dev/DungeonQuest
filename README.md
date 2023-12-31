# DungeonQuest

3D-Roguelike игра на движке Unreal Engine 5.

## Описание алгоритмов генерации комнат и их содержимого

### Генерация комнат

Комнаты представлены в виде *Actor*.
Классы комнат:
- BP_EntryRoom
- BP_Room
- - BP_Room01
- - BP_Room02
- - BP_LastRoom01

Базовый класс для игровых комнат - *BP_Room*.

Дочерние классы представляют собой шаблонные комнаты, в которых расположено большое число статических и динамических ловушек, баффов/дебаффов и прочих объектов.

При создании комнат некоторые ловушки и баффы/дебаффы удаляются с заданной вероятностью (в зависимости от параметров в *DT_GameDifficulties*).

Выбор класса шаблонной комнаты (*BP_Room01* или *BP_Room02*) при вызове *SpawnActor* выполняется случайно. В случае если комната предпоследняя, то следующие две комнаты будут объектами класса *BP_LastRoom01*.

При входе в очередную комнату выполняется генерирование двух следующих, если текущая комната не является последней. Все предыдущие комнаты удаляются.

Логика генерации двух следующих комнат реализована в методе *GenerateNextRooms* в классе *BP_Room*. И практически вся логика, касающаяся генерации комнат, реализована в базовом классе.

### Ловушки

Ловушки представлены двумя типами: *статические* и *динамические*.

Статические ловушки:
- BP_SpikeTrap

Динамические ловушки (движутся по заданной траектории):
- BP_MovingSpikeTrap

### Баффы/дебаффы

В комнатах повышенной сложности имеется бонус в виде сердца, который восстанавливает N очков здоровья. Представлен в виде класса *BP_Bonus_HealthRecovery*.

Баффы/дебаффы имеют форму капсулы. Базовый класс - *BP_PillBonus*.

Баффы:
- BP_PillBonus_Ghost (позволяет проходить сквозь статические ловушки)
- BP_PillBonus_SlowDownDynamicObjects (замедляет динамические ловушки)

Дебаффы:
- BP_PillBonus_SlowDownPlayer (замедляет игрока)

Объект, который может быть как баффом, так и дебаффом (выбирается случайно):
- BP_PillBonus_Random

Баффы и дебаффы действуют определенное время, в зависимости от уровня сложности и параметров, указанных в *DT_GameDifficulties*.

## Описание принципа изменения уровня сложности в вашей игре

Есть 3 параметра, которые влияют на уровень сложности:
- DifficultyFactor
- TrapSpeed
- BonusDuration

*DifficultyFactor* отвечает за количество ловушек и баффов/дебаффов. Значение должно находиться в интервале [0; 1]. При значении равном 0 сложность максимальная (все ловушки и нет баффов), при значении равном 1 сложность минимальная (нет ловушек и есть все баффы).

*TrapSpeed* - "скорость" динамических ловушек. Стандартное значение - 1. Чем больше, тем быстрее ловушки.

*BonusDuration* представляет собой длительность баффов и дебаффов в секундах.

В комнатах повышенной сложности значения *DifficultyFactor* и *BonusDuration* падают, а значение *TrapSpeed* возрастает в зависимости от параметров, указанных в *DT_GameDifficulties*.

## Реализованные дополнения

### Главное меню

Реализовано в *WBP_MainMenu*, имеются следующие кнопки:
- New Game
- Exit

### Меню паузы

Реализовано в *WBP_PauseMenu*, имеются следующие кнопки:
- Continue
- New Game
- Exit

При открытии меню игра ставится на паузу.

Открывается меню на *Escape* или на *P*.

### Примитивная боевая система

Оружие игрока - меч (BP_Sword).

Атака на кнопку ЛКМ, только в стоячем положении. Позволяет ломать ловушки.

Анимация атаки взята с Mixamo.

## Заимствования

### Плагины

[Electronic Nodes](https://www.unrealengine.com/marketplace/en-US/product/electronic-nodes)
- необязателен
- использован для повышения читабельности кода

[UI Navigation 3.0](https://www.unrealengine.com/marketplace/en-US/product/ui-navigation-3)
- обязателен
- использован для главного меню

### Основные ассеты

[Medieval Dungeon](https://www.unrealengine.com/marketplace/en-US/product/a5b6a73fea5340bda9b8ac33d877c9e2)

[Third Person шаблон из Unreal Engine 5](https://www.unrealengine.com/en-US/unreal-engine-5)
