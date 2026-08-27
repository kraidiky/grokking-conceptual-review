# Возникновение признаков (feature emergence / feature learning)

[Обучение структурированных представлений](structured-representation-learning.md) ← предыдущая карточка, следующая → [Фурье-признаки и контуры](fourier-features-circuits.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Возникновение признаков** — появление в ходе обучения структурированных
внутренних представлений (признаков), которые кодируют устройство задачи и
обеспечивают обобщение. Эмпирически это впервые замечено в
[гроккинге](grokking.md) как выученные символьные эмбеддинги, раскрывающие
узнаваемую структуру математических объектов \[[1.1](#ref-1-1)\], причём
генерализация совпадает с возникновением структуры в эмбеддингах
\[[1.2](#ref-1-2)\]. В динамической формулировке то же явление называют
**обучением признаков** (feature learning) — «богатым» (rich) режимом в
противоположность «ленивому» (lazy), ядровому режиму, в который сеть переходит
поздно в обучении \[[1.3](#ref-1-3)\].

![t-SNE выходных эмбеддингов после гроккинга на модульном сложении: выученные признаки образуют регулярную числовую структуру (рис. 3 Power et al.)](assets/feature-emergence-tsne.png)

## Детализация

Термин объединяет два взгляда на одно явление. Эмпирический: во время гроккинга
скрытые слои перестраиваются от «запоминающего» состояния (см.
[фаза запоминания](memorization-phase.md)) к структурированным признакам — для
модулярной арифметики это круговые, фурье-подобные эмбеддинги, «карты признаков»
задачи \[[1.1](#ref-1-1)\]\[[1.5](#ref-1-5)\]. Эта перестройка признаков
предклассификационного слоя протекает всплеском и совпадает с резкими скачками
нормы весов — режимом [slingshot](slingshot.md) \[[1.6](#ref-1-6)\]. Динамический:
гроккинг трактуют как
переход от **ленивого режима** (lazy, ядрового — сеть ведёт себя как линейная
регрессия по касательному ядру, [neural tangent kernel](neural-tangent-kernel-ntk.md) / NTK, и признаки почти не
меняются) к **обучению признаков** (rich-режиму, где скрытые представления
заметно эволюционируют) \[[1.3](#ref-1-3)\]. Скорость этого перехода — «скорость
обучения признаков» — управляется масштабом выхода сети и объявлена ключевым
детерминантом гроккинга \[[1.3](#ref-1-3)\].

Механистически возникновение признаков читают как постепенное усиление
структурированных механизмов, закодированных в весах, — формирование обобщающего
контура (подсети, реализующей алгоритм задачи) с последующим удалением
запоминающих компонентов \[[1.5](#ref-1-5)\]. Тяготеющая к строгой теории версия
(Tian) выводит **доказуемые законы масштабирования возникновения признаков**:
динамика проходит стадии ленивого обучения, независимого и взаимодействующего
обучения признаков, а «возникающие признаки» отождествляются с локальными
максимумами энергетической функции — точками, к которым сходится градиентная
динамика скрытых узлов \[[1.4](#ref-1-4)\].

Само возникновение признаков часто описывают как
[фазовый переход](phase-transition.md): у Rubin и коллег полезные внутренние
представления появляются лишь после перехода, как смешанная фаза после фазового
перехода первого рода \[[3.4](#ref-3-4)\]; у Ersoy каждый такой переход открывает
ровно один новый обучаемый признак \[[3.3](#ref-3-3)\]. Родственные явления —
[эмерджентные способности](emergence.md) и [double descent](double-descent.md)
(немонотонная зависимость тестовой ошибки от размера модели или данных) — тоже
объясняют через момент, когда обобщающие признаки берут верх.

Главный спор — **что именно запускает переход в режим обучения признаков**. Рамку
«lazy → feature learning» оспаривают: Prieto и коллеги показывают, что без
регуляризации переход блокируется численной ошибкой софтмакса —
[Softmax Collapse](softmax-collapse.md), — и что многие аспекты ленивой рамки
(почему задачи вообще индуцируют ленивое обучение, зачем нужно затухание весов)
остаются необъяснёнными \[[2.1](#ref-2-1)\]. С другой стороны, рамку поддерживают
и уточняют: предобусловленный градиентный спуск сокращает ленивую фазу и ускоряет
вход в rich-режим \[[3.1](#ref-3-1)\], а сингулярная теория обучения выводит
формулы локального коэффициента обучения (LLC — local learning coefficient, мера
эффективной сложности решения) отдельно для ленивого режима и режима обучения
признаков \[[3.2](#ref-3-2)\].

## Альтернативные определения и нюансы

### A. Возникновение структуры в представлениях (эмпирическая трактовка)

Признак «возник», когда во внутренних представлениях (эмбеддингах, скрытых
активациях) появляется различимая, привязанная к задаче структура — круговые и
фурье-паттерны в модулярной арифметике, низкоразмерная геометрия
\[[1.1](#ref-1-1)\]\[[1.2](#ref-1-2)\]\[[1.5](#ref-1-5)\]. Источник различия:
критерий здесь — **наблюдаемая геометрия представлений**, а не форма кривой
обучения; момент возникновения датируют по появлению этой структуры (например, по
круговой укладке главных компонент эмбеддинга).

### B. Обучение признаков как режим (lazy → rich)

Возникновение признаков = **переход из ленивого, ядрового режима в богатый**, где
скрытые признаки эволюционируют, а не остаются фиксированными
\[[1.3](#ref-1-3)\]. Источник различия: **управляющий параметр — скорость
обучения признаков** (задаётся масштабом выхода сети); отложенность объясняется
рассогласованностью верхних собственных векторов начального NTK с целевой
функцией. Отличие фиксируется не по геометрии, а по тому, движется ли модель в
пространстве признаков или стоит в ядровом (линейном) приближении.

### C. Доказуемые законы масштабирования возникновения признаков (теоретико-динамическая)

Возникновение признаков описывают как последовательность стадий (ленивое обучение
→ независимое → взаимодействующее обучение признаков), где «возникающие признаки»
— это **локальные максимумы энергетической функции**, к которым сходится
градиентная динамика скрытых узлов \[[1.4](#ref-1-4)\]. Источник различия: признак
определён **формально-динамически** (как аттрактор энергии), что даёт доказуемые
законы масштабирования от гиперпараметров — затухания весов, темпа обучения,
размера выборки.

### Оспаривают

- **Триггер — не [переход lazy](lazy-to-rich-kernel-to-feature-learning.md)→rich, а Softmax Collapse** \[[2.1](#ref-2-1)\]: без
  регуляризации вход в режим обучения признаков блокируется численной ошибкой
  поглощения в софтмаксе; рамка «ленивое → обучение признаков» оставляет
  необъяснённым, почему задачи индуцируют ленивое обучение и зачем нужно затухание
  весов. Источник различия: причина отложенного возникновения признаков —
  численно-оптимизационная, а не свойство ядрового режима.

### Поддерживают

- **Ускорение входа в rich-режим предобусловливанием** \[[3.1](#ref-3-1)\]:
  предобусловленный градиентный спуск укорачивает ленивую фазу перед переходом в
  режим, богатый признаками. Источник различия: тот же переход lazy→rich, но
  управляемый обусловленностью оптимизации, а не масштабом выхода.
- **LLC как проба обоих режимов** \[[3.2](#ref-3-2)\]: аналитические формулы
  локального коэффициента обучения выведены отдельно для ленивого режима и режима
  обучения признаков, а траектория LLC отслеживает начало обобщения. Источник
  различия: дихотомия lazy / feature learning принимается как данность и делается
  измеримой через LLC.
- **Ступенчатое возникновение признаков через фазовые переходы**
  \[[3.3](#ref-3-3)\]: каждый фазовый переход первого рода по силе
  [L2-регуляризации](weight-decay.md) открывает ровно один новый обучаемый признак. Источник
  различия: признаки возникают дискретно, по одному на переход, а не одним
  скачком.
- **Признаки как пост-переходная фаза** \[[3.4](#ref-3-4)\]: полезные внутренние
  представления учителя появляются только после гроккинга, как смешанная фаза
  после фазового перехода первого рода. Источник различия: возникновение признаков
  привязано к моменту фазового перехода и резко отделено от до-переходного
  состояния.

## Ссылки

###### ref-1-1
**\[1.1\]** 2201.02177 — Power et al., «Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets». [`"We visualize the symbol embeddings learned by these networks and ﬁnd that they sometimes uncover recognizable structure of the mathematical objects represented by the symbols"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p2-3). *«[Мы визуализируем эмбеддинги символов, выученные этими сетями, и обнаруживаем, что они иногда выявляют узнаваемую структуру математических объектов, представленных этими символами](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p2-3)»*

###### ref-1-2
**\[1.2\]** 2205.10343 — Liu et al., «Towards Understanding Grokking: An Effective Theory of Representation Learning». [`"We observe that generalization coincides with the emergence of structure in the embeddings"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#fig-1). *«[Мы наблюдаем, что генерализация совпадает с появлением структуры в эмбеддингах](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#fig-1)»*

###### ref-1-3
**\[1.3\]** 2310.06110 — Kumar et al., «Grokking as the Transition from Lazy to Rich Training Dynamics». [`"late-time feature learning where a generalizing solution is identified after train loss is already low"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#p1-3). *«[уже при низкой потере на обучении, происходит позднее обучение признакам, на котором и отыскивается обобщающее решение](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.card.md#p1-3)»*\
Доп.: [`"the key determinants of grokking are the rate of feature learning"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#p1-3) — *«[ключевые определяющие факторы гроккинга — это скорость обучения признакам](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.card.md#p1-3)»*.

###### ref-1-4
**\[1.4\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». [`"Emerging features are the local maxima of an energy function"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p2-5). *«[Возникающие признаки — это локальные максимумы функции энергии](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p2-5)»*\
Доп.: [`"three key stages for the grokking behavior of 2-layer nonlinear networks: (I) Lazy learning, (II) independent feature learning and (III) interactive feature learning"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p1-2) — *«[три ключевые стадии поведения гроккинга у двухслойных нелинейных сетей: (I) **л**енивое обучение (**L**azy learning), (II) **н**езависимое обучение признакам (**i**ndependent feature learning) и (III) **в**заимодействующее обучение признакам (**i**nteractive feature learning)](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p1-2)»*.

###### ref-1-5
**\[1.5\]** 2301.05217 — Nanda et al., «Progress Measures for Grokking via Mechanistic Interpretability». [`"gradual amplification of structured mechanisms encoded in the weights, followed by the later removal of memorizing components"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-2). *«[постепенного усиления структурированных механизмов, закодированных в весах, с последующим удалением запоминающих компонент](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-2)»*

###### ref-1-6
**\[1.6\]** 2206.04817 — Thilak et al., «The Slingshot Mechanism: An Empirical Study of Adaptive Optimizers and the Grokking Phenomenon». [`"The features (pre-classiﬁcation layer) show rapid evolution as the weight norm transitions from rapid growth to a growth plateau"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p2-5). *«[Признаки (слой перед классификацией) обнаруживают быструю эволюцию в момент, когда норма весов переходит от быстрого роста к плато](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p2-5)»*

###### ref-1-7
**\[1.7\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Нюанс: вводит термин «комбинаторное обучение признакам» и выводит устройство возникновения признака из коэффициентов Фурье популяционного градиента; разбора того, что происходит с признаками после появления, в работе нет. [`"our findings comprise an elementary example of *combinatorial feature learning*: SGD can only successfully converge by learning a low-width sparse representation"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p3-5). *«[наши находки составляют элементарный пример *комбинаторного обучения признакам*: SGD может успешно сойтись, только выучив разрежённое представление малой ширины](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p3-5)»*

###### ref-1-8
**\[1.8\]** 2303.06173 — Davies, Langosco, Krueger, «Unifying Grokking and Double Descent». Нюанс: образец здесь — величина рамки, а не сети: параметры сигмоид расставлены руками, к опытным кривым не подгонялись, и «предпочтённый образец», на котором держится итоговый переход к обобщению, ни из чего не выводится. [`"In our model, a neural network consists of a set of $n$ patterns that develop over time during training."`](../papers/2303.06173.unifying-grokking-and-double-descent/2303.06173.unifying-grokking-and-double-descent.card.md#p2-8). *«[В нашей модели нейросеть состоит из набора в $n$ образцов, складывающихся во времени по ходу обучения](../papers/2303.06173.unifying-grokking-and-double-descent/2303.06173.unifying-grokking-and-double-descent.card.md#p2-8)»*\
Доп. (чем отличается от Heckel & Yilmaz 2020 и Pezeshki et al. 2021): [`"separating maximum predictiveness from learning speed, as well as introducing the notion of a “preferred” pattern"`](../papers/2303.06173.unifying-grokking-and-double-descent/2303.06173.unifying-grokking-and-double-descent.card.md#p4-4) — *«[разводит предельную предсказательность со скоростью выучивания, а также вводит понятие «предпочтённого» образца](../papers/2303.06173.unifying-grokking-and-double-descent/2303.06173.unifying-grokking-and-double-descent.card.md#p4-4)»*.

###### ref-1-9
**\[1.9\]** 2310.02541 — Xu et al. 2023. Нюанс: «признаки» здесь — средние гауссовских кластеров, то есть заданы устройством данных; выучивается лишь выстроенность нейронов, а направление задано случайным перевесом порядка $\sqrt{n}$ в том, сколько точек какого кластера активируют нейрон при инициализации. [`"the neurons gradually align with the core features $\pm\mu_{1}$ and $\pm\mu_{2}$, which is sufficient for generalization"`](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/original/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.md#p2-4). *«[нейроны постепенно выстраиваются вдоль ключевых признаков $\pm\mu_{1}$ и $\pm\mu_{2}$, чего достаточно для генерализации](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.card.md#p2-4)»*

###### ref-1-10
**\[1.10\]** 2311.07568 — Morwani et al. 2024, «Feature emergence via margin maximization: case studies in algebraic tasks». Переставляет вопрос с «как сеть считает» на «почему именно так» и даёт корпусу явный контрпример: сеть из $2p^{2}$ нейронов решает модульное сложение точно и при этом **плотна** в спектре (все веса — знаковые one-hot), откуда правильность классификации не влечёт структурированности признаков. Нюанс: речь только об устройстве предельного решения — ни порядка возникновения признаков, ни величин, измеряемых по ходу обучения, кроме самого зазора, в работе нет. [`"Note that even with this activation function, there are solutions that fit all the data points, but where the weights do not exhibit any sparsity in Fourier space—see Appendix D for an example construction. Such solutions, however, have lower margin and thus are not reached by training."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p3-8). *«[Заметим, что даже при таком возбуждении существуют решения, укладывающиеся во все точки данных, но веса которых не обнаруживают никакой разрежённости в фурье-пространстве, — пример построения см. в приложении D. Такие решения, однако, имеют меньший зазор и потому не достигаются обучением.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p3-8)»*\
Доп. (вывод приложения D): [`"This is to show that Fourier sparsity is not present in any correct classifier."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p22-3) — *«[Это показывает, что фурье-разрежённость не присутствует ни в каком правильном классификаторе.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p22-3)»*;
Доп. (исходный вопрос): [`"why does the network consistently prefer such Fourier-based circuits, amidst other potential circuits capable of performing the same modular addition function?"`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p3-3) — *«[почему сеть последовательно предпочитает такие фурье-контуры среди прочих возможных контуров, способных выполнять ту же функцию модульного сложения?](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p3-3)»*.

###### ref-1-11
**\[1.11\]** 2407.20199 — Mallinar et al., «Emergence in non-neural models: grokking modular arithmetic via average gradient outer product». Нюанс: AGOP придан модели, у которой своего выучивания признаков нет, — оттого признаки развиваются, пока обе потери молчат; объяснения тому, отчего AGOP выходит именно на циркулянт, работа не даёт. [`"Importantly, AGOP can be computed for any differentiable predictor, including those such as kernel machines that have no native feature learning mechanism."`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p6-1). *«[Существенно, что AGOP можно вычислить для любого дифференцируемого предсказателя, в том числе для таких, как ядерные машины, у которых своего устройства выучивания признаков нет](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p6-1)»*\
Доп. (Neural Feature Ansatz держится): [`"we find that the square root of the AGOP and the NFM are highly correlated (greater than $0.92$)"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p11-6) — *«[мы обнаруживаем, что квадратный корень из AGOP и NFM сильно связаны (более $0.92$)](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p11-6)»*.

###### ref-1-12
**\[1.12\]** 2406.06158 — Kunin et al., «Get rich quick: exact solutions reveal how unbalanced initializations promote rapid feature learning». Нюанс: признак быстрого обучения признакам — большое изменение расстояния Хэмминга между образцами активации при малом смещении в пространстве параметров; такое сочетание даёт именно upstream с малым $\tau$, а downstream при малом $\tau$ требует, наоборот, большого движения параметров. Что именно выучивается, работа не разбирает нигде, кроме габоровых фильтров CNN. [`"*Rapid feature learning* occurs from a small-$\tau$ upstream initialization that promotes faster learning in early layers, driving a large change in Hamming distance, but a small change in parameter space."`](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#fig-5). *«[*Быстрое обучение признакам* происходит при upstream-инициализации с малым $\tau$, которая способствует более быстрому обучению в ранних слоях, вызывая большое изменение расстояния Хэмминга, но малое изменение в пространстве параметров](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#fig-5)»*

###### ref-1-13
**\[1.13\]** 2503.10483 — Pomarico et al., «Grokking as an entanglement transition in tensor network machine learning». Нюанс: проверки, что классификатор пользуется именно этой картиной, нет — абляций и вмешательств в работе не проводится; сходство с картами значимости у методов объяснимости для vision-трансформеров заявлено как аналогия без сопоставления. [`"we observe positive magnetization corresponding to black pixels in Fig. 3, such that the shape of the corresponding object is recognized"`](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p10-4). *«[мы наблюдаем положительную намагниченность, отвечающую чёрным пикселям на Рис. 3, так что форма соответствующего предмета распознаётся](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p10-4)»*
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». Оспаривает: рамка «lazy → feature learning» неполна, истинный триггер отложенного возникновения признаков — Softmax Collapse. [`"several aspects in this framing of grokking remain unclear. These include why grokking tasks induce lazy training and why weight decay is often needed to enter the feature learning regime when using deeper models or cross-entropy (CE) loss"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-5). *«[несколько сторон такой рамки гроккинга остаются неясными. В их числе — почему задачи гроккинга вызывают ленивое обучение и почему для входа в режим обучения признакам при более глубоких моделях или кросс-энтропийной потере (CE) часто нужен weight decay](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-5)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2601.03162 — Jiang et al., «On the Convergence Behavior of Preconditioned Gradient Descent toward the Rich Learning Regime». Нюанс: подтверждает переход «lazy → feature-rich» и показывает, как предобусловливание сокращает ленивую фазу. [`"training lingers in the “lazy” regime before transitioning into the feature-rich regime"`](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p2-7). *«[обучение задерживается в «ленивом» режиме, прежде чем перейти в богатый признаками](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p2-7)»*

###### ref-3-2
**\[3.2\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». Нюанс: присоединяется к дихотомии lazy / feature learning, выводя формулы LLC для обоих режимов. [`"we derive analytic formulas for the LLC in shallow quadratic networks under both lazy and feature learning regimes"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2). *«[мы выводим аналитические выражения LLC для мелких квадратичных сетей и в ленивом режиме, и в режиме выучивания признаков](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)»*\
Доп. (поздний этап): [`"entering a rich feature-learning phase in which the representation changes sufficiently to uncover a structured generalising solution"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p7-8) — *«[вступлению в богатую фазу выучивания признаков, где представление меняется достаточно, чтобы вскрыть упорядоченное обобщающее решение](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p7-8)»*

###### ref-3-3
**\[3.3\]** 2606.17120 — Ersoy et al., «Noise-Driven Escape from Metastable Phases explains Grokking in Deep Neural Networks». Нюанс: возникновение признаков идёт ступенчато — каждый фазовый переход открывает новый обучаемый признак. [`"with each transition marking the onset of a new learnable feature"`](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/original/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.md#p1-2). *«[и каждый такой переход отмечает наступление нового выучиваемого признака](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.card.md#p1-2)»*

###### ref-3-4
**\[3.4\]** 2310.03789 — Rubin et al., «Grokking as a First Order Phase Transition in Two Layer Networks». Нюанс: полезные внутренние представления возникают именно после перехода, как смешанная фаза после фазового перехода первого рода. [`"the DNN generates useful internal representations of the teacher that are sharply distinct from those before the transition"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-2). *«[DNN порождает полезные внутренние представления учителя, резко отличные от тех, что были до перехода](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p1-2)»*\
Доп.: [`"A key property of deep neural networks (DNNs) is their ability to learn new features during training"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-2) — *«[Ключевое свойство глубоких нейросетей (DNN) — способность выучивать новые признаки в ходе обучения](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p1-2)»*.

###### ref-3-5
**\[3.5\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: одномерное знаковое представление осваивается первым устойчиво, но обобщает плохо, высокоразмерные точные — наоборот (типы 1 и 3 по Davies et al. 2022); строгого порядка по размерности нет, и собственное предсказание авторов о предпочтении простых признаков опровергнуто их же опытом. [`"While it is very easily learned, it also generalizes poorly. In contrast, higher dimensional faithful features are harder to learn but generalize better."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p8-5). *«[Хотя оно выучивается очень легко, оно и обобщает плохо. Напротив, более высокоразмерные точные (faithful) признаки труднее выучиваются, но лучше обобщают](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p8-5)»*\
Доп.: [`"Empirically, we found this claim to be false."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p8-2) — *«[Эмпирически мы обнаружили, что это утверждение ложно](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p8-2)»*.

###### ref-3-6
**\[3.6\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Даёт корпусу две меры: сдвиг нормированной ковариации признаков $\Delta_{C}^{\ell}$ (формула 3, вслед за определением Yang et al. 2022) и сдвиг двоичной картины активаций ReLU $\Delta_{A}^{\ell}$ (формула 4), заведённый именно потому, что ковариация не улавливает употребления нелинейностей. Обе — приращения за промежуток $T$, то есть скорость смены признаков, а не состояние. Нюанс: абсолютной шкалы у них нет, и обе к постановке с гроккингом не приложены — там признаки меряются долей изменившихся картин активаций и рангом выходов внимания. [`"we use changes in the *activation pattern* $A$ (Poole et al. 2016, a matrix of binary indicators of non-zero activations,) produced by a hidden layer in the network as an additional proxy for feature-learning"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#p4-4). *«[мы берём изменения картины активаций $A$ (Poole et al. 2016, матрицы двоичных указателей ненулевых активаций,), порождаемой скрытым слоем сети, как дополнительный заменитель выучивания признаков](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#p4-4)»*

###### ref-3-7
**\[3.7\]** 2604.06256 — Xu, «Spectral Edge Dynamics Reveal Functional Modes of Learning». Нюанс: наследование моды $\omega=26$ у $x^{2}+y^{2}$ показано ростом сосредоточенности $0.09\to 0.20$ на одном семени; абляции «только add / только mul / оба» нет. [`"the same modes that organize simple tasks can be recruited by more complex tasks, suggesting that functional modes are reusable primitives of learned computation"`](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/original/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.md#p12-6). *«[те же моды, которые устраивают простые задачи, могут привлекаться более сложными задачами, что позволяет предположить, что функциональные моды суть переиспользуемые первоосновы выученного вычисления](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.card.md#p12-6)»*

###### ref-3-8
**\[3.8\]** 2604.13082 — Gomezjurado Gonzalez, «The Long Delay to Arithmetic Generalization…». В Pythia-1.4B зонды на $T(n)\bmod 8$ и $n\bmod 16$ читают 1.00 на предобученной контрольной точке при точном совпадении ровно 0.000; разрыв закрывается за ~500 шагов дообучения. [`"in Pythia-1.4B fine-tuning installs the readout, not the features"`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p7-4). *«[в Pythia-1.4B дообучение устанавливает считывание, а не признаки](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p7-4)»*
###### ref-3-9
**\[3.9\]** 2309.03800 — Edelman, Goel, Kakade, Malach & Zhang 2023, «Pareto Frontiers in Neural Feature Learning: Data, Compute, Width, and Luck». Семейство доказуемых механизмов отбора признаков, управляемое одной ручкой — разрежённостью инициализации; ширина монотонно полезна во всех настройках, вопреки оценкам равномерной сходимости. [`"we obtain a family of algorithms which smoothly interpolate between the *small-data/large-width* and *large-data/small-width* regimes of tractability"`](../papers/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck.card.md#p7-5). *«[мы получаем семейство алгоритмов, гладко интерполирующих между режимами разрешимости *малые-данные/большая-ширина* и *большие-данные/малая-ширина*](../papers/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck.card.md#p7-5)»*\
Доп. (монотонность ширины): [`"increasing the model size yields exclusively positive effects on success probability, sample efficiency, and the number of serial steps to convergence"`](../papers/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck.card.md#p9-1) — *«[увеличение размера модели даёт исключительно положительные эффекты на вероятность успеха, выборочную эффективность и число последовательных шагов до сходимости](../papers/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck/2309.03800.pareto-frontiers-in-neural-feature-learning-data-compute-width-and-luck.card.md#p9-1)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2301.02679 — Gromov, «Grokking modular arithmetic». [`"grokking modular arithmetic corresponds to learning speciﬁc feature maps whose structure is determined by the task"`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p1-2). *«[гроккинг модульной арифметики отвечает выучиванию определённых карт признаков, структура которых определяется задачей](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p1-2)»*

**\[4.2\]** 2403.03942 — Bhaskar et al., «The Heuristic Core: Understanding Subnetwork Generalization in Pretrained Language Models». [`"attention heads emerge early in training and compute shallow, non-generalizing features"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#p1-2). *«[головы внимания возникают рано в обучении и вычисляют поверхностные, не обобщающие признаки](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#p1-2)»*

**\[4.3\]** 2405.15071 — Wang et al., «Grokked Transformers are Implicit Reasoners: A Mechanistic Journey to the Edge of Generalization». [`"the gradual formation of the generalizing circuit throughout grokking"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p2-3). *«[постепенное формирование обобщающего контура на протяжении гроккинга](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p2-3)»*

**\[4.4\]** 2411.05353 — Salah et al., «Controlling Grokking with Nonlinearity and Data Symmetry». [`"accompanied by the emergence of structure in the embeddings"`](../papers/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry/original/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry.md#p2-2). *«[сопровождается возникновением структуры во вложениях](../papers/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry.card.md#p2-2)»*

**\[4.5\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». [`"In contrast, polysemantic neurons are activated for multiple features"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-4). *«[Напротив, полисемантические нейроны возбуждаются несколькими признаками](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-4)»*

**\[4.6\]** 2510.25966 — Hutchison et al., «Grokking in the Ising Model». [`"sequentially identify global features of the input classes, which enables generalization to previously unseen patterns"`](../papers/2510.25966.grokking-in-the-ising-model/original/2510.25966.grokking-in-the-ising-model.md#p1-5). *«[последовательно выделяют глобальные признаки входных классов, что делает возможной генерализацию на прежде невиданные узоры](../papers/2510.25966.grokking-in-the-ising-model/2510.25966.grokking-in-the-ising-model.card.md#p1-5)»*

**\[4.7\]** 2511.01938 — Musat, «The Geometry of Grokking: Norm Minimization on the Zero-Loss Manifold». [`"circular representations emerge gradually during the post-memorization phase"`](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/original/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.md#p1-5). *«[круговые представления возникают постепенно в фазе после запоминания](../papers/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold/2511.01938.the-geometry-of-grokking-norm-minimization-on-the-zero-loss-manifold.card.md#p1-5)»*

**\[4.9\]** 2603.01968 — Hwang et al., «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"is characterized by the emergence of low-dimensional representations"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-2). *«[сопровождается возникновением малоразмерных представлений](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p1-2)»*

**\[4.10\]** 2604.00316 — Tomàs et al., «Breaking Data Symmetry is Needed For Generalization in Feature Learning Kernels». [`"the learned feature matrices encode specific elements of the symmetry group"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4). *«[выученные признаковые матрицы кодируют определённые элементы группы симметрий](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p1-4)»*\
Доп. (что именно выучивает AGOP): [`"RFM generalizes by recovering the underlying symmetry group action inherent in the data"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4) — *«[RFM обобщает, восстанавливая подлежащее действие группы симметрий, присущее данным](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p1-4)»*

**\[4.11\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation». Нюанс: ненейронный гроккинг RFM не опровергает и не подтверждает признак — общий вид перехода не описан. [`"RFM grokking and Transformer grokking may share a common feature-learning transition, whose architecture-independent signature remains to be characterised"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p18-3) — *«[гроккинг RFM и гроккинг трансформера могут разделять общий переход выучивания признаков, чей независимый от устройства признак ещё предстоит описать](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p18-3)»*

**\[4.12\]** 2511.12768 — Hong, Hong, «Evidence of Phase Transitions in Small Transformer-Based Language Models». Нюанс: складывание слова прослежено по приставкам — обрывки сливаются в устойчивую форму ровно на эпохе перехода. [`"single-letter (*y*) and two-letter (*yo*) fragments merge into the stable three-letter form (*you*) precisely at the transition epoch"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p12-2) — *«[однобуквенный (*y*) и двухбуквенный (*yo*) обрывки сливаются в устойчивую трёхбуквенную форму (*you*) ровно на эпохе перехода](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p12-2)»*

**\[4.13\]** 2605.08237 — Wang, Ying, Kanamori 2026, «Distributional Spectral Diagnostics for Localizing Grokking Transitions». Нюанс: AGOP взят как чужая величина для качественной сверки; покрытие контрольными точками — один прогон, TPR не определена, связь AGOP и невязки немонотонна. [`"AGOP-based diagnostics provide a parallel route under sufficient checkpoint coverage; in our setup, sparse checkpoint coverage prevents a fair quantitative comparison"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p4-5). *«[Диагностики на основе AGOP дают параллельный путь при достаточном покрытии контрольными точками; в нашей постановке разрежённое покрытие контрольными точками не позволяет честного количественного сравнения](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p4-5)»*


### Внешние работы

###### ref-5-1
**\[5.1\]** 2601.21150 — Внешняя работа (демотирована из корпуса): Kvinge et al., «Can Neural Networks Learn Small Algebraic Worlds? An Investigation Into the Group-theoretic Structures Learned By Narrow Models Trained To Predict Group Operations». [`"sudden drops in loss may correspond to a network gaining a specific capability"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p3-7). *«[внезапные падения потери могут отвечать обретению сетью отдельной способности](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p3-7)»*
