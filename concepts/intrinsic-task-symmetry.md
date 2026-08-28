# Внутренняя симметрия задачи (intrinsic task symmetry)

[Алгоритмы Clock и Pizza](clock-vs-pizza.md) ← предыдущая карточка, следующая → [Частотный принцип / спектральное смещение](frequency-principle-spectral-bias.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Внутренняя симметрия задачи** — инвариантность правила, порождающего данные, относительно преобразования входа: перестановки операндов (коммутативность), перегруппировки (ассоциативность), транзитивности в задачах сравнения, треугольного равенства в задачах на графах. Понятие вводится как объяснение [гроккинга](grokking.md) причинного порядка: *«собственная симметрия задачи и есть главная причина обобщения, определяющая особый вид выучиваемой геометрии в алгоритмических задачах»* \[[1.1](#ref-1-1)\]. Из этого следует разметка обучения на три поры — *«(i) запоминание, (ii) обретение симметрии и (iii) устроение геометрии»*, причём обобщение возникает во второй \[[1.2](#ref-1-2)\].

## Детализация

**Откуда взялось наблюдение.** Оно старше объяснения: исходная работа о гроккинге заметила, что *«такие операции, как правило, требуют меньше данных для генерализации, чем близкие к ним несимметричные аналоги»*, и тут же оговорилась, что действие может отчасти зависеть от архитектуры — трансформеру легко выучить симметричную функцию, пренебрегая позиционным вложением \[[1.3](#ref-1-3)\]. Четыре года спустя из побочного замечания о [доле данных](data-fraction-critical-dataset-size.md) выросла заявка о причине.

**Что превращает симметрию в измеримую величину.** Нарушение симметрии считается как расхождение предсказаний на связанных симметрией парах входов, и у него обнаруживается порог: *«как только нарушение симметрии у прогона падает ниже некоторого уровня, он надёжно достигает безупречной точности на проверке, тогда как прогоны, остающиеся выше этого уровня, — нет»* \[[1.4](#ref-1-4)\]. Тем самым понятие входит в круг [мер прогресса](progress-measures.md) и подчиняется их требованиям: опережать переход и держаться по семенам. Оговорка авторов сохранена дословно — переход *«не вполне резок»*.

**Почему это заявка о причине, а не о сопутствии.** Различение делается вмешательством: к потере добавляется штраф за нарушение симметрии, и время до обобщения сокращается — *«прямое наименьшение нарушений симметрии последовательно сдвигает переход гроккинга»* \[[1.5](#ref-1-5)\]. Тот же ход независимо проверен пополнением данных коммутативными парами \[[3.1](#ref-3-1)\], что относит линию к [ускорению гроккинга](accelerated-grokking.md). Вмешательство работает и без [weight decay](weight-decay.md), чем оспаривается объяснение, где вся работа приписана сглаживанию нормы.

**Где симметрия мешает, а не помогает.** У понятия есть обратная сторона, которая в карточке важнее самой заявки: те же симметрии делают задачу *«принципиально трудной для перестановочно-эквивариантных ядровых методов»* \[[2.2](#ref-2-2)\]. Сеть в [ядровом режиме](neural-tangent-kernel-ntk.md) наследует симметрию задачи и потому обобщать не может, хотя нулевой ошибки на обучении достигает; выход из этого режима и есть условие обобщения. Симметрия, стало быть, — не сила, тянущая к решению, а связь, которую в одном режиме нельзя разорвать, а в другом — нужно усвоить.

**Не путать с симметрией сети.** Понятие говорит о симметрии *правила*, а не о перестановочных симметриях параметров, порождающих вырожденные многообразия в ландшафте (это соседняя линия — [сингулярная теория обучения](singular-learning-theory.md)). Совпадение слова скрывает противоположные роли: симметрия задачи — то, что модель должна усвоить, симметрия параметров — то, что делает решение неединственным.

## Альтернативные определения и нюансы

### A. Симметрия как причина обобщения

Сильная форма: усвоение инвариантности — необходимое условие алгоритмического обобщения, и оно же назначает геометрию представлений \[[1.1](#ref-1-1)\], \[[1.2](#ref-1-2)\]. Различающая черта — проверяемое следствие: если симметрия причина, то поощрение её должно сдвигать срок обобщения, и это измеряется \[[1.5](#ref-1-5)\]. Уязвимость формы в том, что все проверки поставлены на задачах, где симметрия известна заранее и записывается формулой.

### B. Симметрия как свойство, облегчающее задачу

Слабая форма, идущая от исходного наблюдения: симметричные операции требуют меньше данных \[[1.3](#ref-1-3)\]. Источник различия — предмет утверждения: не механизм, а [порог по доле данных](data-fraction-critical-dataset-size.md). Форма скромнее и потому устойчивее: она переживает возражение об архитектурной зависимости, которое сами авторы и высказали.

### C. Симметрия как препятствие в ядровом режиме

Обратная форма: инвариантность задачи запрещает обобщение целому классу методов \[[2.2](#ref-2-2)\]. Различающая черта — доказательство, а не измерение: нижняя граница на популяционную потерю для перестановочно-эквивариантных предсказателей. Именно в этой форме понятие смыкается с [переходом lazy→rich](lazy-to-rich-kernel-to-feature-learning.md): обобщение требует покинуть режим, наследующий симметрию задачи.

### D. Симметрия за пределами коммутативности

Расширение: симметрия задачи не сводится к перестановке операндов. [Композиция в некоммутативной группе](group-composition-non-commutative-s5.md) $S_5$ грокается тоже, хотя *«композиция некоммутативна»* \[[2.3](#ref-2-3)\], а перебор тензорных алгебр по трём независимым свойствам — ассоциативности, коммутативности, унитальности — показывает, что предметом сравнения должно быть свойство, а не его частный случай \[[3.3](#ref-3-3)\]. Тот же разрыв виден со стороны архитектуры: вмешательство, снимающее задержку на коммутативной операции, полностью проваливается на некоммутативной $S_5$, потому что *«коммутативные операции требуют лишь представления „мешок токенов“»* \[[3.2](#ref-3-2)\], а некоммутативные — нет. Различающая черта — здесь симметрия перестаёт быть одной величиной и становится решёткой свойств, и вопрос «какая симметрия движет обобщением» приходится задавать заново для каждой из них.

## Ссылки

###### ref-1-1
**\[1.1\]** 2603.01968 — Hwang & Park, «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"intrinsic task symmetry is the key driver of generalization"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-5). *«[собственная симметрия задачи и есть главная причина обобщения](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p1-5)»*

###### ref-1-2
**\[1.2\]** 2603.01968 — Hwang & Park, «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"(i) memorization, (ii) symmetry acquisition, and (iii) geometric organization"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-2). *«[(i) запоминание, (ii) обретение симметрии и (iii) устроение геометрии](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p1-2)»*

###### ref-1-3
**\[1.3\]** 2201.02177 — Power et al., «Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets». [`"Such operations tend to require less data for generalization than closely related non-symmetrical counterparts"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p3-4). *«[Такие операции, как правило, требуют меньше данных для генерализации, чем близкие к ним несимметричные аналоги](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p3-4)»*

###### ref-1-4
**\[1.4\]** 2603.01968 — Hwang & Park, «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"once a run’s symmetry violation drops below a characteristic level, it reliably reaches perfect test accuracy, while runs that remain above this level do not"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p5-8). *«[как только нарушение симметрии у прогона падает ниже некоторого уровня, он надёжно достигает безупречной точности на проверке, тогда как прогоны, остающиеся выше этого уровня, — нет](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p5-8)»*

###### ref-1-5
**\[1.5\]** 2603.01968 — Hwang & Park, «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"directly minimizing symmetry violations systematically shifts the grokking transition"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p8-5). *«[прямое наименьшение нарушений симметрии последовательно сдвигает переход гроккинга](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p8-5)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2311.06597 — Tan et al., «Understanding Grokking Through A Robustness Viewpoint». Оспаривает: если бы обобщение шло через усвоение симметрии, коммутативность должна была бы выучиваться до перехода, а она не выучивается. [`"standard training on Modulo Addition Dataset fails to learn commutative law before grokking, which contradicts intuition"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p2-4). *«[обычное обучение на наборе модульного сложения не выучивает закон коммутативности до гроккинга, что противоречит интуиции](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p2-4)»*

###### ref-2-2
**\[2.2\]** 2407.12332 — Mohamadi et al., «Why Do You Grok? A Theoretical Analysis of Grokking Modular Addition». Нюанс: та же симметрия в ядровом режиме работает наоборот — она запрещает обобщение, и доказательство идёт нижней границей, а не измерением. [`"this task is *fundamentally hard* for permutation-equivariant kernel methods, due to inherent symmetries"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p3-4). *«[эта задача *принципиально трудна* для перестановочно-эквивариантных ядровых методов — из-за присущих ей симметрий](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p3-4)»*

###### ref-2-3
**\[2.3\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Ограничивает: гроккинг наступает и там, где симметрии порядка операндов нет вовсе. [`"$S_{5}$ is not abelian, so the composition is non-commutative"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p4-5). *«[$S_{5}$ не абелева, так что композиция некоммутативна](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p4-5)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2405.16658 — Park et al., «Acceleration of Grokking in Learning Arithmetic Operations via Kolmogorov-Arnold Representation». Нюанс: независимая проверка причинности — не штрафом за нарушение, а пополнением обучающих данных коммутативными парами. [`"It is empirically verified that the commutative augmentation technique introduced accelerates grokking."`](../papers/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation.card.md#p3-2). *«[Эмпирически проверено, что введённый приём коммутативного пополнения ускоряет гроккинг.](../papers/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation.card.md#p3-2)»*

###### ref-3-2
**\[3.2\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Нюанс: симметрия задачи ставится в пару с архитектурой — коммутативной операции довольно «мешка токенов», и вмешательство, выручающее на ней, проваливается на некоммутативной. [`"commutative operations require only a bag-of-tokens representation"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2). *«[коммутативные операции требуют лишь представления «мешок токенов»](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p1-2)»*

###### ref-3-3
**\[3.3\]** 2602.19533 — Notsawo et al., «Grokking Finite-Dimensional Algebra». Нюанс: симметрия разложена на независимые свойства и перебрана — ассоциативность, коммутативность, унитальность дают восемь разрядов алгебр. [`"Let $a$=$associative$, $c$=$commutative$, $u$=$unital$."`](../papers/2602.19533.grokking-finite-dimensional-algebra/original/2602.19533.grokking-finite-dimensional-algebra.md#p8-1). *«[Пусть $a$=$associative$, $c$=$commutative$, $u$=$unital$.](../papers/2602.19533.grokking-finite-dimensional-algebra/2602.19533.grokking-finite-dimensional-algebra.card.md#p8-1)»*

## Цитирования

Работы, лишь упоминающие понятие (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2506.05718 — Notsawo et al., «Grokking Beyond the Euclidean Norm of Model Parameters». [`"Park et al. (2024) accelerate grokking by using data augmentation for commutative operations"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p17-4). *«[Park et al. (2024) ускоряют гроккинг, применяя обогащение данных для переместительных действий](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.card.md#p17-4)»*
