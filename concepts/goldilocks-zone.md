# Зона Златовласки (Goldilocks zone)

[Варианты регуляризации](regularization-variants.md) ← предыдущая карточка, следующая → [Роль шума градиента](gradient-noise.md)

[Индекс карточек понятий](index.md), категория: [4. Факторы обучения и оптимизации](index.md#cat-4)\
→ Следующая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)\
← Предыдущая категория: [3. Задачи и наборы данных](modular-arithmetic.md)

## Определение

**Зона Златовласки** (Goldilocks zone) — узкая, «в самый раз» подобранная
область параметров нейросети (метафора отсылает к сказке о Златовласке,
выбирающей кашу не слишком горячую и не слишком холодную), внутри которой — и
только внутри которой — сеть обретает генерализацию. Понятие введено Liu et al.
(2022) применительно к [гроккингу](grokking.md): содержательное обучение
представлений происходит лишь в «зоне Златовласки», лежащей между режимами
[запоминания](memorization-phase.md) и «путаницы» \[[1.1](#ref-1-1)\]. В Omnigrok
зона формализована в пространстве весов — как узкий диапазон (сферическая
оболочка) нормы весов около критического значения, где генерализация лучше, чем
вне его \[[1.2](#ref-1-2)\].

![Фазовая диаграмма четырёх фаз обучения (comprehension, grokking, memorization, confusion): содержательное обучение живёт в узкой зоне (рис. 8 Liu et al.)](assets/goldilocks-phase-diagram.png)

## Детализация

Зона Златовласки появилась в «[эффективной теории](effective-theory-statistical-mechanics.md) обучения представлений»
Liu et al.: авторы выделяют четыре фазы обучения — comprehension (понимание),
grokking (гроккинг), memorization (запоминание) и confusion (путаница) — и
показывают, что содержательное обучение представлений идёт только в промежуточной
«зоне Златовласки», между избыточным запоминанием и полной путаницей
\[[1.1](#ref-1-1)\]. В Omnigrok та же идея переносится в пространство весов:
генерализующие решения лежат на тонкой сферической оболочке — узком диапазоне
нормы весов около критического значения, — и гроккинг объясняется тем, что при
большой инициализации сеть сначала быстро попадает в переобученное решение с
большой нормой вне оболочки, а затем [weight decay](weight-decay.md) (регуляризация, штрафующая
большую норму весов) медленно, по радиусу, втягивает вектор весов внутрь зоны,
где включается генерализация \[[1.2](#ref-1-2)\]. В этой картине управляющим
параметром служит норма весов, а сам [фазовый переход](phase-transition.md) —
это пересечение границы зоны. Идею подхватывают и расширяют: Kumar et al.
переносят «зону Златовласки» на ось размера обучающей выборки (генерализация
возможна, но не мгновенна) и связывают её с переходом от «ленивого» (lazy,
околоядерного режима, где [обучение признаков](feature-emergence-feature-learning.md) почти отсутствует) к «богатому»
(rich, режиму активного обучения признаков) \[[3.1](#ref-3-1)\]. Однако ряд работ
оспаривает жёсткость картины: требование именно сферической оболочки называют
слишком сильным и предполагают более сложную геометрию пространства весов
\[[2.1](#ref-2-1)\], а другие показывают, что гроккинг случается и вне типичной
зоны, из-за чего евклидова (L2) норма весов оказывается ненадёжным индикатором
генерализации \[[2.2](#ref-2-2)\].

## Альтернативные определения и нюансы

### A. Зона обучения представлений (между запоминанием и путаницей)

Исходная трактовка Liu et al.: параметр, задающий зону, — степень/качество
выученных представлений, а всё пространство поведений сети разбито на четыре
фазы (comprehension, grokking, memorization, confusion). Зона Златовласки — это
средний коридор между двумя провалами: избыточным запоминанием (представления
недоучены, обобщения нет) и «путаницей» (представления разрушены). Полезное
обучение представлений идёт только внутри этого коридора \[[1.1](#ref-1-1)\].

### B. Оболочка нормы весов (Omnigrok)

Трактовка Liu et al. в Omnigrok: управляющий параметр — скалярная норма весов;
генерализующие решения лежат на тонкой сферической оболочке (узком диапазоне
нормы весов около критического значения). Большая инициализация выбрасывает сеть
за пределы оболочки в переобученное решение, а weight decay затем медленно, по
радиусу, втягивает её в зону — этим и объясняется отложенность гроккинга
\[[1.2](#ref-1-2)\]. Ключевое отличие от трактовки A: зона задаётся напрямую
измеримой и управляемой величиной (нормой весов), а не абстрактным качеством
представлений, — поэтому в зону можно попасть искусственно, например проекцией
весов на сферу нужного радиуса.

### Оспаривают

Miller et al.: механизм с оболочкой правдоподобен, но требование именно
сферической зоны — «слишком строгое»; реальная геометрия пространства весов,
по-видимому, сложнее сферической оболочки \[[2.1](#ref-2-1)\]. Notsawo et al.:
гроккинг наблюдается даже вне типичной зоны Златовласки, из-за чего евклидова
(L2) норма весов оказывается ненадёжным индикатором генерализации; вместо неё
предлагаются иные меры — разреженность активаций и энтропия весов
\[[2.2](#ref-2-2)\].

### Поддерживают

Kumar et al. распространяют «зону Златовласки» с оси нормы весов на ось размера
обучающей выборки: нужен «в самый раз» объём данных — достаточный, чтобы
генерализация была возможна, но не мгновенной (в пределе бесконечных данных
гроккинга нет); попадание в такую зону необходимо, но недостаточно для
наблюдения гроккинга \[[3.1](#ref-3-1)\]. Ту же «золотую середину» они связывают
с трудностью задачи в смысле [NTK](neural-tangent-kernel-ntk.md)-выравнивания (согласованности задачи с
касательным ядром сети) \[[3.1](#ref-3-1)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2205.10343 — Liu et al., «Towards Understanding Grokking: An Effective
Theory of Representation Learning». [`"representation learning to occur only in a “Goldilocks zone” (including comprehension and grokking) between memorization and confusion"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p1-2). *«[обучение представлений происходит только в «зоне Златовласки» (включающей понимание и гроккинг) между запоминанием и путаницей](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#p1-2)»*\
Доп.: [`"Both comprehension and grokking are able to generalize (in the “Goldilocks zone”)"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p7-2) — *«[И понимание, и гроккинг способны генерализовать (находятся в «зоне Златовласки»)](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#p7-2)»*.

###### ref-1-2
**\[1.2\]** 2210.01117 — Liu et al., «Omnigrok: Grokking Beyond Algorithmic Data». [`"There is a spherical shell in the weight space (the "Goldilocks" zone), where generalization is better than outside this zone"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p2-3). *«…«зоне Златовласки»), где генерализация лучше, чем вне этой зоны»*\
Доп.: [`"CE produces a broader “Goldilocks zone" (the weight range where generalization happens) than MSE"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p15-8) — *«зона Златовласки» (диапазон весов, где происходит генерализация)»*.

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2310.17247 — Miller, O'Neill, Bui 2024, «Grokking Beyond Neural Networks: An Empirical Exploration with Model Complexity». Оспаривает: требование именно сферической зоны слишком жёсткое, а в опыте с гауссовым процессом такая геометрия не наблюдается. [`"the requirement of a spherical Goldilocks zone seems too stringent"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p3-1). *«[требование сферической зоны Златовласки представляется чрезмерно строгим](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p3-1)»*\
Доп. (опытная сверка): [`"we did not see a clear example of the spherical geometry mentioned in the Goldilocks zone theory of (Liu et al.(2023 a))"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p8-1) — *«[мы не увидели отчётливого примера сферической геометрии, о которой говорит теория зоны Златовласки (Liu et al.(2023 a))](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p8-1)»*.

###### ref-2-2
**\[2.2\]** 2506.05718 — Notsawo et al., «Grokking Beyond the Euclidean Norm of Model Parameters». Оспаривает: гроккинг случается и вне зоны, поэтому норма весов ненадёжна. [`"it occurs even outside the typical “goldilocks zone”"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p16-2). *«[он случается и вне обычной «зоны Златовласки»](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.card.md#p16-2)»*

###### ref-2-3
**\[2.3\]** 2311.18817 — Lyu et al., «Dichotomy of Early and Late Phase Implicit Biases Can Provably Induce Grokking». Оспаривает: для однородных сетей норма не ограничивает выразимость, поэтому узкая зона по норме сама требует объяснения, а не служит им. Нюанс: оспорено объяснение, а не наблюдение — эмпирический рецепт «большая инициализация плюс малое ненулевое затухание весов» взят у Liu et al. 2023 целиком и служит отправной точкой всей работы. [`"it is impossible to explain the effect of norm just from the expressive power, since all classifiers that can be represented with a certain norm can also be represented by all other norms"`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p4-2). *«[объяснить влияние нормы одной лишь выразительной способностью невозможно, поскольку все классификаторы, представимые при некоторой норме, представимы и при всех остальных нормах](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p4-2)»*\
Доп.: [`"Fort and Scherlis 2019; Liu et al. 2023 did not provide much explanation on why such a narrow Goldilocks zone could exist in the first place"`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p4-2) — *«[Fort and Scherlis 2019; Liu et al. 2023 не дали особых объяснений тому, почему такая узкая зона Златовласки вообще может существовать](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p4-2)»*.


###### ref-2-4
**\[2.4\]** 2405.12755 — Golechha 2024, «Progress Measures for Grokking on Real-world Tasks». Оспаривает: гроккинг вызван при неуклонно растущей норме весов (примерно 100 → 400) добавлением к потере члена $-\delta\sum_{w\in\theta}|w|^{2}$ при $\delta=2e-10$, и отсюда сделан вывод, что явление происходит далеко за пределами зоны. Нюанс: сама зона не измерена — ни критическая норма $w_{c}$, ни ландшафт приведённой тестовой потери по норме, то единственное, чем зона определена у Liu et al., для этой постановки не строятся; показано, что норма растёт, а не что она вне зоны. Собственный рисунок 1 в неизменённой постановке показывает падение нормы одновременно с генерализацией, то есть подтверждает мишень, и в выводах это не отражено. [`"we refute this claim by showing that grokking can occur way outside this ”goldilocks zone”"`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p2-4). *«[мы опровергаем это утверждение, показывая, что гроккинг может происходить далеко за пределами этой «зоны Златовласки»](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p2-4)»*\
Доп. (заявка во вкладах): [`"generalization can occur way outside the “goldilocks zone” of low weight norm"`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p1-8) — *«[генерализация может происходить далеко за пределами «зоны Златовласки» низкой нормы весов](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p1-8)»*.

###### ref-2-5
**\[2.5\]** 2505.11411 — Zhang, Shang, Yang, Zhang, «Is Grokking a Computational Glass Relaxation?». Оспаривает объяснение гроккинга через одну лишь эволюцию нормы к зоне Златовласки: игрушечный оптимизатор WanD обобщает при норме, дорастающей примерно до 175, без всяких ограничений. Существенно, что обе стороны спора воспроизведены в одной работе — жёсткое перемасштабирование к норме 30 гроккинг действительно почти снимает. Нюанс: контрпример сужен самими авторами — выход сети не масштабируется, а задачи Omnigrok не воспроизведены. [`"This provides strictly-defined counterexamples to theory attributing grokking solely to weight norm evolution towards the Goldilocks zone liu2022omnigrok"`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p1-2). *«[Это даёт строго определённые контрпримеры к теории, приписывающей гроккинг исключительно эволюции нормы весов к зоне Златовласки liu2022omnigrok](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p1-2)»*\
Доп. (рецепт оппонента подтверждён): [`"The training results, presented in Appendix Figure 6, show that grokking can be greatly eliminated under this condition."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p7-1) — *«[Результаты обучения, представленные в приложении на рисунке 6, показывают, что при этом условии гроккинг может быть в значительной мере устранён.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p7-1)»*\
Доп. (чем сужен контрпример): [`"but in a more strictly-defined context as we do not scale NN’s output, which could be taken as effectively changing the weight norm"`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p8-3) — *«[но в более строго определённом смысле, поскольку мы не масштабируем выход НС, что можно было бы счесть действенным изменением нормы весов](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p8-3)»*.

###### ref-2-6
**\[2.6\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Требует переформулировки весового извода понятия: узкий промежуток нормы, в котором наблюдается гроккинг, оказывается притягивающим равновесием weight decay, а не условием обобщения, — при зажатой норме сеть грокает и выше, и ниже него, лишь медленнее или быстрее. Сосредоточенность самого значения при этом не оспаривается, а уплотняется: разброс, о котором сообщали Manir & Rupa ($14.5\%$, одно семя на настройку), сведён к $1$–$2\%$ закреплением задачи и $\lambda$. Нюанс: под LayerNorm сосредоточенность полной нормы пропадает вовсе (разброс $0.15$–$0.17$) и возвращается только на развложении, так что «зона» в весовом изводе привязана к постановкам, где норма задаёт масштаб функции. [`"not a value grokking requires, which is why grokking is observed above and below it"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p11-5). *«[а не значение, которого гроккинг требует, отчего гроккинг и наблюдается выше и ниже него](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p11-5)»*\
Доп. (что уплотнено): [`"Our observational findings (§4) corroborate this rate-vs-threshold dissociation and tighten the concentration to CV $1$–$2\%$ by conditioning on task and $\lambda$"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p2-3) — *«[Наши наблюдательные находки (§4) подтверждают это расподобление скорости и порога и уплотняют сосредоточенность до коэффициента изменчивости $1$–$2\%$ за счёт обусловливания задачей и $\lambda$](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p2-3)»*.
### Поддерживают

###### ref-3-1
**\[3.1\]** 2310.06110 — Kumar et al., «Grokking as the Transition from Lazy to Rich». Нюанс: «зона Златовласки» существует и по оси размера данных. [`"being in the “goldilocks zone for data set size” is necessary but not sufficient to see grokking"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#p16-7). *«[пребывание в «зоне Златовласки по размеру набора данных» необходимо, но не достаточно, чтобы наблюдать гроккинг](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.card.md#p16-7)»*\
Доп.: [`"with grokking in a goldilocks somewhere in the middle, when the task is hard (in an NTK alignment sense)"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#p25-1) — *«[гроккинг возникает в златовласкиной середине — когда задача трудна (в смысле согласованности NTK)](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.card.md#p25-1)»*.


###### ref-3-2
**\[3.2\]** 2603.25009 — Manir, Rupa, «A Systematic Empirical Study of Grokking…». [`"exhibiting a narrow “Goldilocks” regime in which grokking occurs, while too little or too much prevents generalization"`](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p1-3). *«[обнаруживающий узкий режим «Златовласки», в котором гроккинг происходит, тогда как и слишком слабый, и слишком сильный не дают генерализации](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p1-3)»*\
Доп. (ширина зоны): [`"a factor-of-two step beyond the optimum collapses training entirely"`](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p10-2) — *«[шаг в два раза за оптимум полностью обрушивает обучение](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p10-2)»*.
###### ref-3-3
**\[3.3\]** 2308.09543 — Hu, Chen, Saphra & Cho, «Delays, Detours, and Forks in the Road: Latent State Models of Training Dynamics». Нюанс: независимое согласие из безнадзорной карты обучения: «just-right» падение нормы прочитано через гипотезу зоны Златовласки. [`"the weight norm is slow to reach a shell of particular $L_{2}$ norm in weight space, previously called the “Goldilocks zone” (Fort & Scherlis 2018)"`](../papers/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics/original/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics.md#p12-3). *«[норма весов медленно достигает оболочки определённой $L_{2}$-нормы в пространстве весов, прежде названной «зоной Златовласки» (Fort & Scherlis 2018)](../papers/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics.card.md#p12-3)»*

###### ref-3-4
**\[3.4\]** 2605.15787 — Hidajat, Stoll, An 2026, «Grokking as Structural Inference: Transformers Need Bayesian Lottery Tickets». Зона Златовласки входит в теорему целиком как нормовое условие $\mathcal{N}$ ($\|\theta_{\text{MLP}}\|_{F}\in[r_{\min},r_{\max}]$), но объявлена необходимой и недостаточной: в факторном опыте норм-подгонка (проекция весов MLP на гиперсферу $r_{\max}$ на каждом шаге) с враждебным вниманием оставляет точность случайной, а оракульное внимание без контроля нормы даёт лишь частичное обобщение. Нюанс: необходимость $\mathcal{N}$ выведена фразой «стандартные границы обобщения предписывают» со ссылкой на Liu и Notsawo — это ссылка, а не вывод; вся теория выведена для линейного ReLU-внимания и «расширена» на softmax гипотезой, названной «наблюдательно полной» по совпадению кривых. [`"Perfect generalization appears only when capacity control and structural routing are present together."`](../papers/2605.15787.grokking-as-structural-inference-transformers-need-bayesian-lottery-tickets/2605.15787.grokking-as-structural-inference-transformers-need-bayesian-lottery-tickets.card.md#p8-2). *«[Совершенная генерализация появляется, только когда контроль ёмкости и структурная маршрутизация присутствуют вместе.](../papers/2605.15787.grokking-as-structural-inference-transformers-need-bayesian-lottery-tickets/2605.15787.grokking-as-structural-inference-transformers-need-bayesian-lottery-tickets.card.md#p8-2)»*

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2309.02390 — Varma et al., «Explaining Grokking Through Circuit Efficiency». [`"it takes longer for regularisation to reduce parameter norm to the “Goldilocks zone” where generalisation occurs"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p10-6). *«[регуляризации требуется больше времени, чтобы привести норму параметров в «зону Златовласки», где происходит обобщение](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p10-6)»*

**\[4.2\]** 2405.19454 — Fan, Pascanu, Jaggi 2024, «Deep Grokking: Would Deep Neural Networks Generalize Better?». [`"the model needs to *grok* slowly into the *Goldilocks zone* (Fort & Scherlis, 2018) for generalization"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p6-2). *«[модели нужно медленно *гроккнуть* в *зону Златовласки* (Fort & Scherlis, 2018) ради генерализации](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p6-2)»*

**\[4.3\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». [`"weight norms need to be in a narrow range or “Goldilocks Zone” for generalization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-5). *«[для генерализации нормы весов должны лежать в узком диапазоне — «зоне Златовласки»](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-5)»*

**\[4.4\]** 2504.13292 — Xu et al., «Let Me Grok For You: Accelerating Grokking». [`"grokking through the concept of a “Goldilocks zone”, a spherical shell of weights"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p3-1). *«[гроккинг через понятие «зоны Златовласки» — сферической оболочки весов](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p3-1)»*

**\[4.5\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». [`"emerges in a specific zone for the weights of a network called the ”Goldilocks Zone”"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p4-1). *«[возникает в определённой зоне для весов сети, названной «зоной Златовласки»](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p4-1)»*

**\[4.6\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». [`"slow formation of useful representations within a “Goldilocks zone” between memorization and confusion [16]"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p2-3). *«[медленным складыванием полезных представлений в «зоне Златовласки» между запоминанием и смятением](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p2-3)»*

**\[4.7\]** 2603.15492 — Acharya et al., «Grokking as a Variance-Limited Phase Transition». [`"Liu et al. [2] identified a "Goldilocks zone" of initialization and data size"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p2-2). *«Liu et al. \[2\] выявили «зону Златовласки» по инициализации и размеру данных»*

**\[4.8\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation». [`"Liu et al. (2022) described a “Goldilocks zone” of weight norms in which generalisation emerges"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-1). *«[Liu et al. (2022) описали «зону Златовласки» по норме весов, где возникает генерализация](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-1)»*

**\[4.9\]** 2305.18741 — Murty et al. 2023, «Grokking of Hierarchical Structure in Vanilla Transformers». Понятие упомянуто только в обзоре величин, связываемых с гроккингом; зона в работе не ищется, границ по норме весов не проводится, а весь дальнейший разбор состоит в том, что норма весов монотонно растёт и различать удачные архитектуры не годится. [`"Liu et al. 2022 identify a “goldilocks zone” in weight norm space where grokking occurs"`](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/original/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.md#p4-1). *«[Liu et al. 2022 выделяют «goldilocks zone» в пространстве нормы весов, где гроккинг и происходит](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.card.md#p4-1)»*

**\[4.10\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Лишь упоминает: «зона Златовласки» приведена как позиция Liu et al. 2022b в споре о причинной стрелке между нормой и обобщением. Работа этой зоны не ищет и не измеряет, но её собственный довод сдвигает искомую «в самый раз» величину с нормы параметров на отношение норм шага и параметров — словом «зона» это не называется. [`"Liu et al. 2022b characterize grokking as the convergence of the parameter norm towards a “goldilocks zone” (Fort & Scherlis 2019) which allows for generalization"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#p3-2). *«[Liu et al. 2022b описывают гроккинг как схождение нормы параметров к «зоне Златовласки» (Fort & Scherlis 2019), допускающей обобщение](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#p3-2)»*
