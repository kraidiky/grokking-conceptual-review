# Разрежённая подсеть / lottery ticket (sparse subnetwork / lottery ticket)

[Сжатие многообразия представлений](manifold-representation-compression.md) ← предыдущая карточка, следующая → [Нейронное касательное ядро](neural-tangent-kernel-ntk.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Разрежённая подсеть / lottery ticket** — трактовка [гроккинга](grokking.md), в
которой отложенная генерализация связывается с выделением внутри плотной сети
малой **разрежённой подсети** (набора нейронов/весов, много меньшего, чем вся
сеть), реализующей обобщающий алгоритм и вытесняющей плотную запоминающую
подсеть. Merrill et al. (2023) ввели рамку «двух контуров»: [фазовый переход](phase-transition.md)
гроккинга соответствует появлению разрежённой подсети, доминирующей в
предсказаниях \[[1.1](#ref-1-1)\]. Minegishi et al. (2023) связали то же явление
с **гипотезой лотерейного билета** (lottery ticket hypothesis, Frankle & Carbin
2019 — гипотеза о том, что в случайно инициализированной плотной сети уже
существует разрежённая подсеть, «выигрышный билет», которую можно дообучить до
полного качества) и ввели понятие «grokked tickets» — лотерейных билетов,
извлечённых из уже сгенерализовавшейся сети \[[1.2](#ref-1-2)\].

![Структура сети при обучении алгоритмической задаче: плотная запоминающая и разрежённая обобщающая подсети (рис. 1 Merrill et al.)](assets/sparse-subnetwork-structure.png)

## Детализация

Обе опорные работы выделяют подсеть через **прунинг** (обрезку весов: удаление
связей с наименьшей магнитудой и проверку, сохраняет ли остаток качество полной
сети). Merrill et al. измеряют «эффективную разреженность» — размер минимальной
активной подсети, воспроизводящей поведение сети, — и показывают, что при
гроккинге доминирование переходит от плотной подсети (контура, т. е. подсети,
реализующей некоторый под-алгоритм) к разрежённой; механически переход движется
**быстрым ростом нормы** (L2-амплитуды весов) у небольшого набора нейронов при
медленном затухании остальных \[[1.1](#ref-1-1)\]. Здесь гроккинг из «поздней
генерализации после [фазы запоминания](memorization-phase.md)» переосмысляется
как **смена доминирующего решения**, что роднит его с языком эмерджентности
(резкого появления способности — [эмерджентности](emergence.md))
для больших языковых моделей.

Внутри самого понятия есть спорный нюанс — *что именно* делает подсеть носителем
генерализации. Merrill et al. акцентируют разреженность и рост нормы
\[[1.1](#ref-1-1)\]. Minegishi et al. оспаривают этот акцент: подставляя
«grokked ticket» вместо плотной сети, они устраняют задержку генерализации и
показывают, что дело не в меньшей норме и не в разреженности как таковой, а в
**хорошей структуре** найденной подсети \[[1.2](#ref-1-2)\]. Ещё дальше идёт
Bhaskar et al.: вместо *непересекающихся* конкурирующих подсетей они находят
общее «эвристическое ядро» (heuristic core) — набор attention-голов, входящих во
все подсети, в том числе не обобщающие, — и тем самым оспаривают буквальную
картину «два разных контура сменяют друг друга» \[[2.1](#ref-2-1)\]. С другой
стороны, разрежённо-подсетевую рамку поддерживает ряд работ: разреженность
(доля неактивных нейронов) служит предсказательной метрикой наступления
гроккинга \[[3.1](#ref-3-1)\]; в задаче классификации конфигураций модели Изинга
гроккинг интерпретируется как переход к группе разрежённых подсетей
\[[3.2](#ref-3-2)\]; при гроккинге на [реальных данных](vision-real-world-data-mnist-cifar.md) большое число нейронов
становится неактивным, сводя сеть к меньшей эффективной подсети
\[[3.3](#ref-3-3)\].

## Альтернативные определения и нюансы

### A. Конкуренция плотной и разрежённой подсетей (Merrill)

Определение через **[параметр порядка](order-parameter.md)** — норму/долю активных нейронов: до
перехода предсказания определяет плотная подсеть, которая плохо обобщает; при
гроккинге небольшой набор нейронов резко наращивает норму и начинает доминировать,
образуя разрежённую обобщающую подсеть. Ключ различия здесь — *механизм*:
целенаправленный рост нормы у немногих нейронов при затухании остальных, а размер
разрежённой подсети совпадает с размером схемы вычисления чётности (parity)
\[[1.1](#ref-1-1)\].

### B. Выигрышный лотерейный билет / grokked ticket (Minegishi)

Определение через **подсеть-маску**: обобщающее решение — это уже присутствующий в
плотной сети «выигрышный билет», и если стартовать прямо с него (маскировав
остальные веса), задержки генерализации не будет. Источник различия по сравнению с
рамкой A — не разреженность и не норма, а именно *структура* подсети: контролируя
по отдельности норму, разреженность и структуру, авторы показывают, что ускоряет
генерализацию только последняя \[[1.2](#ref-1-2)\].

### Оспаривают

Bhaskar et al. оспаривают предпосылку о *непересекающихся* конкурирующих подсетях,
на которой держатся рамки A и B. В их постановке (дообучение языковых моделей)
подсети, обобщающие и не обобщающие, разделяют общее «эвристическое ядро» — набор
из немногих attention-голов, присутствующих во всех подсетях; поэтому картину
«плотный запоминающий контур сменяется разрежённым обобщающим» они считают
неполной, а генерализацию в их случае сопровождает не уменьшение, а рост
эффективного размера \[[2.1](#ref-2-1)\].

### Поддерживают

Разрежённо-подсетевую рамку присоединяют работы, где разреженность выступает как
наблюдаемый признак или прямой механизм гроккинга: Salah et al. используют
динамику разреженности (доли неактивных нейронов) как метрику, предсказывающую
гроккинг, и описывают переход как преобразование плотной подсети в разрежённую
\[[3.1](#ref-3-1)\]; Hutchison et al. на задаче Изинга интерпретируют гроккинг как
переход от связной сети к группе разрежённых подсетей с убыванием числа активных
весов по глубине \[[3.2](#ref-3-2)\]; Zhang et al. наблюдают при гроккинге
«имплицитный архитектурный отбор» — массовую деактивацию нейронов, сводящую сеть к
меньшей эффективной подсети \[[3.3](#ref-3-3)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2303.11873 — Merrill et al., «A Tale of Two Circuits: Grokking as Competition of Sparse and Dense Subnetworks». [`"grokking phase transition corresponds to the emergence of a sparse subnetwork that dominates model predictions"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2). *«[фазовый переход гроккинга отвечает появлению разрежённой подсети, доминирующей в предсказаниях модели](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p1-2)»*\
Доп.: [`"the grokking phase transition can be understood to emerge from competition of two largely distinct subnetworks: a dense one that dominates before the transition and generalizes poorly, and a sparse one that dominates afterwards"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2) — *«[фазовый переход гроккинга можно понимать как возникающий из конкуренции двух в значительной мере различных подсетей: плотной, доминирующей до перехода и плохо обобщающей, и разрежённой, доминирующей после](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p1-2)»*.

###### ref-1-2
**\[1.2\]** 2310.19470 — Minegishi et al., «Bridging Lottery Ticket and Grokking: Understanding Grokking from Inner Structure of Networks». [`"utilizing lottery tickets obtained during the generalizing phase (termed grokked tickets) significantly reduces delayed generalization across various tasks"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2). *«[применение лотерейных билетов, полученных в обобщающей фазе (мы называем их грокнувшими билетами), значительно сокращает отложенную генерализацию в разных задачах](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.card.md#p1-2)»*\
Доп.: [`"mitigation of delayed generalization is not due solely to reduced weight norms or increased sparsity, but rather to the discovery of good subnetworks"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2) — *«[смягчение отложенной генерализации вызвано не одним лишь снижением нормы весов и не ростом разрежённости, а обнаружением хороших подсетей](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.card.md#p1-2)»*.

###### ref-1-3
**\[1.3\]** 2408.11804 — Yunis et al., «Approaching Deep Learning through the Spectral Dynamics of Weights». Вклад, не связанный с гроккингом: глобальное прореживание по величине (верхние 5 % весов, маска с конца обучения, перемотка на эпоху 4 из 164) сохраняет верхние сингулярные векторы итоговой модели, то есть ведёт себя как приближённо низкоранговая обрезка; случайная маска той же послойной разрежённости этого не даёт, и сеть после неё перестаёт учиться задолго до сходимости. Истолкование провала авторы прямо помечают как истолкование. [`"magnitude masks preserve the top singular vectors of parameters, and with increasing sparsity fewer directions are preserved"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p11-3). *«[маски по величине сохраняют верхние сингулярные векторы параметров, а с ростом разрежённости сохраняется меньше направлений](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p11-3)»*\
Доп. (авторская пометка «истолкование»): [`"We interpret this as evidence that the mask has somehow cut signal flow between layers"`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p11-5) — *«[Мы истолковываем это как свидетельство того, что маска как-то перерезала поток сигнала между слоями](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p11-5)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2403.03942 — Bhaskar et al., «The Heuristic Core: Understanding Subnetwork Generalization in Pretrained Language Models». Оспаривает: вместо непересекающихся конкурирующих подсетей — общее эвристическое ядро. [`"distinct subnetworks that compete during training. We instead find evidence of a heuristic core: a set of attention heads that appear in all generalizing subnetworks"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#fig-1). *«[различные подсети, состязающиеся в ходе обучения. Мы же находим свидетельства эвристического ядра: набора голов внимания, встречающихся во всех обобщающих подсетях](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#fig-1)»*\
Доп. (характеристика оспариваемой картины): [`"as the model switches from a dense, memorizing subnetwork to a sparse, generalizing subnetwork"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#p2-1) — *«[по мере того как модель переходит от плотной запоминающей подсети к разрежённой обобщающей](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#p2-1)»*.

###### ref-2-2
**\[2.2\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Нюанс: обрезка по величине проваливается полностью — половина наибольших приращений весов несёт 99.3 % квадрата нормы и даёт случайную точность. Оговорка: дообучения после обрезки не проводилось, тогда как гипотеза лотерейного билета говорит именно о переобучении найденной подсети; ссылка на первоисточник дана с ошибкой в фамилии второго автора («Frankle and Carlin»). [`"keeping 50% of the largest entries (99.3% of $\left\|\Delta\theta\right\|^{2}$) gives chance. Partial recovery begins only at 80% retention."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p13-5). *«[сохранение 50 % наибольших элементов (99.3 % от $\left\|\Delta\theta\right\|^{2}$) даёт случайный уровень. Частичное восстановление начинается лишь при сохранении 80 %](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p13-5)»*\
Доп. (предсказание для практики): [`"Our results predict that pruning beyond ${\sim}10\%$ of transverse capacity should degrade generalization sharply."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p28-3) — *«[Наши результаты предсказывают, что обрезка более чем ${\sim}10\%$ поперечной ёмкости должна резко ухудшать генерализацию](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p28-3)»*.

###### ref-2-3
**\[2.3\]** 2506.23286 — Jeffares & van der Schaar, «Not All Explanations for Deep Learning Phenomena Are Equally Valuable». Нюанс: собственных опытов с билетами нет, рисунок 3 — схема; связь лотерейного билета с гроккингом, которую корпус ведёт отдельной линией, в работе не рассматривается. [`"progress toward a viable technique has been limited and the lottery ticket approach has not materialized as a practical method"`](../papers/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable/original/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable.md#p4-4). *«[продвижение к пригодному приёму было ограниченным, и подход лотерейного билета не воплотился в практический способ](../papers/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable/2506.23286.not-all-explanations-for-deep-learning-phenomena-are-equally-valuable.card.md#p4-4)»*
### Поддерживают

###### ref-3-1
**\[3.1\]** 2507.11645 — Salah et al., «Tracing the Path to Grokking: Embeddings, Dropout, and Network Activation». Нюанс: разреженность как предсказательная метрика; переход плотной подсети в разрежённую. `"a dense, poorly generalizing subnetwork is converted to a sparse"` (line 77). *«плотная, плохо обобщающая подсеть преобразуется в разрежённую»*\
Доп.: `"the evolution of the sparsity provides a further metric that can be employed to predict grokking"` (line 583) — *«эволюция разреженности даёт ещё одну метрику, которую можно использовать для предсказания гроккинга»*.

###### ref-3-2
**\[3.2\]** 2510.25966 — Hutchison et al., «Grokking in the Ising Model». Нюанс: гроккинг как переход к группе разрежённых подсетей. `"from a connected network to a group of sparse subnetworks in which the number of active weights in each layer decreases monotonically with depth"` (line 15). *«от связной сети к группе разрежённых подсетей, в которых число активных весов в каждом слое монотонно убывает с глубиной»*

###### ref-3-3
**\[3.3\]** 2603.29262 — Zhang et al., «Grokking: From Abstraction to Intelligence». Нюанс: имплицитный архитектурный отбор — массовая деактивация нейронов при гроккинге. [`"a vast number of neurons become inactive or perform identity mapping, reducing a model with millions of parameters to a much smaller network with comparable performance"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-5). *«[огромное число нейронов становится бездеятельным или осуществляет тождественное отображение, сводя модель с миллионами параметров к много меньшей сети сравнимого качества](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-5)»*\
Доп. (затвор Оккама): [`"This operator acts as a hard-thresholding non-linearity that annihilates any spectral connection whose signal-to-noise ratio (SNR) falls below the information-theoretic lower bound"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p6-12) — *«[Этот оператор действует как нелинейность жёсткого отсечения, уничтожающая всякую спектральную связь, чьё отношение сигнала к шуму (SNR) падает ниже теоретико-информационной нижней границы](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p6-12)»*

###### ref-3-4
**\[3.4\]** 2505.11411 — Zhang, Shang, Yang, Zhang, «Is Grokking a Computational Glass Relaxation?». Переводит находку Merrill et al. на язык объёма: параметры вне действующей подсети можно возмущать без потери качества, поэтому множество решений разрежённой подсети занимает больший объём, а значит и большую энтропию — и этим объясняет, почему обобщающее решение оказывается высокоэнтропийным. Нюанс: рассуждение, а не измерение — ни разрежённости решений, ни подсетей работа не меряет, энтропия плотного и разрежённого решений порознь не сопоставлена. [`"In the sparse subnetworks, only a few neurons participate in the memorization of the rules, and the remaining degrees of freedom contribute to the entropy, so it should have a larger entropy as we observed."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p10-1). *«[В разрежённых подсетях лишь немногие нейроны участвуют в запоминании правил, а остальные степени свободы вносят вклад в энтропию, так что энтропия должна быть больше, что мы и наблюдали.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p10-1)»*\
Доп. (тот же довод через объём): [`"Perturbing such parameters preserves model performance, implying that the corresponding solution set occupies a larger volume in parameter space."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p18-1) — *«[Возмущение таких параметров сохраняет качество модели, из чего следует, что соответствующее множество решений занимает больший объём в пространстве параметров.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p18-1)»*.
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2504.13292 — Xu et al., «Let Me Grok For You: Accelerating Grokking via Embedding Transfer from a Weaker Model». [`"Minegishi et al. (2024) demonstrated that the gap between memorization and generalization can be nearly eliminated if a lottery ticket, a set of sparse mask matrices, is applied to the model during training"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p3-1). *«[Minegishi et al. (2024) показали: разрыв между запоминанием и генерализацией можно почти устранить, если применять к модели в ходе обучения лотерейный билет — набор разрежённых матриц-масок](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p3-1)»*

**\[4.2\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». [`"attributed grokking to competition between sparse (generalizing) and dense (memorizing) subnetworks"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p2-2). *«[приписали гроккинг состязанию разрежённых (обобщающих) и плотных (запоминающих) подсетей](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p2-2)»*

**\[4.3\]** 2512.03437 — Liang & Li, «Grokked Models are Better Unlearners». [`"This distributed-to-modular shift involves competition between dense memorizing and sparse generalizing circuits (Merrill et al., 2023; Varma et al., 2023)"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p4-1). *«[Этот сдвиг от распределённого к составному включает состязание плотных запоминающих и разрежённых обобщающих контуров (Merrill et al., 2023; Varma et al., 2023)](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p4-1)»*

**\[4.4\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». Нюанс: подавление моносемантичности противоречит отсечению по разрежённости, но может с ним уживаться. [`"When we reduce monosemanticity, a single input would activate multiple neurons, rendering pruning based on sparsity ineffective"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p20-5) — *«[Когда мы уменьшаем моносемантичность, один вход возбуждает несколько нейронов, отчего отсечение по разрежённости перестаёт работать](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p20-5)»*

**\[4.5\]** 2306.17844 — Zhong et al. 2023, «The Clock and the Pizza». Язык лотерейных билетов заимствован в явно объявленной догадке о динамике обучения; ни отслеживания кругов от инициализации, ни перезапуска с масками в работе нет. [`"We conjecture that training dynamics are as follows: (1) At initialization, pizzas #1/#2/#3 correspond to three different “lottery tickets” [9]."`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p6-2). *«[Мы предполагаем, что динамика обучения такова: (1) при инициализации пиццы #1/#2/#3 отвечают трём разным «лотерейным билетам» [9].](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p6-2)»*

**\[4.6\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: лишь упоминание в разделе будущей работы — догадка, что произвольность выбора представлений могла бы объясняться лотерейными билетами в инициализации; опыта в эту сторону нет. [`"lottery tickets (Frankle & Carbin 2019) may be present in initialized weights that could allow the learned features of a trained network to be anticipated *before training*"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p9-1). *«[лотерейные билеты (Frankle & Carbin 2019) могут присутствовать в инициализированных весах, что позволило бы предугадывать выучиваемые признаки обученной сети *до обучения*](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p9-1)»*

**\[4.7\]** 2310.02541 — Xu et al. 2023. Нюанс: состязание плотных и разрежённых подсетей только помянуто; в собственной постановке подсетей нет, все нейроны участвуют, а различие идёт по знаку замороженного второго слоя. [`"studied the learning dynamics in a two-layer neural network on a sparse parity task, attributing grokking to the competition between dense and sparse subnetworks"`](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/original/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.md#p3-2). *«[изучили динамику обучения в двухслойной нейросети на задаче разрежённой чётности, относя гроккинг к состязанию между плотными и разрежёнными подсетями](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.card.md#p3-2)»*

**\[4.8\]** 2405.12755 — Golechha 2024, «Progress Measures for Grokking on Real-world Tasks». Гипотеза лотерейного билета служит единственной опорой словесного довода о том, почему абсолютная энтропия весов должна падать: растущих по величине критических весов немного. Нюанс: никакого билета работа не извлекает — подсеть не выделяется, обрезка не производится, разрежённость весов не измеряется (измеряется разрежённость активаций, другая величина); тем не менее обзор 2605.15787 относит работу к трактовкам гроккинга как «внезапного появления выигрышных билетов». [`"due to the lottery ticket hypothesis (shown to hold to a good degree for MLPs trained on MNIST (Frankle and Carbin 2018)), this is often limited to a much smaller subset of the weights"`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p4-6). *«[из-за гипотезы лотерейного билета (о которой показано, что она с хорошей степенью выполняется для MLP, обучаемых на MNIST (Frankle and Carbin 2018)) это часто ограничено гораздо меньшим подмножеством весов](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p4-6)»*

**\[4.10\]** 2605.09724 — Song & Ye, «Model Capacity Determines Grokking through Competing Memorisation and Generalisation Speeds». Нюанс: разрежённость в работе не измеряется. [`"document grokking as the emergence of a sparse subnetwork via selective norm growth in a small set of neurons"`](../papers/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds/original/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds.md#p3-2). *«[описывают гроккинг как появление разрежённой подсети через избирательный рост нормы у небольшого набора нейронов](../papers/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds.card.md#p3-2)»*

**\[4.11\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Принимает довод о грокинг-билете (согласованная по норме плотная сеть обобщает медленнее разрежённой) не опровержением, а сужением области: норма причинна там, где задаёт масштаб функции. Нюанс: прямого ответа довод не получает. Он о том, что нормы **недостаточно** для генерализации, тогда как измеренное здесь — достаточность состояния нормы для управления **временем** при закреплённой архитектуре; разграничения этих двух заявок в тексте нет, а согласованных по норме разрежённых сетей работа не ставит. [`"Minegishi et al. 2023 show a sparse “grokking ticket” generalizes faster than a *norm-matched* dense network, so the norm is not sufficient."`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p3-1). *«[Minegishi et al. 2023 показывают, что разрежённый «гроккинг-билет» обобщает быстрее *согласованной по норме* плотной сети, так что нормы недостаточно.](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p3-1)»*\
Доп. (как довод учтён): [`"our results delimit *when* the norm is causal: it controls the grokking timescale in settings where it sets the function scale"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p3-1) — *«[очерчивают, *когда* норма причинна: она управляет временным масштабом гроккинга в постановках, где задаёт масштаб функции](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p3-1)»*.

### Внешние работы

###### ref-5-1
**\[5.1\]** 2406.03999 — Внешняя работа (демотирована из корпуса): Song, Tan, Zou, Ma, Huang, «Unveiling the Dynamics of Information Interplay…». [`"even when the model sparsity is 90%, the features extracted by the pruned model maintain a high MIR with those extracted before pruning"`](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p6-2). *«[даже при разрежённости модели 90 % признаки, извлечённые прореженной моделью, сохраняют высокий MIR с признаками, извлечёнными до прореживания](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#p6-2)»*\
Доп. (величина «высокого»): по [рис. 6](../externals/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning/2406.03999.unveiling-the-dynamics-of-information-interplay-in-supervised-learning.card.md#fig-6) MIR держится около 0.5 по правой оси на всём переборе разрежённости от 0 до 0.9, то есть «высокий» здесь означает «не изменившийся», а не «близкий к 1».
