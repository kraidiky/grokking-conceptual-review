# Ландшафт потерь / бассейны (loss landscape / basins)

[Эффективность контуров](circuit-efficiency.md) ← предыдущая карточка, следующая → [Сжатие многообразия представлений](manifold-representation-compression.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Ландшафт потерь** (loss landscape) — геометрия функции потерь как поверхности
над пространством весов сети; её низины называют **бассейнами** (basins, или
бассейнами притяжения — область, из которой градиентный спуск сходится к одному
минимуму). В контексте [гроккинга](grokking.md) этот язык описывает отложенную
генерализацию как перемещение оптимизационной траектории между бассейнами: сеть
надолго застревает в «запоминающем» решении и лишь много позже переходит в
«обобщающий» бассейн. Впервые ландшафт потерь был введён как объясняющая рамка
гроккинга в Omnigrok (Liu et al., 2022): гроккинг трактуется через рассогласование
обучающего и тестового ландшафтов потерь \[[1.1](#ref-1-1)\].

![Редуцированный ландшафт потерь трансформера на модульном сложении — исходной постановке гроккинга (рис. 9 Liu et al.)](assets/loss-landscape-reduced.jpg)

## Детализация

Исходная формализация — «LU-механизм» Omnigrok: если строить потери против нормы
весов, обучающая потеря имеет форму «L» (быстро падает и остаётся низкой), а
тестовая — форму «U» (минимум достигается лишь при определённой норме); гроккинг
возникает из-за этого рассогласования между обучающим и тестовым ландшафтами
\[[1.1](#ref-1-1)\]. Второй распространённый образ — **бассейн притяжения**: сеть
застревает в локальном (запоминающем) решении в течение [фазы запоминания](memorization-phase.md)
и грокает, когда «вырывается» из этого бассейна \[[1.2](#ref-1-2)\]; тот же язык
позволяет предсказывать гроккинг по ранней динамике ландшафта \[[1.2](#ref-1-2)\]\[[3.3](#ref-3-3)\].
Более формально бассейны различают через **сингулярную теорию обучения** (Singular
Learning Theory — байесовский аппарат, описывающий геометрию вырожденных минимумов):
локальный коэффициент обучения (LLC), измеряющий степень вырожденности минимума,
отделяет обобщающий бассейн от запоминающего \[[1.3](#ref-1-3)\]. Практически
все объяснения сводятся к вопросу, что удерживает траекторию на плато: «плоские»
направления с почти нулевым градиентом \[[3.1](#ref-3-1)\], асимметрия скоростей
спуска вдоль главных направлений \[[3.5](#ref-3-5)\] или энергетические барьеры
между метастабильными состояниями, преодолеваемые шумом [SGD](optimizer-adam-adamw-sgd.md) по закону Аррениуса
(вероятность выхода экспоненциально зависит от высоты барьера) \[[3.10](#ref-3-10)\].
Отдельная дискуссия — **плоскостность против остроты** минимума (flat vs sharp
minima — «широкий»/пологий против «узкого»/крутого минимума): ряд работ связывает
генерализацию именно с плоскостностью ландшафта \[[3.4](#ref-3-4)\]\[[3.8](#ref-3-8)\],
тогда как другие оспаривают это для алгоритмических задач, помещая обобщающее
решение в **острый** бассейн \[[2.1](#ref-2-1)\]. Ландшафтный язык смыкается с
трактовкой гроккинга как [фазового перехода](phase-transition.md) (переход между
бассейнами как скачок порядкового параметра) и с описанием через кривизну —
измеряемую, например, коммутаторными дефектами ([некоммутативностью](group-composition-non-commutative-s5.md)
последовательных шагов градиента) \[[3.9](#ref-3-9)\].

Сдерживающий факт для шумовых механизмов преодоления барьеров: основополагающие эмпирические работы обучают полным батчем — см. цепочку в карточке [роли шума градиента](gradient-noise.md).

## Альтернативные определения и нюансы

### A. Рассогласование обучающего и тестового ландшафтов (LU-механизм)

Определение через геометрию потерь по норме весов: обучающий и тестовый ландшафты
не совпадают (формы «L» и «U»), поэтому решение с малой обучающей потерей ещё не
попадает в минимум тестовой потери; гроккинг — это дрейф весов к общей области
низкой потери обеих поверхностей \[[1.1](#ref-1-1)\]. Ключевой источник различия
здесь — норма весов как управляющий параметр, а не время само по себе.

### B. Бассейн притяжения и выход из локального решения

Определение через застревание: сеть попадает в бассейн притяжения запоминающего
решения, а лёгкость выхода зависит от инициализации и гиперпараметров (объём
данных); гроккинг наступает в момент, когда траектория вырывается из этого
бассейна \[[1.2](#ref-1-2)\]. Отличительная машинерия — ранние осцилляции кривой
обучения как индикатор предстоящего выхода, что делает гроккинг предсказуемым до
его наступления \[[1.2](#ref-1-2)\].

### C. Отбор конкурирующих бассейнов (сингулярная теория обучения)

Определение через сравнение бассейнов: запоминающее и обобщающее решения — это
конкурирующие бассейны с разными статистическими свойствами, а не одно решение в
разные моменты \[[1.3](#ref-1-3)\]. Различающий их порядковый параметр — локальный
коэффициент обучения (LLC): более низкий LLC соответствует большей концентрации
байесовской апостериорной массы и меньшей ожидаемой ошибке генерализации, поэтому
отбор «нужного» бассейна и есть гроккинг \[[1.3](#ref-1-3)\].

### Оспаривают

Оспаривается нюанс «плоских минимумов»: для алгоритмических задач обобщающее
решение располагается не в плоском, а в **остром** бассейне, недоступном при малой
дисперсии градиента; «задержка» — это накопление дисперсии, открывающее вход в
острое многообразие \[[2.1](#ref-2-1)\]. Тем самым напрямую оспаривается связка
«генерализация = плоскостность ландшафта».

### Поддерживают

К ландшафтно-бассейновой рамке присоединяются работы, уточняющие конкретный
механизм застревания и выхода: плоские направления с почти нулевым градиентом
\[[3.1](#ref-3-1)\]; медленное вхождение в «[зону Златовласки](goldilocks-zone.md)» по норме весов
\[[3.2](#ref-3-2)\]; предсказание числа эпох до гроккинга из раннего ландшафта
\[[3.3](#ref-3-3)\]; плоскостность как единственный устойчивый предиктор перехода
\[[3.4](#ref-3-4)\]; плато от асимметрии скоростей спуска \[[3.5](#ref-3-5)\];
связь ландшафта с объёмом данных и [weight decay](weight-decay.md) \[[3.6](#ref-3-6)\]; переход из
бассейна запоминания в бассейн генерализации в пространстве весов
\[[3.7](#ref-3-7)\]; плоскостность ландшафта как признак пост-гроккинговых моделей
\[[3.8](#ref-3-8)\]; кривизна ландшафта через коммутаторные дефекты
\[[3.9](#ref-3-9)\]; и метастабильные состояния, разделённые энергетическими
барьерами \[[3.10](#ref-3-10)\]. Обзорно к «loss-based» теориям ландшафта
апеллирует и \[[3.11](#ref-3-11)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2210.01117 — Liu et al., «Omnigrok: Grokking Beyond Algorithmic Data». [`"understand grokking by analyzing the loss landscapes of neural networks, identifying the mismatch between training and test loss landscapes as the cause for grokking"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p1-3). *«[понять гроккинг, анализируя ландшафты потерь нейросетей, и указываем на рассогласование между ландшафтами обучающей и тестовой потерь как на причину гроккинга](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/2210.01117.omnigrok-grokking-beyond-algorithmic-data.card.md#p1-3)»*

###### ref-1-2
**\[1.2\]** 2306.13253 — Notsawo et al., «Predicting Grokking Long Before it Happens: A look into the loss landscape of models which grok». [`"the model achieves grokking when it successfully breaks free from the basin of attraction of such solutions"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p4-1). *«[модель достигает гроккинга, когда ей удаётся вырваться из бассейна притяжения таких решений](../papers/2306.13253.predicting-grokking-long-before-it-happens/2306.13253.predicting-grokking-long-before-it-happens.card.md#p4-1)»*\
Доп.: [`"the model gets stuck in local solutions during the memorization phase"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p4-1) — *«[в фазе запоминания модель застревает в локальных решениях](../papers/2306.13253.predicting-grokking-long-before-it-happens/2306.13253.predicting-grokking-long-before-it-happens.card.md#p4-1)»*.

###### ref-1-3
**\[1.3\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». [`"the presence of competing solution basins with distinct statistical properties"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2). *«[присутствие соперничающих котловин решений с несхожими статистическими свойствами](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)»*\
Доп.: [`"the local learning coefficient (LLC) which quantifies the local degeneracy of the loss surface"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2) — *«[местный коэффициент обучения (LLC), измеряющий местное вырождение поверхности потерь](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)»*.\
Доп. (переход между котловинами): [`"grokking corresponds to a transition from a higher-LLC (memorising) basin to a lower-LLC (generalising) basin that dominates the posterior"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2) — *«[гроккинг отвечает переходу из котловины с большим LLC (запоминающей) в котловину с меньшим LLC (обобщающую), главенствующую в апостериоре](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)»*

###### ref-1-4
**\[1.4\]** 2505.20172 — Boursier et al., «A Theoretical Framework for Grokking: Interpolation followed by Riemannian Norm Minimisation». Нюанс: критическое множество предполагается морс-боттовым многообразием из одних локальных минимумов (равносильно локальному условию Поляка — Лоясевича); из трёх разобранных примеров это верно глобально только в линейной регрессии, а для диагональных сетей и матричного зондирования спасается оговоркой «локально, вне вырожденного множества». Особые точки — то есть ровно искомые разрежённые векторы и матрицы малого ранга — остаются непокрытыми, и авторы выделяют это жирным. [`"our results capture the grokking dynamics near nonsingular points in $\mathcal{M}^{*}$, but do not yet account for potential convergence toward singular points, which represents an important open challenge."`](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p28-3). *«[наши результаты улавливают динамику гроккинга вблизи невырожденных точек в $\mathcal{M}^{*}$, но пока не учитывают возможной сходимости к особым точкам, что представляет собой важный открытый вызов](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p28-3)»*\
Доп. (отказ от сёдел обоснован ссылками, а не доказан здесь): [`"our assumption rules out the possibility that $w^{\mathrm{GF}}$ converges to a saddle point of $F$. This is justified by a large number of works showing that gradient methods avoid saddle points for *almost all* initialisations."`](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p5-1) — *«[наше предположение исключает возможность того, что $w^{\mathrm{GF}}$ сходится к седлу $F$. Это оправдано большим числом работ, показывающих, что градиентные методы избегают сёдел при *почти всех* инициализациях](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p5-1)»*.

###### ref-1-5
**\[1.5\]** 2408.11804 — Yunis et al., «Approaching Deep Learning through the Spectral Dynamics of Weights». Связывает выпуклость бассейна с спектром весов: наличие линейной связности мод (барьер по Neyshabur et al. 2020) сильно соотносится с совпадением верхних сингулярных векторов у концов ветвей, а ветви от разных стволов при том же сроке ветвления и порядке данных не дают ни LMC, ни совпадения. Объяснения выпуклости работа не даёт и прямо это оговаривает. [`"these top directions are not a unique property of the architecture and data, but rather are dependent on initialization"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p13-1). *«[эти верхние направления не есть единственное свойство архитектуры и данных, а зависят от инициализации](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p13-1)»*\
Доп. (вмешательство): [`"with increasing perturbations the LMC property disappears simultaneously with the agreement in top singular vectors"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#fig-13) — *«[с ростом возмущений свойство LMC исчезает одновременно с совпадением верхних сингулярных векторов](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#fig-13)»*.

###### ref-1-6
**\[1.6\]** 2505.11411 — Zhang, Shang, Yang, Zhang, «Is Grokking a Computational Glass Relaxation?». Предлагает заменить локальную геометрию минимума (плоскость, резкость, гессиан) глобальным объёмом решений и читает гроккинг как неполадку исследования: оптимизатор неэффективно обходит бассейны, а не преодолевает преграду. Нюанс: величина, которой оперирует работа, есть двумерная проекция объёма — ни ширины минимума, ни преград между бассейнами она не меряет. [`"we believe that grokking is a dynamic process caused by the optimizer’s inefficient exploration of the loss landscape basins"`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p7-3). *«[мы полагаем, что гроккинг есть динамический процесс, вызванный неэффективным исследованием бассейнов ландшафта потерь со стороны оптимизатора](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p7-3)»*\
Доп. (чем энтропия заменяет локальные меры): [`"the combination of flatness and weight norm actually reflects the entropy, the logarithm of the parameter space volume, of the solutions with the same generalizability"`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p3-1) — *«[сочетание плоскости и нормы весов на деле отражает энтропию — логарифм объёма пространства параметров — у решений с одинаковой способностью к генерализации](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p3-1)»*.

###### ref-1-7
**\[1.7\]** 2505.15624 — AlQuabeh, Bojković, Nwadike, Inui, «Mechanistic Insights into Grokking from the Embedding Layer». Меряет кривизну послойно: максимальные собственные числа гессиана считаны отдельно по $\mathbf{E}$ и по $\mathbf{W}$ степенным методом через произведения гессиана на вектор и расходятся на полтора порядка (около 0,13 против около 3,5), а при Adam-LR сближаются. Нюанс: седловые точки объявлены следствием двулинейной связки, но ни одного седла не найдено и не показано; поправки на разное число параметров в $\mathbf{E}$ и $\mathbf{W}$ не сделано, а вывод о разделении ролей опирается на совпадение двух вертикальных отсечек в одном прогоне. [`"This coupling introduces structural complexity into the optimization landscape, making the process more susceptible to saddle points and increasing the sensitivity to initialization."`](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/original/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.md#p2-2). *«[Эта связка вносит в ландшафт оптимизации устройственную сложность, делая процесс более подверженным седловым точкам и повышая чувствительность к начальному приближению.](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.card.md#p2-2)»*\
Доп. (что именно измерено): [`"With Adam (left), the eigenvalues for $\mathbf{E}$ are significantly smaller than those for $\mathbf{W}$, reflecting differences in dimensionality and update frequency."`](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/original/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.md#fig-7) — *«[При Adam (слева) собственные числа для $\mathbf{E}$ существенно меньше, чем для $\mathbf{W}$, что отражает различия в размерности и частоте обновлений.](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.card.md#fig-7)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2603.15492 — Acharya et al., «Grokking as a Variance-Limited Phase Transition: Spectral Gating and the Epsilon-Stability Threshold». Оспаривает гипотезу «плоских минимумов»: обобщающее решение лежит в остром бассейне. [`"the generalizing solution resides in a sharp basin"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-3). *«[обобщающее решение живёт в острой котловине](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p1-3)»*\
Доп.: [`"we challenge the "Flat Minima" hypothesis for algorithmic tasks"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-4) — *«мы оспариваем гипотезу «плоских минимумов» для алгоритмических задач»*.\
Доп. (обращение остроты): [`"for algorithmic tasks, the generalizing solution is spectrally sharper than the memorization solution"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p2-4) — *«[для алгоритмических задач обобщающее решение спектрально острее запоминающего](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p2-4)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». Нюанс: плоские направления ландшафта с почти нулевым градиентом задерживают динамику у квазиустойчивых решений. [`"in the loss landscape with nearly zero gradient cause training dynamics to linger for arbitrarily long times near quasi-stable solutions"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2). *«[ландшафта потерь с почти нулевым градиентом заставляют динамику обучения сколь угодно долго задерживаться вблизи квазиустойчивых решений](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)»*

###### ref-3-2
**\[3.2\]** 2405.19454 — Fan, Pascanu, Jaggi 2024, «Deep Grokking: Would Deep Neural Networks Generalize Better?». Нюанс: гроккинг как медленное вхождение в «зону Златовласки» по норме весов. [`"the model needs to *grok* slowly into the *Goldilocks zone* (Fort & Scherlis, 2018) for generalization"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p6-2). *«[модели нужно медленно *гроккнуть* в *зону Златовласки* (Fort & Scherlis, 2018) ради генерализации](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p6-2)»*

###### ref-3-3
**\[3.3\]** 2411.05353 — Salah et al., «Controlling Grokking with Nonlinearity and Data Symmetry». Нюанс: число эпох до гроккинга предсказуемо из ландшафта потерь на ранних стадиях. `"predicted the number of epochs required to observe grokking from the loss landscape"` (line 640). *«предсказали число эпох, необходимых для наблюдения гроккинга, из ландшафта потерь»*

###### ref-3-4
**\[3.4\]** 2509.17738 — Han et al., «Flatness is Necessary, Neural Collapse is Not: Rethinking Generalization via Grokking». Нюанс: из коррелятов гроккинга только плоскостность ландшафта устойчиво его предсказывает. [`"flatness of the loss landscape has been theoretically and empirically linked to generalization"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2). *«[плоскостность ландшафта потерь связывалась с генерализацией и теоретически, и опытно](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.card.md#p1-2)»*\
Доп.: [`"only flatness consistently predicts it"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2) — *«[предсказывает её последовательно лишь плоскостность](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.card.md#p1-2)»*.

###### ref-3-5
**\[3.5\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». Нюанс: плато перед гроккингом порождается асимметрией скоростей спуска вдоль главных направлений; выравнивание убирает плато. [`"it is desirable to reduce the length of such plateaus"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p1-2). *«[желательно сократить длину таких плато](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p1-2)»*

###### ref-3-6
**\[3.6\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». Нюанс: разбор ландшафта потерь связывает гроккинг с размером данных, weight decay и обучением представлениям. [`"loss-landscape analysis links grokking to data size, weight decay, and representation learning"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p2-4). *«[разбор ландшафта потерь связывает гроккинг с размером данных, weight decay и обучением представлениям](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p2-4)»*

###### ref-3-7
**\[3.7\]** 2511.12768 — Hong et al., «Evidence of Phase Transitions in Small (Language) Models». Нюанс: в пространстве весов модель переходит из бассейна запоминания в бассейн генерализации. [`"the model moves from a memorization basin to a generalization basin in weight space"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p4-4). *«[модель переходит из котловины запоминания в котловину генерализации в пространстве весов](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p4-4)»*

###### ref-3-8
**\[3.8\]** 2512.03437 — Liang & Li, «Grokked Models are Better Unlearners». Нюанс: гроккнувшие модели отличают три свойства разом, и работа нарочно отделяет плоскость от прочих двух. [`"modularity, gradient orthogonality, and loss landscape flatness"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p3-4). *«[составность, ортогональность градиентов и уплощение ландшафта потерь](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p3-4)»*

###### ref-3-9
**\[3.9\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». Нюанс: кривизна ландшафта потерь измеряется через коммутаторные дефекты — некоммутативность последовательных шагов градиента. [`"We then measure loss-landscape curvature via commutator defects"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2). *«[Затем мы измеряем кривизну ландшафта потерь через коммутаторные дефекты](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p1-2)»*

###### ref-3-10
**\[3.10\]** 2606.17120 — Ersoy et al., «Noise-Driven Escape from Metastable Phases explains Grokking in Deep Neural Networks». Нюанс: сосуществующие метастабильные состояния, разделённые энергетическими барьерами, удерживают сеть; гроккинг — шумовой выход через барьер. [`"coexisting metastable states, separated by energy barriers, can trap the network"`](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/original/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.md#p1-2). *«[сосуществующие метастабильные состояния, разделённые энергетическими преградами, способны запереть сеть](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.card.md#p1-2)»*

###### ref-3-11
**\[3.11\]** 2310.17247 — Miller, O'Neill, Bui 2024, «Grokking Beyond Neural Networks: An Empirical Exploration with Model Complexity». Нюанс: объяснения через ландшафт отнесены к теориям «через потерю», а у гауссова процесса с двумя настроечными параметрами ландшафт «ошибка — сложность» нарисован целиком. [`"appeal to the loss landscape of the training and test sets under different measures of complexity and data fit"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p2-6). *«[обращаются к ландшафту потерь обучающего и тестового наборов при разных мерах сложности и подгонки под данные](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p2-6)»*\
Доп. (три начальных приближения, гроккинг лишь у одного): [`"As evident in Figure 5, we only saw grokking for case B"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p8-1) — *«[Как видно на рисунке 5, гроккинг мы наблюдали только в случае B](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p8-1)»*.

###### ref-3-12
**\[3.12\]** 2411.00247 — Jeffares, Curth, van der Schaar, «Deep Learning Through A Telescoping Lens: A Simple Model Provides Empirical Insights On Grokking, Gradient Boosting & Beyond». Даёт достаточное условие линейной связности мод: если после точки $t^{*}$ градиенты модели перестают меняться, ансамбль и модель с усреднёнными весами становятся почти тождественны, — и подтверждает совпадение исчезновения барьера потерь со стабилизацией градиентов на ResNet-20. Нюанс: авторы сами объявляют объяснение неполным — градиенты прочих слоёв продолжают меняться, а у предобученной модели параметры BatchNorm меняются сильнее; с гроккингом этот случай в работе не связан. [`"the transition into a lazy regime during training – i.e. reaching a point $t^{*}$ after which the model gradients no longer change – can be *sufficient* to imply LMC during training"`](../papers/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond/original/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond.md#p8-4). *«[переход в ленивый режим в ходе обучения — то есть достижение точки $t^{*}$, после которой градиенты модели больше не меняются, — может быть *достаточен*, чтобы влечь LMC в ходе обучения](../papers/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond.card.md#p8-4)»*\
Доп. (признанный предел): [`"while there may be a connection between gradient stabilization and LMC, it cannot fully explain it"`](../papers/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond/original/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond.md#p9-2) — *«[хотя связь между стабилизацией градиентов и LMC может существовать, она не может объяснить её полностью](../papers/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond/2411.00247.deep-learning-through-a-telescoping-lens-a-simple-model-provides-empirical-insights-on-grokking-gradient-boosting-and-beyond.card.md#p9-2)»*.

###### ref-3-13
**\[3.13\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Нюанс: картина перехода седловая — weight decay выталкивает траекторию с седла запоминания в обобщающий бассейн; устойчивость же приписана не плоскости минимума, а существованию нескольких центральных многообразий рядом. Противопоставление плоским минимумам остаётся рассуждением: ни следа гессиана, ни его старших собственных чисел, ни какой-либо меры остроты работа не приводит. [`"robustness in overparameterized models arises not merely from flat minima (Keskar et al. 2017), but from **geometric redundancy in optimization pathways**"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p27-10). *«[устойчивость избыточно параметризованных моделей происходит не просто от плоских минимумов (Keskar et al. 2017), а от **геометрической избыточности путей оптимизации**](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p27-10)»*\
Доп. (седловая картина): [`"weight decay provides the regularization pressure to escape the memorization saddle and reach a generalizing basin"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p9-2) — *«[weight decay даёт регуляризационное давление, позволяющее сбежать с седла запоминания и достичь обобщающего бассейна](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p9-2)»*.

###### ref-3-14
**\[3.14\]** 2602.16967 — Xu, «Early-Warning Signals of Grokking via
Loss-Landscape Geometry». Нюанс: коммутаторный дефект как зонд ландшафта
перенесён с модульной арифметики на кодировщик-декодировщик и причинный
decoder-only трансформер; картина «удержание — накопление поперечных барьеров —
бегство» подтверждена на обеих задачах. [`"A large defect signals that the order of gradient updates matters—the loss landscape has developed curvature structure that breaks path-independence."`](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/original/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.md#p4-8). *«[Большой дефект означает, что порядок градиентных обновлений имеет значение — у ландшафта потерь развилась структура кривизны, нарушающая независимость от пути.](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.card.md#p4-8)»*

###### ref-3-16
**\[3.16\]** 2412.18624 — Kozyrev, «How to explain grokking». Нюанс: приносит в корпус овражный язык описания ландшафта (теснины низкой энтропии и широкие долины высокой, со ссылкой на овражный метод Гельфанда и Цетлина) и раскладывает обучение на падение в овраг и движение вдоль него. Ширина долин ничем не измерена, а несущий тезис «обобщающая область шире переобучающей» постулирован: расхожий счёт говорит скорее обратное — запоминающих решений много, алгоритмических мало. [`"The zero-risk manifold contains narrows, or ravines (regions with lower entropy) and wide valleys (with higher entropy)."`](../papers/2412.18624.how-to-explain-grokking/2412.18624.how-to-explain-grokking.card.md#p4-10). *«[Многообразие нулевого риска содержит теснины, или овраги (области с меньшей энтропией), и широкие долины (с большей энтропией)](../papers/2412.18624.how-to-explain-grokking/2412.18624.how-to-explain-grokking.card.md#p4-10)»*\
Доп. (исходный подход плоских минимумов, на который опирается построение): [`"narrow (sharp) minima of empirical risk (in the hypothesis space) are associated with overfitting, and wide (flat) minima correspond to solutions of the learning problem with generalization"`](../papers/2412.18624.how-to-explain-grokking/2412.18624.how-to-explain-grokking.card.md#p3-11) — *«[узкие (острые) минимумы эмпирического риска (в пространстве гипотез) связывают с переобучением, а широкие (плоские) минимумы отвечают решениям задачи обучения с генерализацией](../papers/2412.18624.how-to-explain-grokking/2412.18624.how-to-explain-grokking.card.md#p3-11)»*.
## Цитирования

**\[4.1\]** 2604.06256 — Xu, «Spectral Edge Dynamics Reveal Functional Modes of Learning». Нюанс: связь с кривизной проведена только ссылкой на предшествующую работу того же автора о коммутаторных дефектах (2602.16746). [`"This is a statement about loss landscape geometry, not representation theory."`](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/original/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.md#p5-10). *«[Это утверждение о геометрии ландшафта потерь, а не о теории представлений.](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.card.md#p5-10)»*

### Внешние работы

###### ref-5-1
**\[5.1\]** 2406.03999 — Внешняя работа (демотирована из корпуса): Song, Tan, Zou, Ma, Huang, «Unveiling the Dynamics of Information Interplay…». Две копии из одной инициализации, разный порядок данных и аугментации, затем линейная интерполяция весов ([с. 5, абз. 1](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p5-1)): на CIFAR-100 связность есть и обе меры вдоль отрезка почти не меняются, на CIFAR-10 при шаге $3e^{-2}$ связности нет — точность падает до случайного угадывания при весе интерполяции между 0.4 и 0.6. [`"Although we find it difficult to explain this anomaly, it does demonstrate that HDR and MIR have distinctive attributes compared to the accuracy metric"`](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p5-2). *«[Хотя нам трудно объяснить эту аномалию, она всё же показывает, что HDR и MIR обладают отличительными свойствами по сравнению с мерой точности](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p5-2)»*\
Доп. (что видно на рисунке): на [рис. 3](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#fig-3), панель (a), у MIR в середине провала горб, а HDR падает почти к нулю при весах около 0.1 и 0.94 — то есть одновременно с падением точности, а не против него.

```
concept:
  category: 2                    # 2. Механизмы и представления (Mechanisms & representations)
  papers_linked: 24             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-25
```
