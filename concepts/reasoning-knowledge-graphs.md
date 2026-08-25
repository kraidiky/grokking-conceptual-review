# Рассуждение и графы знаний (reasoning / knowledge graphs)

[Композиция групп / некоммутативность](group-composition-non-commutative-s5.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [3. Задачи и наборы данных](index.md#cat-3)\
→ Следующая категория: [4. Факторы обучения и оптимизации](weight-decay.md)\
← Предыдущая категория: [2. Механизмы и представления](structured-representation-learning.md)

## Определение

**Рассуждение над графами знаний** в контексте гроккинга — это установленная
Wang et al. (2024) связь между отложенной генерализацией и способностью
трансформера к *неявному рассуждению* (implicit reasoning — вывод новых фактов
без явных промежуточных шагов «в уме», а не путём перечисления цепочки в тексте)
над параметрическим знанием \[[1.1](#ref-1-1)\]. Знание здесь представлено как
**граф знаний** (knowledge graph) — множество фактов-рёбер вида
(субъект, отношение, объект); «рассуждение» — это вывод новых, невиданных фактов
из хранимых по латентным правилам (композиция, сравнение). Ключевое наблюдение:
такой навык надёжно приобретается только через [гроккинг](grokking.md) —
продлённое обучение далеко за пределами переобучения \[[1.1](#ref-1-1)\].

## Детализация

Постановка Wang et al. предельно контролируема: строится случайный граф знаний
G из сущностей и отношений, а его рёбра — «атомарные факты» (axioms) вида
(субъект, отношение, объект) \[[1.1](#ref-1-1)\]. Из атомарных фактов по
латентному правилу выводятся «производные факты» (inferred facts, роль theorems):
для *композиции* (composition) это двухшаговый вывод — из
(h, r1, b) и (b, r2, t) следует связь h с t через промежуточную (bridge)
сущность; для *сравнения* (comparison) — транзитивный порядок над атрибутами
сущностей. Модель обучают предсказывать хвост производного факта; тест на
невиданных производных фактах — в распределении (ID) и вне его (OOD) — измеряет,
выучены ли правила, а не запомнены ли ответы. Навык неявного рассуждения
возникает лишь после долгого плато, то есть через гроккинг; при этом скорость
выхода на генерализацию управляется не абсолютным размером данных, а
*распределением* — отношением числа производных фактов к атомарным
\[[1.1](#ref-1-1)\]. Механистически это трактуется как вытеснение запоминающего
контура (Cmem, хранящего факты в весах; контур — подсеть, реализующая
подалгоритм) обобщающим контуром (Cgen), собирающим ответ из атомарных фактов на
лету; смена решения роднит явление с общей картиной [фазы запоминания](memorization-phase.md)
и последующей генерализации. Эта картина, однако, оспаривается: He et al.
показывают, что гроккнутая и негроккнутая модели идут по одинаковым путям вывода,
а гроккинг «лишь встраивает запомненные факты в уже сложившиеся контуры» и даёт
ограниченную переносимость на новые факты — то есть подлинного *обобщающего
рассуждения* может и не быть \[[2.1](#ref-2-1)\]. С другой стороны, каркас
«задача-как-граф-знаний» подхватывается и развивается: Hwang et al. воспроизводят
задачу сравнения как маленький граф знаний и объясняют генерализацию внутренней
симметрией задачи (task symmetry — инвариантность разметки к перестановкам,
задающая структуру целевого представления) \[[3.1](#ref-3-1)\]. В прикладном
предобучении LLM рассуждение фигурирует уже как один из классов бенчмарков, на
которых отслеживают гроккинг-подобную динамику \[4.1\].

## Альтернативные определения и нюансы

### A. Неявное рассуждение над параметрическим знанием (implicit reasoning over parametric knowledge)

Трактовка Wang et al.: знание модели — это граф фактов, зашитый в весах
(параметрическое, а не подаваемое в контексте), а рассуждение — вывод невиданных
производных фактов из атомарных по латентным правилам, выполняемый «в один проход»
без явной текстовой цепочки \[[1.1](#ref-1-1)\]. Отличительная машинерия здесь —
управляющий параметр в виде *отношения* числа производных фактов к атомарным
(inferred/atomic ratio): именно оно, а не абсолютный объём данных, определяет,
наступит ли и как быстро генерализация. Это переформулирует гипотезу «критического
размера данных» в гипотезу «критического распределения данных» \[[1.1](#ref-1-1)\].

### B. Граф знаний как формат данных задачи (knowledge graph as the task substrate)

Здесь граф знаний — не метафора, а конкретный формат синтетического датасета:
сущности как вершины, отношения как типы рёбер, факты как триплеты; задача
сравнения инстанцируется как «форма, аналогичная небольшому графу знаний», где
супервизия даётся только бинарными попарными отношениями порядка, а глобальную
структуру модель обязана вывести сама \[[3.1](#ref-3-1)\]. Отличие от трактовки A
— в акценте: A определяет *явление* (неявное рассуждение возникает через
гроккинг), B фиксирует *экспериментальную установку* (граф знаний как
воспроизводимый полигон), позволяющую сопоставлять работы этой линии.

### Оспаривают

He et al. оспаривают, что гроккинг на графах знаний даёт подлинное обобщающее
рассуждение: пути вывода у гроккнутой и негроккнутой модели тождественны, гроккинг
«лишь интегрирует запомненные факты в естественно сложившиеся контуры», а зрелый
обобщающий контур плохо переносится на новые факты \[[2.1](#ref-2-1)\]. Источник
различия — разведение поведенческого гроккинга (скачок точности) и формирования
контура: они могут происходить независимо, поэтому скачок точности сам по себе не
доказывает обретения нового «парадигмального» механизма рассуждения.

### Поддерживают

Hwang et al. присоединяются к каркасу «рассуждение-над-графом-знаний» (явно вслед
за Wang et al., 2024) и достраивают его: генерализация в таких задачах наступает
на этапе усвоения симметрии, после чего представления перестраиваются в
структурированную, согласованную с задачей геометрию \[[3.1](#ref-3-1)\].
Отличительный тезис — *внутренняя симметрия задачи* как первопричина организации
представлений (а не размер данных или норма весов).

Abramov et al. присоединяются с другой стороны: они переносят рамку Wang et al. с
синтетического графа на настоящий (2WikiMultiHopQA) и упираются в то, что у
реального графа отношение производных фактов к атомарным на порядок ниже порога
\[[3.2](#ref-3-2)\]. Отличительный ход — вывод, что наращивать сам граф
бесполезно, а недостающие производные факты надо изготавливать: обогащение
языковой моделью поднимает отношение выше порога, и на задаче сравнения гроккинг
наступает там, где без обогащения позднего скачка не было вовсе (на составной
задаче его нет и после обогащения). Тем самым «критическое распределение
данных» из трактовки A становится величиной, которой можно управлять извне, а не
свойством взятого набора.

## Ссылки

###### ref-1-1
**\[1.1\]** 2405.15071 — Wang et al., «Grokked Transformers are Implicit Reasoners: A Mechanistic Journey to the Edge of Generalization». [`"learn to implicitly reason over parametric knowledge"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2). *«[научиться *неявно* рассуждать над параметрическим знанием](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)»*\
Доп.: [`"composition and comparison, we consistently find that transformers can learn implicit reasoning, but only through grokking"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2) — *«[композиции и сравнении, мы неизменно обнаруживаем, что трансформеры *способны* выучить неявное рассуждение, но лишь через *гроккинг*](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)»*; [`"For atomic facts, we generate a random knowledge graph $\mathcal{G}$ consisting of $|\mathcal{E}|$ entities and $|\mathcal{R}|=200$ relations"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p3-4) — *«[Для атомарных фактов мы порождаем случайный граф знаний $\mathcal{G}$, состоящий из $|\mathcal{E}|$ сущностей и $|\mathcal{R}|=200$ отношений](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p3-4)»*; [`"transformers can learn to perform implicit reasoning, but this skill is only robustly acquired through extended training far beyond overfitting"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p2-2) — *«[трансформеры *способны* научиться выполнять неявное рассуждение, но этот навык устойчиво приобретается лишь через *продлённое обучение далеко за пределами переобучения*](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p2-2)»*.

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits in Transformers». Оспаривает: гроккинг не даёт подлинного обобщающего рассуждения, а лишь встраивает запомненные факты в уже сложившиеся контуры. [`"Our findings challenge the view that grokking represents genuine acquisition of generalized reasoning"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-6). *«[Наши находки оспаривают взгляд, по которому гроккинг есть подлинное обретение обобщённой способности рассуждать](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p4-6)»*\
Доп.: [`"grokking merely integrates memorized facts into naturally established circuits"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-6) — *«[гроккинг лишь встраивает запомненные факты в естественно сложившиеся контуры](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p4-6)»*; [`"the “curse of two-hop reasoning” in compositional tasks"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p1-2) — *«[«проклятие двухшагового рассуждения» в составных задачах](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p1-2)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.01968 — Hwang et al., «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». Нюанс: присоединяется к трактовке задачи как графа знаний (вслед за Wang) и объясняет генерализацию внутренней симметрией задачи. [`"form analogous to a small knowledge graph, following prior works (Wang et al., 2024; Allen-Zhu & Li, 2024)"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p3-4). *«[в виде, подобном малому графу знаний, вслед за прежними работами (Wang et al., 2024; Allen-Zhu and Li, 2024)](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p3-4)»*\
Доп.: [`"the model must infer global structure solely from binary pairwise relations"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p3-3) — *«[модель должна вывести всеобщее строение единственно из двоичных попарных отношений](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p3-3)»*.

###### ref-3-2
**\[3.2\]** 2504.20752 — Abramov et al., «Grokking in the Wild: Data Augmentation for Real-World Multi-Hop Reasoning with Transformers». Нюанс: переносит рамку Wang et al. с синтетического графа на настоящий (2WikiMultiHopQA) и достраивает недостающие выведенные факты синтезом. [`"for the first time, we extend grokking to real-world factual data and address the challenge of dataset sparsity by augmenting existing knowledge graphs with carefully designed synthetic data"`](../papers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers.card.md#p1-2). *«[мы впервые распространяем гроккинг на реальные фактические данные и решаем задачу разрежённости набора, обогащая существующие графы знаний тщательно построенными синтетическими данными](../papers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers.card.md#p1-2)»*\
Доп.: [`"even factually incorrect synthetic data can strengthen emergent reasoning circuits rather than degrade accuracy"`](../papers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers.card.md#p1-2) — *«[даже фактически неверные синтетические данные способны укрепить возникающие рассуждающие контуры, а не ухудшить точность](../papers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers.card.md#p1-2)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2506.21551 — Li et al., «Grokking in LLM Pretraining? Monitor Memorization-to-Generalization without Test». [`"generalization on diverse benchmark tasks covering math/commonsense reasoning, code generation, and domain-specific retrieval"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2). *«[генерализации на разнообразных эталонных задачах, охватывающих математическое рассуждение и рассуждение здравого смысла, порождение кода и предметно-ориентированный поиск](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)»*

```
concept:
  category: 3                    # 3. Задачи и наборы данных (Tasks & datasets)
  papers_linked: 5             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-19
```
