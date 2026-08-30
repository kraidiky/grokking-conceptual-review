# Избирательный weight decay (selective / layerwise weight decay)

[Край устойчивости](edge-of-stability.md) ← предыдущая карточка, следующая → [Направление влияния weight decay](weight-decay-direction.md)

[Индекс карточек понятий](index.md), категория: [4. Факторы обучения и оптимизации](index.md#cat-4)\
→ Следующая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)\
← Предыдущая категория: [3. Задачи и наборы данных](modular-arithmetic.md)

## Определение

**Избирательный weight decay** — применение [ослабления весов](weight-decay.md) не ко всем параметрам сети, а к выбранной их части: обычно из-под штрафа выводят вложения, смещения и множители нормировки, но встречается и обратное. Приём стар и в статьях о [гроккинге](grokking.md) чаще всего проходит строкой в описании постановки, а не как предмет изучения: *«weight decay применялся только к декодеру, а не к слою эмбеддингов»*, причём авторы тут же отмечают, что это отличает их постановку от исходной \[[1.1](#ref-1-1)\].

Разграничение существенно, потому что скалярный $\lambda$ в отчёте не говорит, к чему он приложен, — а от этого зависит и срок обобщения, и то, какое решение будет выбрано.

## Детализация

**Что именно выводят из-под штрафа.** В корпусе встречаются все обычные разбиения: только весовые матрицы — *«развязанным weight decay $\lambda$, действующим только на весовые матрицы»* \[[1.2](#ref-1-2)\]; всё, кроме смещений и нормировок \[[3.2](#ref-3-2)\]; вложения токенов и позиций, исключённые «по общепринятой практике» \[[3.1](#ref-3-1)\]. Различие не косметическое: в одной из работ норма определена ровно по тому же набору, по которому действует штраф, — *«weight decay и зажим действуют на $E, W_1, W_2$; смещения не регуляризуются и исключены из $\|W\|$»* \[[1.3](#ref-1-3)\], — и потому величина, которой меряют задержку, зависит от разбиения так же, как и сама задержка.

**Обратный случай.** Штраф накладывают и туда, откуда его обычно убирают. При добавлении layer normalization большой эффективной скорости обучения перестаёт хватать, и приходится понижать норму входов attention-голов, *«чего мы добиваемся, применяя weight decay к масштабным параметрам преобразований layernorm»* \[[2.1](#ref-2-1)\]. Это ограничивает всякий вывод вида «weight decay нужен для гроккинга»: нужен не сам штраф, а понижение определённой нормы, и адресат зависит от архитектуры.

**Группы параметров как приём.** Разбиение бывает и целью, а не подробностью: обратное вложение держат *«отдельной группой, чтобы выходной голове можно было задать отдельные скорость обучения и затухание весов и чтобы её можно было заморозить независимо»* \[[1.4](#ref-1-4)\]. В той же работе развёртка включает настройку с нулевым затуханием на считывающей голове — и коллапс происходит так же, что снимает подозрение на штраф как на его причину. Соседняя работа проверяет группы вмешательством: одноразовое масштабирование любой отдельной группы почти не двигает срок \[[3.4](#ref-3-4)\].

**Разные $\lambda$ вместо «включить/выключить».** Мягкая форма избирательности — не исключать, а назначать свой коэффициент: AdamW с общим $\lambda$ плюс отдельная $L_2$ на вложения \[[5.1](#ref-5-1)\]↗; предложение разделить штраф на липшиц-активные и прочие веса двумя независимыми множителями \[[3.5](#ref-3-5)\]; в теоретических постановках это допускается прямо — *«мы допускаем, чтобы у разных слоёв были разные расписания скорости обучения и weight decay»* \[[3.6](#ref-3-6)\].

**Чего в корпусе нет.** Систематического сравнения разбиений: ни одна работа не ставит один и тот же опыт с несколькими адресатами штрафа, чтобы измерить, насколько срок обобщения от этого зависит. Разбиение почти всегда сообщается как настройка, унаследованная от практики, и потому расхождения между работами по величине $\lambda$ сравнимы лишь условно.

## Альтернативные определения и нюансы

### A. Исключение как гигиена

Слабая форма: вложения, смещения и нормировки выводят из-под штрафа потому, что так принято \[[3.1](#ref-3-1)\], \[[3.2](#ref-3-2)\]. Различающая черта — приём не обсуждается и не проверяется; его цена в том, что число $\lambda$ в двух работах может означать разное, а сопоставление сроков между ними — молчаливое допущение.

### B. Адресат штрафа как управляющая величина

Сильная форма: выбор части параметров сам решает, наступит ли переход \[[2.1](#ref-2-1)\], \[[1.4](#ref-1-4)\]. Источник различия — здесь разбиение проверяется вмешательством, а не наследуется; отсюда и вывод, что действует не штраф вообще, а понижение конкретной нормы.

### C. Послойный decay как неевклидов штраф

Математическая форма, где избирательность перестаёт быть настройкой опыта. Если матрица параметризована факторизованно, то послойный фробениусов штраф на множители равен ядерной норме произведения: *«исключение факторизации индуцирует вариационный штраф… Поэтому явный факторизованный weight decay отвечает ядерной норме»* \[[1.5](#ref-1-5)\]. Для сети из $L$ слоёв показатель зависит от глубины — *«послойный weight decay отвечает шаттен-$p$ штрафу на сквозном отображении с $p=2/L$»*, и такой штраф сильнее сжимает малые сингулярные значения, смещая представление к низкому эффективному рангу \[[1.6](#ref-1-6)\].

Различающая черта — предмет утверждения: не «какие матрицы штрафовать», а какой неявный штраф на выученное отображение получается из выбранного разбиения. Следствия проверяемые: в первой работе ядерная норма отбирает между двумя геометриями решения (циклический код против симплексного ETF) с преимуществом порядка $\Theta(K)$, во второй разрыв между логарифмическими часами подгонки и полиномиальными часами упрощения и есть [задержка](grokking-time.md).

### D. Развязанность против штрафа в потере

Смежное различение, которое легко спутать с избирательностью: применять ли decay через функцию потерь или отдельным шагом после оптимизатора. Все три стратегии сравниваемой работы применены *«развязанно (после шага оптимизатора, не через потерю)»* \[[3.2](#ref-3-2)\]. Различающая черта — здесь меняется не адресат штрафа, а его взаимодействие с адаптивным оптимизатором; для AdamW два способа не эквивалентны, и потому отчёты о $\lambda$ несопоставимы без указания способа.

## Ссылки

###### ref-1-1
**\[1.1\]** 2205.10343 — Liu et al., «Towards Understanding Grokking: An Effective Theory of Representation Learning». [`"weight decay has only been applied to the decoder and not to the embedding layer"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p8-8). *«[weight decay применялся только к декодеру, а не к слою эмбеддингов](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#p8-8)»*

###### ref-1-2
**\[1.2\]** 2606.13753 — Truong et al., «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». [`"decoupled weight decay $\lambda$ applied to weight matrices only"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-6). *«[развязанным weight decay $\lambda$, действующим только на весовые матрицы](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-6)»*

###### ref-1-3
**\[1.3\]** 2606.18465 — Truong, «What Does the Weight Norm Control in Grokking? Logit-Scale Mediation under Cross-Entropy». [`"Weight decay and the clamp act on $E,W_{1},W_{2}$; biases are unregularized and excluded from $\|W\|$."`](../papers/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy.card.md#p10-4). *«[Weight decay и зажим действуют на $E,W_{1},W_{2}$; смещения не регуляризуются и исключены из $\|W\|$.](../papers/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy.card.md#p10-4)»*

###### ref-1-4
**\[1.4\]** 2608.07436 — Janati et al., «Post-Grokking Collapse at the Representation–Readout Interface in Muon-Trained Transformers». [`"The unembedding is kept as its own group so that the output head can be given a separate learning rate and weight decay, and so that it can be frozen independently in the interventions reported below."`](../papers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers/original/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers.md#p4-3). *«[Обратное вложение держится отдельной группой, чтобы выходной голове можно было задать отдельные скорость обучения и затухание весов и чтобы её можно было заморозить независимо во вмешательствах, сообщаемых ниже.](../papers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers/2608.07436.post-grokking-collapse-at-the-representation-readout-interface-in-muon-trained-transformers.card.md#p4-3)»*

###### ref-1-5
**\[1.5\]** 2606.08985 — Tan et al., «Beyond Neural Collapse: Task-Intrinsic Geometry Governs Neural Representations in Modular Arithmetic». [`"Therefore, explicit factorized weight decay corresponds to the nuclear norm"`](../papers/2606.08985.beyond-neural-collapse-task-intrinsic-geometry-governs-neural-representations-in-modular-arithmetic/2606.08985.beyond-neural-collapse-task-intrinsic-geometry-governs-neural-representations-in-modular-arithmetic.card.md#p11-7). *«[Поэтому явный факторизованный weight decay отвечает ядерной норме](../papers/2606.08985.beyond-neural-collapse-task-intrinsic-geometry-governs-neural-representations-in-modular-arithmetic/2606.08985.beyond-neural-collapse-task-intrinsic-geometry-governs-neural-representations-in-modular-arithmetic.card.md#p11-7)»*

###### ref-1-6
**\[1.6\]** 2606.05863 — Tan et al., «Deciphering Two Training Clocks in Grokking via Deep Linear Network Theory with Conditional ReLU Reduction». [`"In a deep linear network, layerwise weight decay corresponds to a Schatten-$p$ penalty on the end-to-end map, with $p=2/L$."`](../papers/2606.05863.deciphering-two-training-clocks-in-grokking-via-deep-linear-network-theory-with-conditional-relu-reduction/2606.05863.deciphering-two-training-clocks-in-grokking-via-deep-linear-network-theory-with-conditional-relu-reduction.card.md#p2-7). *«[В глубокой линейной сети послойный weight decay отвечает шаттен-$p$ штрафу на сквозном отображении с $p=2/L$.](../papers/2606.05863.deciphering-two-training-clocks-in-grokking-via-deep-linear-network-theory-with-conditional-relu-reduction/2606.05863.deciphering-two-training-clocks-in-grokking-via-deep-linear-network-theory-with-conditional-relu-reduction.card.md#p2-7)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Оспаривает вывод «weight decay нужен для гроккинга»: нужен не штраф вообще, а понижение определённой нормы, и его адресат задаётся архитектурой. [`"it becomes necessary to also reduce the norm of the attention head inputs, which we achieve by applying weight decay to the scale parameters of the layernorm transforms in the network"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#fig-2). *«[становится необходимым ещё и снизить норму входов головы внимания, чего мы добиваемся, применяя weight decay к масштабным параметрам преобразований layernorm в сети](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#fig-2)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2605.04396 — Ali, «Critical Windows of Complexity Control: When Transformers Decide to Reason or Memorize». Нюанс: исключение вложений названо общепринятой практикой, а не находкой; сам $\lambda$ при этом вынесен из шага оптимизатора, чтобы включаться и выключаться по расписанию. [`"Token and position embeddings are excluded from weight decay, following standard practice."`](../papers/2605.04396.critical-windows-of-complexity-control-when-transformers-decide-to-reason-or-memorize/original/2605.04396.critical-windows-of-complexity-control-when-transformers-decide-to-reason-or-memorize.md#p4-3). *«[Вложения токенов и позиций из weight decay исключены, следуя стандартной практике.](../papers/2605.04396.critical-windows-of-complexity-control-when-transformers-decide-to-reason-or-memorize/2605.04396.critical-windows-of-complexity-control-when-transformers-decide-to-reason-or-memorize.card.md#p4-3)»*

###### ref-3-2
**\[3.2\]** 2606.04405 — Li, «Low-Rank Decay for Grokking in Scale-Invariant Transformers: A Spectral-Geometric View». Нюанс: разбиение сообщается вместе со способом применения — развязанно, после шага оптимизатора, а не через потерю. [`"**L2 (Frobenius weight decay):** $W\leftarrow W\cdot(1-\eta\lambda)$, applied to all non-bias, non-norm parameters."`](../papers/2606.04405.low-rank-decay-for-grokking-in-scale-invariant-transformers-a-spectral-geometric-view/2606.04405.low-rank-decay-for-grokking-in-scale-invariant-transformers-a-spectral-geometric-view.card.md#p4-11). *«[**L2 (фробениусов weight decay):** $W\leftarrow W\cdot(1-\eta\lambda)$, применяется ко всем параметрам, кроме смещений и норм.](../papers/2606.04405.low-rank-decay-for-grokking-in-scale-invariant-transformers-a-spectral-geometric-view/2606.04405.low-rank-decay-for-grokking-in-scale-invariant-transformers-a-spectral-geometric-view.card.md#p4-11)»*

###### ref-3-4
**\[3.4\]** 2606.13753 — Truong et al., «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Нюанс: группы проверены вмешательством — разовое масштабирование любой из них почти не двигает срок, значит дело не в норме отдельной группы. [`"A one-shot $0.8\times$ scaling of any single parameter group (embedding, hidden, or output) likewise leaves $T_{\mathrm{grok}}$ within"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p7-2). *«[Одноразовое масштабирование на $0.8\times$ любой отдельной группы параметров (вложения, скрытого слоя или выхода) равным образом оставляет $T_{\mathrm{grok}}$ в пределах](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p7-2)»*

###### ref-3-5
**\[3.5\]** 2607.08350 — Pranjić et al., «Grokking and epoch-wise double descent in quantum neural networks». Нюанс: избирательность предложена по неархитектурному признаку — липшиц-активные веса против остальных, с двумя независимыми множителями. [`"one could penalize the Lipschitz-active and remaining weight-norms independently by decomposing the regularization term in Equation 4 using two distinct hyperparameters"`](../papers/2607.08350.grokking-and-epoch-wise-double-descent-in-quantum-neural-networks/2607.08350.grokking-and-epoch-wise-double-descent-in-quantum-neural-networks.card.md#p5-1). *«[можно было бы штрафовать липшиц-активные и остальные нормы весов независимо, разложив регуляризационный член в Уравнении 4 с двумя различными гиперпараметрами](../papers/2607.08350.grokking-and-epoch-wise-double-descent-in-quantum-neural-networks/2607.08350.grokking-and-epoch-wise-double-descent-in-quantum-neural-networks.card.md#p5-1)»*

###### ref-3-6
**\[3.6\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Нюанс: в теоретической постановке разные расписания по слоям допускаются прямо — то есть избирательность заложена в саму формулировку, а не выбрана в опыте. [`"We allow different layers to have different learning rate and weight decay schedules."`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p4-5). *«[Мы допускаем, чтобы у разных слоёв были разные расписания скорости обучения и weight decay.](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p4-5)»*

### Внешние работы

###### ref-5-1
**\[5.1\]** 2502.01628 — **Внешняя работа (выдержка): Liu, Tegmark et al., «Harmonic Loss Trains Interpretable AI Models».** Нюанс: избирательность здесь мягкая — общий weight decay оптимизатора дополнен отдельной $L_2$ на вложения, то есть у вложений свой множитель, а не исключение. [`"a weight decay of $10^{-2}$, and an $L_{2}$ regularization on the embeddings with strength 0.01"`](../externals/2502.01628.harmonic-loss-trains-interpretable-ai-models/original/2502.01628.harmonic-loss-trains-interpretable-ai-models.md#p4-3). *«[weight decay $10^{-2}$ и $L_{2}$-регуляризацией на вложениях силой 0.01](../externals/2502.01628.harmonic-loss-trains-interpretable-ai-models/2502.01628.harmonic-loss-trains-interpretable-ai-models.card.md#p4-3)»*
