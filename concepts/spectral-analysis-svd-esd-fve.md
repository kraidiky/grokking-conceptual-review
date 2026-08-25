# Спектральный анализ / FVE (spectral analysis, SVD, ESD, FVE)

[Механистическая интерпретируемость](mechanistic-interpretability.md) ← предыдущая карточка, следующая → [Каузальная абляция](causal-ablation-intervention.md)

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Спектральный анализ** в исследованиях [гроккинга](grokking.md) — семейство
диагностических методов, которые отслеживают отложенную генерализацию по
**спектру** (набору собственных или сингулярных чисел) внутренних матриц сети:
матриц весов, градиента, ковариации активаций или представлений. Ключевые
инструменты семейства: **SVD** (сингулярное разложение — представление матрицы
через её сингулярные числа и направления), **ESD** (empirical spectral density,
эмпирическая спектральная плотность — распределение собственных значений матрицы)
и **FVE** (Fraction of Variance Explained — доля дисперсии активаций, объяснённая
заданным набором базисных направлений). Общая гипотеза семейства: переход от
запоминания к обобщению проявляется как воспроизводимая перестройка спектра —
например, [HTSR](heavy-tailed-self-regularization-htsr.md)-анализ рассматривает `"empirical spectral density (ESD) of individual layer weight matrices"`
\[[1.3](#ref-1-3)\], а SVD-анализ весов показывает, что `"grokking is preceded by a near-degeneracy of the leading singular values of the attention matrices"`
\[[1.7](#ref-1-7)\].

## Детализация

Работы этого семейства различаются прежде всего **тем, спектр какой матрицы**
берётся за диагностический сигнал. В **весах**: сингулярные числа весовых матриц
через SVD (near-degeneracy — почти-вырождение двух ведущих сингулярных чисел
σ1 ≈ σ2 перед грокингом и его снятие в момент обобщения) \[[1.7](#ref-1-7)\], и
эмпирическая спектральная плотность весов ESD с показателем степенного закона α
из теории тяжёлохвостовой саморегуляризации (HTSR) \[[1.3](#ref-1-3)\]. В том же
ESD-русле аномально большие собственные значения за пределами «объёма»
Марченко–Пастура (Marchenko–Pastur bulk — предсказанная случайной матричной
теорией область, где лежит основная масса собственных значений «чистого шума»)
служат сигналом [анти-гроккинга](anti-grokking.md) и, как показывают авторы, могут
провоцировать [катастрофическое забывание](catastrophic-forgetting.md)
\[[1.6](#ref-1-6)\]. В **градиенте**: плохо обусловленный спектр (большой
condition number — отношение крупнейшего сингулярного числа к наименьшему)
удерживает динамику на плато и вызывает отложенную генерализацию
\[[1.4](#ref-1-4)\]. В **активациях и представлениях**: FVE — доля дисперсии
активаций MLP, объяснённая идеальными фурье-базисными функциями
\[[1.8](#ref-1-8)\], и нормированная спектральная энтропия ковариации
представлений \[[1.11](#ref-1-11)\]. В **представлениях как таковых**: Фурье-спектр
внутренних контуров (подсетей, реализующих подалгоритм) — [фурье-контуры](fourier-features-circuits.md) роднят
эту линию с сюжетом [clock vs pizza](clock-vs-pizza.md) — с прогресс-мерами FFD и
FCR \[[1.2](#ref-1-2)\]; спектр латентного ядра (kernel — матрица попарных
скалярных произведений преактиваций нейронов), у которого в направлении цели
появляются спектральные «пики» \[[1.1](#ref-1-1)\]; и PCA-разбор слоёв (метод
главных компонент — тот же спектральный анализ ковариации активаций)
\[[1.5](#ref-1-5)\]. Наконец, есть работы, интегрирующие спектральные меры с
каузальными и алгоритмическими в единую рамку \[[1.10](#ref-1-10)\], и работа,
делающая спектральную величину **[параметром порядка](order-parameter.md)** (order parameter — величина
из статистической физики, скачком меняющаяся на [фазовом переходе](phase-transition.md))
для проверки, действительно ли грокинг есть конечно-размерный переход
\[[1.9](#ref-1-9)\].

Дискуссия внутри семейства касается **где именно живёт решающий сигнал**. Линия
HTSR настаивает, что спектральная метрика весов (α ≈ 2 и «ловушки корреляций» в
ESD) — универсальный, не требующий доступа к тестовым данным диагностический
признак, тогда как конкурирующие неспектральные меры (разреженность активаций,
энтропия весов, сложность контуров, ℓ2-норма) не отличают анти-гроккинг
\[[1.3](#ref-1-3)\]\[[1.6](#ref-1-6)\]. Линия представлений, напротив, помещает
проксимальный сигнал в спектр ковариации представлений и с помощью
вмешательства показывает, что именно коллапс спектральной энтропии — а не рост
нормы параметров — является первичным драйвером задержки \[[1.11](#ref-1-11)\].

## Альтернативные определения и нюансы

### A. Сингулярные числа весов (weight-SVD)

Диагностическая величина — ведущие сингулярные числа весовых (в частности,
внимательных) матриц. Отличительный механизм: грокингу предшествует
**почти-вырождение** двух ведущих сингулярных чисел (σ1 ≈ σ2), создающее
ориентационную неустойчивость, а обобщение совпадает с **нарушением этой
спектральной симметрии**, когда одна мода начинает доминировать
\[[1.7](#ref-1-7)\]. Источник различия: сигнал берётся из спектра **весов**, и
пересекается (crossing) отношение двух старших сингулярных чисел.

### B. Эмпирическая спектральная плотность весов (ESD / HTSR)

Диагностическая величина — форма всего распределения собственных значений весовой
матрицы (ESD), сжатая до степенного показателя α (HTSR). Отличительный признак:
переход в фазу грокинга виден как изменение α, а последующая деградация
генерализации ([анти-гроккинг](anti-grokking.md)) — как α < 2 и появление
«ловушек корреляций», то есть выбросов за границей Марченко–Пастура
\[[1.3](#ref-1-3)\]\[[1.6](#ref-1-6)\]. Источник различия: используется **вся
масса** спектра весов (а не два старших числа), причём метрика не требует тестовых
данных.

### C. Фурье-спектр представлений (FFD / FCR)

Диагностическая величина — спектр Фурье внутренних представлений грокнутой модели,
сведённый к прогресс-мерам Fourier Frequency Density (FFD, плотность частот) и
Fourier Coefficient Ratio (FCR, отношение коэффициентов). Отличительное свойство:
авторы доказывают, что FFD и FCR **убывают** по мере роста тестовой точности и
отражают устройство внутренних контуров \[[1.2](#ref-1-2)\]. Источник различия:
спектр берётся в **фурье-базисе представлений**, а не из сингулярных чисел весов;
это спектральная мера сложности решения, специфичная для [модульной арифметики](modular-arithmetic.md).

### D. FVE — доля объяснённой дисперсии активаций

Диагностическая величина — Fraction of Variance Explained: для каждой доминантной
частоты k измеряется доля дисперсии активаций MLP, которую объясняют идеальные 2D
базисные функции cos и sin \[[1.8](#ref-1-8)\]. Отличительная роль: FVE — мера
**геометрической когерентности** фурье-контура (высокая FVE = чистая
низкоразмерная структура; её падение = разрушение контура). Источник различия:
спектральная величина считается в пространстве **активаций** относительно
известного идеального базиса, а не как собственные числа матрицы весов.

### E. Спектральная энтропия ковариации представлений

Диагностическая величина — нормированная спектральная энтропия ˜H
ковариационной матрицы представлений (энтропия распределения её собственных
значений). Отличительный тезис: грокинг проходит две фазы — расширение нормы, а
затем **коллапс энтропии**, и именно коллапс (переход ˜H через критический порог)
предшествует обобщению; вмешательство, мешающее коллапсу, откладывает грокинг
\[[1.11](#ref-1-11)\]. Источник различия: сигнал — «ширина» спектра **ковариации
представлений**, а причинным драйвером объявляется энтропия спектра, а не норма
параметров.

### F. Спектр градиента

Диагностическая величина — сингулярный спектр матрицы **градиента** скрытого слоя.
Отличительный механизм: плохая обусловленность (крупнейшее сингулярное число
намного больше наименьшего) заставляет динамику градиентного спуска «застревать»
на произвольно долгое время, что и порождает отложенную генерализацию; выравнивание
всех сингулярных чисел градиента ускоряет грокинг \[[1.4](#ref-1-4)\]. Источник
различия: спектр берётся из **градиента**, а не из весов или активаций, и ключева́
именно обусловленность, а не отдельное пересечение.

### G. Спектр латентного ядра и PCA слоёв

Диагностическая величина — спектр ковариации/ядра активаций слоёв. В варианте
латентного ядра [feature learning](feature-emergence-feature-learning.md) проявляется как появление спектральных **пиков**
ядра в направлении цели \[[1.1](#ref-1-1)\]; в PCA-варианте главные компоненты
активаций слоёв выявляют переход от связной сети к набору разреженных подсетей
\[[1.5](#ref-1-5)\]. Источник различия: спектр считается для **ядра/ковариации
активаций** и интерпретируется как эволюция структуры признаков, а не как
пороговый скаляр.

### H. Спектральный порядковый параметр (HTC)

Диагностическая величина — спектральный **head–tail contrast** (HTC, контраст
«голова–хвост» спектра), поднятый до статуса **параметра порядка**. Отличительная
цель: не просто описать динамику, а дать фальсифицируемый конечно-размерный
критерий — с пересечениями Биндера и анализом восприимчивости проверяется, что
грокинг организован как [фазовый переход](phase-transition.md), а не как гладкий
кроссовер \[[1.9](#ref-1-9)\]. Источник различия: спектральная величина здесь —
не прогресс-мера, а order parameter в строгом физическом смысле, привязанный к
размерной переменной (порядку группы p).

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.03789 — Rubin et al., «Droplets of Good Representations: Grokking as a First Order Phase Transition». [`"the latent kernel itself develops notable spikes in the target direction, indicating feature learning"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-4). *«[само скрытое ядро развивает заметные пики в направлении цели, что указывает на обучение признакам](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p1-4)»*

###### ref-1-2
**\[1.2\]** 2402.16726 — Furuta et al., «Internal Circuits in Grokked Transformers on Modular Polynomials». [`"Our Fourier analysis and novel progress measure for modular arithmetic, *Fourier Frequency Density* and *Fourier Coefficient Ratio*, characterize distinctive internal representations of grokked models per modular operation"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-1). *«[Наш фурье-анализ и новая мера прогресса для модульной арифметики — *плотность частот Фурье* (Fourier Frequency Density) и *отношение коэффициентов Фурье* (Fourier Coefficient Ratio) — описывают отличительные внутренние представления грокнувших моделей для каждой модульной операции](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.card.md#p1-1)»*\
Доп. (связь с точностью): [`"We prove that our proposed FFD and FCR decrease accompanying the test accuracy improvement"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-3). *«[Мы доказываем, что предложенные нами FFD и FCR убывают вместе с улучшением точности на тесте](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.card.md#p1-3)»*

###### ref-1-3
**\[1.3\]** 2506.04434 — Prakash & Martin 2025, «Grokking and Generalization Collapse: Insights from HTSR theory». [`"HTSR theory examines the empirical spectral density (ESD) of individual layer weight matrices $(\mathbf{W})$, quantified by the heavy-tailed power law (PL) exponent $\alpha$"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p1-4). *«[Теория HTSR рассматривает опытную спектральную плотность (ESD) матриц весов отдельных слоёв $(\mathbf{W})$, описываемую тяжелохвостым степенным (PL) показателем $\alpha$](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p1-4)»*\
Доп.: [`"tracking the transition into the grokking phase, and crucially, predicting a subsequent decrease in generalization"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p1-4) — *«[отслеживающую переход в пору гроккинга и, что решающе, предсказывающую последующее падение способности обобщать](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p1-4)»*.

###### ref-1-4
**\[1.4\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». [`"Ill-conditioned Gradient Spectra causes delayed generalization"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#fig-4). *«[Дурно обусловленные спектры градиентов вызывают отложенную генерализацию](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#fig-4)»*\
Доп. (механизм): [`"the largest singular-value (corresponding to a fast direction) is much larger than the smallest (corresponding to slow directions)"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#fig-4). *«[наибольшее сингулярное число (отвечающее быстрому направлению) много больше наименьшего (отвечающего медленным направлениям)](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#fig-4)»*

###### ref-1-5
**\[1.5\]** 2510.25966 — Hutchison et al., «Grokking in the Ising Model». `"novel PCA-based network layer analysis techniques"` (line 13). *«новые техники анализа слоёв сети на основе PCA»*\
Доп. (что выявлено): `"the observed behavior is interpreted as a transition from a connected network to a group of sparse subnetworks"` (line 14). *«наблюдаемое поведение интерпретируется как переход от связной сети к группе разреженных подсетей»*

###### ref-1-6
**\[1.6\]** 2602.02859 — Prakash et al., «Late-Stage Generalization Collapse in Grokking: Detecting anti-grokking with WeightWatcher». [`"large eigenvalues beyond the Marchenko–Pastur bulk in the empirical spectral density of shuffled weight matrices"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p1-4). *«[больших собственных чисел за пределами основной части Марченко — Пастура в эмпирической спектральной плотности перемешанных весовых матриц](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.card.md#p1-4)»*

###### ref-1-7
**\[1.7\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». [`"grokking is preceded by a near-degeneracy of the leading singular values of the attention matrices"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2). *«[гроккингу предшествует почти вырождение ведущих сингулярных чисел матриц внимания](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p1-2)»*\
Доп. (снятие симметрии): [`"Generalization coincides with the breaking of this spectral symmetry as one mode dominates. This spectral structure is confirmed to be basis-independent"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2). *«[Генерализация совпадает с нарушением этой спектральной симметрии, когда одна мода начинает главенствовать. Подтверждено, что эта спектральная структура не зависит от базиса](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p1-2)»*

###### ref-1-8
**\[1.8\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Topology». [`"we computed the Fraction of Variance Explained (FVE)"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p13-1). *«[мы вычислили долю объяснённой дисперсии (FVE)](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p13-1)»*\
Доп. (как считается): [`"we measured the proportion of variance in the MLP activations explained by the ideal 2D basis functions"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p13-1). *«[мы измеряли долю дисперсии активаций MLP, объяснённую идеальными двумерными базисными функциями](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p13-1)»*

###### ref-1-9
**\[1.9\]** 2603.24746 — Bi et al., «Grokking as a Falsifiable Finite-Size Transition». [`"a held-out spectral head–tail contrast (HTC) as the order parameter"`](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-7). *«[отложенный спектральный противопоставительный признак «голова — хвост» (HTC) — за параметр порядка](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-7)»*

###### ref-1-10
**\[1.10\]** 2603.29262 — Zhang et al., «Grokking: From Abstraction to Intelligence». [`"We integrate causal, spectral, and algorithmic complexity measures alongside Singular Learning Theory"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-2). *«[Мы соединяем причинные, спектральные и алгоритмические меры сложности с сингулярной теорией обучения](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-2)»*

###### ref-1-11
**\[1.11\]** 2604.13123 — Truong Xuan Khanh et al., «Spectral Entropy Collapse as an Empirical Signature of Delayed Generalisation». [`"the key quantity is the normalised spectral entropy"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2). *«[ключевой величиной служит приведённая спектральная энтропия](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2)»*\
Доп. (двухфазный механизм): [`"proceeds in two phases — norm expansion followed by entropy collapse — and that entropy collapse is the proximate signal preceding generalisation"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2). *«[протекает в две поры — расширение нормы, а затем схлопывание энтропии, — и что именно схлопывание энтропии есть ближайший признак, предшествующий генерализации](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2)»*\
Доп. (спектр возбуждений, а не весов): [`"we study the spectrum of the *activation covariance* $\hat{\Sigma}=\mathbb{E}[zz^{\top}]$ as a phenomenological order parameter observed during training, without introducing a regulariser"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-4) — *«[мы изучаем спектр *ковариации возбуждений* $\hat{\Sigma}=\mathbb{E}[zz^{\top}]$ как феноменологический параметр порядка, наблюдаемый по ходу обучения, не вводя никакого регуляризатора](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-4)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2306.17844 — Zhong et al. 2023, «The Clock and the Pizza». Нюанс: FVE здесь — средство выбрать между двумя формулами на всех $59^{3}$ логитах (99.18–99.28% против 75.38–75.62%), а не характеристика спектра весов; все числа сняты у обученных моделей. [`"We find that $Q_{abc}({\it Pizza})$ explains substantially more variance than $Q_{abc}({\it Clock})$."`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p5-2). *«[Мы находим, что $Q_{abc}({\it Pizza})$ объясняет существенно больше дисперсии, чем $Q_{abc}({\it Clock})$.](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p5-2)»*

###### ref-1-12
**\[1.12\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: FVE считается по подпространствам неприводимых представлений, а не по частотам; активации перед подсчётом центрируются, иначе положительное среднее после ReLU завышает все доли. Недобор до $100\%$ авторы объясняют тем, что ReLU лишь приближает матричное умножение. [`"our algorithm’s prediction does not explain all of the logits, as can be seen from FVE being less than 100%"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p23-9). *«[предсказание нашего алгоритма объясняет не все логиты, как видно из того, что FVE меньше 100%](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p23-9)»*\
Доп.: [`"Accounting for this would artificially increase all ‘fraction of variance explained’ metrics."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p23-1) — *«[Учёт этого искусственно увеличил бы все метрики «доли объяснённого разброса»](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p23-1)»*.

###### ref-3-2
**\[3.2\]** 2310.16441 — Levi et al. 2023. Нюанс: редкий случай, когда задержка прочитана по спектру матрицы Грама **данных**; нижний край спектра $\lambda_{-}=(1-\sqrt{\lambda})^{2}$ задаёт и скорость позднего спада, и постоянное отношение потерь, из которых складывается формула времени гроккинга. Спектров весовых матриц, ESD и SVD в работе нет. [`"its eigenvalues, $\nu_{i}$, follow the Marchenko-Pastur (MP) distribution [13]"`](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/original/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.md#p4-5). *«[её собственные значения $\nu_{i}$ подчинены распределению Марченко — Пастура (MP) [13]](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.card.md#p4-5)»*

###### ref-1-13
**\[1.13\]** 2408.11804 — Yunis et al., «Approaching Deep Learning through the Spectral Dynamics of Weights». Отдельный вариант семейства: сигнал берётся из спектра весовых матриц и сворачивается в энтропию нормированного распределения сингулярных чисел, делённую на ранг, а сверх спектра чисел добавлен спектр векторов — согласование соседних слоёв $A(t)$ и совпадение с концом обучения $S(t)$. Мера считается на любой архитектуре без знания задачи. [`"we compute the (normalized) effective rank of a matrix $W$ (Roy & Vetterli 2007) with rank $R$ as"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p4-4). *«[мы вычисляем (нормированный) эффективный ранг матрицы $W$ (Roy & Vetterli 2007) с рангом $R$ как](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p4-4)»*\
Доп.: [`"$\text{EffRank}(W)$ is the entropy of the normalized singular value distribution"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p4-5) — *«[$\text{EffRank}(W)$ есть энтропия нормированного распределения сингулярных чисел](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p4-5)»*\
Доп. (граница применимости): [`"we disregard 1D bias and normalization parameters in our analysis"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p6-7) — *«[мы оставляем в стороне одномерные параметры смещения и нормализации](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p6-7)»*.

### Поддерживают

###### ref-3-3
**\[3.3\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Нюанс: спектральных сигнала два и они независимы — зазор $g_{12}=\sigma_{1}(W_{Q})-\sigma_{2}(W_{Q})$ в самих операторах внимания и зазор $g_{23}=\sigma_{2}^{2}-\sigma_{3}^{2}$ скользящей матрицы Грама обновлений; второй разделяет грокающие прогоны и контроли непересекающимися распределениями. Величины «порог кривизны $\omega$» и «замечание 12.22», на которые опирается разбор, взяты из препринта того же автора и в этой статье не определены. [`"*degeneracy breeds instability, symmetry breaking resolves it, and generalization follows*"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p16-2). *«[*вырожденность порождает неустойчивость, нарушение симметрии её разрешает, а следом идёт генерализация*](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p16-2)»*\
Доп. (счёт по прогонам): [`"$g_{23}$ declines before grokking in 18 of 18 runs with weight decay (mean $39\times$, range $8$–$111\times$) and in only 1 of 18 matched controls"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p20-2) — *«[$g_{23}$ спадает перед гроккингом в 18 прогонах из 18 с weight decay (в среднем $39\times$, промежуток $8$–$111\times$) и лишь в 1 из 18 сопоставленных контролей](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p20-2)»*.

### Оспаривают

###### ref-2-1
**\[2.1\]** 2602.16967 — Xu, «Early-Warning Signals of Grokking via
Loss-Landscape Geometry». Нюанс: прямой пересмотр хронологии `2602.16746` —
там раскрытие спектрального зазора $\sigma_{1}\gg\sigma_{2}$ помещалось перед
гроккингом, здесь оно наступает после и объявлено следствием weight decay,
действующего на уже обобщившуюся модель. [`"The dominant-mode separation is a *consequence* of grokking (driven by continued weight decay), not a prerequisite."`](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/original/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.md#p16-9). *«[Разделение главенствующей моды есть *следствие* гроккинга (движимое продолжающимся weight decay), а не его предпосылка.](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.card.md#p16-9)»*\
Доп. (зазор собственных чисел матрицы Грама): [`"The eigenvalue gap $g_{23}$ declines by $\mathbf{33\times}$ (Dyck mean) and $\mathbf{43\times}$ (SCAN mean) during the grokking transition."`](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/original/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.md#p19-2) — *«[Зазор собственных чисел $g_{23}$ спадает в $\mathbf{33\times}$ (среднее по Dyck) и в $\mathbf{43\times}$ (среднее по SCAN) в ходе перехода гроккинга.](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.card.md#p19-2)»*.

###### ref-1-14
**\[1.14\]** 2505.15624 — AlQuabeh, Bojković, Nwadike, Inui, «Mechanistic Insights into Grokking from the Embedding Layer». Прослеживает ранги $\mathbf{E}$, $\mathbf{W}$ и их произведения в трёх настройках и приходит к отрицательному выводу, ценному своей честностью: $\text{rank}(\mathbf{E}\mathbf{W})$ падает во всех трёх, но моментом гроккинга это не управляет. Нюанс: заявление об устойчивости ранга $\mathbf{E}$ верно только для панели с обычным Adam — при Adam-LR ранг $\mathbf{E}$ падает с 99 примерно до 30 и возвращается, а при $wd=0.005$ падает примерно до 20; третья настройка вообще не обобщает, и её обучающая точность обрушивается к нулю, чего текст не оговаривает. [`"However, only Adam-LR shows continued rank changes after generalization, suggesting that rank evolution alone does not capture the onset of grokking."`](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/original/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.md#p15-7). *«[Однако только Adam-LR обнаруживает продолжающиеся изменения ранга после генерализации, что указывает на то, что одна лишь эволюция ранга не улавливает наступление гроккинга.](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.card.md#p15-7)»*\
Доп. (описание трёх пор ранга $\mathbf{W}$): [`"As shown in Figure 10, $\mathbf{W}$ exhibits three distinct phases: an early drop during training loss reduction, a plateau, and a final decline aligned with grokking. In contrast, $\mathbf{E}$’s rank remains largely stable throughout."`](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/original/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.md#p15-6) — *«[Как показано на рисунке 10, $\mathbf{W}$ обнаруживает три отчётливые поры: раннее падение во время снижения обучающей потери, полку и окончательный спад, совпадающий с гроккингом. В отличие от этого, ранг $\mathbf{E}$ остаётся по большей части устойчивым на всём протяжении.](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.card.md#p15-6)»*.

###### ref-1-15
**\[1.15\]** 2503.10483 — Pomarico et al., «Grokking as an entanglement transition in tensor network machine learning». Нюанс: первые оторвавшиеся значения доходят примерно до 0,5, затем собираются в группу с центром 0,018; степенных подгонок, тяжелохвостовых показателей и случайно-матричных эталонов нет — только гистограммы, и причинный глагол «включает» не подкреплён вмешательством. [`"a split off from the continuous distribution of eigenvalues, that activate the grokking transition"`](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p11-3). *«[отрыв от сплошного распределения собственных значений, который включает переход гроккинга](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p11-3)»*

###### ref-1-16
**\[1.16\]** 2604.06256 — Xu, «Spectral Edge Dynamics Reveal Functional Modes of Learning». Нюанс: спектр берётся у матрицы Грама подряд идущих обновлений, а не у весов; окно $W=20$ в пространстве $D=131{,}072$, без случайной опоры и без проверки устойчивости к $W$. [`"Over a sliding window of $W=20$ consecutive updates, we form the *Gram matrix*"`](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/original/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.md#p3-5). *«[На скользящем окне из $W=20$ подряд идущих обновлений мы строим *матрицу Грама*](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.card.md#p3-5)»*\
Доп. (что этим меряется): [`"the $g_{23}$ gap cleanly separates grokking from non-grokking regimes"`](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/original/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.md#p6-7). *«[зазор $g_{23}$ чисто отделяет режимы гроккинга от режимов без гроккинга](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.card.md#p6-7)»*

### Поддерживают

###### ref-3-4
**\[3.4\]** 2602.06702 — Singh, Misra, Orvieto, «Explaining Grokking in Transformers…». [`"we fit a powerlaw to the eigenvalue spectrum of the feature correlation matrix"`](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p7-7). *«[мы подгоняем степенной закон к спектру собственных значений корреляционной матрицы признаков](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p7-7)»* Ниже показатель $\alpha$ — тяжелее хвост; порядок настроек по $\alpha$ совпадает с порядком гроккинга.\
Доп. (обработка): [`"To eliminate noisy spikes caused by slingshots, this is shown under minimum aggregation over $5$ seeds"`](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#fig-6) — *«[Чтобы устранить шумовые всплески, вызванные рогатками, она показана при агрегации минимумом по $5$ семенам](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#fig-6)»*.

### Поддерживают

###### ref-3-5
**\[3.5\]** 2506.12284 — Walker et al., «GrokAlign: Geometric Characterisation and Acceleration of Grokking». Переносит FVE с активаций на входо-выходной якобиан: доля дисперсии, объяснённая первой главной компонентой якобиана и усреднённая по обучающим точкам, отслеживается по ходу обучения и читается как эффективный ранг. Нюанс: величина служит подпоркой допущения о низком ранге, а не самостоятельной мерой перехода — спектра весовых матриц работа не меряет, само допущение приписано пяти чужим работам и признано требующим более глубокой характеризации; при кросс-энтропии за 500 шагов доля доходит лишь до ≈0,8, а кривые под weight decay и под GrokAlign совпадают. [`"we recorded the average explained variance of the first principal component of the Jacobians evaluated at the training data (PC1)"`](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/original/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.md#fig-2). *«[мы записывали среднюю объяснённую дисперсию первой главной компоненты якобианов, вычисленных на обучающих данных (PC1)](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.card.md#fig-2)»*\
Доп. (что стои́т на этом допущении): [`"some of our reasoning is dependent on the assumption that deep network training dynamics minimise the rank of the weight matrices"`](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/original/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.md#p11-3) — *«[часть нашего рассуждения зависит от допущения, что динамика обучения глубоких сетей минимизирует ранг матриц весов](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.card.md#p11-3)»*.

###### ref-1-17
**\[1.17\]** 2605.08237 — Wang, Ying, Kanamori 2026, «Distributional Spectral Diagnostics for Localizing Grokking Transitions». [`"the residual itself should then be interpreted as a transition or fragility signal"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p4-3). *«[саму невязку следует истолковывать как сигнал перехода или хрупкости](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p4-3)»*\
Доп. (место сигнала): [`"Because the diagnostic is computed from the chosen output distribution rather than from hidden-unit coordinates, it does not depend on hidden-unit indexing."`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p2-1) — *«[Поскольку диагностика вычисляется по выбранному выходному распределению, а не по координатам скрытых единиц, она не зависит от индексации скрытых единиц.](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p2-1)»*\
Доп. (почему конвейер одномерен): [`"there is generally no global isometric isomorphism that maps the Wasserstein space into a linear space [44]"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p13-1) — *«[вообще говоря нет глобального изометрического изоморфизма, отображающего вассерштейново пространство в линейное [44]](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p13-1)»*

### Поддерживают

###### ref-3-6
**\[3.6\]** 2605.20441 — Verma 2026, «Weight Decay Regimes in Grokking Transformers: Cheap Online Diagnostics». Переносит спектральную диагностику из весов в активации: аффинно нормированное отношение участия спектра ковариации образцов голов падает с $0.86$ при инициализации до $0.13$ при позднем коллапсе и заявлено как прямая проверка нарушения перестановочной симметрии $S_{H}$. Нюанс: проверка удостоверяет только алгебраическую эквивалентность отношения участия и коэффициента вариации спектра (тождество доказано и машинно проверено в Lean 4 на 183 строках слой-эпоха); причинного порога нарушения симметрии нет, и на архитектуры без внимания диагностика не переносится. [`"initialization $0.86\to$ Phase 1 attention-head coordination $0.71$ at epochs 100 to 500 (grokking onset) $\to$ Phases 2 to 4 differentiation oscillation (epochs 1000 to 12 500) $\to$ Phase 5 collapse $0.13$ at epoch 20 000"`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p8-1). *«[инициализация $0.86\to$ согласование голов внимания фазы 1 $0.71$ на эпохах от 100 до 500 (вход в гроккинг) $\to$ колебание расхождения фаз со 2 по 4 (эпохи с 1000 по 12 500) $\to$ коллапс фазы 5 $0.13$ на эпохе 20 000](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p8-1)»*\
Доп. (что именно меряется): [`"Head indices are exchangeable at initialization (permutation group $S_{H}$ acts transitively on heads); post-Phase-2, head-pattern covariance becomes lower-participation and permutation-distinguishable."`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p7-2) — *«[Индексы голов взаимозаменяемы при инициализации (группа перестановок $S_{H}$ действует на головах транзитивно); после фазы 2 ковариация образцов голов становится менее участвующей и различимой по перестановкам.](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p7-2)»*.
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.2\]** 2511.12768 — Hong et al., «Evidence of Phase Transitions in Small Transformer-Based Language Models». [`"applying singular value decomposition, neuron activation analysis, and embedding alignment to track how grokking unfolds inside the network"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p4-5). *«[приложив сингулярное разложение, разбор возбуждений нейронов и согласование вложений, чтобы проследить, как гроккинг разворачивается внутри сети](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p4-5)»*

**\[4.3\]** 2505.20172 — Boursier et al., «A Theoretical Framework for Grokking: Interpolation followed by Riemannian Norm Minimisation». Лишь пользуется: сингулярные значения восстановленной матрицы $UV^{\top}$ служат главным измеряемым показателем сноса к малому рангу — из десяти семь уезжают к нулю на поре гроккинга, три приходят к истинным. Спектральных мер в смысле корпуса (ESD, тяжёлые хвосты, FVE) работа не вводит. [`"as grokking starts around time $t\approx 10^{2}$, all but three begin decay towards zero. The remaining three approach the true singular values"`](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#fig-2). *«[когда около времени $t\approx 10^{2}$ начинается гроккинг, все, кроме трёх, начинают затухать к нулю. Оставшиеся три подходят к истинным сингулярным значениям](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#fig-2)»*

**\[4.4\]** 2504.16041 — Tveit, Remseth, Skogvold, «Muon Optimizer Accelerates Grokking». Нюанс: ни сингулярных чисел весов, ни спектров, ни норм по слоям в работе не приведено, хотя ограничение спектральной нормы объявлено одним из действующих начал; к тому же названо оно неточно — Muon нормирует и ортогонализует обновление, а не ограничивает спектральную норму весов. [`"Applies spectral norm constraints"`](../papers/2504.16041.muon-optimizer-accelerates-grokking/original/2504.16041.muon-optimizer-accelerates-grokking.md#fig-1). *«[Прилагает ограничения спектральной нормы](../papers/2504.16041.muon-optimizer-accelerates-grokking/2504.16041.muon-optimizer-accelerates-grokking.card.md#fig-1)»*

**\[4.5\]** 2604.20923 — Golwala, «ILDR: Geometric Early Detection of Grokking». О спектральной энтропии сингулярных чисел первых двух весовых матриц сказано: [`"Logged as a diagnostic but not used for flagging."`](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/original/2604.20923.ildr-geometric-early-detection-of-grokking.md#p5-8) — *«[Записывается как диагностика, но для поднятия флага не используется.](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/2604.20923.ildr-geometric-early-detection-of-grokking.card.md#p5-8)»*; во введении те же меры отвергнуты как косвенные и малоопережающие ([с. 2, абз. 3](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/2604.20923.ildr-geometric-early-detection-of-grokking.card.md#p2-3)).

### Внешние работы

###### ref-5-1
**\[5.1\]** 2502.20763 — Внешняя работа (демотирована из корпуса): Tan & Huang 2025, «Information-Theoretic Perspectives on Optimizers». Нюанс: взамен предложен разрыв энтропии спектра гессиана — мера его неравномерности. [`"Traditional methods for analyzing optimizers, such as sharpness metrics based on the Hessian matrix, offer valuable insights into the local landscape around minima or saddle points but fail to fully capture the underlying optimization dynamics"`](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p1-2). *«[Расхожие способы разбора приёмов оптимизации, скажем меры крутизны на основе матрицы Гессе, дают ценные сведения о местном ландшафте вокруг наименьших точек или седловин, но не улавливают вполне лежащего под этим хода оптимизации](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p1-2)»*

```
concept:
  category: 6                    # 6. Аналитические инструменты и метрики (Analytical tools & metrics)
  papers_linked: 28             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-25
```
