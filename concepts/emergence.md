# Эмерджентность / эмерджентные способности (emergence / emergent abilities)

[Эффект рогатки / слингшот](slingshot.md) ← предыдущая карточка, следующая → [Двойной спуск](double-descent.md)

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

**Эмерджентные способности** — качественно новые способности модели, которые
появляются резко (скачкообразно) при пересечении критического порога **масштаба**
(число параметров, объём данных, вычислений) \[[1.1](#ref-1-1)\]\[[3.1](#ref-3-1)\].
В исследованиях [гроккинга](grokking.md) эмерджентность изучают как ту же
скачкообразную появляемость способности, но управляемую **временем обучения** на
малых алгоритмических задачах \[[1.2](#ref-1-2)\]\[[1.3](#ref-1-3)\].

## Детализация

Термин пришёл из исследований больших языковых моделей: при росте **масштаба**
(число параметров, объём данных, вычислений) новая способность появляется
внезапно, а не плавно \[[1.1](#ref-1-1)\]\[[3.1](#ref-3-1)\]. Гроккинг служит
контролируемой «лабораторной» моделью того же явления, но на малых
алгоритмических задачах и без роста масштаба: гроккинг «напоминает эмерджентное
поведение LLM» \[[1.3](#ref-1-3)\].

Механизм, связывающий эмерджентность с гроккингом, — **конкуренция двух контуров**
(двух подсетей внутри одной модели): «запоминающего», который просто хранит
обучающие примеры, и «обобщающего», который реализует общий алгоритм задачи.
Видимый скачок способности — это момент, когда обобщающий контур вытесняет
запоминающий; корпус разбирает это как [эффективность контуров](circuit-efficiency.md). Та же рамка объединяет гроккинг с [double descent](double-descent.md)
(немонотонной зависимостью тестовой ошибки от размера модели или данных) и
эмерджентными способностями \[[1.2](#ref-1-2)\]\[[1.5](#ref-1-5)\]\[[1.6](#ref-1-6)\].

Формально эмерджентность описывают как [фазовый переход](phase-transition.md). В
информационно-теоретической версии его триггер — **синергия признаков**:
формальная величина из разложения совместной информации (partial information
decomposition) — та доля информации о цели, которую признаки несут только
совместно и которой нет ни у одного по отдельности. Её синергетический вклад
резко нарастает в точке перехода \[[1.4](#ref-1-4)\].

Главный спор — **есть ли здесь настоящий разрыв**: истинный скачок на кривой
способности (а не просто крутой, но гладкий подъём) — или лишь его видимость. Одни показывают, что резкость —
**артефакт метрики**: «мираж», возникающий из-за жёстких порогов, которыми
определяют «accuracy»; на гладкой метрике переход размазывается
\[[2.1](#ref-2-1)\]. Другие доказывают, что переход **подлинный**: он синхронно проявляется в наборе
**непрерывных [параметров порядка](order-parameter.md)**. Параметр порядка — понятие из статистической
физики: величина, скачком меняющаяся в точке фазового перехода; здесь это
внутренние скалярные метрики модели, не зависящие от порога accuracy. Их
одновременный разрыв метрический артефакт создать не мог бы \[[3.1](#ref-3-1)\].

## Альтернативные определения и нюансы

### A. Эмерджентные способности при масштабе (scale-driven)

Каноническая трактовка из литературы об LLM (Wei et al.): новая способность
возникает **резко при пересечении порога масштаба** — параметров, данных или
вычислений \[[1.1](#ref-1-1)\]\[[3.1](#ref-3-1)\]. Источник различия: движущий
параметр — **масштаб**, а появление разрывно (ступенькой на кривой способности).

### B. Гроккинг как time-driven аналог эмерджентности

Гроккинг (отложенная генерализация на малых задачах) трактуют как **управляемую
временем обучения** версию эмерджентности: то же внезапное появление способности,
но без роста масштаба; отсюда объединение гроккинга, double descent и
эмерджентных способностей единой рамкой конкуренции контуров
\[[1.2](#ref-1-2)\]\[[1.3](#ref-1-3)\]\[[1.5](#ref-1-5)\]. Источник различия:
движущий параметр — **время обучения**, а не масштаб; явление изучается
механистически в toy-моделях.

### C. Эмерджентность как структурное событие (фазовый переход / контур)

Определение через конкретный внутренний механизм, а не через форму кривой:
эмерджентность = **фазовый переход**, вызванный синергетическими взаимодействиями
\[[1.4](#ref-1-4)\], или **появление генерализующего контура** в ходе обучения
\[[1.6](#ref-1-6)\]. Источник различия: эмерджентность привязана к измеримому
структурному событию внутри сети, а не к скачку внешней метрики.

### Оспаривают

- **Эмерджентность как метрический артефакт («мираж»)** \[[2.1](#ref-2-1)\]:
  резкость кривой точности возникает лишь из-за порогов, которыми определяют
  «accuracy»; при гладкой метрике переход размазывается. Источник различия:
  эмерджентность — не свойство модели, а артефакт выбора метрики.

### Поддерживают

- **Эмерджентность как подлинный переход** \[[3.1](#ref-3-1)\]: реальность
  доказывается синхронными разрывами набора **непрерывных** параметров порядка
  (не зависящих от порога accuracy) — прямое опровержение критики «emergence is a
  mirage». Источник различия: со-локализованные разрывы независимых непрерывных
  проб, которые метрический артефакт не создал бы.

## Ссылки

###### ref-1-1
**\[1.1\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». [`"the phenomenon of a rapid performance increase once the model scale reaches a threshold"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2). *«[явление быстрого роста качества, как только масштаб модели достигает порога](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2)»*

###### ref-1-2
**\[1.2\]** 2402.15175 — Huang et al., «Unified View of Grokking, Double Descent and Emergent Abilities: A Perspective from Circuits Competition». [`"demonstrating how algorithm tasks can be turned into emergent abilities"`](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/original/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.md#p1-2). *«[показывая, как алгоритмические задачи можно превратить в эмерджентные способности](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.card.md#p1-2)»*

###### ref-1-3
**\[1.3\]** 2303.11873 — Merrill et al., «A Tale of Two Circuits: Grokking as Competition of Sparse and Dense Subnetworks». [`"grokking resembles so-called emergent behavior in large language models"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-4). *«[гроккинг напоминает так называемое эмерджентное поведение больших языковых моделей](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p1-4)»*

###### ref-1-4
**\[1.4\]** 2408.08944 — Clauw, Stramaglia, Marinazzo 2024, «Information-Theoretic Progress Measures reveal Grokking is an Emergent Phase Transition». [`"We attribute grokking to an emergent phase transition caused by the synergistic interactions between neurons as a whole"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p1-2). *«[Мы приписываем гроккинг возникающему фазовому переходу, вызванному соработающими взаимодействиями нейронов как целого](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.card.md#p1-2)»*

###### ref-1-5
**\[1.5\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent». [`"grokking has been linked to phenomena such as double descent and emergent abilities"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p4-1). *«[гроккинг связывался с явлениями вроде двойного спуска и возникающих способностей](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p4-1)»*

###### ref-1-6
**\[1.6\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits». [`"this transition is driven by the emergence of a **“Generalization Circuit”**"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p1-7). *«[этот переход движим возникновением «контура генерализации»](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p1-7)»*

###### ref-1-7
**\[1.7\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Нюанс: эмерджентность разбирается с вычислительной стороны — как вопрос об успехе градиентной оптимизации, а не о выразительной ёмкости; перенос на настоящие данные оставлен открытым. [`"There is mounting evidence of *emergent phenomena* in the capabilities of deep learning methods as we scale up datasets, model sizes, and training times."`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p1-2). *«[Свидетельств *эмерджентных явлений* в возможностях методов глубокого обучения при увеличении наборов данных, размеров моделей и времени обучения становится всё больше](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p1-2)»*\
Доп.: [`"the extent to which non-synthetic tasks (in, e.g., natural language processing and program synthesis) embed within them parity-like subtasks of exhaustive combinatorial search"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p11-1) — *«[насколько несинтетические задачи (например, в обработке естественного языка и синтезе программ) содержат внутри себя чётностеподобные подзадачи полного комбинаторного перебора](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p11-1)»*.

###### ref-1-8
**\[1.8\]** 2407.20199 — Mallinar et al., «Emergence in non-neural models: grokking modular arithmetic via average gradient outer product». Нюанс: возникновение приписано целиком выучиванию признаков; по оси вычислений «мираж» Schaeffer et al. опровергается, по оси объёма данных авторы его допускают сами. [`"we show that sharp emergence in modular arithmetic arises entirely from feature learning, independently of other aspects of modeling and training, and is not predicted by the standard measures of progress"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p2-2). *«[мы показываем, что резкое возникновение в модульной арифметике проистекает целиком из выучивания признаков, независимо от прочих сторон построения модели и обучения, и не предсказывается стандартными мерами продвижения](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p2-2)»*\
Доп. (авторская уступка): [`"unlike the emergence with respect to compute, emergence with respect to the training data size may be a “mirage” in the sense of [38]"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p15-6) — *«[в отличие от возникновения по вычислениям, возникновение по объёму обучающих данных может быть «миражом» в смысле [38]](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p15-6)»*\
Доп. (о нескольких навыках): [`"it is possible that these skills are grokked at different rates"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p10-1) — *«[возможно, что эти навыки грокаются с разной скоростью](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p10-1)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2310.06110 — Kumar et al., «Grokking as the Transition from Lazy to Rich». Оспаривает разрывность эмерджентности: резкость есть артефакт метрики. [`"“Grokking” on the corresponding accuracy curves is thus a mirage arising from sweeping over thresholds"`](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/original/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.md#fig-18). *«[«Гроккинг» на соответствующих кривых точности тем самым есть мираж, возникающий от перебора порогов](../papers/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics/2310.06110.grokking-as-the-transition-from-lazy-to-rich-training-dynamics.card.md#fig-18)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2511.12768 — Hong et al., «Evidence of Phase Transitions in Small (Language) Models». Нюанс: эмерджентность — подлинный переход, доказываемый синхронными разрывами непрерывных параметров порядка (опровержение «мираж»). [`"new capabilities appear abruptly once models surpass critical thresholds of scale"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p1-4). *«[новые умения появляются внезапно, как только модель переходит решающие пороги масштаба](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p1-4)»*\
Доп.: [`"a genuine phase-transition-like reorganization rather than a gradual drift"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p7-4) — *«[подлинной перестройке в духе фазового перехода, а не о постепенном сносе](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p7-4)»*.

###### ref-3-2
**\[3.2\]** 2308.15594 — Charton, «Learning the greatest common divisor: explaining transformer predictions». Нюанс: редкий случай, когда содержание внезапно появляющейся способности установлено поимённо (проверка делимости на новое простое) и без обращения к весам; управляющая величина — время обучения, а не масштаб: качество одинаково от 300 тысяч до 700 миллионов параметров. Слова «emergent» автор не употребляет. [`"Experiments indicate that **transformers learn a sieve algorithm for computing GCD**."`](../papers/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions/original/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions.md#p9-4). *«[Опыты указывают на то, что **трансформеры выучивают алгоритм решета для вычисления НОД**](../papers/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions.card.md#p9-4)»*\
Доп.: [`"trained model performance is stable over a wide range of model size (300 thousands to 700 millions parameters)"`](../papers/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions/original/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions.md#p13-1) — *«[качество обученной модели устойчиво в широком диапазоне размеров модели (от 300 тысяч до 700 миллионов параметров)](../papers/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions.card.md#p13-1)»*.

###### ref-3-3
**\[3.3\]** 2409.09281 — Lv et al., «Language Models “Grok” to Copy». Поддерживает time-driven трактовку: скачок способности получен при неизменных 162 млн параметров, ось перехода — число шагов обновления и пройденных токенов. Нюанс: слов «emergent» и «emergence» в работе нет, порогов масштабирования она не измеряет, а сама формулировка хеджирована («мы предполагаем»). [`"the grokked context copying doesn’t emerge until the optimization reaches a specific intensity"`](../papers/2409.09281.language-models-grok-to-copy/2409.09281.language-models-grok-to-copy.card.md#p3-4). *«[гроккнутое копирование из контекста не наступает, пока оптимизация не достигнет определённой силы](../papers/2409.09281.language-models-grok-to-copy/2409.09281.language-models-grok-to-copy.card.md#p3-4)»*

###### ref-3-4
**\[3.4\]** 2603.07323 — Truong, Truong, «Norm-Hierarchy Transitions in Representation Learning…». Нюанс: объяснение через бюджет обучения и разрыв норм; авторы сами называют это догадкой. [`"We stress that this is a theoretical conjecture generating testable predictions; empirical verification at scale is left to future work."`](../papers/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts.card.md#p14-5). *«[Мы подчёркиваем, что это теоретическая догадка, порождающая проверяемые предсказания; опытная проверка в масштабе оставлена дальнейшей работе.](../papers/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts/2603.07323.norm-hierarchy-transitions-in-representation-learning-when-and-why-neural-networks-abandon-shortcuts.card.md#p14-5)»*
###### ref-3-5
**\[3.5\]** 2310.17247 — Miller, O'Neill & Bui, «Grokking Beyond Neural Networks: An Empirical Exploration with Model Complexity». Нюанс: мост от нейросетевой эмерджентности к произвольным обучающимся системам: явление воспроизведено у гауссовых процессов, линейной регрессии и байесовой сети, а его условием объявлено лишь то, что поиском решения правят сложность и ошибка. [`"grokking is not limited to neural networks but occurs in other settings such as Gaussian process (GP) classification, GP regression, linear regression and Bayesian neural networks"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1). *«[гроккинг не ограничен нейронными сетями, а возникает и в иных условиях — при распознавании гауссовым процессом (GP), при регрессии гауссовым процессом, при линейной регрессии и у байесовых нейронных сетей](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1)»*\
Доп.: [`"grokking may be possible in any model where solution search is guided by complexity and error"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1) — *«[гроккинг возможен у всякой модели, где поиск решения направляется сложностью и ошибкой](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1)»*.


###### ref-3-6
**\[3.6\]** 2307.09550 — Gokhale 2023, «The semantic landscape paradigm for neural networks». Возникновение с масштабом объяснено перколяцией по $N$: рост числа параметров расширяет «словарь» сети (узлы-модели), и путь к моделям низкой погрешимости открывается лишь при достаточно большом $N$; в споре о «мираже эмерджентности» (Schaeffer et al.) занята чёткая позиция — эмерджентность должна означать появление *способности*, независимо от метрик потерь, и устанавливается лишь механистическим разбором представлений. Нюанс: опытной части нет вовсе — все четыре рисунка суть схемы, и «show» аннотации относится к рассуждению, а не измерению. [`"The only real way to determine whether a new capability has emerged is to look under the hood and figure out what has changed in the network’s learned representations."`](../papers/2307.09550.the-semantic-landscape-paradigm-for-neural-networks/original/2307.09550.the-semantic-landscape-paradigm-for-neural-networks.md#p10-2). *«[Единственный настоящий способ определить, возникла ли новая способность, — заглянуть под капот и выяснить, что изменилось в выученных представлениях сети](../papers/2307.09550.the-semantic-landscape-paradigm-for-neural-networks/2307.09550.the-semantic-landscape-paradigm-for-neural-networks.card.md#p10-2)»*\
Доп. (роль $N$): [`"Thus, the effect of increasing $N$ is to increase the number of nodes on the graph of heuristic models."`](../papers/2307.09550.the-semantic-landscape-paradigm-for-neural-networks/original/2307.09550.the-semantic-landscape-paradigm-for-neural-networks.md#p6-2). *«[Тем самым действие роста $N$ — увеличение числа узлов графа эвристических моделей](../papers/2307.09550.the-semantic-landscape-paradigm-for-neural-networks/2307.09550.the-semantic-landscape-paradigm-for-neural-networks.card.md#p6-2)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2301.05217 — Nanda et al., «Progress Measures for Grokking via Mechanistic Interpretability». [`"Neural networks often exhibit emergent behavior, where qualitatively new capabilities arise from scaling up the amount of parameters, training data, or training steps"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-2). *«[Нейросети часто демонстрируют эмерджентное поведение, при котором качественно новые способности возникают при увеличении числа параметров, объёма обучающих данных или числа шагов обучения](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-2)»*

**\[4.3\]** 2506.21551 — Li et al., «Grokking in LLM Pretraining?». [`"a mechanistic interpretation of its emergent generalization on downstream tasks"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p2-2). *«[механистического истолкования возникающей генерализации на итоговых задачах](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p2-2)»*

**\[4.4\]** 2603.29262 — Zhang et al., «Grokking: From Abstraction to Intelligence». [`"Emergent capabilities (Elhady et al., 2025; Kaplan et al., 2020) in modern large language models"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-3). *«[Возникающие способности (Elhady et al., 2025; Kaplan et al., 2020) нынешних больших языковых моделей](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-3)»*\
Доп. (сбрасывание битов): [`"The emergence of grokking is thus rigorously characterized as the system shedding algorithmic bits to transition from a complexity class of $\mathcal{O}(p^{2})$ to $\mathcal{O}(p)$."`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p8-1) — *«[Возникновение гроккинга тем самым строго описывается как сбрасывание системой алгоритмических битов при переходе из класса сложности $\mathcal{O}(p^{2})$ в $\mathcal{O}(p)$.](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p8-1)»*

**\[4.5\]** 2306.17844 — Zhong et al. 2023, «The Clock and the Pizza». Понятие упомянуто только в обзоре смежных работ, вместе с оговоркой о зависимости от выбора меры; к собственным переходам работа слова «эмерджентность» не применяет и масштаб модели не меняет. [`"An ability is “emergent” if the performance on a subtask suddenly increases with growing model sizes, though such claims depend on the choice of metric [27]."`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p10-1). *«[Способность «эмерджентна», если качество на подзадаче внезапно растёт с ростом размера модели, хотя такие заявления зависят от выбора меры [27].](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p10-1)»*

**\[4.6\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: гроккинг отнесён к эмерджентности словом автора, а меры прогресса поданы как приём её понимания; опытов с масштабом модели в работе нет. [`"Grokking is a form of emergence, first reported by (Power et al. 2022)"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-1). *«[Гроккинг — форма эмерджентности, впервые сообщённая (Power et al. 2022)](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-1)»*\
Доп.: [`"from mechanistic explanations as a methodology for understanding emergence"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p6-10) — *«[из механистических объяснений как подхода к пониманию эмерджентности](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p6-10)»*.

**\[4.7\]** 2305.18741 — Murty et al. 2023, «Grokking of Hierarchical Structure in Vanilla Transformers». Слово «emergent» встречается только как оборот при пересказе смежных работ («emergent linguistic structure», «emergent hierarchical structure in transformers»); к собственным находкам работа его не прилагает, порогов по масштабу не ищет, а зависимость от глубины у неё не пороговая, а перевёрнутая U. [`"norm growth in transformers leads to attention saturation, an important property for emergent linguistic structure (Merrill et al. 2022)"`](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/original/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.md#p4-2). *«[рост нормы в трансформерах ведёт к насыщению внимания — важному свойству для возникающей языковой структуры (Merrill et al. 2022)](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.card.md#p4-2)»*

**\[4.8\]** 2401.10463 — Zhu et al. 2024. Нюанс: эмерджентные способности помянуты один раз, в перечне открытий о генерализации рядом с законами масштабирования, двойным спуском и гроккингом; собственного разбора нет. [`"researchers have made a series of striking discoveries across generalization abilities, including neural scaling laws [4], double descent [11], grokking [15] and emergent abilities [23]"`](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p1-3). *«[исследователи сделали ряд поразительных открытий, касающихся способностей к генерализации, включая нейросетевые законы масштабирования [4], двойной спуск [11], гроккинг [15] и эмерджентные способности [23]](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p1-3)»*

**\[4.9\]** 2406.06158 — Kunin et al., «Get rich quick: exact solutions reveal how unbalanced initializations promote rapid feature learning». Нюанс: эмерджентность появляется одной придаточной фразой при гроккинге и со ссылкой на Nanda et al. 2023 — ни определения, ни измерения, ни масштабирования в работе нет. [`"believed to be important towards understanding emergent phenomena [72]"`](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#p10-2). *«[что оно важно для понимания эмерджентных явлений [72]](../papers/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning/2406.06158.get-rich-quick-exact-solutions-reveal-how-unbalanced-initializations-promote-rapid-feature-learning.card.md#p10-2)»*

**\[4.10\]** 2506.23286 — Jeffares & van der Schaar, «Not All Explanations for Deep Learning Phenomena Are Equally Valuable». Нюанс: собственного разбора эмерджентности и каких-либо измерений на языковых моделях в работе нет. [`"this echos a similar point which has been made more generally on the so-called *emergent abilities* of large language models"`](../papers/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable/original/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable.md#p4-1). *«[это перекликается со схожим доводом, высказанным более общо о так называемых *эмерджентных способностях* больших языковых моделей](../papers/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable.card.md#p4-1)»*

**\[4.11\]** 2502.01774 — Carvalho et al. 2025, «Grokking Explained: A Statistical Phenomenon». [`"our observations of emergent behavior when training with very limited examples of a pattern that is nevertheless structured into classes and subclasses"`](../papers/2502.01774.grokking-explained-a-statistical-phenomenon/original/2502.01774.grokking-explained-a-statistical-phenomenon.md#p6-7). *«[наши наблюдения эмерджентного поведения при обучении на очень ограниченных примерах узора, который тем не менее устроен в классы и подклассы](../papers/2502.01774.grokking-explained-a-statistical-phenomenon/2502.01774.grokking-explained-a-statistical-phenomenon.card.md#p6-7)»*

**\[4.12\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Упоминает эмерджентность как соседнее семейство внезапных скачков и берёт из спора о разрывных мерах вывод для себя: нужны чётко определённые управляющие величины и конечноразмерный разбор. Нюанс: собственная мера перехода при этом остаётся разрывной (первый шаг с точностью на проверке $\geq 0.9$), и защищается она не непрерывностью, а устойчивостью — при порогах $0.8/0.9/0.95$ измеряемая норма меняется менее чем на $2\%$. [`"Emergent abilities [11] were argued by Schaeffer et al. 2023 to be partly an artifact of discontinuous metrics"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-2). *«[Об эмерджентных способностях [11] Schaeffer et al. 2023 доказывали, что они отчасти суть след разрывных мер](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-2)»*


**\[4.13\]** 2402.09469 — Li, Liang, Shi, Song, Zhou, «Fourier Circuits in Neural Networks and Transformers: A Case Study of Modular Arithmetic with Multiple Inputs». [`"The phenomenon known as “grokking” was initially identified by [69] and is believed to be a way of studying the emerging abilities of LLM [99]"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#p6-2). *«[Явление, известное как «гроккинг», впервые указано в [69] и считается способом изучать возникающие способности больших языковых моделей [99]](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.card.md#p6-2)»*

### Внешние работы

###### ref-5-1
**\[5.1\]** 2406.05335 — Внешняя работа (демотирована из корпуса): Nakaishi, Nishikawa, Hukushima 2024, «Phase transition in large language models and the criticality of natural languages». Нюанс: критическое поведение не задано устройством, а возникает при обучении, причём два строения усваиваются с разными порогами. [`"These results indicate that the model begins to acquire nontrivial structures of the natural language around $k_{c}\approx 10^{2}$, giving rise to a critical phase transition"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md#p5-2). *«[Эти итоги означают, что модель начинает усваивать нетривиальные строения естественного языка около $k_{c}\approx 10^{2}$, что и даёт критический фазовый переход](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.card.md#p5-2)»*\
Доп. (второй порог): [`"The difference between these two onsets suggests that the model acquires two distinct structures at different stages of training"`](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/original/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.md#p5-3) — *«[Различие этих двух начал говорит о том, что модель усваивает два различных строения на разных порах обучения](../externals/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages/2406.05335.phase-transition-in-large-language-models-and-the-criticality-of-natural-languages.card.md#p5-3)»*.
