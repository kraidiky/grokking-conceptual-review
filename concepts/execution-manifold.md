# Многообразие исполнения (execution manifold)

[activation-sparsity](activation-sparsity.md) ← предыдущая карточка, следующая → [polysemanticity-superposition](polysemanticity-superposition.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Многообразие исполнения** — малоразмерная поверхность в пространстве весов, на которой держится траектория обучения: сеть с сотнями тысяч параметров движется фактически по нескольким направлениям. Наблюдение сформулировано как всеобщее свойство: *«траектории оптимизации остаются заперты на эмпирически инвариантном низкоразмерном исполнительном многообразии»* \[[1.1](#ref-1-1)\], а [гроккинг](grokking.md) в этой картине — *«затяжное удержание на низкоразмерном подпространстве пространства весов»*, из которого траектория в конце концов выходит \[[1.2](#ref-1-2)\].

## Детализация

**Как это меряют.** Проще всего — главными компонентами траектории: если первая компонента забирает большую часть дисперсии, движение по существу одномерно. Измерение даёт 68–83% на первую компоненту во всех проверенных условиях гроккинга \[[3.1](#ref-3-1)\], то есть заявка о малоразмерности не качественная, а числовая.

**Что это меняет в объяснении задержки.** Если движение заперто, то долгое плато — не отсутствие движения, а движение в стеснённом направлении; выход из этого режима и есть обобщение. Картина смыкается с [бассейнами ландшафта](loss-landscape-basins.md), но говорит о другом: не о том, в какой яме сидит сеть, а о том, по скольким направлениям она способна двигаться внутри неё.

**Отношение к прочим линиям.** Малоразмерность траектории объясняет, почему одномерные меры вроде нормы весов так хорошо описывают динамику: если движение почти одномерно, любая монотонная координата вдоль него годится в [параметр порядка](order-parameter.md). Это же делает понятие уязвимым: совпадение меры с ходом обучения перестаёт быть свидетельством в пользу механизма, стоящего за мерой.

**Оговорка.** Инвариантность многообразия названа эмпирической: она наблюдается, но не выводится, и проверена на малых трансформерах и модульной арифметике \[[1.1](#ref-1-1)\]. Переносимость на большие модели остаётся открытой — как и у соседних геометрических картин.

## Альтернативные определения и нюансы

### A. Многообразие как наблюдаемая малоразмерность

Операциональная форма: доля дисперсии траектории, забираемая первыми компонентами \[[3.1](#ref-3-1)\]. Различающая черта — величина измеряется прямо и сравнима между прогонами; ограничение — PCA линеен, и криволинейное многообразие он опишет хуже, чем есть на самом деле.

### B. Многообразие как область удержания

Динамическая форма: траектория заперта, и обобщение наступает при выходе \[[1.2](#ref-1-2)\]. Источник различия — предмет объяснения не структура, а событие выхода; отсюда и предсказание, что вмешательства, расширяющие доступные направления, должны сокращать задержку.

### C. Инвариантность как свойство задачи

Третье прочтение — самое сильное: многообразие одно и то же для разных прогонов и задач, то есть определяется задачей, а не случайностью инициализации \[[1.1](#ref-1-1)\]. Различающая черта — проверяется совпадением многообразий между семенами; именно в этом месте картина ближе всего смыкается с вопросом об [универсальности решений](polysemanticity-superposition.md), где согласие между семенами как раз не всегда находится.

## Ссылки

###### ref-1-1
**\[1.1\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». [`"optimization trajectories remain confined to an empirically invariant low-dimensional execution manifold"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p1-3). *«[траектории оптимизации остаются заперты на эмпирически инвариантном низкоразмерном исполнительном мно](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p1-3)»*

###### ref-1-2
**\[1.2\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». [`"We propose that grokking corresponds to prolonged confinement on a low-dimensional subspace in weight space"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2). *«[Мы предполагаем, что гроккингу отвечает затяжное удержание на низкоразмерном подпространстве пространства весов](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p1-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». Нюанс: малоразмерность не качественная, а измеренная — первая главная компонента забирает 68–83% дисперсии траектории. [`"the first principal component captures 68–83% of trajectory variance across all grokking conditions"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p6-7). *«[первая главная компонента улавливает 68–83 % дисперсии траектории во всех грокающих условиях](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p6-7)»*

###### ref-3-2
**\[3.2\]** 2606.17120 — Ersoy & Wiesner, «Noise-Driven Escape from Metastable Phases explains Grokking in Deep Neural Networks». Нюанс: выход из удержания описан как гистерезис при фазовом переходе первого рода по силе L2-сглаживания. [`"grokking is consistent with hysteresis in first-order L2 phase transitions"`](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/original/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.md#p1-2). *«[гроккинг согласуется с гистерезисом при фазовых переходах L2 первого рода](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.card.md#p1-2)»*
