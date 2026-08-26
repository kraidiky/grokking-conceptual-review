# Край устойчивости (edge of stability)

[Роль шума градиента](gradient-noise.md) ← предыдущая карточка, следующая → [Направление влияния weight decay](weight-decay-direction.md)

[Индекс карточек понятий](index.md), категория: [4. Факторы обучения и оптимизации](index.md#cat-4)\
→ Следующая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)\
← Предыдущая категория: [3. Задачи и наборы данных](modular-arithmetic.md)

## Определение

**Край устойчивости** (edge of stability, EoS) — режим обучения градиентным
спуском, при котором «резкость» [ландшафта потерь](loss-landscape-basins.md) (sharpness — максимальное
собственное значение гессиана, то есть матрицы вторых производных функции
потерь по параметрам) в ходе «прогрессирующего заострения» дорастает до порога
устойчивости 2/η (η — [скорость обучения](learning-rate.md)) и далее удерживается около него: за
этим порогом шаг уже не гарантирует убывание потери, и обучающая потеря
становится немонотонной на коротких временных интервалах \[[1.1](#ref-1-1)\].
Понятие введено Cohen et al. (2021); в дискурс [гроккинга](grokking.md) его
перенесли Thilak et al., связав нестабильность
[слингшота](slingshot.md) с EoS \[[1.1](#ref-1-1)\].

![Резкость обновлений против роста нормы: циклы слингшота разворачиваются у порога устойчивости (рис. 3 Thilak et al.)](assets/edge-of-stability-sharpness.png)

## Детализация

Механизм EoS двухступенчатый. Сначала идёт **прогрессирующее заострение**
(progressive sharpening): максимальное собственное значение гессиана потерь
монотонно растёт по мере обучения \[[1.1](#ref-1-1)\]. Как только оно достигает
порога 2/η, дискретный шаг градиентного спуска перестаёт быть сжимающим — шаг
может увеличить, а не уменьшить потерю, — и система входит в самостабилизующийся
режим: резкость колеблется около критического значения, а обучающая потеря
даёт серии высокочастотных всплесков. Именно этот **[фазовый
переход](phase-transition.md)** между устойчивой и неустойчивой динамикой Cohen
et al. и назвали краем устойчивости.

В теории гроккинга EoS используют как объяснение того, почему генерализация
запаздывает. Классическая оговорка: исходно EoS показан для полнопакетного
(full-batch, то есть по всей выборке сразу) градиентного спуска, тогда как
циклические всплески слингшота наблюдаются у **адаптивных [оптимизаторов](optimizer-adam-adamw-sgd.md)**
(Adam/AdamW — методы, масштабирующие шаг покомпонентно по накопленной дисперсии
градиента) \[[1.1](#ref-1-1)\]. Отсюда спор о том, одно ли это явление. Одна
линия (Acharya et al.) трактует нестабильность как **функциональную**: край
устойчивости — это ворота доступа к «острому» генерализующему бассейну (области
минимума с высокой кривизной), войти в который оптимизатор может, лишь когда
накопленная дисперсия градиента временно поднимает потолок устойчивости; в этом
прочтении «нестабильность управляет [обучением признаков](feature-emergence-feature-learning.md)», а слингшот — не
аномалия, а необходимый впрыск дисперсии \[[3.1](#ref-3-1)\]. Встречная линия
(Liu et al.) оспаривает отождествление поздней нестабильности гроккинга с EoS:
при кросс-энтропийной потере у интерполирующего решения максимальное
собственное значение гессиана стремится к нулю, то есть обучение идёт **далеко
ниже** порога 2/η, и поздние всплески слингшота — численный артефакт малой
разрядности, а не подлинный край устойчивости, который остаётся внутренним
свойством ландшафта \[[2.1](#ref-2-1)\].

## Альтернативные определения и нюансы

### A. Классическая динамическая трактовка (прогрессирующее заострение → самостабилизация)

Исходное определение (Cohen et al. в изложении Thilak et al.): EoS —
детерминированный режим *полнопакетного* градиентного спуска, задаваемый одним
[параметром порядка](order-parameter.md) — резкостью λ_max относительно порога 2/η. Механизм:
резкость сама растёт (progressive sharpening) до порога, после чего перестаёт
расти и осциллирует вокруг него, а обучающая потеря ведёт себя немонотонно на
коротких интервалах \[[1.1](#ref-1-1)\]. Ключевое отличие этой трактовки —
источник нестабильности **эндогенный и геометрический**: он определяется
кривизной ландшафта и скоростью обучения, а не оптимизатором или арифметикой.

### B. EoS против слингшота: одно ли это явление

Отдельный нюанс — соотношение EoS с эффектом [слингшота](slingshot.md). Thilak
et al. прямо разводят их по **типу оптимизатора**: край устойчивости показан для
полнопакетного градиентного спуска, тогда как слингшот они наблюдают у
адаптивных оптимизаторов (прежде всего Adam/AdamW) и с *повторяющимся
циклическим* поведением, которого нет у канонического EoS \[[1.1](#ref-1-1)\].
Источник различия здесь — не механизм заострения, а **режим оптимизации**: то,
считать ли поздние циклические всплески проявлением того же порога устойчивости
или отдельным явлением, и определяет весь последующий спор.

### Оспаривают

Liu et al. (2605.06152) оспаривают тезис, что поздняя нестабильность гроккинга —
это EoS. Различающая машинерия: при кросс-энтропийной потере у интерполирующего
решения (когда предсказанная вероятность сходится к one-hot метке) максимальное
собственное значение гессиана стремится к нулю, поэтому «прогрессирующее
заострение, необходимое для EoS, не происходит на поздних стадиях»
\[[2.1](#ref-2-1)\]. Значит, обучение идёт далеко ниже порога 2/η, и всплески
берутся не из кривизны, а из конечной точности вычисления потери: переход к
float64 полностью убирает спайки слингшота, тогда как настоящие осцилляции EoS в
двойной точности сохранились бы. Итог: EoS реален, но поздний слингшот — это
**численный артефакт малой разрядности**, а не край устойчивости; подлинный EoS
у них проявляется лишь на ранней стадии \[[2.1](#ref-2-1)\].

### Поддерживают

Acharya et al. (2603.15492) поддерживают и усиливают EoS-прочтение гроккинга:
«нестабильность управляет обучением признаков», а «гроккинг, по-видимому,
происходит вблизи края устойчивости» \[[3.1](#ref-3-1)\]. Различающая машинерия:
генерализующий бассейн для задач модулярной арифметики **острее** начального
порога устойчивости оптимизатора, поэтому вход в него заблокирован, пока
накопленная дисперсия градиента не поднимет потолок 2/η_eff над кривизной
бассейна. В этом прочтении слингшот переопределён из аномалии в **механизм
впрыска дисперсии**, обслуживающий именно ограничение края устойчивости.
Источник различия с трактовкой A — EoS здесь не побочный геометрический режим, а
**причинный шлюз** генерализации: без выхода на край устойчивости острый
генерализующий контур недостижим.

## Ссылки

###### ref-1-1
**\[1.1\]** 2206.04817 — Thilak et al., «The Slingshot Mechanism: An Empirical Study of Adaptive Optimizers and the Grokking Phenomenon». [`"Cohen et al. [4] call Edge of Stability where-in the model shows non-monotonic training loss behavior over short time spans."`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p3-4). *«[Cohen et al. [4] называют *краем устойчивости*, где модель обнаруживает немонотонное поведение потери на обучении на коротких временны́х промежутках](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p3-4)»*\
Доп.: [`"describe a "progressive sharpening" phenomenon in which the maximum eigenvalue of the loss Hessian increases"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p3-4) — *«описывают явление «прогрессирующего заострения», при котором максимальное собственное значение гессиана потерь растёт»*; [`"Edge of Stability is shown for full-batch gradient descent while we observe Slingshot Mechanism with adaptive optimizers"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p3-4) — *«[*Край устойчивости* показан для полнобатчевого градиентного спуска, тогда как механизм рогатки мы наблюдаем при адаптивных оптимизаторах](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p3-4)»*.

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2605.06152 — Liu, Cao, Li, Zhou 2026, «Grokking or Glitching? How Low-Precision Drives Slingshot Loss Spikes». Оспаривает отождествление поздней неустойчивости при гроккинге с краем устойчивости: EOS подлинен и происходит из ландшафта, а поздние пращевые всплески суть численный след малой разрядности. [`"EOS is an intrinsic property of the optimization landscape that persists regardless of numerical precision"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p16-6). *«[EOS есть собственное свойство ландшафта оптимизации, сохраняющееся независимо от численной точности](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p16-6)»*\
Доп.: [`"the progressive sharpening required for EOS does not occur in the late stages of training"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p16-3) — *«[постепенное обострение, нужное для EOS, на поздних порах обучения не происходит](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p16-3)»*; [`"This confirms that the early instability is genuine EOS, while the late-stage instability is strictly a result of numerical breakdown"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p16-7) — *«[Это подтверждает, что ранняя неустойчивость есть подлинный EOS, а поздняя строго есть следствие численного отказа](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p16-7)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.15492 — Acharya et al., «Grokking as a Variance-Limited Phase Transition». Нюанс: EoS — причинный шлюз генерализации; нестабильность управляет обучением признаков, а слингшот — механизм впрыска дисперсии, обслуживающий порог устойчивости. [`"Edge of Stability (EoS): Research arguing that instability drives feature learning [6, 18]."`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-7). *«[**Край устойчивости (EoS):** работы, доказывающие, что неустойчивость движет выучиванием признаков [6, 18]](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p1-7)»*\
Доп.: [`"Grokking appears to happen near the edge of stability. The optimizer needs enough instability to generate variance for exploration"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p10-7) — *«[Гроккинг, по-видимому, происходит близ края устойчивости. Оптимизатору нужно достаточно неустойчивости, чтобы выработать разброс для обследования](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p10-7)»*.\
Доп. (условие затвора): [`"The optimizer can only converge into a basin with Hessian curvature $\lambda_{max}^{H}$ if it satisfies the **Spectral Gating Condition**"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p4-2) — *«[Оптимизатор способен сойтись в котловину с кривизной гессиана $\lambda_{max}^{H}$, лишь если выполнено **условие спектрального затвора**](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p4-2)»*

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2306.13253 — Notsawo et al., «Predicting Grokking Long Before it Happens». [`"the model to enter an Edge of Stability regime where loss shows non-monotonic training behaviour over short time"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p12-12). *«приводя модель в режим края устойчивости, где потеря демонстрирует немонотонное обучающее поведение на коротких интервалах»* — EoS упомянут как следствие прогрессирующего заострения

**\[4.2\]** 2408.08944 — Clauw, Stramaglia, Marinazzo 2024, «Information-Theoretic Progress Measures reveal Grokking is an Emergent Phase Transition». [`"Recent work observed distinct phases during optimization when training neural networks (Kalra & Barkeshli, 2023) related to the edge of stability (Cohen et al., 2020)"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p5-7). *«[Недавние работы наблюдали различные поры при оптимизации в ходе обучения нейронных сетей (Kalra & Barkeshli, 2023), связанные с краем устойчивости (Cohen et al., 2020)](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.card.md#p5-7)»* — *EoS назван в перечне фаз оптимизации*

**\[4.3\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». [`"possibly relating to catapults (Lewkowycz et al., 2020) or the edge of stability mechanism"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p9-6). *«[возможно, в связи с катапультами (Lewkowycz et al., 2020) или механизмом края устойчивости](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p9-6)»* — EoS упомянут как направление для будущего изучения при больших скоростях обучения.

**\[4.4\]** 2603.13331 — Truong et al., «The Norm-Separation Delay Law of Grokking: A First-Principles Theory of Delayed Generalization». [`"Alternative mechanisms—such as grokking without weight decay or through edge-of-stability effects (Thilak et al., 2022)—may require separate treatment."`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p26-2). *«[Иные устройства — вроде гроккинга без weight decay или через явления края устойчивости (Thilak et al., 2022) — могут потребовать отдельного рассмотрения](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p26-2)»* — EoS назван как ограничение области применимости их теории.

**\[4.5\]** 2210.15435 — Žunkovič, Ilievski, «Grokking phase transitions in learning local rules with gradient descent». Нюанс: край устойчивости упомянут одной скобкой при изложении механизма рогатки; ни кривизны, ни собственных значений гессиана, ни каких-либо относящихся к краю устойчивости величин работа не считает. [`"A sling-shot mechanism (related to edge of stability [15]) has been proposed as a necessary condition for grokking."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p4-3). *«[Механизм рогатки (sling-shot, связанный с краем устойчивости [15]) был предложен как необходимое условие гроккинга](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p4-3)»*

**\[4.6\]** 2310.16441 — Levi et al. 2023. Нюанс: катапульта и край устойчивости названы в списке направлений — как то, что осталось за пределом градиентного потока; измерений остроты и больших шагов в работе нет. [`"combining catapult/edge of stability dynamics with grokking analysis"`](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/original/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.md#p11-3). *«[соединив динамику катапульты / края устойчивости с разбором гроккинга](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.card.md#p11-3)»*

**\[4.7\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Лишь упоминает, но содержательно: величина разогрева оправдана через остроту местного бассейна и механизм катапульты — шаг должен превысить остроту, чтобы оптимизатор выпрыгнул из бассейна. Ни остроты, ни собственных значений гессиана работа при этом не измеряет; край устойчивости входит в обзорный перечень связей между кривизной и действенным размером шага. [`"We propose to instead escape from a local minimum not by changing the loss landscape, but by increasing the learning rate, for example to a value which exceeds the sharpness of the local basin (Lewkowycz et al. 2020)"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#p7-5). *«[Мы предлагаем взамен выбираться из местного минимума не изменением ландшафта потерь, а повышением скорости обучения — например, до значения, превышающего остроту местного бассейна (Lewkowycz et al. 2020)](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#p7-5)»*

**\[4.8\]** 2605.08237 — Wang, Ying, Kanamori 2026, «Distributional Spectral Diagnostics for Localizing Grokking Transitions». Нюанс: кривизна в работе не измеряется вовсе; противопоставление проведено по месту сигнала. [`"the edge-of-stability phenomenon in which the top Hessian eigenvalue saturates near $2/\eta$ [7, 9]"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p31-2). *«[явление края устойчивости, при котором старшее собственное значение гессиана насыщается вблизи $2/\eta$ [7, 9]](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p31-2)»*
