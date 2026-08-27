# Сжатие многообразия представлений (manifold / representation compression)

[Ландшафт потерь / бассейны](loss-landscape-basins.md) ← предыдущая карточка, следующая → [Разрежённая подсеть / lottery ticket](sparse-subnetwork-lottery-ticket.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Сжатие многообразия представлений** — трактовка [грокинга](grokking.md), при
которой отложенная генерализация наступает в тот момент, когда внутренняя
структура сети — её представления (активации скрытых слоёв) либо траектория её
весов — сжимается из высокоразмерной, «раздутой» запоминающей конфигурации в
компактное низкоразмерное или низкоранговое (описываемое немногими
независимыми направлениями) многообразие. Именно эта компрессия сложности
отличает обобщающее решение от запоминающего: обобщающее решение проще, а
потому эффективнее, и появляется позже. Наиболее явно идею «грокинг = сжатие»
сформулировали Liu et al. \[[1.1](#ref-1-1)\].

![Сжатие по ходу гроккинга: точность, число линейных отображений (LMN) первого слоя и норма параметров — сложность растёт при запоминании и падает при генерализации (рис. 2 Liu, Zhong, Tegmark)](assets/representation-compression-lmn.png)

## Детализация

Разные работы измеряют «сжатие» по-разному, и это ключевой источник расхождений.
Liu et al. предлагают меру сложности LMN (linear mapping number — число линейных
отображений, обобщение числа линейных областей ReLU-сети, интерпретируемое как
количество информации/вычислений), которая, в отличие от L2-нормы весов, линейно
связана с тестовыми потерями в фазе сжатия \[[1.1](#ref-1-1)\]; ту же меру
сложности как аргумент баланса «ошибка + сложность» перенимают Miller et al. и
показывают, что грокинг воспроизводится даже вне нейросетей \[[3.1](#ref-3-1)\].
Другая линия измеряет сжатие геометрически — на уровне самих представлений.
Sakamoto et al. связывают грокинг с нейросхлопыванием ([neural collapse](neural-collapse.md) — поздняя
геометрическая перестройка, при которой представления одного класса стягиваются к
своему среднему) и с информационным бутылочным горлышком (information bottleneck —
принцип, по которому сеть отбрасывает нерелевантную задаче входную информацию),
формализуя сжатие как стягивание внутриклассовой дисперсии \[[3.3](#ref-3-3)\].
Fan et al. в глубоких сетях наблюдают «туннельный эффект»: поздние слои сжимают
признаки в низкоранговое представление, и ранг признаков падает вдоль обучения
\[[3.2](#ref-3-2)\]. Третья линия помещает сжатие в пространство весов: Xu
описывает грокинг как затяжное удержание траектории на низкоразмерном
подпространстве («исполнительном многообразии») в пространстве весов
\[[3.5](#ref-3-5)\], Hutchison et al. — как переход весовых матриц к
низкоранговой конфигурации и [разрежённой подсети](sparse-subnetwork-lottery-ticket.md) \[[3.4](#ref-3-4)\], а Gu et al.
трактуют обучение как выучивание многообразия пространства эмбеддингов
\[[1.2](#ref-1-2)\]. Zhang et al. объединяют обе стороны, выводя грокинг из
принципа экономности (parsimony) как «физический коллапс избыточных многообразий»
и глубокое информационное сжатие \[[1.3](#ref-1-3)\]. Механизм, однако,
оспаривается: часть работ считает, что сжатие представлений (в форме
нейросхлопывания) не является необходимым условием генерализации
\[[2.1](#ref-2-1)\]; что тенденцию к сжатию объясняет [weight decay](weight-decay.md), но не
конкретную геометрию итогового многообразия \[[2.2](#ref-2-2)\]; и что медленную
компрессию вообще можно обойти архитектурными средствами, устранив [фазу
запоминания](memorization-phase.md) \[[2.3](#ref-2-3)\].

## Альтернативные определения и нюансы

### A. Сжатие сложности сети (колмогоровская/LMN-трактовка)

Определение через меру сложности всей функции сети: грокинг — это фаза, в которой
сложность отображения «вход-выход» падает до генерализующего минимума, при том что
train-точность уже максимальна. Отличительная машинерия здесь — [параметр порядка](order-parameter.md) не
геометрический, а сложностный: число линейных отображений LMN (для кусочно-линейных
ReLU-сетей), которое трактуется как аппроксимация [колмогоровской сложности](kolmogorov-complexity.md) и растёт
при запоминании, но падает при компрессии \[[1.1](#ref-1-1)\]. Именно тем, что LMN
интерпретируется как информация/вычисление (а L2-норма — нет), эта трактовка
отделяет себя от норм-ориентированных: сжатие определяется как убывание
описательной сложности, а не нормы весов, и переносится на не-нейросетевые модели
\[[3.1](#ref-3-1)\].

### B. Сжатие геометрии представлений (нейросхлопывание / информационное горлышко)

Определение через геометрию активаций: грокинг — это стягивание облака
представлений к низкоразмерной, структурированной форме. Отличительный параметр
порядка — внутриклассовая дисперсия представлений: генерализация наступает, когда
population within-class variance (популяционная внутриклассовая дисперсия)
сжимается, что одновременно объясняет и грокинг, и информационное бутылочное
горлышко \[[3.3](#ref-3-3)\]. В глубоких сетях та же компрессия проявляется как
падение ранга признаков в поздних («туннельных») слоях \[[3.2](#ref-3-2)\]. От
трактовки A этот вариант отличается тем, что сжимается наблюдаемое представление
данных, а не абстрактная сложность функции.

### C. Коллапс на низкоразмерное многообразие в пространстве весов/эмбеддингов

Определение через геометрию параметров: грокинг — это схождение обучающей
траектории на компактное многообразие в пространстве весов либо эмбеддингов.
Отличительная машинерия — размерность несущего подпространства: Xu показывает, что
эволюция весов при грокинге по существу одномерна (один главный компонент
объясняет 68-83 % дисперсии) и удерживается на «исполнительном многообразии»
\[[3.5](#ref-3-5)\]; Hutchison et al. фиксируют переход весовых матриц к
низкоранговой конфигурации \[[3.4](#ref-3-4)\]; Gu et al. определяют цель обучения
как выучивание разделяющего многообразия в пространстве эмбеддингов
\[[1.2](#ref-1-2)\]. От варианта B отличие в том, что параметр порядка живёт в
пространстве параметров/эмбеддингов, а не в пространстве активаций поштучных
примеров.

### Оспаривают

Три линии возражений. Han et al. темпорально разделяют грокингом причину и
следствие и показывают, что модели, которых принуждают к схлопыванию или, наоборот,
которым его запрещают, генерализуют одинаково хорошо, — значит, сжатие
представлений (нейросхлопывание) не необходимо, а необходимым и более
фундаментальным предиктором оказывается плоскостность (relative flatness — мера
пологости минимума потерь) \[[2.1](#ref-2-1)\]. Hwang et al. соглашаются, что
оптимизация со снижением нормы объясняет саму *тенденцию* к сжатию, но настаивают,
что она не объясняет *конкретную форму* итоговой геометрии — её задаёт внутренняя
симметрия задачи \[[2.2](#ref-2-2)\]. Yildirim оспаривает неизбежность медленной
компрессии: полностью ограниченная сферическая топология остаточного потока
полностью устраняет фазу запоминания, и train- и test-точность растут вместе с
инициализации \[[2.3](#ref-2-3)\].

### Поддерживают

Присоединяющиеся работы усиливают конкретные измерения сжатия: как перенос меры
сложности LMN на широкий класс моделей \[[3.1](#ref-3-1)\], как падение ранга
признаков в глубоких сетях \[[3.2](#ref-3-2)\], как стягивание внутриклассовой
дисперсии и отбрасывание нерелевантной информации \[[3.3](#ref-3-3)\], как переход
весов к низкоранговой/разрежённой конфигурации \[[3.4](#ref-3-4)\] и как удержание
траектории на низкоразмерном исполнительном многообразии в пространстве весов
\[[3.5](#ref-3-5)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.05918 — Liu et al., «Grokking as Compression: A Nonlinear
Complexity Perspective». [`"We attribute grokking, the phenomenon where generalization is much delayed after memorization, to compression."`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2). *«[Мы относим гроккинг — явление, при котором генерализация сильно запаздывает за запоминанием, — к сжатию](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p1-2)»*\
Доп.: [`"There exist a generalization solution and a memorization solution; the memorization solution is easier to be learned so learned at first, but the generalization solution is more efficient so emerges later."`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-3) — *«[существуют решение-обобщение и решение-запоминание; решение-запоминание выучить проще, поэтому оно выучивается первым, но решение-обобщение эффективнее и потому возникает позже](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p1-3)»*

###### ref-1-2
**\[1.2\]** 2504.03162 — Gu et al., «Beyond Progress Measures: Theoretical
Insights into the Mechanism of Grokking». [`"The goal of deep learning is to learn the manifold of the embedding space that needs to be separated from the convex hull of discrete samples."`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p5-13). *«[Цель глубокого обучения — выучить многообразие пространства вложений, которое надо отделить от выпуклой оболочки разрывных примеров](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p5-13)»*

###### ref-1-3
**\[1.3\]** 2603.29262 — Zhang et al., «Grokking: From Abstraction to Intelligence». [`"from memorization to generalization corresponds to the physical collapse of redundant manifolds and deep information compression"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-2). *«[переход от запоминания к генерализации отвечает вещественному схлопыванию избыточных многообразий и глубокому сжатию сведений](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-2)»*\
Доп. (кольцо, изоморфное группе): [`"2D PCA projections confirm that the embeddings evolve from a high-entropy disordered cloud (Step 1k) into a compact 1D circular manifold (Step 100k) perfectly isomorphic to the target cyclic group $\mathbb{Z}_{97}$"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p4-3) — *«[Двумерные проекции PCA подтверждают, что вложения развиваются от беспорядочного облака высокой энтропии (шаг 1 тыс.) к плотному одномерному круговому многообразию (шаг 100 тыс.), в точности изоморфному целевой циклической группе $\mathbb{Z}_{97}$](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p4-3)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2509.17738 — Han et al., «Flatness is Necessary, Neural Collapse is
Not: Rethinking Generalization via Grokking». Оспаривает: сжатие представлений
(нейросхлопывание) не необходимо — необходима плоскостность минимума.
[`"only flatness consistently predicts it. Models encouraged to collapse or prevented from collapsing generalize equally well"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2). *«[предсказывает её последовательно лишь плоскостность. Модели, которых побуждают к коллапсу, и модели, которым коллапсировать не дают, обобщают одинаково хорошо](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.card.md#p1-2)»*

###### ref-2-2
**\[2.2\]** 2603.01968 — Hwang et al., «Intrinsic Task Symmetry Drives
Generalization in Algorithmic Tasks». Оспаривает: компрессия объясняет тенденцию,
но не конкретную форму итоговой геометрии — её задаёт симметрия задачи.
[`"while *optimization*-centric explanations like weight decay account for the *tendency* toward compression, they alone cannot explain the *specific form* of the resulting geometry"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-4). *«[объяснения, средоточие которых — *оптимизация*, вроде ослабления весов, отвечают за *склонность* к сжатию, но сами по себе не объясняют *особого вида* получающейся геометрии](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p1-4)»*\
Доп.: [`"collapse into low-dimensional, structured manifolds"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-4) — *«[сжимаются в малоразмерные устроенные многообразия](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p1-4)»*.

###### ref-2-3
**\[2.3\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking».
Оспаривает неизбежность медленной компрессии: архитектурная топология устраняет
фазу запоминания. [`"A fully bounded spherical topology, enforcing L2 normalization throughout the residual stream, eliminates the memorization phase entirely on modular addition"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2). *«[Полностью ограниченная сферическая топология, навязывающая $L_{2}$-нормирование по всему остаточному потоку, полностью устраняет фазу запоминания на модульном сложении](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p1-2)»*\
Доп.: [`"slow compression of the representation manifold"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p3-3) — *«[медленным сжатием многообразия представлений](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p3-3)»*

###### ref-2-4
**\[2.4\]** 2602.16967 — Xu, «Early-Warning Signals of Grokking via
Loss-Landscape Geometry». Нюанс: разрежение первой главной компоненты
траектории весов предшествует гроккингу на Dyck (пик на шаге 600 против
гроккинга на 2 900), но **следует** за ним на SCAN, где PC1 продолжает расти
сквозь переход; отсюда прямой вывод автора, что спектральная сосредоточенность
не универсальна. Отрицательный результат приведён честно и служит опорой
вывода о преимуществе коммутаторного дефекта. [`"it shows that PC1 de-concentration, while observed in modular arithmetic and Dyck, is *not* a universal precursor to grokking"`](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/original/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.md#p11-3). *«[оно показывает, что разрежение PC1, хотя и наблюдается в модульной арифметике и на Dyck, *не* есть универсальный предвестник гроккинга](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.card.md#p11-3)»*

###### ref-2-5
**\[2.5\]** 2604.13082 — Gomezjurado Gonzalez, «The Long Delay to Arithmetic Generalization…». [`"the participation ratio of encoder representations falls from $5.2$ to $1.0$ in a single checkpoint"`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p19-8). *«[отношение участия представлений энкодера падает с $5.2$ до $1.0$ за одну контрольную точку](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p19-8)»* — при этом точность обрушивается в нуль и не восстанавливается до 500k шагов.
###### ref-2-6
**\[2.6\]** 2602.18649 — Xu 2026, «Global Low-Rank, Local Full-Rank: The Holographic Encoding of Learned Algorithms». Контрпример к низкоранговой сжимаемости грокнутых весов: восстановление требует восьмидесяти и более сингулярных компонент на матрицу, откуда предупреждение для LoRA-подобных методов; заявка об обратном пути сжатия через компоненты траектории подорвана разбором — одна компонента траектории есть полный вектор параметров ствола. [`"the grokked solution actively uses the full rank of each weight matrix. Compressing any individual matrix destroys the cross-matrix correlations that encode the algorithm"`](../papers/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms/original/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms.md#p11-5). *«[грокнутое решение деятельно использует полный ранг каждой матрицы весов. Сжатие любой отдельной матрицы разрушает межматричные корреляции, кодирующие алгоритм](../papers/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms.card.md#p11-5)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2310.17247 — Miller, O'Neill, Bui 2024, «Grokking Beyond Neural
Networks: An Empirical Exploration with Model Complexity». Нюанс: сжатие как убывание меры
сложности LMN, переносимой на ненейросетевые модели. [`"use a more advanced measure such as the linear mapping number (LMN)"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p2-3). *«[пользоваться более тонкой мерой вроде числа линейных отображений (LMN)](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p2-3)»*\
Доп. (как «не замечать» ложные измерения у гауссова процесса): [`"length scale increases for for input dimensions above three but decrease for input dimensions three and below"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p27-1) — *«[длина растёт для входных измерений выше третьего, но убывает для измерений с третьего и ниже](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p27-1)»*.

###### ref-3-2
**\[3.2\]** 2405.19454 — Fan, Pascanu, Jaggi 2024, «Deep Grokking: Would Deep Neural
Networks Generalize Better?». Нюанс: сжатие как падение ранга признаков в поздних
(«туннельных») слоях, и это падение совпадает по времени с началом генерализации. [`"the higher layers (i.e. *Tunnel*) tend to compress it into a low-ranked representation"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p3-5). *«[верхние слои (*туннель*) склонны сжимать его в представление низкого ранга](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p3-5)»*\
Доп. (совпадение со сменой режима): [`"the test accuracy (and linear probe accuracy) begins to rise notably as the feature ranks exhibit a significant decrease"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p3-4) — *«[точность на тесте (и точность линейного щупа) начинает заметно расти тогда, когда ранги признаков являют существенное убывание](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p3-4)»*.

###### ref-3-3
**\[3.3\]** 2509.20829 — Sakamoto et al., «Explaining Grokking and Information
Bottleneck through Neural Collapse Emergence». Нюанс: сжатие как стягивание
внутриклассовой дисперсии и отбрасывание нерелевантной информации.
[`"population within-class variance is a key factor underlying both grokking and information bottleneck"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2). *«[популяционной внутриклассовой дисперсии есть ключевой множитель, лежащий в основе и гроккинга, и информационного бутылочного горлышка](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)»*.\
Доп.: [`"enter a fitting phase where they memorize the training data, followed by a later compression phase in the later training stage during which task-irrelevant input information is discarded."`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-3) — *«[входят в фазу подгонки, где запоминают обучающие данные, а затем, на более поздней поре обучения, — в фазу сжатия, в ходе которой не относящиеся к задаче сведения о входе отбрасываются](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-3)»*.

###### ref-3-4
**\[3.4\]** 2510.25966 — Hutchison et al., «Grokking in the Ising Model».
Нюанс: сжатие как переход весовых матриц к низкоранговой конфигурации/разрежённой
подсети. [`"transition to a low-rank configuration resulting in a sparse subnetwork"`](../papers/2510.25966.grokking-in-the-ising-model/original/2510.25966.grokking-in-the-ising-model.md#p2-3). *«[переходят к малоранговой конфигурации, дающей разреженную подсеть](../papers/2510.25966.grokking-in-the-ising-model/2510.25966.grokking-in-the-ising-model.card.md#p2-3)»*\
Доп.: [`"the weight-space trajectory during grokking lies on a low-dimensional execution manifold"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p2-1) — *«[траектория в пространстве весов при гроккинге лежит на низкоразмерном исполнительном многообразии](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p2-1)».*

###### ref-3-6
**\[3.6\]** 2210.15435 — Žunkovič, Ilievski, «Grokking phase transitions in learning local rules with gradient descent». Нюанс: даёт корпусу дешёвую операциональную меру сжатия — экспоненту энтропии долей объяснённой дисперсии главных компонент признаков — и наблюдение, что она резко падает непосредственно перед переходом, причём сильнее всего при $L_{1}$. Теоретический вывод о предпочтительности малой эффективной размерности выведен при $\lambda_{1}=0$: при $\lambda_{1}>0$ та же модель даёт обратную зависимость вероятности гроккинга от размерности, и в широких формулировках работы эта оговорка опущена. [`"As shown in Fig. 15, the average effective dimension drops significantly just before the grokking transition."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p27-2). *«[Как показано на рис. 15, средняя эффективная размерность значительно падает непосредственно перед переходом гроккинга](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p27-2)»*\
Доп. (определение меры): [`"Finally, the effective dimension is obtained as the exponent of the entropy, i.e. $D_{\rm eff}=\mathrm{e}^{S}$."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p24-2) — *«[Наконец, эффективная размерность получается как экспонента энтропии, то есть $D_{\rm eff}=\mathrm{e}^{S}$](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p24-2)»*\
Доп. (вывод со стороны теории): [`"Therefore, latent-space distribution with a low effective dimension is preferred for better generalisation."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p13-5) — *«[Поэтому распределение в латентном пространстве с малой эффективной размерностью предпочтительнее для лучшей генерализации](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p13-5)»*.

###### ref-3-7
**\[3.7\]** 2408.11804 — Yunis et al., «Approaching Deep Learning through the Spectral Dynamics of Weights». Вариант сжатия, измеряемый в весах, а не в представлениях: обрезка SVD с сохранением верхней половины сингулярных чисел всех матриц, кроме последнего слоя, почти не портит модель, а сохранение нижней — портит. Авторы подчёркивают, что исход не был предрешён. Ни размерности представлений, ни внутренней размерности возбуждений работа не считает. [`"keeping the top half of the SVD is close to the full model performance"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#fig-4). *«[сохранение верхней половины SVD близко к качеству полной модели](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#fig-4)»*\
Доп. (оговорка о непредрешённости): [`"This may not have been the case."`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p7-1) — *«[Могло быть и иначе](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p7-1)»*.

###### ref-3-8
**\[3.8\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Нюанс: на одной модели показаны две противоположные вещи — решение восстанавливается по 4–10 направлениям траектории PCA и при этом не сжимается ничем послесловным; сочетание названо голографической несжимаемостью. Оговорка: «низкорангового строения нет» показано двумя точками — ранг 64 и ранг 128, промежуточных не пробовали. [`"the essential algorithmic information appears to be near-maximally compressed within the execution subspace, and further dimensional reduction destroys global function"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p25-8). *«[существенные алгоритмические сведения оказываются почти предельно сжаты внутри исполнительного подпространства, и дальнейшее понижение размерности разрушает глобальную работу](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p25-8)»*\
Доп. (послойное SVD): [`"There is no low-rank structure in the weight matrix deltas."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p13-4) — *«[Низкорангового строения в приращениях матриц весов нет](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p13-4)»*.

###### ref-3-9
**\[3.9\]** 2211.13239 — Brown et al., «Relating Regularization and Generalization through the Intrinsic Dimension of Activations». Нюанс: самое раннее в корпусе (ноябрь 2022) прямое наблюдение сжатия геометрии представлений в пору гроккинга; мера — оценщик TwoNN по активациям, из которого сделаны LLID (последний слой) и PID (наибольший по слоям). Вмешательств нет ни одного: показано совпадение по времени на 16 прогонах одной задачи, часть окошек даёт плавное убывание задолго до скачка, часть — падение с возвратом наверх; PID у трансформеров не измеряется вовсе. [`"we observe a sudden drop in LLID co-occurent with the sudden rise in validation accuracy observed in models that exhibit sudden generalization"`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p6-1). *«[мы наблюдаем внезапное падение LLID, совпадающее по времени с внезапным ростом проверочной точности, наблюдаемым у моделей, обнаруживающих внезапную генерализацию](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p6-1)»*\
Доп.: [`"models that grok have an average minimium LLID of $8.53$, much lower than the average minimum LLID – $16.16$ – of models that remain at a low validation accuracy in the allotted compute budget"`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p6-1) — *«[у моделей, которые грокают, средний наименьший LLID равен $8.53$ — много ниже среднего наименьшего LLID ($16.16$) у моделей, остающихся при низкой проверочной точности в отведённом вычислительном бюджете](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p6-1)»*\
Доп. (авторская оговорка о самой мере): [`"We note that intrinsic dimension cannot increase across neural network layers, which are functions of only their previous activations, and we attribute this inconsistency to errors in the ID estimator."`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p3-4) — *«[Заметим, что внутренняя размерность не может расти вдоль слоёв нейросети, которые суть функции лишь предшествующих им активаций, и мы относим это несоответствие на счёт погрешностей оценщика ID](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p3-4)»*.

###### ref-3-10
**\[3.10\]** 2602.06702 — Singh, Misra, Orvieto, «Explaining Grokking in Transformers…». Одна мера сшивает два рычага — положение LN и силу weight decay: [`"configurations that generalize earlier arrive at greater compressibilities earlier"`](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p7-10). *«[настройки, генерализующие раньше, раньше приходят к большей сжимаемости](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p7-10)»*\
Доп. (мера): эффективный ранг $\operatorname{ER}(Z)$ корреляционной матрицы пре-логитов токена «=», считаемый только по обучающей выборке ([с. 7, абз. 7](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p7-7)).\
Доп. (оговорка авторов): $\mathbf{A^{*}}$ не достигает меньшего минимального эффективного ранга, чем **No LN**, — она лишь приходит в пределы $10\%$ от своего минимума чуть раньше ([с. 7, абз. 9](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p7-9)).

###### ref-3-11
**\[3.11\]** 2605.06352 — Tang et al., «Topological Signatures of Grokking». LID оценивается TwoNN (skdim) по 2000 точкам при $L=64$ ([с. 4, абз. 1](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p4-1)). [`"LID rises to roughly $20$–$25$ during memorization and then drops sharply at the grokking step, stabilizing near $5$"`](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p4-7). *«[LID поднимается примерно до $20$–$25$ при запоминании, а затем резко падает на шаге гроккинга, устанавливаясь около $5$](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p4-7)»*\
Доп. (заявленное разделение труда): [`"PH captures the dual aspect that LID misses: the global loop structure that becomes geometrically apparent only after this compression."`](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p4-7) — *«[PH схватывает ту двойственную сторону, которую LID упускает: всемирное петлевое строение, геометрически проявляющееся лишь после этого сжатия.](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p4-7)»*

###### ref-3-12
**\[3.12\]** 2604.20923 — Golwala, «ILDR: Geometric Early Detection of Grokking». [`"same class examples contract toward shared centroids while distinct classes pull apart, causing ILDR to rise"`](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/original/2604.20923.ildr-geometric-early-detection-of-grokking.md#p4-2). *«[примеры одного класса стягиваются к общим центроидам, тогда как различные классы расходятся, отчего ILDR растёт](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/2604.20923.ildr-geometric-early-detection-of-grokking.card.md#p4-2)»*\
Доп. (устройство меры): считается на подвыборке до $N = 1500$ отложенных примеров — около 15 на класс при $p=97$ и около 12 на класс в $S_5$ ([с. 5, абз. 5](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/2604.20923.ildr-geometric-early-detection-of-grokking.card.md#p5-5)); внутриклассовый разброс в 128–256 измерениях оценивается по десятку точек, и устойчивость такой оценки не обсуждается.
###### ref-3-13
**\[3.13\]** 2504.12700 — de Mello Koch & Ghosh 2025, «A Two-Phase Perspective on Deep Learning Dynamics». Информационное бутылочное горлышко как рамка второй поры гроккинга: после малой ошибки обучения $I(T;X)$ убывает при примерно постоянной $I(T;Y)$, а сжатие истолковано по образцу ренормгруппы с тремя геометрическими ходами огрубления. Нюанс: способ оценки взаимной осведомлённости не описан вовсе, размах сжатия около двух процентов, ходы огрубления не подсчитаны ни в одной сети. [`"the network enters the *compression phase*, during which $I(T;X)$ gradually decreases while $I(T;Y)$ remains roughly constant"`](../papers/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics/original/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics.md#p7-6). *«[сеть входит в *пору сжатия*, в которую $I(T;X)$ постепенно убывает при примерно постоянной $I(T;Y)$](../papers/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics.card.md#p7-6)»*\
Доп. (три хода огрубления): [`"**Alignment:** Zero-locus lines reorient so that they no longer intersect, eliminating vertices and reducing region count"`](../papers/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics/original/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics.md#p13-5) — *«[**Выравнивание:** нулевые линии переориентируются так, что больше не пересекаются, устраняя вершины и сокращая счёт областей](../papers/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics.card.md#p13-5)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase
Transition in Delayed Generalisation». [`"order parameter for representation geometry, distinct from compression-based complexity measures"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p4-3). *«[параметр порядка геометрии представлений, отличный от мер сложности на основе сжатия](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p4-3)»*

### Внешние работы

###### ref-5-1
**\[5.1\]** 2406.03999 — Внешняя работа (демотирована из корпуса): Song, Tan, Zou, Ma, Huang, «Unveiling the Dynamics of Information Interplay…». Через лемму 4.5 (хвост сингулярных чисел) и теорему 4.6 (граница $1-\frac{\text{rank}(\mathbf{Z}_{2})}{\text{rank}(\mathbf{Z}_{1})}$) разность энтропий увязана с ошибкой линейной аппроксимации: [`"making the difference of entropy a good surrogate bound for approximation error"`](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p4-9). *«[что делает разность энтропий хорошей суррогатной границей ошибки приближения](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p4-9)»* Переход от рангов к энтропии держится на приближении $\exp{(\operatorname{H}(\mathbf{G}(\mathbf{Z}))}\approx\text{rank}(\mathbf{Z})$; погрешность этого приближения нигде не оценена.
