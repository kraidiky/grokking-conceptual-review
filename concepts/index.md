# Глобальный индекс понятий грокинга (Global Grokking Concept Index)

Построено инверсией по-статейных экстракций в индекс «понятие -> статьи» по 66 статьям arXiv о грокинге (исходно — по txt-экстракциям репозитория, из которого вики выделена; они здесь не хранятся). Каждая ссылка quote-anchored: точный английский фрагмент оригинала, связанный с хранимым текстом статьи в `papers/<папка>/original/` — якорем абзаца, а для заголовков и формул — ссылкой на файл. [Решение task-004, 2026-08-07: у трёх PDF-only работ без карточек — 2411.05353, 2507.11645, 2510.25966 — записи сохраняют историческую форму `"цитата" (line N)`; номер указывает в отсутствующую здесь txt-экстракцию и локально не проверяем, поисковым якорем служит сама цитата.] Синонимичные ярлыки (напр. frequency-principle / spectral-bias; lazy-to-rich / kernel-to-feature-learning; anti-grokking / generalization-collapse; weight-norm-minimization / zero-loss-manifold) сведены к одному каноническому понятию, слитые алиасы указаны в скобках.

Формат ссылки: `arxivid: ["verbatim English quote"](../papers/<папка>/original/<файл>.md#якорь)` — цитаты приведены дословно (verbatim), на английском, в сериализации хранимого оригинала; не изменяйте их. Дословность проверяет quote_check.py.

###### cat-1
## 1. Явления (Phenomena)

### [Грокинг / отложенная генерализация](grokking.md) (grokking / delayed generalization) — 111 статей


### [Фаза меморизации / плато](memorization-phase.md) (memorization phase / plateau) — 74 статей


### [Фазовый переход](phase-transition.md) (phase transition) — 60 статей


### [Эффект рогатки](slingshot.md) (slingshot effect / mechanism) — 29 статей


### [Эмерджентность / эмерджентные способности](emergence.md) (emergence / emergent abilities) — 24 статей

- 2310.17247: [`"grokking is not limited to neural networks but occurs in other settings"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1)
- 2402.09469: [`"Grokking and Emergent Ability"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#p6-2)

### [Двойной спуск](double-descent.md) (double descent) — 22 статей

- 2410.04489: [`"grokking and double descent."`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p10-1)
- 2411.05353: "Unifying grokking and double descent" (line 717)
- 2504.03162: [`"view of grokking, double descent and emergent abilities"`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p9-11)

### [Коллапс softmax](softmax-collapse.md) (Softmax Collapse) — 8 статей


### [Катастрофическое забывание](catastrophic-forgetting.md) (catastrophic forgetting) — 7 статей


### [Унгрокинг](ungrokking.md) (ungrokking) — 7 статей

- 2506.05718: [`"makes it possible to grok or ungrok"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p1-2)

### [Полу-грокинг](semi-grokking.md) (semi-grokking) — 6 статей


### [Анти-грокинг / коллапс генерализации](anti-grokking.md) (anti-grokking / generalization collapse) — 5 статей
Слитые алиасы: anti-grokking / generalization-collapse.


### [Наивная минимизация потерь](nlm.md) (Naive Loss Minimization) — 2 статей


###### cat-2
## 2. Механизмы и представления (Mechanisms & representations)

### [Обучение структурированных представлений](structured-representation-learning.md) (structured representation learning) — 46 статей


### [Возникновение признаков](feature-emergence-feature-learning.md) (feature emergence / feature learning) — 34 статей


### [Фурье-признаки и контуры](fourier-features-circuits.md) (Fourier features / circuits) — 30 статей

- 2406.05335: [`"denotes the Fourier"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md#p4-4) ↗
- 2411.05353: "the corresponding Fourier spectrum in Fig. 2 is sharply peaked" (line 300)
- 2511.01938: [`"FOURIER FEATURES"`](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/original/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.md#fig-6)
- 2603.05228: [`"structured representations based on discrete Fourier features"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-1)
- 2603.29262: [`"menting the Fourier Multiplication Algorithm (FMA) for"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-5)
- 2604.13123: [`"Fourier alignment $A_{\mathrm{Fourier}}$ exhibit strong coupled anti-correlation"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2)

### [Переход lazy→rich](lazy-to-rich-kernel-to-feature-learning.md) (lazy-to-rich / kernel-to-feature-learning transition) — 29 статей
Слитые алиасы: lazy-to-rich / kernel-to-feature-learning / lazy-learning-stage / lazy-rich-regime-transition.


### [Эффективность контуров](circuit-efficiency.md) (circuit efficiency) — 25 статей

- 2410.04489: [`"circuits: Grokking as competition of sparse and dense"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p10-17)
- 2505.18266: [`"require only $\mathcal{O}(\log n)$ features"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2)

### [Ландшафт потерь / бассейны](loss-landscape-basins.md) (loss landscape / basins) — 24 статей

- 2502.20763: [`"the local landscape around minima or saddle points"`](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p1-2) ↗

### [Сжатие многообразия представлений](manifold-representation-compression.md) (manifold / representation compression) — 21 статей


### [Разрежённая подсеть / lottery ticket](sparse-subnetwork-lottery-ticket.md) (sparse subnetwork / lottery ticket) — 21 статей


### [Нейронное касательное ядро](neural-tangent-kernel-ntk.md) (neural tangent kernel, NTK) — 18 статей
Слитые алиасы: neural-tangent-kernel / NTK-task-kernel-alignment.


### [Маршрутизация внимания](attention-routing-heads.md) (attention routing / heads) — 16 статей


### [Групповые представления и cosets](group-representations-cosets.md) (group representations / cosets) — 12 статей


### [Нейронный коллапс](neural-collapse.md) (neural collapse) — 12 статей


### [Алгоритмы Clock и Pizza](clock-vs-pizza.md) (Clock vs Pizza) — 9 статей


### [Частотный принцип / спектральное смещение](frequency-principle-spectral-bias.md) (frequency principle / spectral bias) — 7 статей
Слитые алиасы: frequency-principle / spectral-bias / F-Principle / frequency-perspective.


### [Ортогональность градиента](orthogonal-gradient-perp-grad.md) (orthogonal gradient / perp-Grad) — 7 статей
Слитые алиасы: gradient-orthogonality / orthogonal-gradient-flow / perp-Grad.


###### cat-3
## 3. Задачи и наборы данных (Tasks & datasets)

### [Модульная арифметика](modular-arithmetic.md) (modular arithmetic) — 74 статей

- 2405.17479: [`"algorithmic datasets (Power et al., 2022)"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-4)

### [Эталонный полигон гроккинга](canonical-grokking-testbed.md) (canonical grokking testbed / Power et al. setup) — 47 статей

- 2201.02177: [`"The datasets we consider are binary operation tables of the form $a\circ b=c$ where $a,b,c$ are discrete symbols with no internal structure, and $\circ$ is a binary operation"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p2-1)
- 2301.02679: [`"fully-connected two-layer networks that exhibit grokking on various modular arithmetic tasks under vanilla gradient descent with the MSE loss function in the absence of any regularization"`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p1-2)
- 2301.05217: [`"In our mainline experiment, we take $P=113$ and use a one-layer ReLU transformer"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#sec-3)
- 2402.16726: [`"learning Transformers on complex modular arithmetic tasks, including polynomials"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-1)
- 2405.20233: [`"the same modular multiplication task devised to report the grokking phenomenon"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p7-1)
- 2511.04760: [`"We begin training with 35% of the dataset to first observe grokking, as demonstrated in prior work"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p3-3)
- 2603.01968: [`"beyond the standard modular arithmetic testbed to a diverse suite of algebraic, structural, and relational domains"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-5)
- 2603.29262: [`"four primary modular arithmetic operations ($+,-,\times,\div$) over the finite field $\mathcal{Z}_{p}=\{0,\dots,p-1\}$ with $p=97$"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p2-6)


### [Реальные данные и зрение](vision-real-world-data-mnist-cifar.md) (vision / real-world data: MNIST, CIFAR, ...) — 34 статей

- 2205.10343: [`"We now demonstrate, for the first time, that grokking (significantly delayed generalization) is a more general phenomenon in machine learning that can occur not only on algorithmic datasets, but also on mainstream benchmark datasets"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p9-2)
- 2206.04817: [`"The FCN is trained with 200 randomly chosen CIFAR-10 samples with Adam"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#fig-1)
- 2210.01117: [`"induce grokking on tasks involving images, language and molecules"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p1-3)
- 2310.06110: [`"this transition from lazy (linear model) to rich training (feature learning) can control grokking in more general settings, like on MNIST, one-layer Transformers, and student-teacher networks"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#p1-3)
- 2310.19470: [`"significantly reduces delayed generalization across various tasks, including multiple modular arithmetic operations, polynomial regression, sparse parity, and MNIST classification"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2)
- 2311.06597: [`"initially observed in training on unconventional Algorithmic datasets [11], and later found in traditional tasks such as image classification [6]"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p1-3)
- 2405.17479: [`"We observe this phenomenon across both synthetic and real datasets, offering a novel viewpoint"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-2)
- 2501.04697: [`"recent findings suggest that grokking may be a more pervasive phenomenon, also manifesting in more complex tasks involving vision and language (Lv et al., 2024; Humayun et al., 2024)"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-4)
- 2504.03162: [`"we design a dedicated dataset to validate our theory on ResNet-18, successfully showcasing the occurrence of grokking"`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p1-2)
- 2504.13292: [`"the grokking phenomenon has since been observed in other settings such as learning group operations (Chughtai et al., 2023), sparse parity (Barak et al., 2022), and image classification (Liu et al., 2023)"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-4)
- 2509.17738: [`"regularizing against flatness, i.e., encouraging sharp solutions, reliably delays generalization and induces grokking-like behavior even in settings where it does not typically arise, including ResNets on CIFAR-10, ViTs on ImageNet"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p2-3)
- 2509.20829: [`"We first analyze the relationship between grokking, within-class variance, and neural collapse. Following Liu et al. (2023a), we train an MLP on the MNIST dataset"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p7-10)
- 2510.25966: `"This paper demonstrates that grokking can occur when a dense neural network is applied to the Ising model in the presence of weight decay"`
- 2512.03437: [`"We compare applying standard unlearning methods *before* versus *after* the grokking transition across vision (CNNs/ResNets on CIFAR, SVHN and ImageNet) and language (a transformer on a TOFU‑style setup)"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)
- 2602.02859: [`"We revisit two canonical grokking setups: a 3-layer MLP trained on a subset of MNIST"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p1-3)


### [Разрежённая чётность](sparse-parity.md) (sparse parity) — 20 статей

- 2301.02679: [`"the authors of [1] studied online learning of the (k, n) sparse parity problem where the network function is asked to compute parity of k bits in a length-n string of random bits"`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p2-3)
- 2303.11873: [`"of networks undergoing grokking on the sparse parity task, and ﬁnd that the"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2)
- 2310.05918: [`"intriguing phenomenon of the XOR network switching between two generalization solutions, while $L_{2}$ does not"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2)
- 2310.19470: [`"we also demonstrate that delayed generalization is reduced by the grokked ticket in both the polynomial regression and sparse parity tasks"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p5-3)
- 2402.09469: [`"$(n,k)$-sparse parity problem is notorious hard to learn, i.e., Statistical Query (SQ) hardness"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#p14-2)
- 2405.17479: [`"we consider one-dimensional synthetic data and high-dimensional parity"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-6)
- 2407.12332: [`"grokking can happen in tasks beyond modular arithmetic: in learning sparse parities (Barak et al., 2022; Bhattamishra et al., 2023)"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-3)
- 2410.03569: [`"Our techniques also help ML models learn other well-studied problems better, including copy, associative recall, and parity"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p1-3)
- 2501.04697: [`"We also validate some of our results on the Sparse Parity task outlined in Barak et al. (2022)"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p3-3)
- 2504.13292: [`"the sparse parity task. Our experiments verify that GrokTransfer effectively reshapes the training dynamics and eliminate delayed generalization"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p8-5)
- 2510.04930: [`"we empirically show that on classical arithmetic problems like modular addition and sparse parity"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p1-2)
- 2603.13331: [`"The theory also correctly predicts when grokking does not occur : sparse parity tasks"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p2-10)
- 2604.00316: [`"RFM fails to generalize only when using train-test partitions that are invariant under the action of a non-singleton subgroup"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p4-2)


### [Шум в метках / случайные метки](label-noise-random-labels.md) (label noise / random labels; у Power et al. — outliers) — 18 статей

- 2201.02177: [`"Effect on data efficiency of introducing $k\in[0,10,100,1000,2000,3000]$ outliers (examples with random labels) into the training data"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#fig-6)
- 2309.02390: [`"We produce $C_{\text{mem}}\,$-only networks by using completely random labels for the training data (Zhang et al., 2021)"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p7-12)
- 2402.15175: [`"For this purpose, we assign random labels to the training data (Zhang et al., 2021)"`](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/original/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.md#p4-5)
- 2412.09810: [`"should have low complexity despite having many parameters, while a network memorizing random labels"`](../papers/2412.09810.the-complexity-dynamics-of-grokking/original/2412.09810.the-complexity-dynamics-of-grokking.md#p3-1)
- 2506.21551: [`"Training labels $y_{i}=f^{*}(x_{i})+\epsilon_{i}$ with $\epsilon_{i}\sim\mathcal{N}(0,\sigma^{2})$, independent of $x_{i}$"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p15-1)
- 2509.17738: [`"tion: a network can generalize without NC and some setups (like noisy labels) can break the link"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p3-5)
- 2603.29262: [`"1. Random Labels: When trained on unlearnable random labels, the model does not grok, and BDM remains high. This"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p13-3)

### [Композиция групп / некоммутативность](group-composition-non-commutative-s5.md) (group composition, non-commutative, S5) — 15 статей

- 2201.02177: [`"for the problem of learning the product in the abstract group S5"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#fig-2)
- 2311.06597: [`"from group theory, it shall obey the commutative law once it actually learns the general addition rule"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p7-1)
- 2504.13292: [`"the grokking phenomenon has since been observed in other settings such as learning group operations (Chughtai et al., 2023)"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-4)
- 2504.17243: [`"NEURALGROK significantly accelerates generalization, ranging from simple operations"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p2-1)
- 2505.18266: [`"the universality hypothesis as a testable conjecture across all group-theoretic datasets"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-1)
- 2509.21519: [`"Non-Abelian groups with $\max_{k}d_{k}=2$ (maximal irreducible dimension $2$)"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#fig-5)
- 2601.21150: [`"The symmetric group $S_{n}$ is the set of all permutations of $n$ elements with the binary operation of composition of permutations"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p3-4) ↗
- 2603.05228: [`"using non-commutative $S_{5}$ permutation composition as a negative control, we find that the same spherical constraint fails entirely"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-5)
- 2604.00316: [`"we examine how RFM learns features on a broader class of algebraic tasks, extending beyond modular arithmetic to other Abelian groups"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p2-1)
- 2604.13123: [`"The same entropy-collapse signature appears across modular arithmetic ($\mathbb{Z}/p\mathbb{Z}$, abelian) and $S_{5}$ permutation composition (non-abelian, 120 classes)"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2)


### [Рассуждение и графы знаний](reasoning-knowledge-graphs.md) (reasoning / knowledge graphs) — 5 статей

- 2405.15071: [`"learn to implicitly reason over parametric knowledge"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)
- 2504.20752: [`"for the first time, we extend grokking to real-world factual data and address the challenge of dataset sparsity by augmenting existing knowledge graphs with carefully designed synthetic data"`](../papers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers/2504.20752.grokking-in-the-wild-data-augmentation-for-real-world-multi-hop-reasoning-with-transformers.card.md#p1-2)
- 2506.21551: [`"generalization on diverse benchmark tasks covering math/commonsense reasoning, code generation, and domain-specific retrieval"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)
- 2601.09049: [`"Our findings challenge the view that grokking represents genuine acquisition of generalized reasoning"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-6)
- 2603.01968: [`"form analogous to a small knowledge graph, following prior works (Wang et al., 2024; Allen-Zhu & Li, 2024)"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p3-4)


###### cat-4
## 4. Факторы обучения и оптимизации (Training / optimization factors)

### [Weight decay / L2-регуляризация](weight-decay.md) (weight decay) — 82 статей

- 2511.12768: [`"depends on regularization strength (weight decay) and dataset\ndiversity"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p4-4)

### [Доля данных / критический размер набора](data-fraction-critical-dataset-size.md) (data fraction / critical dataset size) — 54 статей


### [Оптимизатор](optimizer-adam-adamw-sgd.md) (optimizer: Adam / AdamW / SGD) — 49 статей


### [Минимизация нормы весов](weight-norm-minimization.md) (weight-norm minimization / zero-loss manifold) — 40 статей
Слитые алиасы: weight-norm minimization / norm minimization on the zero-loss manifold.


### [Масштаб инициализации](initialization-scale.md) (initialization scale) — 38 статей


### [Когда регуляризация необходима](regularization-necessity.md) (regularization necessity) — 34 статей


### [Скорость обучения](learning-rate.md) (learning rate) — 30 статей


### [Варианты регуляризации](regularization-variants.md) (regularization variants) — 22 статей


### [Зона Златовласки](goldilocks-zone.md) (Goldilocks zone) — 20 статей


### [Роль шума градиента](gradient-noise.md) (gradient noise / full-batch training) — 18 статей

### [Край устойчивости](edge-of-stability.md) (edge of stability) — 11 статей


### [Направление влияния weight decay](weight-decay-direction.md) (weight decay direction / 1/gamma law) — 9 статей


###### cat-5
## 5. Интервенции и методы (Interventions & methods)

### [Grokfast / фильтрация градиента](gradient-low-pass-filtering.md) (gradient low-pass filtering) — 12 статей


### [StableMax / perp-Grad](numerical-stability-fix.md) (numerical-stability fix) — 12 статей


### [Замораживание подсети / edge-popup](freezing-subnetwork.md) (freezing subnetwork) — 11 статей


### [Сферическое ограничение нормы](spherical-weight-norm-constraint.md) (spherical weight-norm constraint) — 10 статей


###### cat-6
## 6. Аналитические инструменты и метрики (Analytical tools & metrics)

### [Меры прогресса](progress-measures.md) (progress measures) — 53 статей


### [Механистическая интерпретируемость](mechanistic-interpretability.md) (mechanistic interpretability) — 44 статей


### [Спектральный анализ / FVE](spectral-analysis-svd-esd-fve.md) (spectral analysis, SVD, ESD, FVE) — 28 статей


### [Каузальная абляция](causal-ablation-intervention.md) (causal ablation / intervention) — 23 статей


### [Параметр порядка](order-parameter.md) (order parameter) — 12 статей


### [Тяжёлохвостовая саморегуляризация](heavy-tailed-self-regularization-htsr.md) (heavy-tailed self-regularization, HTSR) — 8 статей


### [Линейное зондирование](linear-sparse-probing.md) (linear / sparse probing) — 6 статей


### [Корреляционные ловушки](correlation-traps.md) (correlation traps) — 4 статей


###### cat-7
## 7. Теория и формальные результаты (Theory & formal results)

### [Эффективная теория / статистическая механика](effective-theory-statistical-mechanics.md) (effective theory / statistical mechanics) — 24 статей


### [Максимизация зазора](margin-maximization-implicit-bias.md) (margin maximization / implicit bias) — 21 статей


### [Границы генерализации](generalization-bounds.md) (generalization bounds) — 15 статей


### [Закон задержки через норм-сепарацию](norm-separation-delay-law.md) (Norm-Separation Delay Law) — 7 статей


### [Колмогоровская сложность](kolmogorov-complexity.md) (Kolmogorov complexity) — 6 статей


###### cat-8
## 8. Специализированные / редкие понятия (Specialized / rare — appear in only 1-2 papers)

### Абсолютная энтропия градиента (Absolute Gradient Entropy, AGE) — 1 статей

- 2504.17243: [`"a novel Absolute Gradient Entropy (AGE) metric"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p1-2)

### Ускоренный грокинг (accelerated grokking, EGD) — 1 статей

- 2510.04930: [`"Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md)

### Контроль через нелинейность активации (activation nonlinearity control) — 1 статей

- 2411.05353: "can be controlled by modifying the profile of the activation function" (line 13)

### Разрежённость активаций (activation sparsity) — 1 статей

- 2510.25966: "The activation sparsity, e.g. the fraction of" (line 68)

### Адаптивное ядро для feature learning (adaptive kernel feature learning) — 1 статей

- 2310.03789: [`"the adaptive kernel approach, to two teacher-student models"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-2)

### Приближённая китайская теорема об остатках (approximate CRT) — 1 статей

- 2505.18266: [`"we call the approximate Chinese Remainder Theorem"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2)

### Архитектурное индуктивное смещение (architectural inductive bias) — 1 статей

- 2603.05228: [`"bypassing the generalization delay is possible—but strictly depends on alignment between architectural priors and task symmetry"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2)

### Аррениусовское масштабирование (Arrhenius scaling) — 1 статей

- 2606.17120: [`"with escape times following Arrhenius scaling"`](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/original/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.md#p1-2)

### Поведенческий против истинного грокинга (behavioral vs true grokking) — 1 статей

- 2601.09049: [`"such “Fake grokked” transformer lacks"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p3-4)

### Анализ каузальной роли (causal role analysis) — 1 статей

- 2509.17738: [`"the causal role of either"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2)

### Конкуренция плотной и разрежённой подсетей (dense vs sparse circuit competition) — 1 статей

- 2303.11873: [`"two largely distinct subnetworks: a dense one that dominates before the transition"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2)

### Метрика сложности контура (circuit complexity metric) — 1 статей

- 2506.04434: [`"Approximate Local Circuit Complexity"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p2-1)

### Круговая геометрия модуля (circular modular geometry) — 1 статей

- 2410.03569: [`"$0$ and $2\pi$, which corresponds to $q$, are equal in this geometry"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p3-4)

### Очистка убирает меморизацию (cleanup removes memorization) — 1 статей

- 2301.05217: [`"removes the memorization components"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-3)

### Со-грокинг в мультизадачном обучении (co-grokking) — 1 статей

- 2402.16726: [`"some multi-task mixtures may lead to co-grokking"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-1)

### Кривизна через коммутаторный дефект (commutator defect curvature) — 1 статей

- 2602.16746: [`"commutator defects—the non-commutativity of"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2)

### Оптимизация с ограничениями (constrained optimization) — 1 статей

- 2511.01938: [`"understood through the lens of constrained optimization"`](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/original/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.md#p1-2)

### Критическое замедление (critical slowing down) — 1 статей

- 2406.05335: [`"Emergence of critical slowing down in training"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md) ↗

### Критичность / критическая точка (criticality / critical point) — 1 статей

- 2406.05335: [`"natural languages are critical : They lie near a phase transition point"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md#p1-2) ↗

### Межслойное разделение памяти (cross-layer memory sharing) — 1 статей

- 2405.15071: [`"encouraging cross-layer knowledge sharing"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)

### Кросс-задачная переносимость (cross-task transferability) — 1 статей

- 2402.16726: [`"grokked models obtain common features transferable among similar operations"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-1)

### Куррикулум easy→hard (curriculum easy-hard data) — 1 статей

- 2410.03569: [`"exposes models to easy and harder versions of modular arithmetic"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p2-2)

### Анализ кривизны (curvature analysis) — 1 статей

- 2512.03437: [`"Analyses of features and curvature further suggest that post‑grokking models learn"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Нарушение симметрии данных (data symmetry breaking) — 1 статей

- 2604.00316: [`"Breaking Data Symmetry is Needed For Generalization in Feature Learning Kernels"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-1)

### Сдвиг распределения (distribution shift) — 1 статей

- 2511.04760: [`"in data-scarce settings under distribution shift"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p3-1)

### Dropout устраняет грокинг (dropout eliminates grokking) — 1 статей

- 2510.25966: "dropout can eliminate grokking" (line 57)

### Кривая устойчивости к dropout (Dropout Robustness Curve, DRC) — 1 статей

- 2507.11645: "a Dropout Robustness Curve (DRC)" (line 19)

### Раннее предсказание грокинга (early grokking prediction) — 1 статей

- 2306.13253: [`"predict grokking without training for a large number"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p1-2)

### Степенной закон раннего предупреждения (early-warning power law) — 1 статей

- 2602.16746: [`"the lead time obeys a power law ∆t ∝ t α"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2)

### Край линейной разделимости (edge of linear separability) — 1 статей

- 2410.04489: [`"the training dataset is nearly linearly separable"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)

### Косинусная близость эмбеддингов (embedding cosine similarity) — 1 статей

- 2507.11645: "similarity between embeddings in a high-dimensional spaces" (line 99)

### Униформность эмбеддинг-пространства (embedding-space uniformity) — 1 статей

- 2504.03162: [`"the uniformity of the embedding space and the"`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p1-2)

### Ускорение через перенос эмбеддингов (embedding transfer acceleration) — 1 статей

- 2504.13292: [`"a simple and principled method for accelerating grokking"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-2)

### Эмерджентные способности при масштабировании (emergent abilities scaling) — 1 статей

- 2511.12768: [`"characterized such emergent behaviors as phase transitions"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p1-6)

### Градиентный подъём энергетической функции (energy-function gradient ascent) — 1 статей

- 2509.21519: [`"gradient ascent of an energy function E"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p1-2)

### Метрика entropy gap (entropy gap metric) — 1 статей

- 2502.20763: [`"introduces information-theoretic metrics called entropy gap to better help analyze"`](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p1-1) ↗

### Многообразие исполнения (execution manifold) — 1 статей

- 2602.16746: [`"trajectory during grokking lies on a low-dimensional execution manifold"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p2-1)

### Ядро feature learning / RFM (feature-learning kernel, RFM) — 1 статей

- 2604.00316: [`"via the Recursive Feature Machine (RFM)"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4)

### Коллапс ранга признаков (feature rank collapse) — 1 статей

- 2405.19454: [`"the decreasing of feature ranks and the"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p1-3)

### Конечно-размерное масштабирование (finite-size scaling, FSS) — 1 статей

- 2603.24746: [`"finite-size scaling (FSS) pro-"`](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-4)

### Плоские минимумы и генерализация (flat minima generalization) — 1 статей

- 2603.01192: [`"“flatter” regions of the loss landscape generalise better"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-4)

### Регуляризация к плоскостности (flatness regularization) — 1 статей

- 2509.17738: [`"models regularized away from flat"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2)

### Точность плавающей запятой (floating-point precision) — 1 статей

- 2605.06152: [`"of floating-point arithmetic precision limits"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-2)

### Четыре фазы обучения (four learning phases) — 1 статей

- 2205.10343: [`"four learning phases: comprehension, grokking, memorization, and"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p1-2)

### Фурье-голова (Fourier head) — 1 статей

- 2603.05228: [`"Fourier-constrained output heads"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p3-4)

### Фурье-инициализация (Fourier initialization) — 1 статей

- 2603.05228: [`"deterministically initialized with cosine and sine values at five key frequencies"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p6-8)

### Фурье-энтропия (Fourier / frequency entropy) — 1 статей

- 2310.19470: [`"we introduce Fourier Entropy (FE) as follows"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p10-5)

### Частотное рассогласование (frequency misalignment) — 1 статей

- 2405.17479: [`"a misalignment between the preferred frequency in the"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-5)

### Гауссово feature learning (Gaussian Feature Learning, GFL) — 1 статей

- 2310.03789: [`"that of Gaussian Feature Learning (GFL)"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-4)

### Контур генерализации (Generalization Circuit) — 1 статей

- 2601.09049: [`"the emergence of a “Generalization Circuit”"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p1-7)

### Геометрические сигнатуры генерализации (geometric signatures) — 1 статей

- 2509.17738: [`"geometric signatures of generalization"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-3)

### Выравнивание градиентов forget/retain (gradient alignment forget-retain) — 1 статей

- 2512.03437: [`"reduced gradient alignment between forget and retain subsets"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Спектральное разложение градиента (gradient spectral decomposition) — 1 статей

- 2405.20233: [`"we can spectrally decompose the parameter trajectories under"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p1-2)

### Графовые свойства подсети (graph properties of subnetwork) — 1 статей

- 2310.19470: [`"beneficial graph properties such as increased average"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2)

### Grokked tickets (grokked lottery tickets) — 1 статей

- 2310.19470: [`"lottery tickets obtained during the generalizing phase (termed grokked tickets)"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2)

### Грокинг как сжатие (grokking as compression) — 1 статей

- 2310.05918: [`"many of them share a similar high-level idea which is"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-3)

### Грокинг вне нейросетей (grokking in non-neural models: GP, linear regression) — 1 статей

- 2310.17247: [`"such as Gaussian process (GP) classification, GP regression, linear regression"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1)

### Время грокинга (grokking time) — 1 статей

- 2601.19791: [`"which we refer to as the “grokking time”"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p1-2)

### Переносимость грокнутых моделей (grokking transferability) — 1 статей

- 2601.09049: [`"the downstream transferability of “grokked” Transformers remains largely underexplored"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-1)

### Ослабление грокинга с ростом сложности (grokking weakens with complexity) — 1 статей

- 2402.09469: [`"as $k$ increases, the grokking phenomenon becomes weak"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#fig-4)

### Грокинг без регуляризации (grokking without regularization) — 1 статей

- 2301.02679: [`"in the absence of any regularization"`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p1-2)

### Эвристическое ядро (heuristic core) — 1 статей

- 2403.03942: [`"evidence of a *heuristic core*: a set of attention heads that appear in all generalizing subnetworks but, on their own, do not generalize"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#fig-1)

### Настройка гиперпараметров (hyperparameter tuning) — 1 статей

- 2601.19791: [`"through proper hyperparameter tuning"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p1-2)

### Гистерезис / выход из метастабильности (hysteresis / metastable escape) — 1 статей

- 2606.17120: [`"grokking is consistent with hysteresis in first-order L2 phase transitions"`](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/original/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.md#p1-2)

### Неявное смещение Adam (implicit bias of Adam) — 1 статей

- 2407.12332: [`"closely related to the implicit bias of Adam [52, 55]"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p3-6)

### Неявное смещение адаптивных оптимизаторов (implicit bias of adaptive optimizers) — 1 статей

- 2206.04817: [`"characterizing an implicit bias of such optimizers"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p2-8)

### Неявное смещение градиентного спуска (implicit bias of gradient descent) — 1 статей

- 2410.04489: [`"by appealing to the implicit bias of gradient descent"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)

### Метрика неактивных нейронов (inactive neurons metric) — 1 статей

- 2507.11645: "percentage of inactive neurons decreases during generalization" (line 27)

### Информационное узкое место (information bottleneck) — 1 статей

- 2509.20829: [`"the information bottleneck principle"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)

### Недостаточная выборка / алиасинг (insufficient sampling / aliasing) — 1 статей

- 2405.17479: [`"caused by insufficient sampling"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-6)

### Критичность порога интерполяции (interpolation threshold criticality) — 1 статей

- 2410.04489: [`"the interpolation threshold, reminiscent of critical"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)

### Внутренняя симметрия задачи (intrinsic task symmetry) — 2 статей

- 2201.02177: [`"Some of the operations listed in Figure 2 (right) are symmetric with respect to the order of the"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p3-4)
- 2603.01968: [`"we propose that intrinsic task symmetry is"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-5)

### Дистилляция знаний (knowledge distillation) — 1 статей

- 2511.04760: [`"Knowledge Distillation (KD) from a model that has already"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p1-3)

### L-бесконечность регуляризация (l-infinity regularization) — 1 статей

- 2407.12332: [`"can be found by gradient descent with small ℓ∞ regularization"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-2)

### Циклирование нормы последнего слоя (last-layer weight-norm cycling) — 1 статей

- 2206.04817: [`"cyclic behavior of the norm of the"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p1-2)

### Обучаемое преобразование градиента (learnable gradient transformation, NeuralGrok) — 1 статей

- 2504.17243: [`"learns an optimal gradient transformation to accelerate"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p1-2)

### Фазы: confusion и comprehension (confusion / comprehension phases) — 1 статей

- 2306.13253: [`"memorization, comprehension, confusion and grokking"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p6-1)

### Сначала менее заметные частоты (less-salient frequency first) — 1 статей

- 2405.17479: [`"initially learn the less salient"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-2)

### Число линейных отображений (linear mapping number, LMN) — 1 статей

- 2310.05918: [`"We define linear mapping number (LMN) to"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2)

### Детекция в линейном пространстве обучения (linear training-space detection) — 1 статей

- 2511.12768: [`"detected directly in *linear training space*, rather than only after log rescaling"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p1-4)

### LLC-траектория как зонд (LLC trajectory probe) — 1 статей

- 2603.01192: [`"LLC trajectories estimated from training data track the onset of generalisation"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)

### Локальная сложность контура (local circuit complexity) — 1 статей

- 2602.02859: [`"Approximate Local Circuit Complexity, which capture broader"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p2-2)

### Анализ локальной сложности (local complexity analysis) — 1 статей

- 2512.03437: [`"quantifies the density of linear regions in a neural network’s input space partition"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p9-8)

### Локальный асинхронный грокинг (local asynchronous grokking) — 1 статей

- 2506.21551: [`"enter their grokking stages asynchronously"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)

### Локальный коэффициент обучения (local learning coefficient, LLC) — 1 статей

- 2603.01192: [`"The key measure is the local learning coefficient (LLC)"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)

### Неограниченный рост логитов (logit scaling growth) — 1 статей

- 2501.04697: [`"a direction of uncontrolled logit growth"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-4)

### Низкочастотный фильтр градиента (low-pass gradient filter) — 1 статей

- 2405.20233: [`"low-pass filtered gradients which is added to the current"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p3-10)

### Низкоранговая факторизация матриц (low-rank matrix factorization) — 1 статей

- 2506.05718: [`"sparse recovery and low rank matrix factorization"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p4-3)

### LU-механизм (LU mechanism) — 1 статей

- 2210.01117: [`"the "LU mechanism" because training and test losses"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p1-3)

### Машинное разучивание (machine unlearning) — 1 статей

- 2512.03437: [`"*machine unlearning*, i.e., removing the influence of specified data without full retraining"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Прунинг по магнитуде для активной подсети (magnitude pruning) — 1 статей

- 2303.11873: [`"weight magnitude pruning (Mozer & Smolensky, 1989) to ﬁnd active"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p2-5)

### Многообразие стохастических процессов (manifold of stochastic processes) — 1 статей

- 2406.05335: [`"dimensional manifold of stochastic processes parametrized by"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md#fig-1) ↗

### Матричная энтропия (matrix Renyi entropy metric) — 1 статей

- 2311.06597: [`"The $\alpha$-order (Rényi) entropy for matrix $\mathbf{R}$ is defined as follows"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p3-4)

### Коллапс модели (model collapse) — 1 статей

- 2410.03569: [`"Innovation 2: Loss Regularization to Avoid Model Collapse"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md)

### Компромисс сложность–ошибка (model complexity/error tradeoff) — 1 статей

- 2310.17247: [`"\mathcal{L} = \text{error} + \text{complexity}"`](https://arxiv.org/abs/2310.17247)

### Снижение сложности модели (model complexity reduction) — 1 статей

- 2504.17243: [`"the intrinsic complexity of the model leveraging the absolute weight"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p2-1)

### Модульные представления (modular representations) — 1 статей

- 2512.03437: [`"post‑grokking models learn *more modular representations*"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Пути экспертов MoE (MoE expert pathways) — 1 статей

- 2506.21551: [`"expert choices across layers in MoE"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)

### Подавление моносемантичных нейронов (monosemantic neuron inhibition) — 1 статей

- 2503.23298: [`"actively inhibit monosemantic neurons in relatively small neural networks"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2)

### Многостадийная генерализация (multi-stage generalization) — 1 статей

- 2405.19454: [`"We observe a multi-stage progress in generalization"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p1-9)

### Мультизадачная эмерджентность (multi-task emergence) — 1 статей

- 2402.15175: [`"By extending our framework to multi-task learning"`](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/original/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.md#p3-2)

### Взаимная информация как мера прогресса (mutual information progress measure) — 1 статей

- 2408.08944: [`"higher-order mutual information to analyze the"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p1-2)

### Натуральный градиентный спуск (natural gradient descent) — 1 статей

- 2510.04930: [`"formal links to natural gradient descent"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p1-6)

### nGPT / гиперсфера (normalized Transformer hypersphere) — 1 статей

- 2603.05228: [`"a normalized Transformer that constrains all vectors to the unit hypersphere"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p4-2)

### Численное раздувание признаков (Numerical Feature Inflation, NFI) — 1 статей

- 2605.06152: [`"this mechanism Numerical Feature Inflation (N FI)"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-2)

### Разрыв OOD-генерализации (OOD generalization gap) — 1 статей

- 2403.03942: [`"generalize very differently to adversarial out-of-domain (OOD) evaluation sets"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#p1-5)

### Переопараметризация / глубина и грокинг (overparameterization / depth) — 1 статей

- 2506.05718: [`"depth makes it possible to grok or ungrok simply from"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p2-3)

### Принцип экономии / MDL (parsimony / MDL complexity) — 1 статей

- 2603.29262: [`"and the Minimum Description Length (MDL) principle"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-5)

### Метрика похожести путей (pathway similarity metric) — 1 статей

- 2506.21551: [`"one computes the pathway similarity"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)

### PCA-факторизация симметрии (PCA symmetry factorization) — 1 статей

- 2411.05353: "can yield a factorization of the modulus" (line 106)

### PCA-анализ траекторий (PCA trajectory analysis) — 1 статей

- 2602.16746: [`"Using PCA on attention weight trajectories and commutator defect analysis"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2)

### Перестановочная эквивариантность (permutation equivariance) — 1 статей

- 2407.12332: [`"no permutation-equivariant model can achieve small population error"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-2)

### Фазовая диаграмма (phase diagram) — 1 статей

- 2205.10343: [`"phase diagrams from a grid search of hyperparameters"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p2-1)

### Пуассоновская статистика (Poisson statistics) — 1 статей

- 2511.12768: [`"we apply Poisson and sub-Poisson statistics to quantify how words connect"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p1-4)

### Полисемантичность / суперпозиция (polysemanticity / superposition) — 1 статей

- 2503.23298: [`"polysemantic neurons are activated for multiple features"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-4)

### Пост-меморизационная динамика (post-memorization dynamics) — 1 статей

- 2511.01938: [`"post-memorization learning dynamics"`](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/original/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.md#p2-2)

### Предобусловленный градиентный спуск (preconditioned gradient descent) — 1 статей

- 2601.03162: [`"the impact of preconditioned gradient descent"`](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p1-2)

### Проблемно-специфичная регуляризация потерь (problem-specific loss regularization) — 1 статей

- 2410.03569: [`"Design a custom loss function with a penalty term specific to modular"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p2-3)

### Доказуемый грокинг в ридж-регрессии (provable grokking in ridge regression) — 1 статей

- 2601.19791: [`"To Grok Grokking: Provable Grokking in Ridge Regression"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md)

### Квадратичные сети (quadratic networks) — 1 статей

- 2603.01192: [`"a basin-selection perspective on grokking in quadratic networks"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)

### Относительная плоскостность (relative flatness) — 1 статей

- 2509.17738: [`"the Hessian trace normalized by the weight"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p2-2)

### Магнитуда residual stream (residual magnitude) — 1 статей

- 2603.05228: [`"unbounded residual magnitude and data-dependent attention routing"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2)

### Спад нормы как достаточное условие (robustness sufficient condition) — 1 статей

- 2311.06597: [`"the decrease of weight norm usually happens before the grokking on the test dataset, making it seemingly a sufficient condition for grokking but not a necessary condition"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p1-4)

### Граница выборочной сложности (sample complexity bound) — 1 статей

- 2407.12332: [`"can generalize with many fewer samples"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p3-8)

### Законы масштабирования (scaling laws) — 1 статей

- 2503.23298: [`"Studies on Scaling Laws (Henighan et al., 2020; Kaplan et al., 2020)"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-3)

### Селективный рост нормы нейронов (selective norm growth) — 1 статей

- 2303.11873: [`"a small subset of neurons undergoes rapid norm growth"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2)

### Метрика остроты минимума (sharpness metric) — 1 статей

- 2502.20763: [`"the traditionally used sharpness metric does not fully explain the intricate interplay and introduces information-theoretic metrics called entropy gap"`](../externals/2502.20763.information-theoretic-perspectives-on-optimizers/2502.20763.information-theoretic-perspectives-on-optimizers.card.md#p1-1) ↗

### Упрощённые границы решений (simplified decision boundaries) — 1 статей

- 2510.25966: "simplified decision boundaries in the input space" (line 68)

### Сингулярная теория обучения (Singular Learning Theory, SLT) — 1 статей

- 2603.01192: [`"through the lens of Singular Learning"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)

### Разрежённая аугментация данных (sparse data augmentation) — 1 статей

- 2410.03569: [`"Sparse data elements are critical for learning"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p7-1)

### Разрежённое восстановление (sparse recovery) — 1 статей

- 2506.05718: [`"Sparse Recovery"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md)

### Разрежённые решения и скрытый прогресс (sparse solutions / hidden progress) — 1 статей

- 2405.20233: [`"amplifying sparse solutions through hidden progress"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p9-2)

### Коллапс спектральной энтропии (spectral entropy collapse) — 1 статей

- 2604.13123: [`"norm expansion followed by entropy collapse"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2)

### Спектральный гейтинг (spectral gating) — 1 статей

- 2603.15492: [`"revealing a “Spectral Gating” mechanism that regulates the transition"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-2)

### Спектральная сигнатура осцилляций как предиктор (spectral signature predictor) — 1 статей

- 2306.13253: [`"We propose spectral signature to quantify the oscilla-"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p2-2)

### Ложные измерения индуцируют грокинг (spurious dimensions induce grokking) — 1 статей

- 2310.17247: [`"via the addition of dimensions containing spurious information"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1)

### Зондирование подгрупп (subgroup probing) — 1 статей

- 2601.21150: [`"capture subgroup structure within their internal representations which we can access via the linear probing"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p7-6) ↗

### Синергетическая подсеть (synergistic subnetwork) — 1 статей

- 2408.08944: [`"generalizing synergistic sub-network that is growing as it"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p3-3)

### Систематическая OOD-генерализация (systematic generalization) — 1 статей

- 2405.15071: [`"fail to systematically generalize for composition but succeed for comparison"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)

### Задачи: изображения, языки, графы (images / languages / graphs) — 1 статей

- 2405.20233: [`"diverse tasks involving images, languages,"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p1-2)

### Модель Изинга (Ising model task) — 1 статей

- 2510.25966: "Grokking in the Ising Model" (line 1)

### Ограниченный выход через температуру (temperature-bounded output) — 1 статей

- 2603.05228: [`"logit magnitudes are strictly bounded by the temperature parameter"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p6-7)

### Температурный softmax-сэмплинг (temperature softmax sampling) — 1 статей

- 2406.05335: [`"the $t$-th token $x_{t}$ is drawn from the softmax distribution"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md#p7-5) ↗

### Терминальная фаза обучения (terminal phase of training) — 1 статей

- 2206.04817: [`"Terminal Phase of Training (TPT)"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p1-3)

### Трёхстадийная динамика обучения (three-stage training dynamic) — 1 статей

- 2603.01968: [`"we identify a consistent three-stage training dynamic:"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-2)

### Разделение временных масштабов (time-scale separation) — 1 статей

- 2509.20829: [`"distinct time scales between fitting the training set"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)

### Униформность токенов (token uniformity) — 1 статей

- 2504.03162: [`"this optimization merely leads to token uniformity"`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p1-2)

### Эффект туннеля (tunnel effect) — 1 статей

- 2405.19454: [`"Emergence of *Tunnel* on various depth of models"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#fig-3)

### Униформное внимание / CBOW (uniform attention / bag-of-tokens) — 1 статей

- 2603.05228: [`"reducing the attention mechanism to a Continuous Bag-of-Words"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-4)

### Гипотеза универсальности (universality hypothesis) — 1 статей

- 2505.18266: [`"We propose a testable universality hypothesis"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2)

### Энтропия весов (weight entropy metric) — 1 статей

- 2411.05353: "a metric for the generalization ability of" (line 21)

### Сжатие внутриклассовой дисперсии (within-class variance contraction) — 1 статей

- 2509.20829: [`"population within-class variance is a key factor underlying both grokking"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)

### Задача XOR (XOR classification task) — 1 статей

- 2504.13292: [`"on a synthetic XOR task where delayed generalization"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-3)

### Ограничение нулевой суммы градиентов (zero-sum gradient constraint) — 1 статей

- 2605.06152: [`"This breaks the zero-sum constraint of gradients across classes"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-2)
