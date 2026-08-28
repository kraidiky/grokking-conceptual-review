# Разброс по семенам и воспроизводимость (seed variance / reproducibility)

[Время гроккинга](grokking-time.md) ← предыдущая карточка, следующая → [Параметр порядка](order-parameter.md)

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Разброс по семенам** — расхождение исходов между прогонами, отличающимися только случайным семенем (инициализацией, порядком данных, разбиением выборки), а **воспроизводимость** — вопрос о том, что из сообщённого переживает смену этого семени. Для [гроккинга](grokking.md) вопрос не служебный: величина, вокруг которой строится большинство работ, — [время гроккинга](grokking-time.md) — на одной и той же настройке меняется от «грокнул рано» до «не грокнул в пределах бюджета», и одиночный прогон этого не показывает: *«Одиночные прогоны лгут в этом режиме, систематически»* \[[1.1](#ref-1-1)\].

Наглядная мера цены вопроса: на одной глубине из пяти семян грокают четыре, одно опаздывает на порядок, а одно не грокает вовсе \[[1.2](#ref-1-2)\]. Разброс задевает не только сроки, но и то, что сеть выучивает: при обучении на одной и той же группе выученные представления между семенами не согласованы \[[1.3](#ref-1-3)\].

## Детализация

Семя в этих работах меняет сразу несколько вещей — начальное приближение весов, порядок предъявления данных, а часто и само разбиение на обучение и тест, — поэтому «пять семян» означают пять розыгрышей всей задачи, а не пять инициализаций на закреплённых данных. Отсюда три разных употребления разброса в корпусе.

**Как отчётная величина.** Минимальная дисциплина — сообщать число семян и разброс рядом со средним: «ради воспроизводимости» три семени \[[3.5](#ref-3-5)\], десять семян на каждую глубину \[[3.2](#ref-3-2)\], сорок семян на настройку \[[3.1](#ref-3-1)\], разброс по восьми прогонам прямо в таблице сравнения \[[3.8](#ref-3-8)\]. Сюда же относится полнота отчёта о настройках, без которой чужой прогон невоспроизводим в принципе \[[3.7](#ref-3-7)\]. Отдельная тонкость — как считать среднее, когда часть семян не грокнула: прогоны, не дошедшие до порога, помечаются DNF и из средней задержки исключаются, что смещает её вниз, и потому число таких семян сообщается рядом \[[1.2](#ref-1-2)\].

**Как порог значимости.** Разброс превращается в критерий, когда по нему решают, есть ли эффект. Толчки вдоль коммутатора не ускоряют гроккинг: все 27 прогонов обобщают в сроки, неразличимые *«в пределах разброса между сидами»* \[[3.4](#ref-3-4)\] — отрицательный результат, который без разброса не сформулировать. В обратную сторону работает та же логика у ранних признаков: признак негоден, если его опережение неустойчиво, *«стандартное отклонение превышает среднее опережение»* \[[3.3](#ref-3-3)\].

**Как предмет устройства опыта.** Сильнейшая форма — сделать сравнение парным: две ветви растут из одного и того же состояния и до вмешательства совпадают побитово, так что всё последующее различие приписывается вмешательству, а не семени \[[2.1](#ref-2-1)\]. Слабейшая — сообщить выигрыш по одному семени: вариант, выигрывающий по первому касанию порога, при меньшей скорости обучения гроккает лишь в одном семени из четырёх \[[2.2](#ref-2-2)\].

Разброс проникает и в само определение момента: критерий обнаружения гроккинга требует не только среднего выше порога, но и малого стандартного отклонения внутри окна замеров \[[3.6](#ref-3-6)\] — то есть устойчивость встроена в определение, а не проверяется после. Отдельно стоит вопрос о числовой обстановке: часть разброса приходит не от семени, а от недетерминизма вычислений, и работы, добивающиеся побитовой воспроизводимости, отделяют одно от другого \[[1.1](#ref-1-1)\].

## Альтернативные определения и нюансы

### A. Разброс сроков против разброса решений

Первое употребление — разброс измеряемой величины (шаг гроккинга, конечная точность): семена дают распределение, и вопрос в его ширине и хвостах \[[1.2](#ref-1-2)\]. Второе — разброс самого выученного: два семени с одинаковой точностью реализуют разные представления одной и той же группы \[[1.3](#ref-1-3)\]. Различающая черта: в первом случае усреднение осмысленно, во втором оно уничтожает предмет — среднее двух несовместимых решений решением не является, и сравнивать приходится распределения структур, а не числа.

### B. Парное сравнение против сравнения средних

Здесь источник различия — устройство опыта, а не статистика. Парная схема ветвит две траектории из одного состояния и потому исключает семя как источник различия по построению \[[2.1](#ref-2-1)\]; сравнение средних требует достаточного числа семян и оставляет открытым вопрос, не объясняется ли разница удачной выборкой. Практическое следствие: заявка об ускорении, подкреплённая одним семенем, и заявка, подкреплённая парными ветвями, — утверждения разной силы, хотя обе сообщают «стало быстрее» \[[2.2](#ref-2-2)\].

### C. Разброс как шум и разброс как предмет

В большинстве работ разброс — помеха, которую усредняют. Но там, где семя меняет разбиение выборки, оно разыгрывает саму задачу, и распределение исходов становится предметом: доля грокнувших семян несёт больше сведений, чем средний срок по грокнувшим \[[1.1](#ref-1-1)\]. Управляющая величина здесь — покрытие обучающей выборки и регуляризация, а не оптимизатор; отсюда и вывод, что переход условен, а не универсален.

### D. Недетерминизм вычислений

Отдельный источник расхождений, который легко спутать с семенем: повторное проигрывание сохранённой траектории на ускорителе не побитово, и два прогона одной настройки расходятся без всякой смены семени. Работы, которым нужна парность ветвей, уходят ради этого на CPU или ветвятся внутри одного процесса \[[2.1](#ref-2-1)\], а работы о числовой устойчивости прямо разделяют вклад среды и вклад инициализации \[[1.1](#ref-1-1)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2607.05104 — Ootani, «Grokking Is Conditional and Fragile: A Fully-Tractable, Multi-Seed Study at 12K Parameters». [`"Single runs lie in this regime, systematically."`](../papers/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters/original/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters.md#p11-2). *«[Одиночные прогоны лгут в этом режиме, систематически.](../papers/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters.card.md#p11-2)»*

###### ref-1-2
**\[1.2\]** 2603.25009 — Manir et al., «A Systematic Empirical Study of Grokking: Depth, Architecture, Activation, and Regularization». [`"4 of 5 grokked; one seed exhibited a very late grokking at step 212,000 and one DNF"`](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p6-2). *«[4 из 5 грокнули; одно семя показало очень поздний гроккинг на шаге 212,000, ещё одно — DNF](../papers/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization/2603.25009.a-systematic-empirical-study-of-grokking-depth-architecture-activation-and-regularization.card.md#p6-2)»*

###### ref-1-3
**\[1.3\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». [`"Under strong universality, we would expect the representations learned to be consistent across random seeds when trained on the same group. In general, we do not find this to be true"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p7-6). *«[При сильной универсальности мы ожидали бы, что выученные представления согласованы между случайными сидами при обучении на одной и той же группе. В общем случае мы этого не обнаруживаем](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p7-6)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2608.07436 — Janati et al., «Post-Grokking Collapse at the Representation–Readout Interface in Muon-Trained Transformers». Оспаривает сравнение средних: две ветви растут из одного состояния и до вмешательства совпадают побитово, так что семя исключено как источник различия по построению. [`"The two arms are identical before the freeze to a maximum absolute difference of zero across every logged evaluation."`](../papers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers/original/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers.md#fig-3). *«[Две ветви тождественны до заморозки с наибольшим абсолютным различием, равным нулю, на каждом занесённом замере.](../papers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers.card.md#fig-3)»*

###### ref-2-2
**\[2.2\]** 2607.20512 — Wang, «The Active Ingredient in Muon’s Grokking». Оспаривает выигрыш, сообщённый одним числом: при меньшей скорости обучения вариант гроккает лишь в одном семени из четырёх. [`"Spec groks in only 1/4 seeds at $3\!\times\!10^{-4}$"`](../papers/2607.20512.the-active-ingredient-in-muons-grokking/original/2607.20512.the-active-ingredient-in-muons-grokking.md#p6-1). *«[при $3\!\times\!10^{-4}$ Spec гроккает лишь в 1/4 семян](../papers/2607.20512.the-active-ingredient-in-muons-grokking/2607.20512.the-active-ingredient-in-muons-grokking.card.md#p6-1)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2308.09543 — Hu et al., «Delays, Detours, and Forks in the Road: Latent State Models of Training Dynamics». Нюанс: сорок семян берутся не для отчётности, а чтобы измерить саму чувствительность обучения к случайности. [`"we add layer normalization back in and train over 40 random seeds"`](../papers/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics/original/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics.md#p9-1). *«[мы возвращаем layer normalization и обучаем по 40 случайным семенам](../papers/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics/2308.09543.delays-detours-and-forks-in-the-road-latent-state-models-of-training-dynamics.card.md#p9-1)»*

###### ref-3-2
**\[3.2\]** 2305.18741 — Murty et al., «Grokking of Hierarchical Structure in Vanilla Transformers». Нюанс: десять семян на каждую глубину — минимальная дисциплина при сравнении архитектур. [`"we train models with 10 random seeds for 300k (400k for Dyck) steps"`](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/original/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.md#p2-6). *«[мы обучаем модели с 10 случайными семенами по 300 тысяч шагов (400 тысяч для Dyck)](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.card.md#p2-6)»*

###### ref-3-3
**\[3.3\]** 2604.20923 — Golwala, «ILDR: Geometric Early Detection of Grokking». Нюанс: разброс превращён в критерий пригодности признака — опережение бесполезно, если его отклонение больше его самого. [`"proves unstable across seeds with standard deviation exceeding mean lead time"`](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/original/2604.20923.ildr-geometric-early-detection-of-grokking.md#p1-2). *«[неустойчивым по семенам — стандартное отклонение превышает среднее опережение](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/2604.20923.ildr-geometric-early-detection-of-grokking.card.md#p1-2)»*

###### ref-3-4
**\[3.4\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». Нюанс: разброс по семенам служит порогом значимости, и отрицательный результат сформулирован именно через него. [`"within seed-to-seed variability). This negative result demonstrates that defect accumulation alone is insufficient to induce grokking"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p12-7). *«[в пределах разброса между сидами). Этот отрицательный результат показывает, что одного накопления дефекта недостаточно, чтобы вызвать гроккинг](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p12-7)»*

###### ref-3-5
**\[3.5\]** 2310.19470 — Minegishi et al., «Bridging Lottery Ticket and Grokking: Understanding Grokking from Inner Structure of Networks». Нюанс: три семени названы прямо как мера воспроизводимости, а не как усреднение. [`"To ensure reproducibility, we conduct experiments with three different seeds"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p20-1). *«[Ради воспроизводимости мы проводим опыты с тремя разными сидами](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.card.md#p20-1)»*

###### ref-3-6
**\[3.6\]** 2603.24746 — Bi et al., «Grokking as a Falsifiable Finite-Size Transition». Нюанс: устойчивость встроена в само определение момента гроккинга — окно требует малого отклонения, а не только высокого среднего. [`"require the held-out standard deviation in that window to stay below $0.02$"`](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p8-47). *«[чтобы отложенное стандартное отклонение в этом окне оставалось ниже $0.02$](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p8-47)»*

###### ref-3-7
**\[3.7\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». Нюанс: воспроизводимость понимается как полнота отчёта о низкоуровневых настройках, а не только как число семян. [`"For reproducibility, information about low-level details like learning rate, amount of weight decay"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p9-6). *«[Ради воспроизводимости сведения о низкоуровневых подробностях вроде скорости обучения, величины weight decay](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p9-6)»*

###### ref-3-8
**\[3.8\]** 2410.03569 — Saxena et al., «Making Hard Problems Easier with Custom Data Distributions and Loss Regularization: A Case Study in Modular Arithmetic». Нюанс: разброс сообщается рядом со средним прямо в таблице сравнения — иначе «лучше» неотличимо от «повезло». [`"We report the *average* $\tau=0.5\%$ accuracy (see §3) and the variance across 8 trials."`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#tab-4). *«[Мы приводим *среднюю* точность при $\tau=0.5\%$ (см. раздел 3) и разброс по 8 прогонам.](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.card.md#tab-4)»*
