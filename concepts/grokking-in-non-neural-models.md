# Гроккинг вне нейросетей (grokking in non-neural and solvable models)

[Рассуждение и графы знаний](reasoning-knowledge-graphs.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [3. Задачи и наборы данных](index.md#cat-3)\
→ Следующая категория: [4. Факторы обучения и оптимизации](weight-decay.md)\
← Предыдущая категория: [2. Механизмы и представления](structured-representation-learning.md)

## Определение

**Гроккинг вне нейросетей** — наблюдение отложенной генерализации в моделях, которые либо вовсе не являются глубокими сетями (гребневая и логистическая регрессия, ядровые методы), либо являются линейными и потому решаются аналитически. Смысл линии в том, что [гроккинг](grokking.md) перестаёт быть свойством трансформера: он *«может, как ни удивительно, возникать в линейных сетях, выполняющих линейные задачи»* \[[1.1](#ref-1-1)\], а в гребневой регрессии для него доказывается полное ручательство — переобучение рано, плохое обобщение долго, обобщение в конце \[[1.2](#ref-1-2)\].

## Детализация

Ценность решаемых постановок в том, что в них определения перестают быть операциональными. В бинарной логистической классификации *«запоминающее» и «обобщающее» решения можно определить строго* \[[1.3](#ref-1-3)\], а значит переход между ними — не наблюдение на кривой, а событие, о котором можно доказать теорему. Там же исчезает и главная неопределённость трансформерных работ: [время гроккинга](grokking-time.md) выводится в замкнутой форме, а не измеряется по порогу.

**Что это даёт корпусу.** Во-первых, разделение необходимого и случайного: если гроккинг воспроизводится без внимания, без эмбеддингов и без нелинейности, то ни одно из них не является его причиной. Во-вторых, проверяемость механизмов: линейная постановка позволяет прямо сопоставить объяснение через переход от ленивого режима к богатому с объяснением через сжатие нормы \[[3.1](#ref-3-1)\].

**Физические модели.** Отдельная ветвь — не «проще нейросети», а «ближе к статистической физике»: плотная сеть, классифицирующая конфигурации двумерной модели Изинга, грокает при наличии [weight decay](weight-decay.md), и источник задержки прослеживается до перестройки архитектуры, при которой в последовательных слоях остаётся всё меньше деятельных нейронов \[[3.2](#ref-3-2)\]. Здесь задача даёт управляемый генератор данных с известной структурой корреляций \[[3.3](#ref-3-3)\], чего у алгоритмических наборов нет.

**Оговорка о переносе.** Ни одна решаемая модель не воспроизводит трансформер: у линейного «учителя — ученика» нет ни [фурье-контуров](fourier-features-circuits.md), ни конкуренции подсетей, и совпадение внешней кривой не означает совпадения механизма. Сама линия это признаёт: динамика неглубокого трансформера на алгоритмических данных названа «совершенно иной», и именно поэтому её понадобилось объяснять отдельно \[[3.4](#ref-3-4)\].

## Альтернативные определения и нюансы

### A. Линейная модель со строгими определениями

Постановка, где «запоминание» и «обобщение» — определения, а не пороги: бинарная логистическая классификация, в которой оба решения выписываются явно \[[1.3](#ref-1-3)\]. Различающая черта — в такой модели вопрос «грокает ли она» имеет ответ «да/нет» по построению, и спор смещается к тому, тот ли это гроккинг, что у трансформера.

### B. Доказанное ручательство вместо наблюдения

Гребневая регрессия с градиентным спуском и weight decay: доказываются все три поры сразу — раннее переобучение, долгое плохое обобщение, итоговое обобщение \[[1.2](#ref-1-2)\], \[[3.5](#ref-3-5)\]. Источник различия с эмпирическими работами — статус утверждения: здесь оно теорема с условиями, и потому проверяемо не воспроизведением, а проверкой условий.

### C. Аналитически решаемая динамика

Линейная модель «учитель — ученик» с гауссовыми входами: полная динамика обучения выписывается, и гроккинг оказывается свойством решения уравнений, а не находкой на графике \[[1.1](#ref-1-1)\]. Отличие от предыдущей формы — предмет вывода: не факт наличия трёх пор, а форма зависимости срока от параметров.

### D. Физическая модель как источник данных

Модель Изинга даёт не упрощение сети, а упрощение **данных**: известное распределение с настраиваемой корреляцией \[[3.3](#ref-3-3)\]. Механизм задержки при этом ищется в самой сети — в постепенном разрежении деятельных нейронов по слоям \[[3.2](#ref-3-2)\], — так что постановка проверяет не «нужна ли нелинейность», а «что делает регуляризация с архитектурой».

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.16441 — Levi et al., «Grokking in Linear Estimators – A Solvable Model that Groks without Understanding». [`"We show both analytically and numerically that grokking can surprisingly occur in linear networks performing linear tasks in a simple teacher-student setup with Gaussian inputs."`](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/original/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.md#p1-2). *«[Мы показываем — аналитически и численно, — что гроккинг может, как ни удивительно, возникать в линейных сетях, выполняющих линейные задачи, в простой постановке «учитель — ученик» с гауссовыми входами.](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.card.md#p1-2)»*

###### ref-1-2
**\[1.2\]** 2601.19791 — Xu et al., «To Grok Grokking: Provable Grokking in Ridge Regression». [`"We prove end-to-end grokking results for learning over-parameterized linear regression models using gradient descent with weight decay."`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p1-2). *«[Мы доказываем полные, от начала до конца, итоги о гроккинге при обучении переопределённых линейных регрессионных моделей градиентным спуском с weight decay.](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.card.md#p1-2)»*

###### ref-1-3
**\[1.3\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». [`"in a simple binary logistic classification task, for which "memorizing" and "generalizing" solutions can be strictly defined"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2). *«[в простой задаче бинарной логистической классификации, для которой «запоминающее» и «обобщающее» решения можно определить строго](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2601.19791 — Xu et al., «To Grok Grokking: Provable Grokking in Ridge Regression». Нюанс: решаемая постановка позволяет сопоставить объяснение через переход от ленивого режима к богатому с другими механизмами напрямую. [`"Several existing theoretical papers attribute the occurrence of grokking to a transition in the optimization dynamics from the lazy to the rich regime"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p1-4). *«[Несколько имеющихся теоретических работ приписывают возникновение гроккинга переходу в динамике оптимизации от ленивого режима к богатому](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.card.md#p1-4)»*

###### ref-3-2
**\[3.2\]** 2510.25966 — Hutchison & Yevick, «Grokking in the Ising Model». Нюанс: источник задержки прослежен до перестройки архитектуры — уменьшения числа деятельных нейронов по слоям, — а не до свойств данных. [`"The origin of grokking in this system results from the evolution of the network to an architecture in which successive layers contain a monotonically smaller number of active neurons."`](../papers/2510.25966.grokking-in-the-ising-model/original/2510.25966.grokking-in-the-ising-model.md#p1-7). *«[Источник гроккинга в этой системе происходит от эволюции сети к архитектуре, в которой последовательные слои содержат монотонно меньшее число деятельных нейронов.](../papers/2510.25966.grokking-in-the-ising-model/2510.25966.grokking-in-the-ising-model.card.md#p1-7)»*

###### ref-3-3
**\[3.3\]** 2510.25966 — Hutchison & Yevick, «Grokking in the Ising Model». Нюанс: физическая модель ценна как управляемый генератор данных с известной структурой корреляций. [`"The Ising model, viewed as a generator of images with maximum randomness subject to a given correlation among spins, provides an optimal framework in which to examine neural network evolution during training."`](../papers/2510.25966.grokking-in-the-ising-model/original/2510.25966.grokking-in-the-ising-model.md#p1-8). *«[Модель Изинга, рассматриваемая как генератор изображений с наибольшей случайностью при заданной корреляции спинов, даёт оптимальную рамку для изучения эволюции нейронной сети при обучении.](../papers/2510.25966.grokking-in-the-ising-model/2510.25966.grokking-in-the-ising-model.card.md#p1-8)»*

###### ref-3-4
**\[3.4\]** 2310.16441 — Levi et al., «Grokking in Linear Estimators – A Solvable Model that Groks without Understanding». Нюанс: сама линия признаёт, что динамика трансформера на алгоритмических данных «совершенно иная», — совпадение кривых не означает совпадения механизма. [`"found that a shallow transformer trained on algorithmic datasets features drastically different dynamics"`](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/original/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.md#p1-4). *«[обнаружено, что у неглубокого трансформера, обучаемого на алгоритмических наборах данных, динамика совершенно иная](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.card.md#p1-4)»*

###### ref-3-5
**\[3.5\]** 2601.19791 — Xu et al., «To Grok Grokking: Provable Grokking in Ridge Regression». Нюанс: цель прямо названа — доказать полное ручательство, а не воспроизвести кривую. [`"our goal is to prove an end-to-end grokking guarantee"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p2-1). *«[наша цель — доказать полное ручательство о гроккинге](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.card.md#p2-1)»*
