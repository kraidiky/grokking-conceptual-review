# StableMax / perp-Grad (numerical-stability fix)

[Grokfast / фильтрация градиента](gradient-low-pass-filtering.md) ← предыдущая карточка, следующая → [Замораживание подсети / edge-popup](freezing-subnetwork.md)

[Индекс карточек понятий](index.md), категория: [5. Интервенции и методы](index.md#cat-5)\
→ Следующая категория: [6. Аналитические инструменты и метрики](progress-measures.md)\
← Предыдущая категория: [4. Факторы обучения и оптимизации](weight-decay.md)

## Определение

**Исправление численной нестабильности** (numerical-stability fix) — семейство
вмешательств, предложенных Prieto et al. (2025), которые устраняют численный сбой
softmax ([Softmax Collapse](softmax-collapse.md)) или порождающую его компоненту
градиента ([наивную минимизацию потерь, NLM](nlm.md)) и тем самым включают
[гроккинг](grokking.md) без регуляризации. Два ключевых средства — **StableMax**
(замена softmax, не насыщающаяся при больших логитах) и **⊥Grad** ([оптимизатор](optimizer-adam-adamw-sgd.md),
удаляющий NLM-направление из градиента); простейший вариант — повышение разрядности
арифметики с float32 до float64 \[[1.1](#ref-1-1)\].

![Гроккинг, включённый вмешательствами против численного сбоя softmax (рис. 11 Prieto et al.)](assets/stablemax-interventions.png)

## Детализация

Все три средства бьют по одной цепочке: за точкой переобучения градиент почти
целиком уходит в NLM-направление — рост **логитов** (предсофтмаксных выходов сети)
без изменения предсказаний, — что рано или поздно вызывает **ошибку поглощения**
(absorption error: малое слагаемое теряется при сложении с много большим) в softmax
и обнуляет градиент, останавливая обучение уже после
[фазы запоминания](memorization-phase.md) \[[1.1](#ref-1-1)\]. Средства
различаются тем, на каком звене они вмешиваются.

**StableMax** заменяет экспоненту в softmax на «мягкую» рамп-функцию, которая при
положительном аргументе растёт линейно, а не экспоненциально, и медленнее убывает
при отрицательном; поэтому суммирование не порождает экстремальных слагаемых, и
потеря вычисляется устойчиво даже при неограниченно растущих логитах
\[[1.1](#ref-1-1)\]. Показательно, что StableMax даёт гроккинг, **пока норма весов
растёт** (Fig. 4), — то есть отвязывает гроккинг от привычного падения нормы весов и
показывает, что уменьшение нормы весов для гроккинга не обязательно
\[[1.1](#ref-1-1)\]. **Повышение точности** до float64 действует так же по духу:
оно лишь **отодвигает** порог ошибки поглощения, но не может продлеваться
бесконечно, так что при малых наборах данных не спасает \[[1.1](#ref-1-1)\].
**⊥Grad** («орто-град») бьёт не по симптому, а по причине: он оставляет в шаге
только ту часть градиента, что ортогональна текущему направлению весов (то есть
вырезает NLM-направление), и потому приводит к генерализации вообще без начальной
фазы переобучения, там где без регуляризации улучшения на тесте обычно нет
\[[1.1](#ref-1-1)\].

Присоединившиеся работы по-разному оценивают эти средства. Yıldırım возражает, что
StableMax лишь **терпит** неограниченное масштабирование, оставляя модель численно
устойчивой во время роста логитов, тогда как ограниченная по норме сферическая
топология архитектурно **удаляет саму степень свободы**, ответственную за рост
\[[2.1](#ref-2-1)\]. Liu et al. подтверждают приборную часть: приведение выходных
логитов к float64 только на этапе потери убирает численный спайк
([слингшот](slingshot.md)), — но подчёркивают, что точность лечит симптом, а не
причину: рост логитов от повышения разрядности не прекращается
\[[3.2](#ref-3-2)\]. Singh et al. просто **берут StableMax на вооружение** и
сообщают, что он уже сам по себе ускоряет наступление гроккинга
\[[3.1](#ref-3-1)\].

## Альтернативные определения и нюансы

### A. Fix по симптому (устойчивое вычисление softmax)

Трактовка через устранение численного сбоя: StableMax и повышение точности не
трогают динамику весов, а делают так, чтобы вычисление softmax/потери оставалось
корректным при сколь угодно больших логитах \[[1.1](#ref-1-1)\]. Источник различия:
средство фиксируется по **симптому** (арифметика softmax), логиты при этом
по-прежнему растут — устраняется лишь их численное последствие.

### B. Fix по причине (удаление NLM-направления)

Трактовка через устранение порождающего механизма: ⊥Grad проецирует градиент на
гиперплоскость, ортогональную вектору весов, вырезая направление, которое только
масштабирует логиты; в результате патология не возникает и гроккинг наступает без
предшествующего переобучения \[[1.1](#ref-1-1)\]. Источник различия: вмешательство
привязано к **причине** (NLM-компонента градиента), а не к моменту численного сбоя.

### Оспаривают

- **Fix как архитектурное удаление степени свободы** \[[2.1](#ref-2-1)\]: Yıldırım
  считает, что StableMax лишь позволяет модели оставаться численно устойчивой во
  время неограниченного масштабирования, тогда как правильнее убрать саму радиальную
  степень свободы остаточного потока (bounded [spherical](spherical-weight-norm-constraint.md) topology). Источник различия:
  численную стабилизацию трактуют как полумеру, оставляющую первопричину (рост
  величин) нетронутой.

### Поддерживают

- **Fix как готовый инструмент ускорения** \[[3.1](#ref-3-1)\]: Singh et al.
  применяют StableMax Cross Entropy как рабочее средство и наблюдают, что оно уже
  само даёт ускорение гроккинга. Источник различия: средство берётся утилитарно, без
  переоценки механизма.
- **Fix точностью как симптоматическое средство** \[[3.2](#ref-3-2)\]: Liu et al.
  подтверждают, что приведение логитов к float64 устраняет численный спайк, но
  показывают, что повышение разрядности не останавливает рост логитов. Источник
  различия: точностный вариант fix признаётся действенным против симптома и
  недостаточным против причины.

## Ссылки

###### ref-1-1
**\[1.1\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». [`"stable version of Softmax (StableMax), cause grokking in settings where it was previously absent without regularization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-4). *«[устойчивая версия софтмакса ($\mathrm{StableMax}$) — вызывают гроккинг в постановках, где ранее без регуляризации он отсутствовал](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-4)»*\
Доп. (StableMax, механизм): [`"we propose using a softer version of Softmax to transform logits into probabilities before calculating the CE Loss"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-4) — *«[мы предлагаем использовать более мягкую версию $\mathrm{Softmax}$ для превращения логитов в вероятности перед вычислением CE-потери](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-4)»*.\
Доп. (StableMax, форма): [`"a simple ramp function that scales linearly instead of exponentially"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-7) — *«[простая линейно-кусочная функция, растущая линейно, а не экспоненциально](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-7)»*.\
Доп. (повышение точности): [`"The simplest way to avoid SC is to extend the FP precision from float32 to float64 for the Softmax calculation"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-3) — *«[Простейший способ избежать SC — расширить FP-точность с $\mathrm{float32}$ до $\mathrm{float64}$ при вычислении $\mathrm{Softmax}$](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-3)»*.\
Доп. (предел точности): [`"FP precision cannot be extended indefinitely to allow for generalization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-3) — *«[FP-точность нельзя расширять бесконечно ради генерализации](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-3)»*.\
Доп. (⊥Grad, правило): [`"the part of the gradient that is orthogonal to the current direction of the weights"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-6) — *«[той части градиента, что ортогональна текущему направлению весов](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-6)»*.\
Доп. (⊥Grad, результат): [`"lead to generalization without a phase of initial overfitting, in contexts where no improvement in test performance is usually observed without weight decay"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-11) — *«[ведут к генерализации без фазы начального переобучения — в условиях, где без weight decay улучшения тестового качества обычно не наблюдается](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-11)»*.\
Доп. (отвязка от нормы весов): [`"this happens while the norm of the weights increases substantially"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-10) — *«[при этом норма весов существенно растёт](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-10)»*.

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2603.05228 — Yıldırım, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Оспаривает: StableMax лишь допускает численно устойчивое масштабирование, а не устраняет его; правильнее убрать саму степень свободы архитектурно. [`"While recent work proposes numerical stabilizations (e.g., StableMax) that allow models to remain numerically stable during this unconstrained scaling phase"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p10-2). *«[Тогда как недавние работы предлагают вычислительные стабилизации (например, StableMax), позволяющие моделям оставаться вычислительно устойчивыми в этой фазе неограниченного масштабирования](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p10-2)»*\
Доп. (альтернатива): [`"our bounded spherical topology instead removes this internal magnitude degree of freedom at the architectural level"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p10-2) — *«[наша ограниченная сферическая топология вместо этого изымает эту внутреннюю степень свободы по величине на уровне архитектуры](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p10-2)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». Нюанс: StableMax берётся как готовое средство и ускоряет наступление гроккинга. [`"We utilize StableMax Cross Entropy [27] since cross entropy with softmax function causes numerical instability"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p3-3). *«[Мы берём перекрёстную энтропию со StableMax (Prieto et al., 2025), поскольку перекрёстная энтропия с функцией softmax вызывает численную неустойчивость](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p3-3)»*\
Доп. (ускорение): [`"We observe that the usage of StableMax [27] already gives a prior speedup in inducing grokking"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p3-5) — *«[Мы замечаем, что применение StableMax (Prieto et al., 2025) уже само по себе ускоряет наступление гроккинга](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p3-5)»*.

###### ref-3-2
**\[3.2\]** 2605.06152 — Liu, Cao, Li, Zhou 2026, «Grokking or Glitching? How Low-Precision Drives Slingshot Loss Spikes». Нюанс: правка через точность устраняет численный всплеск, но у языковой модели роста логитов не убирает. [`"Even when model parameters are stored in float32, casting the output logits to float64 solely during the loss computation is sufficient to eliminate the Slingshot effect"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p3-4). *«[Даже когда параметры модели хранятся в float32, довольно перевести выходные логиты в float64 лишь на время вычисления потери, чтобы пращевое действие исчезло](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p3-4)»*\
Доп. (обратный случай): [`"higher precision does not reduce logit growth in this setting. After $10^{5}$ steps, the mean logit is $183$ under float32 training, but increases to $498$ when the loss is computed in float64"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p9-4) — *«[более высокая точность здесь роста логитов не уменьшает. После $10^{5}$ шагов средний логит при обучении с float32 равен $183$, а при вычислении потери в float64 возрастает до $498$](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p9-4)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». [`"and of numerical precision (Prieto et al., 2025), which may significantly impact grokking in certain scenarios"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-3). *«[и численной точности (Prieto et al., 2025), которая в некоторых постановках способна существенно повлиять на гроккинг](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-3)»*

**\[4.2\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». [`"(Prieto et al., 2025) uses stable softmax (linear form) rather than regular softmax (exponential form) in computing probability"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p11-8). *«[(Prieto et al., 2025) применяет устойчивый софтмакс (линейного вида) вместо обычного (экспоненциального) при вычислении вероятности](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p11-8)»*

**\[4.3\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». [`"Prieto et al. (2025) argue that operating near the edge of numerical stability can induce grokking-like delays and propose remedies that restore or accelerate test performance"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p4-1). *«[Prieto et al. (2025) доказывают, что работа близ края численной устойчивости способна вызывать задержки гроккингового вида, и предлагают средства, восстанавливающие или ускоряющие качество на тесте](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p4-1)»*

**\[4.4\]** 2603.15492 — Acharya, Dhakal, «Grokking as a Variance-Limited Phase Transition: Spectral Gating and the Epsilon-Stability Threshold». Нюанс: параметр устойчивости $\epsilon$ поставлен управляющим: гроккинг живёт в узкой полосе около уровня [шума градиента](gradient-noise.md). [`"Grokking occurs when $\epsilon$ is balanced"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p3-11) — *«[Гроккинг наступает, когда $\epsilon$ уравновешен](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p3-11)»*

**\[4.5\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». Нюанс: знаменатель меры моносемантичности рвёт градиенты, что снимается логарифмированием. [`"we discovered that the denominator term $S^{2}$ could become extremely small, leading to unstable gradients"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p7-5) — *«[мы обнаружили, что знаменатель $S^{2}$ может становиться крайне малым, отчего градиенты делаются неустойчивыми](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p7-5)»*

**\[4.6\]** 2504.16041 — Tveit, Remseth, Skogvold, «Muon Optimizer Accelerates Grokking». Нюанс: план опыта был двухфакторным (2 приёма x 3 варианта softmax) и позволял измерить, не перекрывается ли выигрыш Muon с починкой численной устойчивости, но сообщён только главный эффект приёма — о том, что дали stablemax и sparsemax, не сказано ни слова; ⊥Grad не испытан. [`"we also explored the potential influence of the softmax activation function, motivated by research suggesting numerical stability issues in standard softmax can affect grokking [4]"`](../papers/2504.16041.muon-optimizer-accelerates-grokking/original/2504.16041.muon-optimizer-accelerates-grokking.md#p3-1). *«[мы исследовали также возможное влияние функции активации softmax, будучи движимы работами, указывающими на то, что затруднения численной устойчивости в обычном softmax могут сказываться на гроккинге [4]](../papers/2504.16041.muon-optimizer-accelerates-grokking/2504.16041.muon-optimizer-accelerates-grokking.card.md#p3-1)»*\
Доп.: [`"A variant using a piecewise transformation designed to enhance numerical stability"`](../papers/2504.16041.muon-optimizer-accelerates-grokking/original/2504.16041.muon-optimizer-accelerates-grokking.md#p3-2) — *«[вариант, употребляющий кусочное преобразование, устроенное так, чтобы повысить численную устойчивость](../papers/2504.16041.muon-optimizer-accelerates-grokking/2504.16041.muon-optimizer-accelerates-grokking.card.md#p3-2)»*.

**\[4.7\]** 2505.15624 — AlQuabeh, Bojković, Nwadike, Inui, «Mechanistic Insights into Grokking from the Embedding Layer». Лишь упоминает: работа Prieto et al. названа связывающей отложенную генерализацию с численной неустойчивостью и объявлена дополняющей — там численная неустойчивость softmax, здесь несоразмерность обновлений. Нюанс: ни StableMax, ни $\perp$Grad, ни само явление коллапса softmax в работе не воспроизводятся и не проверяются; общий мотив «гроккинг как беда оптимизации» остаётся заявленным. [`"Prieto et al. (prieto2025grokking) connect delayed generalization to numerical instability (Softmax Collapse), proposing solutions that complement our focus on structural coupling and gradient imbalance."`](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/original/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.md#p3-1). *«[Prieto et al. (prieto2025grokking) связывают отложенную генерализацию с численной неустойчивостью (коллапс Softmax), предлагая решения, дополняющие наше внимание к устройственной связке и несоразмерности градиентов.](../papers/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer/2505.15624.mechanistic-insights-into-grokking-from-the-embedding-layer.card.md#p3-1)»*.

**\[4.8\]** 2506.23286 — Jeffares & van der Schaar, «Not All Explanations for Deep Learning Phenomena Are Equally Valuable». Нюанс: ни разбора механизма, ни оценки области применимости замены; в том же абзаце Prieto et al. приписан и чужой результат об отложенной состязательной устойчивости. [`"On the methodological front, grokking encouraged Prieto et al. 2025 to highlight numerical instabilities in the *Softmax* function and develop a more stable alternative"`](../papers/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable/original/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable.md#p4-2). *«[По методической части гроккинг подтолкнул Prieto et al. 2025 обратить внимание на численные неустойчивости функции *Softmax* и разработать более устойчивую замену](../papers/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable.card.md#p4-2)»*

```
concept:
  category: 5                    # 5. Интервенции и методы (Interventions & methods)
  papers_linked: 12             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-20
```
