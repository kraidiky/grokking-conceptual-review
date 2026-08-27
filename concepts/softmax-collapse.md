# Коллапс softmax (Softmax Collapse)

[Двойной спуск](double-descent.md) ← предыдущая карточка, следующая → [Катастрофическое забывание](catastrophic-forgetting.md)

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

**Softmax Collapse (SC, «коллапс softmax»)** — численная нестабильность, при
которой обучение без регуляризации выталкивает модель на «край численной
стабильности», и в функции softmax появляются ошибки арифметики с плавающей
точкой; SC **останавливает обучение** и потому препятствует
[гроккингу](grokking.md) \[[1.1](#ref-1-1)\]. Понятие введено Prieto et al.
(2025).

![Искусственно вызванный коллапс softmax действует как естественный: модель, которая обобщила бы (зелёная), перестаёт учиться (рис. 8 Prieto et al.)](assets/softmax-collapse-induced.png)

## Детализация

Механизм SC — в **[наивной минимизации потери](nlm.md)** (naïve loss minimization, NLM). За
точкой переобучения, когда обучающие примеры уже классифицированы верно, градиент
почти целиком выравнивается с направлением, которое **не меняет предсказаний**, а
снижает потерю перекрёстной энтропии, просто **масштабируя логиты** (предсофтмаксные
выходы сети) вверх — обычно через рост норм весов вдоль текущего направления
\[[1.1](#ref-1-1)\]. Неограниченный рост логитов раздувает разрыв между
максимальным и остальными логитами; когда он превышает порог, определяемый
точностью арифметики с плавающей точкой, softmax насыщается, и вычисление даёт
нулевые или искажённые градиенты — обучение встаёт уже после
[фазы запоминания](memorization-phase.md). Именно это затянутое масштабирование
логитов, по Prieto et al., и **объясняет отложенность генерализации**,
характерную для гроккинга: пока SC не смягчён, генерализация не наступает
\[[1.1](#ref-1-1)\].

SC привязывает объяснение гроккинга к **численной устойчивости**: устранение
коллапса даёт генерализацию без регуляризации. Prieto et al. предлагают два
средства — **[StableMax](numerical-stability-fix.md)** (замена softmax на активацию, не насыщающуюся при
больших логитах) и **[⊥Grad](orthogonal-gradient-perp-grad.md)** (обучение, из которого удалена NLM-компонента
градиента) \[[1.1](#ref-1-1)\]. Присоединившиеся работы уточняют разные стороны
SC. Одни берут рост логитов за первопричину и проектируют **ограниченный по норме
выходной слой** (bounded unembedding), заранее исключающий коллапс
\[[3.1](#ref-3-1)\]. Другие раскрывают сам численный механизм: SC наступает,
когда разрыв между наибольшим и прочими логитами превышает floating-point-порог,
и **ошибка поглощения** (absorption error — малое слагаемое теряется при сложении
с много большим) делает вычисленный softmax отличным от точного
\[[3.2](#ref-3-2)\].

Отдельно проверяли, снимается ли SC простым **повышением точности** арифметики.
Приведение softmax и потери к `float64` (double) **отодвигает** коллапс: при float64
SC наступает позже, чем при float32, и модель успевает добрать тестовую точность
\[[1.1](#ref-1-1)\]; приведение к float64 одних лишь логитов и потери и вовсе
**устраняет** спайки на MLP/CNN/ViT, что подтверждает численную, а не оптимизационную
природу явления \[[3.2](#ref-3-2)\]. Но это лечит **симптом, а не причину**: порог
ошибки поглощения для double во много раз выше, тогда как сам рост логитов (NLM)
продолжается — на длинном обучении языковой модели повышение точности рост логитов
не остановило (средний логит даже вырос со 183 до 498) \[[3.2](#ref-3-2)\]. Поэтому
«настоящими» средствами остаются StableMax/⊥Grad и ограниченный выходной слой, а не
увеличение разрядности \[[1.1](#ref-1-1)\].

## Альтернативные определения и нюансы

### A. SC по симптому (сбой softmax)

Определение через наблюдаемый эффект: SC — это floating-point-ошибки в softmax,
которые останавливают обучение за точкой переобучения \[[1.1](#ref-1-1)\].
Источник различия: понятие фиксируется по **симптому** (сбой в вычислении
softmax), безотносительно к тому, что его вызвало.

### B. SC по причине (неограниченный рост логитов)

Определение через порождающий механизм: NLM гонит логиты вверх, не меняя
предсказаний, и рано или поздно это приводит к коллапсу
\[[1.1](#ref-1-1)\]\[[3.1](#ref-3-1)\]. Источник различия: понятие привязано к
**причине** (уход весов в чистое масштабирование логитов), а не к моменту сбоя.

### Поддерживают

- **SC как обходимое проектированием препятствие** \[[3.1](#ref-3-1)\]:
  неограниченный рост логитов берут за первопричину и заранее ограничивают норму
  выходного слоя, чтобы коллапс не наступал. Источник различия: SC трактуется как
  инженерно устранимый дефект выходной геометрии.
- **SC как эффект конечной точности** \[[3.2](#ref-3-2)\]: коллапс объясняют
  пределами арифметики с плавающей точкой (absorption error при большом разрыве
  логитов). Источник различия: SC — не абстрактная нестабильность, а конкретный
  численный артефакт, зависящий от разрядности.

## Ссылки

###### ref-1-1
**\[1.1\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». [`"grokking tasks push models to the edge of numerical stability, introducing floating point errors in the Softmax that we refer to as Softmax Collapse"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2). *«[задачи, на которых наблюдается гроккинг, подводят модели к границе численной устойчивости, внося в Softmax ошибки плавающей точки, которые мы называем коллапсом софтмакса (Softmax Collapse, SC)](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2)»*\
Доп. (следствие): [`"We show that SC prevents grokking and that mitigating SC leads to grokking without regularization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2) — *«[Мы показываем, что SC препятствует гроккингу и что устранение SC ведёт к гроккингу без регуляризации](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2)»*.\
Доп. (механизм): [`"the gradient becomes aligned with a direction that corresponds to scaling up the logits by a constant"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-1) — *«[градиент согласуется с направлением, отвечающим увеличению логитов на постоянный множитель](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-1)»*.\
Доп. (точность): [`"networks trained using $\mathrm{float64}$ in the $\mathrm{Softmax}$ face SC later in training which allows for a further increase in test performance"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-3) — *«[сети, обучаемые с $\mathrm{float64}$ в $\mathrm{Softmax}$, сталкиваются с SC позже в обучении, что позволяет тестовому качеству вырасти сильнее](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p5-3)»*.

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.05228 — Yıldırım, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Нюанс: рост логитов берётся за первопричину SC и мотивирует ограниченный по норме выходной слой. [`"unconstrained logit growth leads to Softmax Collapse, motivating our bounded unembedding design"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p3-4). *«[неограниченный рост логитов ведёт к коллапсу softmax, что и побудило наше устройство ограниченного обратного вложения](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p3-4)»*

###### ref-3-2
**\[3.2\]** 2605.06152 — Liu, Cao, Li, Zhou 2026, «Grokking or Glitching? How Low-Precision Drives Slingshot Loss Spikes». Нюанс: SC раскрывается как действие конечной точности (ошибка поглощения при большом разрыве логитов), и у него найдено второе следствие — нарушение условия нулевой суммы по родам. [`"when the gap between the largest logit $z_{m}$ and the other logits exceeds a threshold determined by floating-point precision, absorption error causes the computed result to differ from the exact real-number value"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-5). *«[когда разрыв между наибольшим логитом $z_{m}$ и прочими логитами превосходит порог, задаваемый точностью с плавающей точкой, ошибка поглощения приводит к расхождению вычисленного значения с точным вещественным](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p1-5)»*\
Доп. (устранение): [`"casting the output logits to float64 solely during the loss computation is sufficient to eliminate the Slingshot effect"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p3-4) — *«[довольно перевести выходные логиты в float64 лишь на время вычисления потери, чтобы пращевое действие исчезло](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p3-4)»*.\
Доп. (встречный случай): [`"higher precision does not reduce logit growth in this setting. After $10^{5}$ steps, the mean logit is $183$ under float32 training, but increases to $498$ when the loss is computed in float64"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p9-4) — *«[более высокая точность здесь роста логитов не уменьшает. После $10^{5}$ шагов средний логит при обучении с float32 равен $183$, а при вычислении потери в float64 возрастает до $498$](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.card.md#p9-4)»*.

###### ref-3-3
**\[3.3\]** 2602.12039 — Beck, Bar-Sinai & Levi 2026, «The Implicit Bias of Logit Regularization». Заявленное продолжение наблюдения Prieto et al. о логит-регуляризации — но механизм здесь иной и несовместимый, чего статья не проговаривает: у Prieto et al. штраф лишь удерживает логиты от расхождения до обнуления градиента конечной арифметикой, здесь арифметика точная, а задержку создаёт сам штраф; о разрядности вычислений базовой линии не сказано ни слова. [`"Logit regularization in this context was briefly discussed in Prieto et al. 2025; we identify an analogous delayed generalization region with a shifted threshold"`](../papers/2602.12039.the-implicit-bias-of-logit-regularization/original/2602.12039.the-implicit-bias-of-logit-regularization.md#p12-5). *«[Логит-регуляризация в этом контексте кратко обсуждалась в Prieto et al. 2025; мы обнаруживаем аналогичную область отложенной генерализации со сдвинутым порогом](../papers/2602.12039.the-implicit-bias-of-logit-regularization/2602.12039.the-implicit-bias-of-logit-regularization.card.md#p12-5)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2506.05718 — Notsawo et al., «Grokking Beyond the Euclidean Norm of Model Parameters». [`"Prieto et al. (2025) explain grokking as a result of Softmax Collapse—numerical instability from floating-point errors that halts learning after memorization"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p16-2). *«[Prieto et al. (2025) объясняют гроккинг обвалом softmax — численным сбоем из-за ошибок в числах с плавающей запятой, останавливающим обучение после запоминания](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.card.md#p16-2)»*

**\[4.2\]** 2601.19791 — Xu et al., «To Grok Grokking: Provable Grokking in Ridge Regression». [`"the dependence of grokking on regularization by showing that Softmax Collapse (i.e. floating point errors due to numerical instability) is responsible for the absence of grokking without regularization"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p3-2). *«[зависимость гроккинга от упорядочения, обнаружив, что за отсутствие гроккинга без упорядочения отвечает схлопывание softmax (то есть ошибки с плавающей точкой из-за численной неустойчивости)](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.card.md#p3-2)»*

**\[4.3\]** 2603.15492 — Acharya et al., «Grokking as a Variance-Limited Phase Transition: Spectral Gating and the Epsilon-Stability Threshold». [`"Prieto et al. [21] suggest that numerical instability (like Softmax Collapse) prevents learning"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p10-4). *«[Prieto et al. [21] полагают, что числовая неустойчивость (каков обвал softmax) обучению мешает](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.card.md#p10-4)»*

**\[4.4\]** 2504.16041 — Tveit, Remseth, Skogvold, «Muon Optimizer Accelerates Grokking». Нюанс: связь Muon с уходом от коллапса softmax заявлена строкой таблицы гипотез во введении и после опыта никак не проверена — ни логитов, ни признаков поглощения при сложении с плавающей точкой, ни сравнения обычного softmax со stablemax под Muon в работе не измерено. [`"Keeps training stable and avoids “softmax collapse”"`](../papers/2504.16041.muon-optimizer-accelerates-grokking/original/2504.16041.muon-optimizer-accelerates-grokking.md#fig-1). *«[Держит обучение устойчивым и избегает «softmax collapse»](../papers/2504.16041.muon-optimizer-accelerates-grokking/2504.16041.muon-optimizer-accelerates-grokking.card.md#fig-1)»*

**\[4.5\]** 2605.08237 — Wang, Ying, Kanamori 2026, «Distributional Spectral Diagnostics for Localizing Grokking Transitions». Нюанс: логиты в работе появляются как альтернативная наблюдаемая (3/4 тревог, ни одной до onset), но связи с коллапсом softmax не проводится. [`"stability-based accounts link it to logit scaling and softmax collapse [42, 32]"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p30-7). *«[объяснения на основе устойчивости связывают его с масштабированием логитов и коллапсом softmax [42, 32]](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p30-7)»*
