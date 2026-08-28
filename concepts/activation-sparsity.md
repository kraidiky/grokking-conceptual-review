# Разрежённость (activation and structural sparsity)

[sparse-solutions-hidden-progress](sparse-solutions-hidden-progress.md) ← предыдущая карточка, следующая → [execution-manifold](execution-manifold.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Разрежённость** в корпусе [гроккинга](grokking.md) — общее имя для трёх разных наблюдений: разрежённость в фурье-области (немного частот несут вычисление), разрежённость структуры (немного весов или нейронов участвуют в ответе) и разрежённость отклика (немного активаций отличны от нуля на входе). Их связь и есть содержание понятия: *«Nanda et al. (2023) наблюдают разрежённость в фурье-области после гроккинга, тогда как мы обнаружили её и в обычной структуре сети»* \[[1.1](#ref-1-1)\].

Отсюда рабочая гипотеза линии, сформулированная проверяемо: *«грокнувшие билеты — более разрежённые подсети по сравнению с базовой моделью, и именно эта бо́льшая разрежённость даёт сокращение отложенной генерализации»* \[[1.2](#ref-1-2)\].

## Детализация

**Разрежённость как следствие, а не причина.** Проверка гипотезы устроена честно: строятся модели с той же разрежённостью, что у грокнувшего билета, и смотрят, ускоряют ли они обобщение сами по себе \[[1.2](#ref-1-2)\]. Такой контроль отделяет «разрежённость сопутствует» от «разрежённость вызывает» — различение, которого в большинстве упоминаний нет.

Механизм, которым разрежённость возникает, в корпусе назван **избирательным (селективным) ростом нормы** (selective norm growth).

**Откуда она берётся.** Механизм называется избирательным ростом нормы: не все веса растут одинаково, и часть подсети выделяется на фоне остальных \[[3.1](#ref-3-1)\]. Это связывает разрежённость с линией [нормы весов](weight-decay.md) и объясняет, почему [weight decay](weight-decay.md) и разрежённость упоминаются вместе как кандидаты на объяснение задержки \[[3.2](#ref-3-2)\].

**Разрежённость отклика.** Третье прочтение — про активации: обобщающие сети отвечают меньшим числом нейронов, и вводится мера «эффективной разрежённости», призванная объяснить гроккинг \[[3.3](#ref-3-3)\]. Это ближайшая к [полисемантичности](polysemanticity-superposition.md) форма: чем разрежённее отклик, тем осмысленнее чтение по нейронам.

**Чего не хватает.** В корпусе разрежённость чаще цитируется, чем измеряется во времени: кривых разрежённости рядом с кривыми точности мало, а именно они отличали бы её от [скрытого прогресса](sparse-solutions-hidden-progress.md) как отдельную меру.

## Альтернативные определения и нюансы

### A. Разрежённость в базисе задачи

Немного частот несут вычисление — форма, доступная там, где алгоритм известен \[[1.1](#ref-1-1)\]. Различающая черта — базис задан задачей, а не сетью, и потому мера сравнима между моделями; ограничение — вне модульной арифметики базиса нет.

### B. Структурная разрежённость подсети

Немного весов достаточно для решения; проверяется прунингом и лотерейными билетами \[[1.2](#ref-1-2)\], \[[3.1](#ref-3-1)\]. Источник различия — предмет: не отклик, а связность; следствие — разрежённость измерима без данных, по одной сети.

### C. Разрежённость отклика

Немного активаций отличны от нуля на типичном входе \[[3.3](#ref-3-3)\]. Различающая черта — зависимость от распределения данных: величина определена относительно выборки, и её изменение может отражать сдвиг данных, а не перестройку сети.

## Ссылки

###### ref-1-1
**\[1.1\]** 2303.11873 — Merrill et al., «A Tale of Two Circuits: Grokking as Competition of Sparse and Dense Subnetworks». [`"Nanda et al. (2023) observe sparsity in the Fourier domain after grokking, whereas we have found it in the conventional network structure as well."`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p3-4). *«[Nanda et al. (2023) наблюдают разрежённость в фурье-области после гроккинга, тогда как мы обнаружили её и в обычной структуре сети.](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p3-4)»*

###### ref-1-2
**\[1.2\]** 2310.19470 — Minegishi et al., «Bridging Lottery Ticket and Grokking: Understanding Grokking from Inner Structure of Networks». [`"**Hypothesis 2 (Sparsity):** *Grokked tickets are sparser subnetworks compared to the base model, and this higher degree of sparsity leads to the reduction in delayed generalization.*"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p7-3). *«[**Гипотеза 2 (разрежённость):** *грокнувшие билеты — более разрежённые подсети по сравнению с базовой моделью, и именно эта бо́льшая разрежённость даёт сокращение отложенной генерализации.*](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.card.md#p7-3)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2303.11873 — Merrill et al., «A Tale of Two Circuits: Grokking as Competition of Sparse and Dense Subnetworks». Нюанс: разрежённость выводится из избирательного роста нормы, а не постулируется. [`"Motivated by the discovery of this sparse subnetwork, we now turn our attention to understanding why this subnetwork emerges and its structure."`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p3-4). *«[Побуждаемые открытием этой разрежённой подсети, мы теперь обращаем внимание на то, почему она возникает и какова её структура.](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p3-4)»*

###### ref-3-2
**\[3.2\]** 2310.19470 — Minegishi et al., «Bridging Lottery Ticket and Grokking: Understanding Grokking from Inner Structure of Networks». Нюанс: разрежённость и норма весов идут в корпусе парой как два кандидата на объяснение задержки. [`"While factors such as weight norms and sparsity have been proposed to expla"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2). *«[Хотя для объяснения этой задержки предлагались такие множители, как нормы весов и разрежённость](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.card.md#p1-2)»*

###### ref-3-3
**\[3.3\]** 2405.12755 — Golechha, «Progress Measures for Grokking on Real-world Tasks». Нюанс: третья форма — разрежённость отклика, для которой вводится мера эффективной разрежённости. [`"Li et al. 2022 find sparse MLP layers in networks that generalize well"`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p3-6). *«[Li et al. 2022 находят разрежённые слои MLP у сетей, которые хорошо обобщают](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p3-6)»*
