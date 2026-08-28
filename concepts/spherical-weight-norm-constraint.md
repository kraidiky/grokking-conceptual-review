# Сферическое ограничение нормы (spherical weight-norm constraint)

[Замораживание подсети / edge-popup](freezing-subnetwork.md) ← предыдущая карточка, следующая → [accelerated-grokking](accelerated-grokking.md)

[Индекс карточек понятий](index.md), категория: [5. Интервенции и методы](index.md#cat-5)\
→ Следующая категория: [6. Аналитические инструменты и метрики](progress-measures.md)\
← Предыдущая категория: [4. Факторы обучения и оптимизации](weight-decay.md)

## Определение

**Сферическое ограничение нормы** — это интервенция, при которой во время
обучения L2-норма объекта (весов модели или активаций остаточного потока)
удерживается фиксированной, то есть параметры или представления принудительно
кладутся на сферу (гиперсферу) выбранного радиуса; тем самым устраняется
радиальная (масштабная) степень свободы, и информация может кодироваться только
направлением вектора. В наиболее явном виде понятие центрирует Yildirim как
«полностью ограниченную сферическую топологию» (fully bounded spherical
topology), которая обеспечивает L2-нормализацию по всему остаточному потоку и
полностью убирает [фазу запоминания](memorization-phase.md), то есть отложенную
генерализацию ([гроккинг](grokking.md)), на [модульном сложении](modular-arithmetic.md)
\[[1.1](#ref-1-1)\].

![Динамика обучения модели со сферическим ограничением нормы весов (рис. 2 Yildirim)](assets/spherical-constraint-dynamics.png)

## Детализация

Механизм у двух форм ограничения разный, но идея общая: лишить сеть возможности
кодировать информацию в норме. В **весовой** форме (Omnigrok) ограничение
реализуется как проекция — после каждого обычного шага оптимизации веса
пересчитываются обратно к исходной норме, что удерживает их на сфере в
пространстве весов \[[3.1](#ref-3-1)\]. В **репрезентационной** форме (Yildirim)
проекционный оператор применяет строгую L2-нормализацию к остаточному потоку
(residual stream — накопительный вектор-канал, по которому слои трансформера
обмениваются информацией) перед каждым подслоем и после каждого остаточного
сложения, а матрица распроецирования (unembedding — линейный слой, переводящий
скрытый вектор в логиты) тоже нормируется, и логиты масштабируются фиксированной
температурой \[[1.1](#ref-1-1)\].

Связь с гроккингом такова: без ограничения сеть под кросс-энтропийной потерей
склонна раздувать норму логитов ради снижения потери — это так называемая наивная
минимизация потерь ([Naïve Loss Minimization](nlm.md)), ведущая к
[коллапсу softmax](softmax-collapse.md); именно высоконормовые «запоминающие»
решения создают длинное плато перед резким [фазовым переходом](phase-transition.md)
к генерализации. Сферическое ограничение структурно запрещает такие решения: оно
схлопывает разрыв норм между запоминающим и обобщающим решением и потому убирает
или резко сокращает задержку \[[3.2](#ref-3-2)\].

Эффект, однако, не универсален. Yildirim показывает, что на коммутативных задачах
(модульное сложение и умножение) ограничение выравнивает представления с
непрерывной круговой геометрией Фурье-решения (алгоритм «Clock» в
противоположность фрагментарному запоминающему «Pizza», см.
[Clock vs Pizza](clock-vs-pizza.md)) и убирает фазу запоминания полностью, но на
[некоммутативной](group-composition-non-commutative-s5.md) композиции группы S5 та же связь проваливается: успех там опирается
на дискретные структуры смежных классов, а не на непрерывные [Фурье-признаки](fourier-features-circuits.md)
\[[1.1](#ref-1-1)\]. Часть работ оспаривает чистоту эффекта: GrokTransfer отмечает,
что ограничение нормы весов сферой «вносит нестабильность в обучение и всё ещё
включает фазовый переход» \[[2.2](#ref-2-2)\], а эмпирическое исследование за
пределами нейросетей не обнаруживает сферической геометрии «[зоны Златовласки](goldilocks-zone.md)»
(Goldilocks zone — гипотетическая сферическая оболочка в пространстве весов, где
генерализация лучше, чем вне её) и называет требование такой оболочки «слишком строгим»
\[[2.1](#ref-2-1)\]. Поддерживают трактовку исходная находка Omnigrok — «обучение с
ограниченной нормой весов может почти устранить гроккинг» \[[3.1](#ref-3-1)\] — и
[норм-сепарационная](norm-separation-delay-law.md) теория Truong, выводящая, что длина задержки пропорциональна
логарифму отношения норм запоминающего и обобщающего решений и что сферическая
топология действует прямо на этот механизм \[[3.2](#ref-3-2)\].

## Альтернативные определения и нюансы

### A. Ограничение нормы весов (весовое пространство)

Исходная, весовая трактовка (Omnigrok): контролируемый объект — сами параметры
модели, а порядковый параметр — L2-норма весов. Ограничение реализуется как
проекция в пространстве оптимизации: после каждого безограниченного шага веса
масштабируются обратно к целевой норме, так что траектория идёт по сфере
фиксированного радиуса. Различающая машинерия — где живёт сфера: в пространстве
весов, а её радиус соответствует «радиусу Златовласки» (той оболочке, на которой
генерализация лучше). Эта форма прямо манипулирует картиной LU / зоны Златовласки
\[[3.1](#ref-3-1)\], но именно её эмпирическую жёсткость и стабильность оспаривают
\[[2.1](#ref-2-1)\]\[[2.2](#ref-2-2)\].

### B. Ограничение нормы представлений (остаточный поток)

Репрезентационная трактовка Yildirim: контролируемый объект — не вектор параметров,
а активации остаточного потока (плюс матрица распроецирования). Сфера живёт в
пространстве представлений: выход каждого подслоя ренормируется к единичной норме, а
логиты ограничиваются косинусной геометрией (косинусное сходство × температура).
Различающая машинерия — ограничение убирает масштаб как репрезентационную ось, так
что информация становится чисто угловой, и это архитектурная интервенция (до
обучения), а не пересчёт нормы во время оптимизации. Формально ограничивается лишь
активация, а не весь вектор параметров, однако разрыв норм, задающий задержку, всё
равно схлопывается, и фаза запоминания исчезает даже при нулевом [weight decay](weight-decay.md)
\[[1.1](#ref-1-1)\]\[[3.2](#ref-3-2)\].

### Оспаривают

- **Miller et al.** \[[2.1](#ref-2-1)\]: требование сферической «зоны Златовласки»
  слишком жёсткое, а в изученном ими случае гроккинга (гауссовская регрессия) такая
  сферическая геометрия эмпирически не наблюдается — вместо неё более сложная
  геометрия пространства весов.
- **Xu et al. (GrokTransfer)** \[[2.2](#ref-2-2)\]: ограничение нормы весов сферой
  подходящего радиуса действительно ускоряет генерализацию, но привносит
  нестабильность в обучение и не устраняет сам фазовый переход — то есть это не
  чистое решение проблемы задержки.

### Поддерживают

- **Liu et al. (Omnigrok)** \[[3.1](#ref-3-1)\]: обучение с ограниченной нормой
  весов может почти полностью устранить гроккинг — исходное эмпирическое
  подтверждение эффекта в рамках механизма LU / зоны Златовласки.
- **Truong et al.** \[[3.2](#ref-3-2)\]: норм-сепарационный закон задержки
  комплементарен сферической топологии — она механически схлопывает отношение норм
  ‖θ_mem‖/‖θ_post‖, которое по их теории и задаёт длину задержки, что объясняет,
  почему связанная модель генерализует без фазы запоминания.

## Ссылки

###### ref-1-1
**\[1.1\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking:
Bypassing Phase Transitions via Architectural Topology». [`"A fully bounded spherical topology, enforcing L2 normalization throughout the residual stream, eliminates the memorization phase entirely on modular addition"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2). *«[Полностью ограниченная сферическая топология, навязывающая $L_{2}$-нормирование по всему остаточному потоку, полностью устраняет фазу запоминания на модульном сложении](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p1-2)»*\
Доп.: [`"Fully Bounded Spherical Topology that enforces strict L2 normalization throughout the residual stream and applies a normalized unembedding matrix with a fixed temperature scale"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-3) — *«[полностью ограниченную сферическую топологию, навязывающую строгое $L_{2}$-нормирование по всему остаточному потоку и применяющую нормированную матрицу обратного вложения с закреплённым температурным масштабом](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p2-3)»*. [`"By projecting activations onto a fixed-norm hypersphere, we remove magnitude as a representational axis, restricting the model to encode information purely through angular relationships"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p6-3) — *«[Проецируя активации на гиперсферу закреплённой нормы, мы изымаем величину как представленческую ось, вынуждая модель кодировать сведения исключительно угловыми отношениями](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p6-3)»*.

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2310.17247 — Miller, O'Neill, Bui 2024, «Grokking Beyond Neural
Networks: An Empirical Exploration with Model Complexity». Оспаривает: требование сферической
«зоны Златовласки» слишком жёсткое, а опытно такая геометрия не наблюдается.
[`"the requirement of a spherical Goldilocks zone seems too stringent"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p3-1). *«[требование сферической зоны Златовласки представляется чрезмерно строгим](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p3-1)»*\
Доп.: [`"we did not see a clear example of the spherical geometry mentioned in the Goldilocks zone theory of (Liu et al.(2023 a))"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p8-1) — *«[мы не увидели отчётливого примера сферической геометрии, о которой говорит теория зоны Златовласки (Liu et al.(2023 a))](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p8-1)»*.

###### ref-2-2
**\[2.2\]** 2504.13292 — Xu et al., «Let Me Grok For You: Accelerating Grokking
via Embedding Transfer from a Weaker Model». Оспаривает практичность: ограничение
нормы весов сферой ускоряет генерализацию, но привносит нестабильность и не убирает
фазовый переход. [`"restricting the weight norm to a sphere of the appropriate radius during training can accelerate generalization"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p3-1). *«[удержание нормы весов на сфере подходящего радиуса в ходе обучения способно ускорить генерализацию](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p3-1)»*\
Доп.: [`"However, this method introduces instability in the training process and still involves a phase transition"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p3-1) — *«[Однако этот приём вносит в обучение неустойчивость и всё же оставляет фазовый переход](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p3-1)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2210.01117 — Liu et al., «Omnigrok: Grokking Beyond Algorithmic Data».
Нюанс: ограничение нормы весов почти устраняет гроккинг (механизм LU / зоны
Златовласки). [`"training with constrained weight norm can almost eliminate grokking"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p1-9). *«[обучение с ограниченной нормой весов способно почти полностью устранить гроккинг](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/2210.01117.omnigrok-grokking-beyond-algorithmic-data.card.md#p1-9)»*\
Доп.: [`"In practice, we perform the constrained minimization by rescaling the model weights back to their original norm after each unconstrained optimization step"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p2-2) — *«[на практике мы выполняем эту минимизацию с ограничением, перемасштабируя веса модели обратно к их исходной норме после каждого шага оптимизации без ограничений](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/2210.01117.omnigrok-grokking-beyond-algorithmic-data.card.md#p2-2)»*.

###### ref-3-2
**\[3.2\]** 2603.13331 — Truong et al., «The Norm-Separation Delay Law of Grokking:
A First-Principles Theory of Delayed Generalization». Нюанс: норм-сепарационная
теория объясняет, почему сферическая топология убирает задержку — она схлопывает
отношение норм, задающее её длину. [`"FBST acts directly on this mechanism: by mechanically constraining the residual stream and unembedding to a fixed-norm hypersphere, it removes the architectural capacity to construct a high-norm memorisation interpolant"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p29-1). *«[FBST действует прямо на это устройство: механически заключая остаточный поток и обратное вложение на гиперсферу закреплённой нормы, она изымает архитектурную возможность построить запоминающий интерполянт с большой нормой](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p29-1)»*\
Доп.: [`"These results are highly complementary to the theory developed here"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p29-1) — *«[Эти результаты весьма дополняют развиваемую здесь теорию](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p29-1)»*.


###### ref-3-3
**\[3.3\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Даёт корпусу рабочий вид вмешательства: Normalize and Project (Lyle et al. 2024a) — слои нормировки перед каждой нелинейностью плюс периодическая проекция весов каждого линейного слоя на единичную сферу по норме Фробениуса; проекцию можно применять раз в $k$ шагов, что промежуточно между проекцией и регуляризацией нормы. Роль проекции двоякая: она делает ELR прямо считываемой с расписания и она же необходимая составляющая способа (её удаление ухудшает качество в отсечке). Нюанс, важный для карточки: с layer normalization проекция лишает сеть единственного пути снизить норму входов внимания, и обобщение исчезает вовсе, пока не добавлен «scale decay». [`"NaP inserts normalization layers prior to each nonlinearity in the network and periodically projects the weights in each linear layer to the unit sphere (w.r.t. Frobenius norm $\|\cdot\|_{F}$)"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#p3-4). *«[NaP вставляет слои нормировки перед каждой нелинейностью в сети и периодически проецирует веса каждого линейного слоя на единичную сферу (по норме Фробениуса $\|\cdot\|_{F}$)](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#p3-4)»*

###### ref-3-4
**\[3.4\]** 2505.11411 — Zhang, Shang, Yang, Zhang, «Is Grokking a Computational Glass Relaxation?». Пользуется жёстким перемасштабированием весов к норме 30 после каждого шага как инструментом и добавляет к известному следствию (гроккинг почти снимается) новое: при таком ограничении сам энтропийный ландшафт перестаёт доставать до состояний сильного запоминания, то есть до «вычислительного стекла». Нюанс: значение нормы одно, взято у Omnigrok; зависимости преимущества высокой энтропии от нормы работа не строит. [`"after each iteration step, we compute the weight norm of the neural network parameters and scale them proportionally to maintain a fixed norm of 30"`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p7-1). *«[после каждого шага итерации мы вычисляем норму весов параметров нейронной сети и пропорционально масштабируем их, чтобы поддерживать закреплённую норму 30](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p7-1)»*\
Доп. (побочное следствие ограничения): [`"under this restriction the entropy landscape cannot explore severe memorization states, which is the computational glassy state in our analogy"`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p7-2) — *«[при этом ограничении энтропийный ландшафт не способен исследовать состояния сильного запоминания, каковые в нашей аналогии суть вычислительное стеклоподобное состояние](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p7-2)»*.

###### ref-3-5
**\[3.5\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Даёт понятию первую дозовую развёртку: пошаговая проекция всех весов на $\rho\,\|W\|_{c}$ ставится на десяти уровнях $\rho\in\{0.85,0.90,\dots,1.30\}$ по 16 семян, и получается экспонента по удерживаемой норме с $R^{2}=0.994$ на тридцатикратном размахе времён. Отсюда же первая в корпусе отрицательная сверка на сам поступок проецирования: рука $\rho=1.00$ применяет ту же проекцию при естественной норме сети и грокает по свободному расписанию, так что действует значение нормы, а не проекция. Нюанс: зажим действует одним общим множителем и потому закрепляет лишь полную величину — послойные доли под ним дрейфуют так же, как при свободном обучении; непрерывного послойного закрепления не ставили, и о нём ничего не утверждается. [`"The norm *clamp* projects the weights after each step so that $\|W\|=\rho\,\|W\|_{c}$ for a chosen multiple $\rho$, holding the norm at a multiple of the critical value"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-10). *«[*Зажим* нормы проецирует веса после каждого шага так, что $\|W\|=\rho\,\|W\|_{c}$ для выбранного кратного $\rho$, удерживая норму на кратном от критического значения](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-10)»*\
Доп. (сверка на проекцию как таковую): [`"The $\rho=1.00$ arm is the control for this: it applies the identical per-step projection but at the network’s natural norm, and it groks on essentially the free-training timescale (no delay)."`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p12-5) — *«[Рука $\rho=1.00$ и есть сверка для этого: она применяет тождественную пошаговую проекцию, но при естественной норме сети, и грокает по существу на временном масштабе свободного обучения (без задержки).](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p12-5)»*.

###### ref-3-6
**\[3.6\]** 2606.32000 — Tiwari, Chauhan & Singh 2026, «Radial Suppression Accelerates Algorithmic Generalization: A Geometric Analysis of Delayed Generalization». Мягкая родня жёсткой сферической топологии: вместо проекции — штраф-релаксация с целевым радиусом $\sqrt{d}$, в пределе $\lambda\to\infty$ в точности риманов спуск на сфере, при конечном $\lambda$ — оптимизация в «утолщённой сфере»; в таксономии вмешательств жёсткая проекция отнесена к «полному» радиальному подавлению с обрушением ранга, мягкий штраф — к «прямому» с сохранением. Нюанс: сопоставление с жёсткой проекцией опытом не подкреплено — только строка в таксономии, а связь рабочего $\lambda{=}0.05$ с предельным случаем не измерена. [`"The activation norm penalty acts as a continuous Lagrangian relaxation of a Riemannian gradient flow on the hypersphere $\mathbb{S}^{d-1}(\sqrt{d})$, where the penalty multiplier $\lambda$ dictates the stiffness of the manifold retraction."`](../papers/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization.card.md#p8-6). *«[Штраф на норму активаций действует как непрерывная лагранжева релаксация риманова градиентного потока на гиперсфере $\mathbb{S}^{d-1}(\sqrt{d})$, где множитель штрафа $\lambda$ задаёт жёсткость ретракции на многообразие](../papers/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization.card.md#p8-6)»*\
Доп. (жёсткая родня): [`"Yıldırım (2026) enforce hard $L_{2}$ projection onto a bounded spherical topology. Our approach provides a *soft*, *loss-based* variant operating in *activation* space."`](../papers/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization.card.md#p2-6). *«[Yıldırım (2026) навязывает жёсткую $L_{2}$-проекцию на ограниченную сферическую топологию. Наш подход даёт *мягкий*, *встроенный в потерю* вариант, работающий в пространстве *активаций*](../papers/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization/2606.32000.radial-suppression-accelerates-algorithmic-generalization-a-geometric-analysis-of-delayed-generalization.card.md#p2-6)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2405.19454 — Fan, Pascanu, Jaggi 2024, «Deep Grokking: Would Deep Neural Networks Generalize Better?». [`"Liu et al. (2023) presents a hypothesis of *grokking* from large initialization based on the *Godilocks Zone* theory: there exist a thick, hollow, spherical shell in the space of model’s weight around the initialization which could lead to a great generalization behavior"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p3-6). *«[Liu et al. (2023) предлагают догадку о *гроккинге* из большого начального приближения на основе теории *зоны Златовласки*: в пространстве весов модели вокруг начального приближения существует толстая полая сферическая оболочка, способная давать прекрасное поведение при генерализации](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p3-6)»*

**\[4.2\]** 2605.06352 — Tang et al., «Topological Signatures of Grokking». [`"Relatedly, [21] reduced grokking delay by constraining representations to lie on a sphere."`](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p2-10). *«[Родственным образом [21] сократили задержку гроккинга, ограничив представления лежать на сфере.](../papers/2605.06352.topological-signatures-of-grokking/2605.06352.topological-signatures-of-grokking.card.md#p2-10)»*
