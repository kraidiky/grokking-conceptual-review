# Ортогональность градиента (orthogonal gradient / perp-Grad)

[Частотный принцип / спектральное смещение](frequency-principle-spectral-bias.md) ← предыдущая карточка, следующая → [generalization-circuit](generalization-circuit.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Ортогональность градиента** — свойство и одноимённый приём, при которых из
градиента функции потерь берётся только компонента, ортогональная некоторому
выделенному направлению (направлению вектора весов, оно же направление
[«наивной минимизации потерь» (NLM)](nlm.md)) или ортогональная многообразию
нулевой потери (множеству параметров с нулевой обучающей ошибкой). В контексте
[гроккинга](grokking.md) это та компонента, движение вдоль которой реально
меняет предсказания модели и ведёт к обобщению, тогда как параллельная
(не-ортогональная) компонента лишь масштабирует логиты (ненормированные выходы
сети перед softmax), не меняя предсказаний. Понятие в явном виде введено
Prieto et al. (2025) как [оптимизатор](optimizer-adam-adamw-sgd.md) ⊥Grad (perp-Grad), сохраняющий только
ортогональную к направлению весов часть градиента \[[1.1](#ref-1-1)\], и
формализовано Musat (2025) как теорема о том, что вблизи многообразия нулевой
потери градиент становится ортогонален касательным направлениям
\[[1.2](#ref-1-2)\].

![Оптимизаторы с ортогонализацией градиента против базовых: удаление NLM-компоненты включает генерализацию без регуляризации (рис. 6 Prieto et al.)](assets/perp-grad-comparison.png)

## Детализация

Prieto et al. связывают ортогональность градиента с механизмом задержки
генерализации. За точкой переобучения градиент кросс-энтропии всё сильнее
выравнивается с направлением, которое они называют
наивной минимизацией потерь (NLM), — масштабированием логитов
(домножением на константу): это снижает потерю, не меняя предсказаний, но в
пределе приводит к [коллапсу softmax](softmax-collapse.md) (переполнению и
поглощению значений в арифметике с плавающей точкой, останавливающему
обучение). Оптимизатор ⊥Grad проецирует градиент на гиперплоскость,
ортогональную текущему вектору весов, тем самым удаляя NLM-компоненту, и ведёт
к быстрой генерализации без начальной [фазы запоминания](memorization-phase.md)
\[[1.1](#ref-1-1)\]. Musat приходит к ортогональности с другой стороны: в
пост-меморизационной фазе, при малых [скоростях обучения](learning-rate.md) и [weight decay](weight-decay.md)
(затухании весов — L2-регуляризации), обучение сводится к минимизации нормы
весов на [многообразии нулевой потери](weight-norm-minimization.md), а градиент там становится всё более
ортогонален касательным к этому многообразию \[[1.2](#ref-1-2)\]. Xu усиливает
причинную сторону: каузальные интервенции (прямое подавление движения вдоль
низкоразмерного «execution manifold» — подпространства весов, по которому идёт
обучение) показывают, что ортогональный поток градиента необходим, но не
достаточен для гроккинга \[[3.1](#ref-3-1)\]. Так три работы образуют спектр:
⊥Grad — управляющее вмешательство, теорема Musat — эмергентная геометрия,
интервенции Xu — причинная проверка. Это связывает понятие с более широким
кругом трактовок гроккинга — как [фазового перехода](phase-transition.md) и как
смены доминирующего решения.

## Альтернативные определения и нюансы

### A. ⊥Grad: удаление NLM-компоненты (операциональная трактовка)

Prieto et al. определяют ортогональность градиента операционально — через
правило обновления θ_{t+1} = θ_t − η∇⊥L, где ∇⊥L есть проекция градиента на
гиперплоскость, ортогональную текущему вектору весов. Отличительный параметр
здесь — что именно вычитается: параллельная весам компонента, которая у
однородных сетей (сетей, для которых f(αθ) кратно f(θ)) тождественна
направлению NLM. Удаляя её, ⊥Grad предотвращает бесконтрольный рост логитов и
приводит к обобщению без фазы переобучения, определяющей гроккинг
\[[1.1](#ref-1-1)\]. Ключевое отличие этой трактовки: ортогональность — это
управляющий рычаг (интервенция внутри оптимизатора), а не наблюдаемое свойство
динамики.

### B. Ортогональность к многообразию нулевой потери (геометрическая трактовка)

Musat определяет ортогональность градиента как эмергентное геометрическое
свойство: по мере приближения траектории к множеству нулевой потери градиент
становится ортогонален любому касательному направлению этого множества (с
точностью до константы). Управляющий параметр здесь — не оптимизатор, а
расстояние до многообразия: чем ближе траектория, тем полнее ортогональность, и
оставшееся движение сводится к минимизации нормы весов вдоль многообразия
\[[1.2](#ref-1-2)\]. Отличие от трактовки A: ортогональность не навязывается
проекцией, а сама возникает в пост-меморизационной динамике под действием
weight decay; ⊥Grad фактически принудительно воспроизводит то, к чему эта
геометрия приходит естественно.

### Поддерживают

Xu присоединяется к трактовке ортогонального потока градиента как драйвера
гроккинга, но уточняет её каузально. Серия интервенций с монотонной
зависимостью «доза–отклик» (постепенное подавление движения вдоль execution
manifold) показывает, что ортогональный поток градиента **необходим** — его
подавление блокирует генерализацию, — но **не достаточен**: искусственное
усиление кривизны потерь само по себе гроккинг не вызывает \[[3.1](#ref-3-1)\].
Отличие от трактовок A и B: это не новое определение, а причинная граница —
понятие получает статус необходимого условия гроккинга, а не просто
корреляционного признака.

## Ссылки

###### ref-1-1
**\[1.1\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical
Stability». [`"only preserves the part of the gradient that is orthogonal to the NLM direction"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-2). *«[сохраняет лишь ту часть градиента, что ортогональна направлению NLM](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-2)»*\
Доп.: [`"the part of the gradient that is orthogonal to the current direction of the weights"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-6) — *«[той части градиента, что ортогональна текущему направлению весов](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-6)»*; [`"that directly prevents scaling along the NLM direction"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-11) — *«[прямо предотвращающее масштабирование вдоль направления NLM](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p8-11)»*.

###### ref-1-2
**\[1.2\]** 2511.01938 — Musat, «The Geometry of Grokking: Norm Minimization on
the Zero-Loss Manifold». [`"the loss gradients become perfectly orthogonal to the zero-loss set as we approach it"`](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/original/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.md#p4-3). *«[по мере приближения градиенты потери становятся в точности ортогональны множеству нулевой потери](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.card.md#p4-3)»*\
Доп.: [`"effectively minimizes the weight norm on the zero-loss manifold"`](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/original/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.md#p1-2) — *«[по сути минимизирует норму весов на многообразии нулевой потери](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.card.md#p1-2)»*.

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved
Optimization Dynamics in Grokking». Нюанс: ортогональный поток градиента
причинно необходим, но не достаточен для гроккинга. [`"orthogonal gradient flow is necessary but not sufficient for grokking"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2). *«[ортогональный поток градиента необходим, но не достаточен для гроккинга](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p1-2)»*\
Доп.: [`"that orthogonal gradient flow is causally necessary for generalization"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p13-3) — *«[что ортогональный поток градиента причинно необходим для генерализации](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p13-3)»*; [`"suppressing orthogonal gradient flow prevents grokking with a monotonic dose–response across four operations"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p21-1) — *«[подавление ортогонального потока градиента предотвращает гроккинг с монотонным откликом на дозу по четырём операциям](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p21-1)»*.


###### ref-3-2
**\[3.2\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Нюанс: продолжение линии \[3.1\] в многозадачной постановке, с количественной оценкой — причинно необходимая доля градиента составляет менее 1 % его дисперсии, и удаление 10 % ортогональной составляющей убивает гроккинг и при двух, и при трёх задачах. В отличие от \[3.1\], контроля на случайное подпространство для меры $\rho$ здесь нет. [`"These results demonstrate that grokking depends critically on rare transverse updates that constitute $<1\%$ of gradient variance."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p26-7). *«[Эти результаты показывают, что гроккинг решающе зависит от редких поперечных обновлений, составляющих $<1\%$ дисперсии градиента](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p26-7)»*\
Доп. (та же оценка в трёхзадачной постановке): [`"This establishes that $<1\%$ of the gradient variance (orthogonal to the PCA manifold) is causally necessary for grokking to occur."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p15-1) — *«[Это устанавливает, что $<1\%$ дисперсии градиента (ортогональной многообразию PCA) причинно необходим для того, чтобы гроккинг произошёл](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p15-1)»*.

###### ref-3-3
**\[3.3\]** 2602.16967 — Xu, «Early-Warning Signals of Grokking via
Loss-Landscape Geometry». Нюанс: необходимость ортогонального потока
подтверждена на трёх семействах задач из трёх (модульная арифметика, SCAN,
Dyck); на SCAN мягкое подавление штрафом предотвращает гроккинг вовсе, а
жёсткое проецированием лишь задерживает. [`"across all three tasks, suppression of orthogonal gradient flow delays or prevents grokking, establishing the *necessity* of transverse curvature dynamics as a universal finding"`](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/original/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.md#p13-6). *«[во всех трёх задачах подавление ортогонального потока градиента задерживает или предотвращает гроккинг, устанавливая *необходимость* динамики поперечной кривизны как универсальную находку](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.card.md#p13-6)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2512.03437 — Liang & Li, «Grokked Models are Better Unlearners». Нюанс: измерены косинусная близость градиентов забываемого и сохраняемого наборов (0,999 до гроккинга против 0,426 после) и соответствующий угол (2,57 против 64,78 градуса). [`"Grokked models show substantially lower correlations (0.521 for CNN, 0.426 for ResNet), indicating that grokking creates more orthogonal gradient spaces"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p9-5). *«[Гроккнувшие модели дают заметно меньшую корреляцию (0,521 у CNN, 0,426 у ResNet), то есть гроккинг создаёт более ортогональные градиентные пространства](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p9-5)»*

**\[4.2\]** 2504.16041 — Tveit, Remseth, Skogvold, «Muon Optimizer Accelerates Grokking». Нюанс, важный для разграничения: у Muon ортогонализуется матрица обновления (итерацией Ньютона—Шульца по буферу момента), а не снимается компонента градиента вдоль направления весов, как в ⊥Grad Prieto et al.; это разные операции с разными целями. Формулы обновления Muon, числа итераций Ньютона—Шульца и отсечения этой операции в работе нет, так что проверить, что дело именно в ортогонализации, здесь нечем. [`"Uses orthogonalized gradient updates"`](../papers/2504.16041.muon-optimizer-accelerates-grokking/original/2504.16041.muon-optimizer-accelerates-grokking.md#fig-1). *«[Употребляет ортогонализованные градиентные обновления](../papers/2504.16041.muon-optimizer-accelerates-grokking/2504.16041.muon-optimizer-accelerates-grokking.card.md#fig-1)»*
