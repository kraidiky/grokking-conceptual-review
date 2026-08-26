# Максимизация зазора (margin maximization / implicit bias)

[Эффективная теория / статистическая механика](effective-theory-statistical-mechanics.md) ← предыдущая карточка, следующая → [Границы генерализации](generalization-bounds.md)

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Максимизация зазора** (margin maximization) — принцип, согласно которому
градиентный спуск на разделимых данных с функцией потерь типа кросс-энтропии /
логистической среди всех интерполирующих (доводящих обучающую ошибку до нуля)
решений неявно выбирает решение с максимальным *зазором* (margin — минимальный
по выборке отступ верного логита от ближайшего неверного, нормированный на норму
весов). Эта неявная предпочтительность [оптимизатора](optimizer-adam-adamw-sgd.md) называется **имплицитной
предвзятостью** (implicit bias). Применительно к [гроккингу](grokking.md) отсюда
следует объяснение отложенной генерализации: сеть быстро достигает нулевой
обучающей ошибки, но зазор растёт крайне медленно (для логистической регрессии —
со скоростью O(1/log t)), поэтому генерализация проявляется лишь по мере того,
как направление весов доходит до max-margin-решения \[[1.1](#ref-1-1)\]. Классический
результат о сходимости градиентного спуска к максимальному зазору (Soudry et al.,
2018) переносится на гомогенные (положительно-однородные по параметрам) сети и
применяется к гроккингу целым рядом теоретических работ \[[1.3](#ref-1-3)\].

![Зазоры отдельных примеров при гроккинге: после перехода к генерализации распределение зазоров сдвигается вверх (рис. 5 Sakamoto, Sato)](assets/margins-during-grokking.png)

## Детализация

Механизм опирается на разделение обучения на две стадии — так называемую
дихотомию ранней и поздней имплицитных предвзятостей \[[1.3](#ref-1-3)\]. На ранней
стадии сеть ведёт себя как ядерный (kernel) предиктор — «ленивый» режим, в котором
признаки почти не меняются, а модель линейна по своим начальным касательным
признакам; здесь происходит запоминание. На поздней, «богатой» (rich) стадии
включается имплицитная предвзятость к максимизации зазора, которая и вытягивает
сеть из [фазы запоминания](memorization-phase.md) к обобщающему решению
\[[1.3](#ref-1-3)\]. Поскольку в гомогенных сетях зазор можно произвольно
масштабировать, увеличивая норму весов, содержательной величиной служит
*нормированный зазор* (зазор, делённый на норму параметров); именно к максимуму
нормированного зазора сходится направление весов, тогда как сама норма растёт без
предела. Разные работы уточняют, по какой норме берётся зазор: по ℓ2 в задаче
регрессии и по ℓ∞ в задаче классификации \[[1.3](#ref-1-3)\]. Для конкретных
алгоритмических задач max-margin-решение выписывается явно: в [модульном сложении](modular-arithmetic.md)
сеть достигает максимального зазора, когда её нейроны представляют достаточный
набор частот Фурье (спектральных компонент, кодирующих операцию по модулю), и
именно это решение оказывается обобщающим \[[1.2](#ref-1-2)\]\[[1.5](#ref-1-5)\].
Максимизация зазора тесно связана с трактовкой гроккинга через конкуренцию
контуров (подсетей, реализующих под-алгоритмы): «[эффективность контура](circuit-efficiency.md)» —
способность выдавать бо́льшие логиты при той же норме весов — есть по сути
бо́льший нормированный зазор, поэтому обобщающий контур описывают как решение с
большим зазором \[[3.1](#ref-3-1)\]. В минимальной постановке (бинарная
логистическая регрессия) гроккинг возникает у «края линейной разделимости»: когда
данные едва отделимы от начала координат, имплицитная предвзятость к решению
жёсткого (hard-margin) SVM всё же уводит модель к max-margin-решению, но по «плоским»
направлениям [ландшафта потерь](loss-landscape-basins.md) с почти нулевым градиентом — отсюда длительная
задержка \[[1.4](#ref-1-4)\]. Вопрос о том, что именно приводит в движение эту
предвзятость, остаётся спорным: часть теорий связывает дрейф к максимальному
зазору с [weight decay](weight-decay.md) (штрафом на норму весов), но это оспаривается — такие
«дрейфовые» рамки не объясняют, почему при том же weight decay изотропный шум или
обычный SGD не грокают \[[2.1](#ref-2-1)\]. Отдельная линия работ поддерживает, что
именно имплицитная максимизация зазора под кросс-энтропией лепит структурированную
геометрию представлений, а задержка гроккинга — это время, нужное этой предвзятости
на «скульптурирование» решения \[[3.2](#ref-3-2)\].

## Альтернативные определения и нюансы

### A. Классическое: имплицитная сходимость к max-margin на разделимых данных

Базовая формулировка через теорию имплицитной предвзятости: на линейно разделимых
данных градиентный спуск с логистической/кросс-энтропийной потерей сходится по
направлению к решению с максимальным зазором, причём зазор улучшается медленно
(O(1/log t)), тогда как норма классификационного слоя растёт быстро (O(t))
\[[1.1](#ref-1-1)\]. Ключевое для гроккинга: две скорости разнесены на порядки,
поэтому уверенная генерализация «догоняет» нулевую обучающую ошибку с большим
запаздыванием. Отличительная машинерия — направленная (по направлению весов, а не
по их норме) сходимость к max-margin-классификатору \[[1.1](#ref-1-1)\].

### B. Максимизация зазора как формирователь признаков (feature emergence)

Здесь зазор — не только свойство итогового классификатора, но и причина того,
*какие признаки* появляются. Для одно-скрытослойных сетей на модульной арифметике
доказывается: при достаточной ширине сеть достигает максимального L2,k+1-зазора, и
в этом решении каждый скрытый нейрон выравнивается с конкретной компонентой спектра
Фурье \[[1.2](#ref-1-2)\]. Отличие от версии A: max-margin фиксирует не просто
границу решения, а конкретную структуру внутренних представлений (набор частот),
что делает предвзятость к зазору теорией *[возникновения признаков](feature-emergence-feature-learning.md)*, а не только
теорией разделяющей гиперплоскости.

### C. Ранняя vs поздняя предвзятость (kernel → rich переход)

Определение через смену режима: гроккинг — переход от ранней имплицитной
предвзятости (ядерный/ленивый режим, запоминание) к поздней (максимизация зазора в
«богатом» режиме), и именно поздняя предвзятость к нормированному ℓ∞-зазору даёт
доказуемую генерализацию с контролируемой выборочной сложностью
\[[1.3](#ref-1-3)\]. Отличительный параметр — не факт максимизации зазора сам по
себе, а *момент* его включения: разрыв между стадиями и есть источник задержки.

### D. Край линейной разделимости

Определение через управляющий параметр близости к разделимости: в бинарной
логистической регрессии гроккинг возникает, когда данные почти линейно отделимы от
начала координат при сильном шуме в перпендикулярных направлениях; имплицитная
предвзятость к жёсткому SVM тянет к max-margin, но «плоские» направления ландшафта
задерживают приход к глобальному минимуму \[[1.4](#ref-1-4)\]. Ключевое отличие —
явный контрольный параметр (степень разделимости), у критического значения которого
время гроккинга расходится.

### Оспаривают

Дрейф к максимальному зазору, вызванный weight decay, оспаривается как достаточное
объяснение: «геометрические» теории предполагают общий градиентный поток и не
объясняют, почему изотропный шум (SGLD) или обычный SGD не грокают даже при том же
weight decay \[[2.1](#ref-2-1)\]. То есть max-margin-дрейф отделяется от
специфической ковариационной структуры адаптивных оптимизаторов, которая, по этой
критике, и является настоящим спусковым механизмом.

### Поддерживают

Присоединение через эффективность контуров: обобщающее решение определяется как
более «эффективное» — выдающее бо́льшие логиты при той же норме весов, то есть
имеющее больший нормированный зазор \[[3.1](#ref-3-1)\]. Присоединение через
геометрию представлений: возникновение структурированных контуров прямо связывается
с имплицитной максимизацией зазора под кросс-энтропией \[[3.2](#ref-3-2)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2206.04817 — Thilak et al., «The Slingshot Mechanism: An Empirical Study of Adaptive Optimizers and the Grokking Phenomenon». [`"converges to the maximum margin solution"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p2-1). *«[сходится к решению с максимальным зазором](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p2-1)»*\
Доп.: [`"the implicit bias of the optimizer coupled with the CE loss, pushes the direction of the classiﬁcation layer to coincide with that of the maximum margin classiﬁer"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p4-2) — *«[неявное смещение оптимизатора вместе с CE-потерей толкает направление классифицирующего слоя к совпадению с направлением классификатора с максимальным зазором](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p4-2)»*.

###### ref-1-2
**\[1.2\]** 2402.09469 — Li, Liang, Shi, Song, Zhou 2024, «Fourier Circuits in Neural Networks and Transformers: A Case Study of Modular Arithmetic with Multiple Inputs». Нюанс: доказательство идёт о решении наибольшего отступа, а не о ходе обучения. [`"the elucidation of how the principle of margin maximization shapes the features adopted by one-hidden layer neural networks"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#p1-2). *«[выяснение того, как правило наибольшего отступа задаёт признаки, принимаемые сетями с одним скрытым слоем](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.card.md#p1-2)»*\
Доп.: [`"has highlighted the implicit bias towards margin maximization inherent in neural network optimization"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#p5-2) — *«[показали неявное предпочтение наращивания отступа, свойственное оптимизации нейронных сетей](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.card.md#p5-2)»*.

###### ref-1-3
**\[1.3\]** 2407.12332 — Mohamadi et al., «A Theoretical Analysis of Grokking Modular Addition». [`"focus on the weights learned through margin-maximization implicit bias of gradient descent"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p10-2). *«[сосредоточиваемся на весах, выученных благодаря неявному смещению градиентного спуска к максимизации зазора](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p10-2)»*\
Доп.: [`"a dichotomy of early and late implicit biases of the training algorithm"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-5) — *«[дихотомию раннего и позднего неявных смещений алгоритма обучения](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p1-5)»*.

###### ref-1-4
**\[1.4\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». [`"implicit bias of gradient descent, we show that logistic regression can exhibit grokking when the training dataset is nearly linearly separable"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2). *«[неявному смещению градиентного спуска, мы показываем, что логистическая регрессия способна обнаруживать гроккинг, когда обучающий набор почти линейно отделим](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)»*\
Доп.: [`"determined by the max-margin solution"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p25-8) — *«[определяемой решением с наибольшим отступом](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p25-8)»*.

###### ref-1-5
**\[1.5\]** 2505.18266 — McCracken et al. 2025, «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"have learned the maximum margin solution"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-2). *«[выучили решение с наибольшим отступом](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-2)»*\
Доп.: [`"solution maximizes the margin [7], while independently and simultaneously, work connected margin maximization to grokking in similar networks [12]"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-5) — *«[это решение наибольшит отступ [7], а независимо и одновременно другая работа связала наибольшение отступа с гроккингом в схожих сетях [12]](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-5)»*.


###### ref-1-6
**\[1.6\]** 2311.18817 — Lyu et al., «Dichotomy of Early and Late Phase Implicit Biases Can Provably Induce Grokking». Нюанс: первоисточник дихотомии ранней и поздней неявных смещений; позднее смещение задано задачей максимального зазора (R1) в классификации и минимальной нормы (R2) в регрессии, а срок смены — $\frac{1}{\lambda}\log\alpha$. [`"our theory suggests a sharp transition at time $\frac{1}{\lambda}\log\alpha$ from max $L^{2}$-margin linear classifier to max $L^{1}$-margin linear classifier"`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p7-5). *«[наша теория предполагает резкий переход в момент $\frac{1}{\lambda}\log\alpha$ от линейного классификатора максимального $L^{2}$-зазора к линейному классификатору максимального $L^{1}$-зазора](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p7-5)»*\
Доп. (граница результата, теряемая при цитировании): [`"the KKT condition is a first-order necessary condition for global optimality, but it is not sufficient in general since $f_{i}({\boldsymbol{\theta}})$ can be highly non-convex"`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p6-8) — *«[условие KKT есть необходимое условие первого порядка для глобальной оптимальности, но в общем случае оно не достаточно, поскольку $f_{i}({\boldsymbol{\theta}})$ может быть сильно невыпуклой](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p6-8)»*.

###### ref-1-7
**\[1.7\]** 2311.07568 — Morwani, Edelman, Oncescu, Zhao, Kakade 2024, «Feature emergence via margin maximization: case studies in algebraic tasks». Первая работа, где максимизация зазора употреблена не для оценки генерализации, а как условие, **полностью задающее** выучиваемые признаки; отсюда корпус берёт приём «сертификатной пары» $(\theta^{*},q^{*})$ и взвешенный по классам зазор, возвращающий разложимость по нейронам в многоклассовой задаче. Нюанс: доказано свойство множества решений максимального зазора, а не результата обучения, — связь с обучением идёт через теорему Wei et al. 2019a о **глобальных** минимумах слабо регуляризованной кросс-энтропии при $\lambda\to 0$, и сами авторы называют максимальный зазор «прокси». [`"the margin maximization property alone — typically used for the study of generalization — is sufficient to *comprehensively and precisely* characterize the richly structured features that are actually learned by neural networks in these settings"`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p2-1). *«[одного лишь свойства максимизации зазора — обычно употребляемого для изучения генерализации — достаточно, чтобы *исчерпывающе и точно* описать богато устроенные признаки, которые в этих условиях действительно выучиваются нейросетями](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p2-1)»*\
Доп. (граница результата): [`"This provides the motivation behind studying maximum margin classifiers as a proxy for understanding the global minimizers of ${\mathcal{L}}_{\lambda}$ as $\lambda\to 0$."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p5-11) — *«[Это даёт основание изучать классификаторы максимального зазора как прокси для понимания глобальных минимизаторов ${\mathcal{L}}_{\lambda}$ при $\lambda\to 0$.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p5-11)»*;
Доп. (сила приёма): [`"By just identifying this single solution, we can characterize the set of *all* maximum margin solutions."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p7-3) — *«[Выявив всего лишь это одно решение, мы можем описать множество *всех* решений максимального зазора.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p7-3)»*.

###### ref-1-8
**\[1.8\]** 2406.06158 — Kunin et al., «Get rich quick: exact solutions reveal how unbalanced initializations promote rapid feature learning». Расширяет теорему 2 Azulay et al. 2021 на область $\delta<0$: интерполирующее решение минимизирует размен между минимальной нормой и сохранением направления начала, и при исчезающей инициализации upstream и balanced функционально неразличимы — различие даёт только downstream. Нюанс: из зеркального потока на сингулярных числах утверждение об индуктивном смещении при интерполяции авторы прямо не выводят, поскольку сингулярные векторы меняются в ходе обучения. [`"the dynamics of the singular values of $\beta$ can be described as a mirror flow with a *hyperbolic entropy* potential, which smoothly interpolates between an $\ell^{1}$ and $\ell^{2}$ penalty on the singular values for the rich ($\delta\to 0$) and lazy ($\delta\to\pm\infty$) regimes respectively"`](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#p8-1). *«[динамика сингулярных чисел $\beta$ может быть описана как зеркальный поток с потенциалом *гиперболической энтропии*, который плавно интерполирует между штрафами $\ell^{1}$ и $\ell^{2}$ на сингулярные числа для богатого ($\delta\to 0$) и ленивого ($\delta\to\pm\infty$) режимов соответственно](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#p8-1)»*\
Доп. (глубина): [`"This inductive bias strikes a depth-dependent balance between attaining the minimum norm solution and preserving the initialization direction."`](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#p8-2) — *«[Это индуктивное смещение устанавливает зависящий от глубины баланс между достижением решения минимальной нормы и сохранением направления инициализации](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#p8-2)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2603.15492 — Acharya et al., «Grokking as a Variance-Limited Phase Transition». Оспаривает: дрейф к max-margin под действием weight decay недостаточен как объяснение. [`"these frameworks assume a generic gradient flow. They often fail to explain why isotropic noise (SGLD) or standard SGD does not grok, even when weight decay is present"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-7). *«[эти рамки предполагают общий градиентный поток. Они часто не объясняют, отчего изотропный шум (SGLD) или обычный SGD не гроккают даже при наличии weight decay](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p1-7)»*\
Доп.: [`"weight decay slowly pushes the model toward max-margin"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-7) — *«[weight decay медленно подталкивает модель к решениям с наибольшим отступом](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p1-7)»* (оспариваемый тезис «геометрического дрейфа»).\
Доп. (направление важнее величины): [`"isotropic noise helps the model move, but geometric rectification ensures it moves in the right direction"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p10-3) — *«[изотропный шум помогает модели двигаться, а геометрическое выпрямление обеспечивает движение в верную сторону](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p10-3)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2309.02390 — Varma et al., «Explaining Grokking Through Circuit Efficiency». Нюанс: эффективность обобщающего контура = больший зазор при той же норме. [`"producing larger logits with the same parameter norm"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3). *«[производит бо́льшие логиты при той же норме параметров](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3)»*

###### ref-3-2
**\[3.2\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking». Нюанс: возникновение структурированных контуров движимо имплицитной максимизацией зазора. [`"the emergence of structured circuits is driven by implicit margin maximization under cross-entropy loss"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p3-3). *«[возникновение устроенных контуров движимо неявной максимизацией отступа при перекрёстно-энтропийной потере](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p3-3)»*


###### ref-3-3
**\[3.3\]** 2505.20172 — Boursier et al., «A Theoretical Framework for Grokking: Interpolation followed by Riemannian Norm Minimisation». Поддерживает и делит роли: неявное предпочтение отвечает за первую пору, вторая же пора есть явное предпочтение, наводимое регуляризатором, и в примерах оно выписано — $\ell_{1}$-норма для диагональных линейных сетей, ядерная норма (то есть малый ранг) для матричного зондирования. [`"While prior work has extensively analysed the first phase via the implicit bias of optimisation algorithms, the second phase—norm minimisation constrained to the interpolation manifold—has received little attention."`](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p10-3). *«[Тогда как предшествующие работы подробно разобрали первую пору через неявное предпочтение алгоритмов оптимизации, вторая пора — минимизация нормы с ограничением на многообразие интерполяции — получила мало внимания](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p10-3)»*\
Доп. (что именно предпочитает вторая пора): [`"we conclude that in the second, slow phase of the dynamics, the Riemannian gradient flow which minimizes $\|u\|^{2}+\|v\|^{2}$ on $\mathcal{M}^{*}$ tends to drift towards solutions of low $\ell_{1}$ norm"`](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p27-4) — *«[мы заключаем, что на второй, медленной поре динамики риманов градиентный поток, минимизирующий $\|u\|^{2}+\|v\|^{2}$ на $\mathcal{M}^{*}$, склонен сноситься к решениям малой $\ell_{1}$-нормы](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p27-4)»*.

###### ref-3-4
**\[3.4\]** 2603.07323 — Truong, Truong, «Norm-Hierarchy Transitions in Representation Learning…». Нюанс: медленность неявного смещения объявлена измеримой через разрыв норм. [`"implicit bias is slow, and the slowness is quantifiable via $\Delta V$"`](../papers/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts.card.md#p16-4). *«[неявное смещение медленно, и эта медленность измерима через $\Delta V$](../papers/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts.card.md#p16-4)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». [`"attributed this increased robustness to a bias of SGD towards a max-margin solution"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p10-1). *«[приписали этот рост устойчивости смещению SGD к решению с максимальным зазором](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p10-1)»*

**\[4.2\]** 2506.05718 — Notsawo et al., «Grokking Beyond the Euclidean Norm of Model Parameters». [`"This behavior arises from the principle of margin maximization, even without explicit regularization, providing a theoretical explanation for grokking"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p17-2). *«[Это поведение проистекает из начала наибольшения отступа даже без явного сглаживания и даёт теоретическое объяснение гроккингу](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.card.md#p17-2)»*

**\[4.3\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». [`"correspond to the large margin of the training data (their separation is 2s)"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#fig-5). *«[отвечают широкому зазору обучающих данных (их разделение равно $2s$)](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#fig-5)»*

**\[4.4\]** 2604.00316 — Tomàs et al., «Breaking Data Symmetry is Needed For Generalization in Feature Learning Kernels». [`"These works attempt to explain grokking via margin maximization (Morwani et al., 2024)"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p8-3). *«[Эти работы пытаются объяснить гроккинг через наибольший отступ (morwani2024featureemergencemarginmaximization)](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p8-3)»*

**\[4.5\]** 2312.06581 — Stander et al. 2024, «Grokking Group Multiplication with Cosets». Догадка о причине сосредоточенности coset-контура ровно на одном неприводимом представлении; опыта в эту сторону нет. [`"We hypothesize that it may have something to do with the margin maximization effect discussed in [Morwani et al. 2024]."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p23-5). *«[Мы выдвигаем гипотезу, что это может быть как-то связано с действием максимизации зазора, обсуждаемым в [Morwani et al. 2024]](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p23-5)»*

**\[4.6\]** 2310.02541 — Xu et al. 2023. Нюанс: генерализация отнесена к неявной регуляризации GD, а нижняя граница выводится через нормированный зазор ${\mathbb{E}}_{x\sim N(\nu,I_{p})}[f(x;W)]/\|W\|_{F}$; но предельного направления при $t\to\infty$ работа не рассматривает, так что о сходимости к максимизирующему зазор решению по ней судить нельзя. [`"showed how stationary points of margin-maximization problems associated with homogeneous neural network training problems can exhibit benign overfitting"`](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/original/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.md#p3-1). *«[показали, как стационарные точки задач максимизации зазора, связанных с задачами обучения однородных нейросетей, могут проявлять доброкачественное переобучение](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.card.md#p3-1)»*

**\[4.7\]** 2407.20199 — Mallinar et al., «Emergence in non-neural models: grokking modular arithmetic via average gradient outer product». Нюанс: линия максимизации отступа помянута как многообещающая, но не разобрана; собственного разбора отступов работа не ведёт. [`"While the direction is promising, general underlying principles have not yet been elucidated."`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p15-2). *«[Хотя направление многообещающее, общие лежащие в основе начала пока не выяснены](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p15-2)»*

**\[4.8\]** 2606.13753 — Truong, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Нюанс: дихотомия неявных смещений поздней поры помянута одной фразой обзора рядом с lazy-to-rich; собственных измерений отступа в работе нет. [`"Related accounts cast the jump as a lazy-to-rich transition [7], or as a late-phase implicit-bias dichotomy [8], connected to the classical max-margin bias of gradient descent [9]."`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p2-3). *«[Смежные объяснения подают скачок как переход от ленивого к богатому [7] или как дихотомию неявных смещений поздней поры [8], связанную с классическим смещением градиентного спуска к наибольшему отступу [9].](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p2-3)»*

```
concept:
  category: 7                    # 7. Теория и формальные результаты (Theory & formal results)
  papers_linked: 21             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-20
```
