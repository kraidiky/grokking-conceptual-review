# Границы генерализации (generalization bounds)

[Максимизация зазора](margin-maximization-implicit-bias.md) ← предыдущая карточка, следующая → [Закон задержки через норм-сепарацию](norm-separation-delay-law.md)

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Границы генерализации** — доказуемые (формально выводимые) верхние или нижние
оценки на обобщающую способность модели: на **ошибку генерализации** (разрыв
между обучающей и популяционной ошибкой, то есть ошибкой в среднем по всему
распределению данных), на **выборочную сложность** (sample complexity — число
примеров, необходимое, чтобы модель начала обобщать) или — в приложении к
[гроккингу](grokking.md) — на **длительность задержки** перед наступлением
генерализации. В контексте гроккинга такие границы превращают эмпирическое
наблюдение отложенной генерализации в количественную теорию: они формализуют,
при каких условиях и как быстро генерализация наступает. Первые строгие оценки
для этого режима вводятся в анализе [модульного сложения](modular-arithmetic.md), где доказываются нижние
границы на популяционную потерю \[[1.2](#ref-1-2)\], и распространяются далее на
PAC-байесовские \[[5.1](#ref-5-1)\]↗, байесовские \[[1.6](#ref-1-6)\] и
информационно-геометрические \[[1.5](#ref-1-5)\] семейства оценок.

## Детализация

Границы генерализации в корпусе о гроккинге распадаются на несколько ветвей,
различающихся тем, какая величина оценивается и через какой **параметр
сложности** она контролируется. Первая ветвь оценивает **саму ошибку
генерализации или выборочную сложность**. Mohamadi et al. доказывают нижние
границы на популяционную ℓ2-потерю (ожидаемую квадратичную ошибку по всему
распределению) для обучения модульного сложения ядерными предсказателями и
показывают, что малая ℓ∞-норма позволяет обобщать при существенно меньшем числе
примеров \[[1.2](#ref-1-2)\]; здесь ключевую роль играет выбор **меры сложности**
(ℓ2 против ℓ∞-нормы, внутренняя размерность предсказателя). Rubin et al.
формулируют основанный на гроккинге механизм, снижающий выборочную сложность по
сравнению с пределом гауссовского процесса (GP-пределом — режимом «ленивого»
обучения, где сеть эквивалентна фиксированному ядру и не выучивает признаки)
\[[1.1](#ref-1-1)\]. Вторая ветвь строит оценки через специализированные
параметры сложности: PAC-байесовскую границу (класс вероятностных оценок, где
разрыв контролируется KL-дивергенцией апостериорного распределения весов от
априорного), выявляющую, какие метрики сети влияют на генерализацию
\[[5.1](#ref-5-1)\]↗; верхнюю границу на ошибку генерализации через популяционную
внутриклассовую дисперсию представлений (связанную с явлением [neural collapse](neural-collapse.md) —
стягивания представлений одного класса в точку) \[[1.5](#ref-1-5)\]; оценку в
рамках сингулярной теории обучения (SLT), где локальный коэффициент обучения λ
(local learning coefficient — координатно-инвариантная мера локальной
статистической сложности минимума) управляет асимптотической байесовской ошибкой
генерализации \[[1.6](#ref-1-6)\]; и границу для MoE-архитектуры (mixture of
experts — сети с маршрутизацией входа по «экспертам»), улучшаемую более
структурированными путями маршрутизации \[[1.4](#ref-1-4)\].

Отдельная, специфичная именно для гроккинга ветвь оценивает не ошибку, а **время
задержки** между [фазой запоминания](memorization-phase.md) и наступлением
генерализации. Xu et al. дают первые строгие количественные границы «времени
гроккинга» в терминах гиперпараметров обучения, показывая, что задержку можно
усилить или устранить настройкой гиперпараметров \[[3.1](#ref-3-1)\]; Truong et
al. доказывают совпадающие верхнюю и нижнюю границы на задержку, растущую
логарифмически с отношением норм конкурирующих решений \[[3.2](#ref-3-2)\]. Обе
работы делают из этого один и тот же вывод: раз задержка допускает строгие
границы, гроккинг — не загадочный сбой, а предсказуемое следствие условий
обучения. Тем самым границы на задержку сближают гроккинг с трактовкой в духе
[фазового перехода](phase-transition.md), но переводят её из качественного
описания в количественный закон.

## Альтернативные определения и нюансы

### A. Границы на ошибку генерализации и выборочную сложность

Классическая трактовка: граница генерализации оценивает разрыв между обучающей и
популяционной ошибкой либо число примеров, нужное для обобщения. Отличительная
машинерия — **мера сложности**, входящая в оценку. У Mohamadi et al. это ℓ∞-норма
(против ℓ2), при которой сеть провабельно обобщает с гораздо меньшим числом
примеров, чем требует ядерный режим; техника доказательства анализирует
популяционную ℓ2-потерю классов функций через внутреннюю размерность
предсказателя \[[1.2](#ref-1-2)\]. У Rubin et al. параметром служит сравнение с
GP-пределом: грокинг-механизм снижает выборочную сложность относительно ленивого
ядерного режима за счёт [обучения признаков](feature-emergence-feature-learning.md) \[[1.1](#ref-1-1)\].

### B. Информационные, байесовские и геометрические оценки

Здесь граница определяется не через норму весов, а через иной [параметр порядка](order-parameter.md),
считываемый из внутренней структуры или апостериорной геометрии. PAC-байесовская
оценка контролирует разрыв через KL-дивергенцию апостериора весов и связывает
генерализацию с конкретными метриками сети \[[5.1](#ref-5-1)\]↗. В SLT
контролирующая величина — локальный коэффициент обучения λ, задающий
асимптотическую байесовскую ошибку генерализации, так что более простые
(меньший λ) минимумы статистически предпочтительнее \[[1.6](#ref-1-6)\]. У
Sakamoto et al. параметром выступает популяционная внутриклассовая дисперсия
представлений: её стягивание даёт верхнюю границу на ошибку генерализации
\[[1.5](#ref-1-5)\]. У Li et al. в анализе одно-слойного MoE границу улучшает
структурированность путей маршрутизации \[[1.4](#ref-1-4)\].

### Поддерживают

Работы, присоединяющиеся к нюансу «строгие границы ставятся не на ошибку, а на
саму задержку генерализации, из-за чего гроккинг предсказуем и управляем». Xu et
al. дают первые количественные границы «времени гроккинга» через гиперпараметры,
явно показывая, что задержку можно усилить или обнулить настройкой
\[[3.1](#ref-3-1)\]. Truong et al. доказывают совпадающие (tight) верхнюю и нижнюю
границы задержки, растущей логарифмически с отношением норм запоминающего и
обобщающего решений \[[3.2](#ref-3-2)\]. Обе оценивают одну и ту же
специфическую для гроккинга величину — интервал между запоминанием и
генерализацией, а не популяционный риск.

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.03789 — Rubin et al., «Grokking as a First Order Phase Transition in Two Layer Networks». [`"We flesh out a Grokking-based mechanism that can reduce the sample complexity compared to the GP limit"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p2-3). *«[Мы намечаем основанный на гроккинге механизм, способный снизить выборочную сложность по сравнению с пределом GP](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p2-3)»*

###### ref-1-2
**\[1.2\]** 2407.12332 — Mohamadi et al., «A Theoretical Analysis of Grokking Modular Addition». [`"To prove the generalization lower bounds, we develop a novel general technique to analyze the population ℓ2 loss of learning general function classes with predictors of intrinsic dimension n"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p3-5). *«[Чтобы доказать нижние оценки на генерализацию, мы разрабатываем новый общий приём разбора популяционной $\ell_{2}$-потери при выучивании общих классов функций предсказателями внутренней размерности $n$](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p3-5)»*\
Доп.: [`"prove lower bounds on population ℓ2 loss for the general case of learning modular addition on m summands (rather than 2 or 3) with kernels"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p3-5) — *«[доказать нижние оценки популяционной $\ell_{2}$-потери для общего случая выучивания модульного сложения $m$ слагаемых (а не 2 или 3) ядрами](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p3-5)»*.

###### ref-1-4
**\[1.4\]** 2506.21551 — Li et al., «Grokking in LLM Pretraining? Monitor Memorization-to-Generalization without Test». [`"MoE, showing that more structured pathways improve the generalization bound"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2). *«[MoE, показывая, что более устроенные пути улучшают границу генерализации](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)»*

###### ref-1-5
**\[1.5\]** 2509.20829 — Sakamoto et al., «Explaining Grokking and Information Bottleneck through Neural Collapse Emergence». [`"generalization error in terms of the population within-class variance of the learned representations"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p2-1). *«[ошибки генерализации через популяционную внутриклассовую дисперсию выученных представлений](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p2-1)»*

###### ref-1-6
**\[1.6\]** 2603.01192 — Cullen, «A Basin-Selection Perspective on Grokking». [`"The same quantity also controls the asymptotic Bayes generalisation error"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-4). *«[Та же величина управляет и асимптотической байесовской ошибкой генерализации](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-4)»*\
Доп. (инвариантность): [`"the LLC is a diffeomorphism-invariant measure of degeneracy Lau et al. (2025), with strong theoretical and empirical links to Bayesian generalisation"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p3-4) — *«[LLC есть мера вырождения, инвариантная относительно диффеоморфизмов Lau et al. (2025), с крепкими теоретическими и опытными связями с байесовской генерализацией](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p3-4)»*

###### ref-1-7
**\[1.7\]** 2311.18817 — Lyu et al., «Dichotomy of Early and Late Phase Implicit Biases Can Provably Induce Grokking». Нюанс: разрыв выборочной сложности между ранним и поздним решениями получен радемахеровской техникой (теоремы E.4 и E.5), но обе оценки — **верхние** границы ошибки, поэтому строго показано не «ядерное решение не обобщает», а «его гарантия слабее». [`"Applying standard generalization bounds based on Rademacher complexity can show that the max $L^{2}$-margin linear classifier needs $\mathcal{O}(kd)$ to generalize, while the max $L^{1}$-margin linear classifier only needs $\mathcal{O}(k^{2}\log d)$."`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p7-7). *«[Применение стандартных границ обобщения на основе радемахеровской сложности позволяет показать, что линейному классификатору максимального $L^{2}$-зазора требуется $\mathcal{O}(kd)$, чтобы обобщать, тогда как линейному классификатору максимального $L^{1}$-зазора требуется лишь $\mathcal{O}(k^{2}\log d)$](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p7-7)»*

###### ref-1-8
**\[1.8\]** 2310.02541 — Xu et al. 2023. Нюанс: даны обе границы — почти случайная ошибка при $t=1$, зажатая с двух сторон, и $\exp(-\Omega(n^{2.01}))$ на окне $[Cn^{0.01},\sqrt{n}]$; выведены траекторным разбором, а не через ёмкость класса, и только внутри окна с верхней границей на число шагов. [`"the network continues to overfit to the training data while simultaneously achieving test error $\exp(-\Omega(n^{2.01}))$, which guarantees a near-zero test error for large $n$"`](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/original/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.md#p6-6). *«[сеть продолжает переобучаться на обучающие данные и одновременно достигает ошибки на тесте $\exp(-\Omega(n^{2.01}))$, что гарантирует почти нулевую ошибку на тесте при больших $n$](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.card.md#p6-6)»*

###### ref-1-9
**\[1.9\]** 2211.12316 — Bhattamishra et al., «Simplicity Bias in Transformers and their Ability to Learn Sparse Boolean Functions». Нюанс: чувствительность встроена в классическую машинерию ёмкости — VC-размерность класса с максимальной чувствительностью не выше $k$ есть $\mathcal{O}(n^{2k})$, — но это утверждение о классе функций, а не о найденной сетью функции; численных значений границы в работе нет. [`"Any function $f$ with a maximum sensitivity $k$ can be uniquely determined by its values on any Hamming ball of radius $2k$ in $\{0,1\}^{n}$ (Gopalan et al. 2016)"`](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p18-2). *«[Любая функция $f$ с максимальной чувствительностью $k$ однозначно определяется своими значениями на любом шаре Хэмминга радиуса $2k$ в $\{0,1\}^{n}$ (Gopalan et al. 2016)](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p18-2)»*\
Доп. (опытная половина): [`"We observe a positive correlation between the measures and generalization gap indicating that when sensitivity is higher, the models are more likely to overfit and achieve poorer generalization performance"`](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p18-7) — *«[Мы наблюдаем положительную корреляцию между этими мерами и разрывом обобщения, что указывает, что при более высокой чувствительности модели вероятнее переобучаются и достигают худшего качества обобщения](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p18-7)»*.
## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2601.19791 — Xu et al., «To Grok Grokking: Provable Grokking in Ridge Regression». Нюанс: границы ставятся на саму задержку генерализации («время гроккинга»), из-за чего гроккинг оказывается управляемым. [`"these are the first rigorous quantitative bounds on the generalization delay (which we refer to as the “grokking time”) in terms of training hyperparameters"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p1-2). *«[это первые строгие количественные границы на задержку генерализации (которую мы называем «временем гроккинга») через гиперпараметры обучения](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.card.md#p1-2)»*

###### ref-3-2
**\[3.2\]** 2603.13331 — Truong et al., «The Norm-Separation Delay Law of Grokking». Нюанс: доказаны совпадающие верхняя и нижняя границы на задержку, растущую логарифмически с отношением норм. [`"no existing result establishes tight bounds on the delay or proves that it scales logarithmically with the norm ratio"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p1-2). *«[ни один существующий результат не устанавливает точных границ задержки и не доказывает, что она растёт логарифмически с отношением норм](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p1-2)»*\
Доп.: [`"upper bound follows from a discrete Lyapunov contraction argument; the matching lower bound follows from dynamical constraints of regularised first-order optimisation"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p1-2) — *«[Верхняя граница следует из дискретного рассуждения о сжатии по Ляпунову; отвечающая ей нижняя граница следует из динамических ограничений регуляризованной оптимизации первого порядка](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p1-2)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2201.02177 — Power et al., «Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets». [`"Some papers study the sample complexity on algorithmic tasks Reed & De Freitas (2015), but mostly focus on the impact of architectural choices"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p7-6). *«[Некоторые статьи изучают выборочную сложность на алгоритмических задачах Reed & De Freitas (2015), но в основном сосредоточены на влиянии архитектурного выбора](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p7-6)»*

**\[4.2\]** 2410.03569 — Saxena et al. 2024, «Making Hard Problems Easier with Custom Data Distributions and Loss Regularization: A Case Study in Modular Arithmetic». [`"our sampling technique allows for a linear sample complexity, while $f_{\mathrm{default}}$ needs an exponential sample complexity to tackle the problem"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p16-3). *«[наша выборка даёт линейную сложность по числу примеров, тогда как $f_{\mathrm{default}}$ требует показательного их числа](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.card.md#p16-3)»*

**\[4.3\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». [`"provides a generalization bound that establishes fast convergence of the expected risk"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p3-11). *«[дают границу генерализации, устанавливающую быстрое схождение ожидаемого риска](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p3-11)»*

**\[4.4\]** 2603.01968 — Hwang et al., «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"There are also theoretical studies that link the learning of invariance to generalization bounds (Xu et al., 2020; Benton et al., 2020)"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p2-5). *«[Есть и теоретические работы, связывающие выучивание неизменности с оценками обобщения (Xu et al., 2020; Benton et al., 2020)](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p2-5)»*

**\[4.5\]** 2412.18624 — Kozyrev, «How to explain grokking». Упоминание в одну фразу: подход плоских минимумов подпирается ссылками на алгоритмическую устойчивость (Bousquet & Elisseeff, Kutin & Niyogi, Poggio et al.), после чего устойчивость не используется — ни одной оценки в работе не выписано. [`"This known observation is related to algorithmic stability, i.e. stability of the solution of a learning problem to perturbations of the training sample [5], [6], [7]."`](../papers/2412.18624.how-to-explain-grokking/2412.18624.how-to-explain-grokking.card.md#p3-11). *«[Это известное наблюдение связано с алгоритмической устойчивостью, то есть устойчивостью решения задачи обучения к возмущениям обучающей выборки [5], [6], [7]](../papers/2412.18624.how-to-explain-grokking/2412.18624.how-to-explain-grokking.card.md#p3-11)»*.


### Внешние работы

###### ref-5-1
**\[5.1\]** 2502.20763 — Внешняя работа (демотирована из корпуса): Tan & Huang 2025, «Information-Theoretic Perspectives on Optimizers». Нюанс: в оценку входят норма решения, след гессиана и разрыв энтропии — отсюда и берётся предлагаемая мера. [`"We can now present a PAC-Bayes generalization bound which shows what metrics of the network mainly affect generalization"`](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p5-8). *«[Теперь мы можем привести оценку генерализации PAC-Байеса, показывающую, какие меры сети главным образом сказываются на генерализации](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p5-8)»*
