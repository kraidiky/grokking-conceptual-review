# Варианты регуляризации (regularization variants)

[Скорость обучения](learning-rate.md) ← предыдущая карточка, следующая → [Зона Златовласки](goldilocks-zone.md)

[Индекс карточек понятий](index.md), категория: [4. Факторы обучения и оптимизации](index.md#cat-4)\
→ Следующая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)\
← Предыдущая категория: [3. Задачи и наборы данных](modular-arithmetic.md)

## Определение

**Варианты регуляризации** (regularization variants) — семейство приёмов
регуляризации, ОТЛИЧНЫХ от канонического [weight decay](weight-decay.md)
(штрафа на квадрат L2-нормы весов), которые в контексте
[гроккинга](grokking.md) — отложенной генерализации, при которой сеть сначала
почти идеально запоминает обучающую выборку, а обобщение наступает на порядки
позже, — вызывают или ускоряют этот отложенный переход. Ключевое обобщение
состоит в том, что гроккинг не привязан именно к L2: если существует модель с
некоторым свойством P (property — например, разреженность весов или их малый
ранг), подгоняющая данные, то градиентный спуск с малой ненулевой
регуляризацией этого свойства (l1-норма или ядерная норма — сумма сингулярных
чисел матрицы весов) тоже приводит к гроккингу \[[1.1](#ref-1-1)\].

![Гроккинг под разными регуляризаторами — ℓ1, ℓ2 и ядерной нормой — на модульном сложении (рис. 1 Notsawo, Dumas, Rabusseau)](assets/regularization-variants-norms.png)

## Детализация

Помимо weight decay корпус описывает несколько самостоятельных
регуляризаторов, каждый из которых по-своему инициирует или ускоряет гроккинг.
Обобщённая теория свойства P показывает, что штраф на l1-норму (сумму модулей
весов, поощряющую разреженность) или на ядерную норму (поощряющую
низкоранговость) работает наравне с L2, а сама L2-норма перестаёт быть
надёжным предиктором, когда искомое свойство — не малая евклидова норма
\[[1.1](#ref-1-1)\]. Второй вариант — возмущающее обучение (perturbation-based
training): исходя из связи гроккинга с устойчивостью (robustness — способность
сети сохранять предсказание при малых возмущениях входа/весов), в обучение
вносят контролируемые возмущения, что ускоряет генерализацию
\[[1.2](#ref-1-2)\]. Третий — регуляризация-подобное торможение моносемантичных
нейронов (monosemantic neurons — нейроны, каждый из которых кодирует ровно один
человеко-понятный признак): подавление таких нейронов вносится как
регуляризирующий член и служит способом индуцировать [эмерджентность](emergence.md)
(скачкообразный рост качества при достижении порога масштаба)
\[[1.3](#ref-1-3)\]. Четвёртый — dropout (случайное отключение части нейронов
при обучении, классический регуляризатор), устойчивость к которому используется
как отдельный диагностический сигнал перехода от запоминания к генерализации
\[[1.4](#ref-1-4)\]. Вокруг всего семейства идёт спор о необходимости: часть
работ демонстрирует гроккинг вовсе без явной регуляризации — как переход из
«ленивого» (lazy, близкого к линейной [NTK](neural-tangent-kernel-ntk.md)-аппроксимации) в «богатый» (rich,
с [обучением признаков](feature-emergence-feature-learning.md)) режим \[[2.1](#ref-2-1)\], как результат ухода от края
численной устойчивости (устранение [коллапса softmax](softmax-collapse.md))
\[[2.3](#ref-2-3)\] или в аналитически решаемых постановках, где регуляризация
не требуется вовсе \[[2.2](#ref-2-2)\]; другие, напротив, показывают, что без
регуляризующего члена гроккинг в их постановке не наступает
\[[3.1](#ref-3-1)\].

Является ли регуляризация необходимым условием гроккинга — открытый спор с противоположными ответами в разных постановках; все версии собраны в карточке [когда регуляризация необходима](regularization-necessity.md).

## Альтернативные определения и нюансы

### A. Обобщённая регуляризация свойства P (l1 / ядерная норма)

Определение через искомое свойство модели, а не через конкретную норму:
регуляризатор — это любой малый штраф на свойство P (разреженность, малый ранг),
которым обладает подгоняющее данные решение; при достаточно большой выборке и
достаточно сложной модели такой штраф порождает гроккинг точно так же, как L2
\[[1.1](#ref-1-1)\]. Ключевой источник различия с трактовкой через
[weight decay](weight-decay.md) в том, что здесь драйвер — не евклидова норма
как таковая, а согласованность штрафа с истинным свойством обобщающего решения;
если P — не малая L2-норма, то L2-норма весов вообще не обязана убывать при
наступлении генерализации.

### B. Регуляризация через устойчивость: возмущающее обучение

Определение через устойчивость (robustness): затухание L2-нормы объясняется тем,
что оно повышает устойчивость сети, а раз так, то устойчивость можно повышать
напрямую — контролируемыми возмущениями в обучении, и это ускоряет генерализацию
\[[1.2](#ref-1-2)\]. Отличие от варианта A — источник различия не в норме и не в
свойстве весов, а в целевой величине устойчивости предсказаний: регуляризатор
здесь определяется через инвариантность выхода к возмущениям, а не через штраф на
параметры.

### C. Регуляризация-подобное торможение моносемантичных нейронов

Определение через архитектурную статистику активаций: регуляризатор вводится как
член, штрафующий моносемантичность нейронов, чтобы индуцировать
[эмерджентность](emergence.md) на сравнительно малых или предобученных моделях
\[[1.3](#ref-1-3)\]. Ключевое отличие — управляемая величина не норма и не
устойчивость, а степень моносемантичности представлений: гроккинг/эмерджентность
трактуется как следствие целенаправленного смещения распределения признаков по
нейронам.

### D. Dropout как регуляризатор и сигнал устойчивости

Определение через устойчивость к стохастическому прореживанию: dropout —
классический регуляризатор (случайное обнуление нейронов), а изменение точности
при варьировании его интенсивности («кривая устойчивости к dropout») служит
практической метрикой перехода от запоминания к генерализации
\[[1.4](#ref-1-4)\]. Отличие в том, что здесь регуляризатор работает не как явный
штраф в потерях, а как шум в прямом проходе, а его диагностическая ценность — в
том, как устойчивость к этому шуму меняется по ходу гроккинга.

### Оспаривают

Несколько работ оспаривают необходимость какой-либо явной регуляризации для
гроккинга. Гроккинг воспроизводится без регуляризации как переход из «ленивого» в
«богатый» режим обучения — так, что прежние теории его не объясняют
\[[2.1](#ref-2-1)\]. В аналитически решаемой постановке на границе линейной
разделимости авторы подчёркивают, что их объяснение не требует ни [slingshot](slingshot.md),
ни осцилляций, ни регуляризации \[[2.2](#ref-2-2)\]. Наконец, показано, что
типичная связка «нет регуляризации — нет гроккинга» объясняется не отсутствием
регуляризатора, а уходом задачи на край численной устойчивости: устранение
[коллапса softmax](softmax-collapse.md) даёт гроккинг без регуляризации вовсе
\[[2.3](#ref-2-3)\].

### Поддерживают

Обратная линия показывает, что в некоторых постановках без регуляризующего члена
гроккинга нет. В гауссовско-процессной постановке эксперименты без члена
сложности, возникающего из вариационного приближения, дают отсутствие гроккинга,
что демонстрирует необходимость «некоторой формы регуляризации» в этом сценарии
\[[3.1](#ref-3-1)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2506.05718 — Notsawo et al., «Grokking Beyond the Euclidean Norm of Model Parameters». [`"If there exists a model with a property P (e.g., sparse or low-rank weights) that fits the data, then GD with a small non-zero regularization of P"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p1-4). *«[если существует модель со свойством $P$ (скажем, разрежёнными или малоранговыми весами), приближающая данные, то GD с малым ненулевым сглаживанием по $P$](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.card.md#p1-4)»*

###### ref-1-2
**\[1.2\]** 2311.06597 — Tan et al., «Understanding Grokking Through A Robustness Viewpoint». [`"Motivated by improving robustness, we use perturbation-based training to speed up generalization."`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p2-4). *«[Побуждаемые повышением устойчивости, мы применяем обучение с возмущениями для ускорения генерализации.](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p2-4)»*

###### ref-1-3
**\[1.3\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». [`"proposes a regularization-style inhibition approach, which addresses the limitations of previous approaches in both efficiency and effectiveness"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2). *«[предлагает подавление в духе регуляризации, чем снимает ограничения прежних подходов и по действенности, и по затратам](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2)»*

###### ref-1-4
**\[1.4\]** 2507.11645 — Salah et al., «Tracing the Path to Grokking: Embeddings, Dropout, and Network Activation». [`"introduces  several  practical  metrics  including  variance  under  dropout,  robustness,  embedding"`](../papers/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation/original/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation.md#p1-5). *«[вводит несколько практических метрик, включая дисперсию под дропаутом, устойчивость, близость вложений](../papers/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation.card.md#p1-5)»*


###### ref-1-5
**\[1.5\]** 2210.15435 — Žunkovič, Ilievski, «Grokking phase transitions in learning local rules with gradient descent». Нюанс: единственная работа корпуса, где различие $L_{1}$ и $L_{2}$ выведено аналитически, а не измерено: $L_{2}$ действует на зазор мультипликативно, $L_{1}$ аддитивно, поэтому при бесконечно малом зазоре и конечном $N$ конечную вероятность нулевой тестовой ошибки удерживает только $L_{1}$. Численно то же держится на клеточном автомате (большая вероятность гроккинга, меньшее время, меньшая эффективная размерность), но перенос на глубокие сети заявлен как ожидание, а при обучении полной модели критический показатель начинает от регуляризации зависеть ($\nu\approx 2.5$ без неё, $\approx 0.7$ при $L_{1}$, $\approx 0.9$ при $L_{2}$). [`"The $L_{2}$ regularisation is multiplicative, and the $L_{1}$ regularisation is additive concerning the gap between the positive and negative samples $\epsilon$."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p8-7). *«[$L_{2}$-регуляризация мультипликативна, а $L_{1}$-регуляризация аддитивна по отношению к зазору $\epsilon$ между положительными и отрицательными образцами](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p8-7)»*\
Доп. (следствие в шаровой модели): [`"In contrast, for $\lambda_{1}>0$ the grokking probability can increase even above $90\%$ for any $D\geq 2$."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p14-4) — *«[Напротив, при $\lambda_{1}>0$ вероятность гроккинга может подниматься даже выше $90\%$ при любом $D\geq 2$](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p14-4)»*\
Доп. (где различие ломается): [`"Larger regularisation leads to a sharper transition to zero test error, in contrast with the linear case studied in the Section 3.2.2 and in the Section 4.3.1, where the critical exponent was found to be independent of the regularisation strengths $\lambda_{1,2}$."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p27-1) — *«[Большая регуляризация приводит к более резкому переходу к нулевой тестовой ошибке, в отличие от линейного случая, изученного в разделе 3.2.2 и в разделе 4.3.1, где критический показатель оказался не зависящим от сил регуляризации $\lambda_{1,2}$](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p27-1)»*.

###### ref-1-6
**\[1.6\]** 2402.15555 — Humayun, Balestriero, Baraniuk 2024, «Deep Networks Always Grok and Here is Why». Находит вмешательство, **гасящее** гроккинг, а не сдвигающее его: батч-нормализация убирает гроккинг состязательных образцов, убирает первый спуск и поднимает уровень LC в целом. Нюанс: обещанного обоснования нет — приложение B обрывается на формуле (11), а содержательное утверждение (нормировка придвигает границы разбиения вплотную к обучающим данным) заимствовано у Balestriero & Baraniuk 2022. [`"Batch normalization removes grokking."`](../papers/2402.15555.deep-networks-always-grok-and-here-is-why/original/2402.15555.deep-networks-always-grok-and-here-is-why.md#p9-3). *«[Батч-нормализация убирает гроккинг.](../papers/2402.15555.deep-networks-always-grok-and-here-is-why/2402.15555.deep-networks-always-grok-and-here-is-why.card.md#p9-3)»*\
Доп.: [`"With the presence of batchnorm, the LC values increase, the initial descent gets removed and most importantly, grokking does not occur for adversarial samples."`](../papers/2402.15555.deep-networks-always-grok-and-here-is-why/original/2402.15555.deep-networks-always-grok-and-here-is-why.md#fig-12) — *«[При наличии батч-нормализации значения LC растут, начальный спуск пропадает и, самое главное, гроккинг для состязательных образцов не происходит.](../papers/2402.15555.deep-networks-always-grok-and-here-is-why/2402.15555.deep-networks-always-grok-and-here-is-why.card.md#fig-12)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2310.06110 — Kumar et al., «Grokking as the Transition from Lazy to Rich Training Dynamics». Оспаривает необходимость регуляризации: гроккинг возникает без неё. [`"grokking without regularization in a way that cannot be explained by existing theories"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#p1-3). *«[гроккинг без всякой регуляризации и притом так, что имеющиеся теории этого объяснить не могут](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.card.md#p1-3)»*

###### ref-2-2
**\[2.2\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». Оспаривает: объяснение не требует регуляризации. [`"that our work requires none of these in order to exhibit or explain grokking"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-3). *«[нашей работе **ничего** из перечисленного не требуется ни для того, чтобы обнаружить гроккинг, ни для того, чтобы его объяснить](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-3)»*\
Доп. (контекст, чего именно не требуется): [`"role of regularization (Power et al., 2022; Liu et al., 2023)"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-3) — *«[роли регуляризации (Power et al., 2022; Liu et al., 2023)](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-3)»*.

###### ref-2-3
**\[2.3\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». Оспаривает: причина не в отсутствии регуляризации, а в численной устойчивости. [`"why grokking often does not happen without regularization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2). *«[почему гроккинг часто не происходит без регуляризации](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2)»*\
Доп.: [`"mitigating SC leads to grokking without regularization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2) — *«[устранение SC ведёт к гроккингу без регуляризации](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2310.17247 — Miller, O'Neill, Bui 2024, «Grokking Beyond Neural Networks: An Empirical Exploration with Model Complexity». Нюанс: необходимость сглаживания проверена отсечением — без члена сложности гроккинга нет ни у гауссова процесса, ни у линейной модели. [`"This demonstrates that some form of regularisation is needed in this scenario and provides further evidence for the possible necessity of the grokking mechanism we propose in Section 3"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p5-8). *«[Это показывает, что в такой постановке необходимо некоторое сглаживание, и даёт дальнейшее свидетельство возможной необходимости устройства гроккинга, предлагаемого нами в разделе 3](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p5-8)»*\
Доп. (линейная модель без ослабления весов): [`"we show that without weight decay we do not see grokking"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p5-3) — *«[мы показываем, что без ослабления весов гроккинга не видно](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p5-3)»*.


###### ref-3-2
**\[3.2\]** 2405.12755 — Golechha 2024, «Progress Measures for Grokking on Real-world Tasks». Приложение C даёт вмешательство, **гасящее** явление, а не сдвигающее его: с dropout на каждом линейном слое отложенная генерализация исчезает полностью, а вместе с ней и зона переобучения. Нюанс: предсказания и проверки нет — связь с третьей мерой заявлена словами «это согласуется с нашими опытами», хотя мера обнуляет веса «подобно dropout» и обучение с dropout должно бы прямо оптимизировать её; развёртки по доле $p$ нет, показано одно значение. [`"the addition of dropout makes delayed generalization (grokking) completely vanish"`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p9-8). *«[добавление dropout заставляет отложенную генерализацию (гроккинг) полностью исчезнуть](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p9-8)»*\
Доп. (значение): [`"We set the dropout fraction as $p=0.3$."`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p9-7) — *«[Мы задаём долю dropout как $p=0.3$.](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p9-7)»*

###### ref-3-3
**\[3.3\]** 2310.12977 — Humayun, Balestriero, Baraniuk 2023, «Training Dynamics of Deep Network Linear Regions». Первая в этой линии постановка, где батч-нормализация выступает вмешательством, гасящим явление: она убирает второй спуск местной сложности и одновременно поднимает её общий уровень — на обучающих, проверочных и случайных точках сразу (CNN и ResNet18 на CIFAR10). Нюанс: объяснение (нормировка послойно придвигает границы разбиения вплотную к обучающим данным) взято готовым у Balestriero & Baraniuk 2022 и здесь не выводится, а рост общего уровня меры не разобран вовсе. [`"The first thing to note is that BN largely removes the second descent for all the experiments."`](../papers/2310.12977.training-dynamics-of-deep-network-linear-regions/original/2310.12977.training-dynamics-of-deep-network-linear-regions.md#p7-2). *«[Первое, что стоит отметить, — BN в значительной мере убирает второй спуск во всех опытах.](../papers/2310.12977.training-dynamics-of-deep-network-linear-regions/2310.12977.training-dynamics-of-deep-network-linear-regions.card.md#p7-2)»*\
Доп. (подпись рисунка): [`"Batch normalization removes second descent while increasing overall LC."`](../papers/2310.12977.training-dynamics-of-deep-network-linear-regions/original/2310.12977.training-dynamics-of-deep-network-linear-regions.md#fig-4) — *«[Батч-нормализация убирает второй спуск, повышая при этом общую LC.](../papers/2310.12977.training-dynamics-of-deep-network-linear-regions/2310.12977.training-dynamics-of-deep-network-linear-regions.card.md#fig-4)»*.

###### ref-3-4
**\[3.4\]** 2211.13239 — Brown et al., «Relating Regularization and Generalization through the Intrinsic Dimension of Activations». Нюанс: dropout и weight decay сравниваются по общему следствию — оба единообразно понижают LLID, — и отсюда выведен размен: регуляризация понижает и ID ранних слоёв, то есть PID, а за порогом наилучшей регуляризации PID и точность обрушиваются вместе. Послойно различающийся регуляризатор при этом только назван желательным, но не предложен и не проверен; высокие значения отношения PID/LLID дают исключительно прогоны с weight decay, так что связь может быть межсемейственной. [`"one cannot arbitrarily decrease LLID through regularization and expect a guaranteed improvement in generalization"`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p2-1). *«[нельзя произвольно понижать LLID регуляризацией и ожидать гарантированного улучшения генерализации](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p2-1)»*\
Доп.: [`"soon after regularization is increased beyond the value that gives optimal validation accuracy, there is a rapid decrease in both PID and validation accuracy"`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p5-3) — *«[вскоре после увеличения регуляризации сверх значения, дающего наилучшую проверочную точность, наступает быстрое падение и PID, и проверочной точности](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p5-3)»*\
Доп. (призыв, оставленный без исполнения): [`"calls for more targeted regularization techniques that are layer-dependent"`](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/original/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.md#p4-1) — *«[призывает к более прицельным, послойно различающимся приёмам регуляризации](../papers/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations/2211.13239.relating-regularization-and-generalization-through-the-intrinsic-dimension-of-activations.card.md#p4-1)»*.

###### ref-3-5
**\[3.5\]** 2409.09281 — Lv et al., «Language Models “Grok” to Copy». Нюанс: единственная в корпусе проверка переноса регуляризаций из малых алгоритмических задач в предобучение LM — два приёма, по одному значению параметра, без выравнивания по вычислительным затратам и без указанного числа семян; Grokfast назван в перечне работ, но не испробован. [`"dropout accelerates the grokking process, advancing the abrupt accuracy increase from 15B tokens to 10B tokens, albeit with increased fluctuation in the evolutionary dynamics"`](../papers/2409.09281.language-models-grok-to-copy/2409.09281.language-models-grok-to-copy.card.md#fig-7). *«[dropout ускоряет процесс гроккинга, придвигая резкий рост точности с 15 млрд токенов к 10 млрд токенов, хотя и с возросшей изменчивостью хода развития](../papers/2409.09281.language-models-grok-to-copy/2409.09281.language-models-grok-to-copy.card.md#fig-7)»*\
Доп. (состав): [`"We train models using (1) 10% attention dropout and (2) weight decay ($\lambda=0.1$)."`](../papers/2409.09281.language-models-grok-to-copy/2409.09281.language-models-grok-to-copy.card.md#p5-1) — *«[Мы обучаем модели с (1) 10 % dropout на внимании и (2) weight decay ($\lambda=0.1$).](../papers/2409.09281.language-models-grok-to-copy/2409.09281.language-models-grok-to-copy.card.md#p5-1)»*

###### ref-3-6
**\[3.6\]** 2506.12284 — Walker et al., «GrokAlign: Geometric Characterisation and Acceleration of Grokking». Вводит GrokAlign — штраф на среднюю норму Фробениуса входо-выходных якобианов, вычисленных на обучающих данных, с коэффициентом λ_Jac; в отличие от прочих вариантов корпуса штраф наложен не на веса, а на якобиан, и объявлен управляющей ручкой в обе стороны: при удержании нормы якобиана высокой тот же приём генерализацию тормозит. Нюанс: сама регуляризация якобиана не нова и авторами это оговорено; правила выбора λ_Jac нет — по опытам он меняется на четыре порядка (1,0, 0,001, 0,0001) без разбора чувствительности; сравнения с штрафами на ранг или норму весовых матриц нет. [`"By explicitly enforcing the Jacobian norm constraint of Theorem 2 -- a method we introduce as GrokAlign -- we can guarantee that optimising the training objective will lead to a grokked network."`](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/original/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.md#p4-1). *«[Явно навязывая ограничение на норму якобиана из теоремы 2 — метод, который мы вводим как GrokAlign, — мы можем гарантировать, что оптимизация обучающей цели приведёт к гроккнутой сети.](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.card.md#p4-1)»*\
Доп. (родословная приёма): [`"forcing the network to maintain a low Lipschitz constant, which is equivalent to having a low Jacobian norm, has been identified to balance the observed trade-off between generalisation and robustness"`](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/original/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.md#p4-2) — *«[принуждение сети удерживать низкую константу Липшица, что равносильно низкой норме якобиана, было определено как уравновешивающее наблюдаемый размен между генерализацией и устойчивостью](../papers/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking/2506.12284.grokalign-geometric-characterisation-and-acceleration-of-grokking.card.md#p4-2)»*.
###### ref-3-7
**\[3.7\]** 2602.12039 — Beck, Bar-Sinai & Levi 2026, «The Implicit Bias of Logit Regularization». Label smoothing формально сведён к выпуклому логит-штрафу ($f_{\mathrm{LS}}$ при $\alpha=2\varepsilon$ в двоичном случае), а все выводы — скучивание, дискриминант Фишера, сдвиг порога, гроккинг — заявлены для целого класса выпуклых логит-регуляризаторов, к которому принадлежат и энтропийные, и логит-норменные штрафы. [`"We show that label smoothing (LS) can be written as a convex *logit regularizer* added to cross-entropy"`](../papers/2602.12039.the-implicit-bias-of-logit-regularization/original/2602.12039.the-implicit-bias-of-logit-regularization.md#p12-7). *«[Мы показываем, что label smoothing (LS) можно записать как выпуклый *логит-регуляризатор*, добавленный к кросс-энтропии](../papers/2602.12039.the-implicit-bias-of-logit-regularization/2602.12039.the-implicit-bias-of-logit-regularization.card.md#p12-7)»*.


###### ref-3-8
**\[3.8\]** 2601.22450 — Huang, Mirzasoleiman 2026, «Tuning the Implicit Regularizer of Masked Diffusion Language Models: Enhancing Generalization via Insights from $k$-Parity». Упорядочиватель, встроенный в саму цель обучения: MD-потеря аналитически разлагается в $P_{S}\mathbb{E}_{S}[\ldots(f_\theta-f^{*})^{2}]+P_{N}\mathbb{E}_{N}[\ldots f_\theta^{2}]$, где второе слагаемое требует от модели молчать на информационно невосстановимых входах — регуляризация без всякого явного штрафа на веса, закрывающая, по замыслу, дорогу к чистому запоминанию; распространено на перекрёстную энтропию, чтобы механизм не выглядел артефактом квадратичной потери. Нюанс: weight decay при этом закреплён на $0.1$ во всех прогонах — взаимодействие двух упорядочивателей не проверено (прогона при нулевом weight decay нет), а на порождающих задачах лучшим оказывается диапазон почти полного маскирования, который собственная теория относит к шуму. [`"The decomposition in Theorem 4.3 reveals that the MD objective functions as a composite loss with a built-in regularizer."`](../papers/2601.22450.tuning-the-implicit-regularizer-of-masked-diffusion-language-models-enhancing-generalization-via-insights-from-k-parity/original/2601.22450.tuning-the-implicit-regularizer-of-masked-diffusion-language-models-enhancing-generalization-via-insights-from-k-parity.md#p5-4). *«[Разложение теоремы 4.3 раскрывает, что MD-цель функционирует как составная потеря со встроенным упорядочивателем.](../papers/2601.22450.tuning-the-implicit-regularizer-of-masked-diffusion-language-models-enhancing-generalization-via-insights-from-k-parity/2601.22450.tuning-the-implicit-regularizer-of-masked-diffusion-language-models-enhancing-generalization-via-insights-from-k-parity.card.md#p5-4)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2301.02679 — Gromov, «Grokking modular arithmetic». [`"Various forms of regularization improve how quickly grokking happens."`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p1-5). *«[Различные формы регуляризации улучшают то, насколько быстро наступает гроккинг](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p1-5)»*

**\[4.2\]** 2410.03569 — Saxena et al. 2024, «Making Hard Problems Easier with Custom Data Distributions and Loss Regularization: A Case Study in Modular Arithmetic». [`"two changes to the training process—using custom training data distributions and problem-specific loss regularization—may help models better learn modular arithmetic"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p2-2). *«[два изменения в ходе обучения — свои распределения обучающих данных и сглаживание потери, приспособленное к задаче, — способны помочь моделям лучше выучить остаточную арифметику](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.card.md#p2-2)»*

**\[4.3\]** 2504.13292 — Xu et al., «Let Me Grok For You: Accelerating Grokking». [`"regularization methods including no regularization, weight decay, and dropout"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p2-4). *«[приёмам регуляризации, включая её отсутствие, weight decay и dropout](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p2-4)»*

**\[4.4\]** 2601.03162 — Jiang et al., «On the Convergence Behavior of Preconditioned Gradient Descent Toward the Rich Learning Regime». [`"attempt to explain the delay including regularization with weights, dynamics of adaptive optimizers, and circuit efficiencies"`](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p1-5). *«[объяснить задержку: упорядочение весами, динамика приспосабливающихся оптимизаторов, действенность контуров](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p1-5)»*

**\[4.5\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: гроккинг групповой композиции получен и при dropout, и меры прогресса там работают, — но всё это сказано одной фразой со ссылкой на Nanda et al. 2023, без чисел, кривых и таблицы. [`"Other regularizers are also capable of exhibiting grokking."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p7-1). *«[Другие регуляризаторы тоже способны проявлять гроккинг](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p7-1)»*

**\[4.6\]** 2505.20172 — Boursier et al., «A Theoretical Framework for Grokking: Interpolation followed by Riemannian Norm Minimisation». Лишь упоминает: перенос разбора на иные регуляризаторы, в частности на SAM, объявлен одной фразой заключения и хеджирован («мы полагаем»); ни выкладки, ни опыта за ним не стоит. [`"we believe empirical observations reported for training with Sharpness-Aware Minimization (1, p. 7) may also be interpreted through the lens of grokking, albeit driven by SAM-style regularisation rather than standard weight decay"`](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p10-6). *«[мы полагаем, что опытные наблюдения, сообщённые для обучения с Sharpness-Aware Minimization (1, с. 7), тоже могут быть истолкованы через призму гроккинга, хотя и движимого регуляризацией в духе SAM, а не обычным weight decay](../papers/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation/2505.20172.a-theoretical-framework-for-grokking-interpolation-followed-by-riemannian-norm-minimisation.card.md#p10-6)»*

**\[4.7\]** 2603.07323 — Truong, Truong, «Norm-Hierarchy Transitions in Representation Learning…». Нюанс: граница применимости объявлена самими авторами. [`"Theory assumes $\ell_{2}$ regularisation; dropout may produce transitions through different pathways."`](../papers/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts.card.md#p15-4). *«[теория предполагает $\ell_{2}$-регуляризацию; dropout может давать переходы иными путями](../papers/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts.card.md#p15-4)»*
