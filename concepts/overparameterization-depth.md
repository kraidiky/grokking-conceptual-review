# Переопараметризация и глубина (overparameterization / depth)

[Направление влияния weight decay](weight-decay-direction.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [4. Факторы обучения и оптимизации](index.md#cat-4)\
→ Следующая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)\
← Предыдущая категория: [3. Задачи и наборы данных](modular-arithmetic.md)

## Определение

**Переопараметризация** — избыток параметров относительно объёма обучающих данных, при котором сеть способна подогнать выборку целиком, а **глубина** — число слоёв, через которые эта подгонка проходит. Обе величины входят в разговор о [гроккинге](grokking.md) как управляющие: от них зависит, случится ли отложенная генерализация и когда. Общая рамка задачи звучит так: полной теории генерализации в переопараметризованных моделях нет, и гроккинг рядом с [нейронным коллапсом](neural-collapse.md) — одно из наблюдений, способных её приблизить \[[1.1](#ref-1-1)\].

Систематическая проверка глубины даёт неожиданный ответ: *«глубина действует немонотонно»* — MLP глубины 4 неизменно не грокают, тогда как остаточные сети глубины 8 возвращают генерализацию \[[1.2](#ref-1-2)\].

## Детализация

**Немонотонность и её причина.** Наивное ожидание «глубже — лучше» не выполняется: между глубиной 2 и глубиной 8 лежит провал, из которого сеть вытаскивают не дополнительные слои, а стабилизация — остаточные связи и нормализация \[[3.1](#ref-3-1)\]. Отсюда вывод, ради которого понятие и держат: наблюдаемое действие глубины смешано с действием устойчивости оптимизации, и работы, меняющие только число слоёв, меряют не глубину, а способность обучения не разваливаться \[[1.2](#ref-1-2)\].

**Переопараметризация как условие, а не причина.** Гроккинг обычно наблюдают в переизбыточно параметризованных сетях, но необходимости в этом нет: результаты о [разрежённой чётности](sparse-parity.md) получены в режимах малой ширины, где никакое закреплённое ядро, включая [NTK](neural-tangent-kernel-ntk.md), задачу решить не может \[[3.2](#ref-3-2)\], \[[3.3](#ref-3-3)\]. То есть отложенная генерализация не требует избытка параметров — она требует, чтобы задача была вычислительно трудной для быстрого решения.

**Почему избыток вообще не мешает обобщению.** Стандартное объяснение — внутренняя размерность выученного представления много меньше объемлющей: сеть с огромным числом параметров живёт в малоразмерном многообразии \[[3.4](#ref-3-4)\]. Для гроккинга это важно тем, что превращает вопрос «почему избыточная сеть обобщает» в вопрос «когда она находит малоразмерное решение», а это уже вопрос о [сжатии представления](manifold-representation-compression.md) и о сроке.

**Как это меряют.** Глубина и ширина входят в развёртки наравне с модулем и долей данных: преимущество оптимизатора проверяют на устойчивость по модулю, доле обучения, ширине и глубине \[[3.5](#ref-3-5)\], а сама сетка глубин задаётся вместе с числом семян, потому что при большей глубине разброс растёт \[[3.6](#ref-3-6)\].

## Альтернативные определения и нюансы

### A. Глубина как управляющий параметр

Прямое употребление: менять число слоёв при прочих равных и смотреть на исход \[[1.2](#ref-1-2)\], \[[3.6](#ref-3-6)\]. Различающая черта — немонотонность: результат не выражается фразой «глубже лучше» или «хуже», и любое утверждение о глубине требует указания, стабилизирована ли сеть остаточными связями и нормализацией.

### B. Переопараметризация как режим

Здесь важно не число слоёв, а отношение числа параметров к объёму данных. Оговорка корпуса: гроккинг воспроизводится и вне этого режима \[[3.2](#ref-3-2)\], поэтому переопараметризация — обычное условие опытов, а не необходимая часть явления.

### C. Малоразмерность выученного как примиряющее объяснение

Третья позиция снимает противоречие «избыток параметров против простоты решения»: объемлющее пространство велико, но выученное представление лежит в малоразмерном подмногообразии \[[3.4](#ref-3-4)\]. Различающая черта — предмет измерения: не число параметров, а внутренняя размерность представления, и потому проверяется спектральными мерами, а не подсчётом весов.

## Ссылки

###### ref-1-1
**\[1.1\]** 2210.15435 — Žunkovič et al., «Grokking phase transitions in learning local rules with gradient descent». [`"we still do not have a complete theory of generalisation in over-parameterised models"`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p3-1). *«[у нас до сих пор нет полной теории генерализации в переопараметризованных моделях](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p3-1)»*

###### ref-1-2
**\[1.2\]** 2603.25009 — Manir et al., «A Systematic Empirical Study of Grokking: Depth, Architecture, Activation, and Regularization». [`"**depth has a non-monotonic effect**, with depth-4 MLPs consistently failing to grok while depth-8 residual networks recover generalization"`](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p1-3). *«[**глубина действует немонотонно** — MLP глубины 4 неизменно не грокают, тогда как остаточные сети глубины 8 возвращают генерал](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p1-3)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.25009 — Manir et al., «A Systematic Empirical Study of Grokking: Depth, Architecture, Activation, and Regularization». Нюанс: провал на средней глубине лечится не слоями, а стабилизацией — остаточными связями и нормализацией. [`"**Depth requires stabilization.** Grokking exhibits a non-monotonic dependence on depth"`](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p2-2). *«[**Глубина требует стабилизации.** Гроккинг обнаруживает немонотонную зависимость от глубины](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p2-2)»*

###### ref-3-2
**\[3.2\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Нюанс: отложенная генерализация воспроизводится без избытка параметров — в режимах малой ширины. [`"Our theoretical and empirical results hold in non-overparameterized regimes"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p3-5). *«[Наши теоретические и опытные результаты справедливы в режимах без избыточной параметризации](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p3-5)»*

###### ref-3-3
**\[3.3\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Нюанс: в этих режимах никакое закреплённое ядро задачу не решает, то есть дело не в ёмкости, а в вычислительной трудности. [`"no fixed kernel (including the neural tangent kernel (Jacot et al. 2018), whose dimensionality is the network’s parameter count) can solve the sparse parity problem"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p8-4). *«[никакое закреплённое ядро (включая нейронное касательное ядро (Jacot et al. 2018), размерность которого есть число параметров сети) не может решить задачу разрежённой чётности](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p8-4)»*

###### ref-3-4
**\[3.4\]** 2211.13239 — Brown et al., «Relating Regularization and Generalization through the Intrinsic Dimension of Activations». Нюанс: избыток параметров совместим с простотой решения, потому что выученное представление лежит в малоразмерном подмногообразии. [`"the success of vastly over-parameterized neural networks (Zhang et al. 2016) seems counterintuitive"`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p2-1). *«[успех крайне переопараметризованных нейросетей (Zhang et al. 2016) выглядит противоречащим интуиции](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p2-1)»*

###### ref-3-5
**\[3.5\]** 2608.07436 — Janati et al., «Post-Grokking Collapse at the Representation–Readout Interface in Muon-Trained Transformers». Нюанс: глубина входит в развёртку наравне с модулем, долей обучения и шириной — как ось проверки устойчивости результата. [`"the advantage holds across modulus, training fraction, width, and depth"`](../papers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers/original/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers.md#p4-3). *«[преимущество держится по модулю, доле обучения, ширине и глубине](../papers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers.card.md#p4-3)»*

###### ref-3-6
**\[3.6\]** 2603.25009 — Manir et al., «A Systematic Empirical Study of Grokking: Depth, Architecture, Activation, and Regularization». Нюанс: сетка глубин задаётся вместе с числом семян, потому что на большей глубине обучение менее устойчиво. [`"We evaluate three depths: $d\in\{2,4,8\}$."`](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p4-9). *«[Мы рассматриваем три глубины: $d\in\{2,4,8\}$.](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p4-9)»*
