# Групповые представления и cosets (group representations / cosets)

[Маршрутизация внимания](attention-routing-heads.md) ← предыдущая карточка, следующая → [Нейронный коллапс](neural-collapse.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Групповые представления и cosets** — язык теории представлений групп,
на котором описывают обобщающее решение, найденное сетью, обученной на
операции конечной группы ([модульная арифметика](modular-arithmetic.md) — циклические группы;
композиция перестановок — симметрические группы). Утверждение состоит в том,
что после [гроккинга](grokking.md) сеть реализует не произвольную подгонку, а
структуру, диктуемую **представлениями** группы: неприводимыми представлениями
(irreps — минимальными блоками, на которые раскладывается действие группы; для
абелевых/циклических групп это одномерные фурье-базисы) и **смежными классами**
(cosets — сдвигами подгруппы `gH`), на которых избирательно активируются
нейроны и контуры сети \[[1.1](#ref-1-1)\]\[[1.2](#ref-1-2)\]\[[5.1](#ref-5-1)\]↗.

## Детализация

Исходно интерпретируемость гроккинга на модульном сложении показала, что сеть
строит решение из фурье-компонент (частот) — это «Clock»-алгоритм из пары
[Clock и Pizza](clock-vs-pizza.md); фурье-базисы суть в точности неприводимые
представления циклической группы. При переходе к неабелевым группам (например,
композиции перестановок `S5`) непрерывных частот уже недостаточно, и сеть
опирается на дискретные структуры смежных классов (coset circuits — подсети,
активные на конкретных `gH`). Работа 2505.18266 объединяет оба случая, вводя
**приближённые cosets**: доказывается, что ReLU-нейроны на модульном сложении
активируются только на приближённых смежных классах или их линейных
комбинациях, а сам универсальный алгоритм трактуется как приближённая китайская
теорема об остатках (Chinese Remainder Theorem — разложение вычетов по взаимно
простым модулям) \[[1.1](#ref-1-1)\]. Теоретическую опору даёт 2509.21519:
регулярное представление группы (действие группы на самой себе) раскладывается
на неприводимые блоки, и для абелевой группы все комплексные irrep
одномерны, то есть совпадают с фурье-базисом \[[1.2](#ref-1-2)\]. Работа
2601.21150 ставит смежный вопрос — способна ли узкая сеть вообще «улавливать»
теоретико-групповые понятия (коммутативность, единицу, подгруппы) и как это
считывается из её внутренних представлений \[[5.1](#ref-5-1)\]↗. Вокруг концепта
есть спор о том, универсален ли континуальный (фурье/сферический) геометрический
приор: 2603.05228 показывает, что для неабелевой `S5` дискретная coset-структура
не согласуется с непрерывной сферической геометрией, и потому ускорение,
работающее на циклической арифметике, там проваливается \[[2.1](#ref-2-1)\].
С другой стороны, 2604.00316 поддерживает представление-центричную трактовку:
обобщение наступает именно тогда, когда сеть **восстанавливает** действие
скрытой в данных группы симметрии, а обученные матрицы признаков кодируют
конкретные её элементы \[[3.1](#ref-3-1)\]. Здесь понятие тесно связано с
[фазовым переходом](phase-transition.md) и с выходом сети из
[фазы запоминания](memorization-phase.md): смена меморизационного решения
представление-структурированным обобщающим и есть то, что наблюдается как скачок.

## Альтернативные определения и нюансы

### A. Представление-теоретическая (irreps / фурье-базисы)

Обобщающее решение отождествляется с разложением по неприводимым представлениям
группы. Ключевой различающий признак — регулярное представление раскладывается
в прямую сумму irrep, и для **абелевой** группы каждый комплексный irrep
одномерен, то есть буквально является фурье-гармоникой; для неабелевой группы
появляются многомерные блоки, и фурье-описание перестаёт быть полным
\[[1.2](#ref-1-2)\]. В этой трактовке «выучить группу» = «выучить её таблицу
неприводимых характеров», а частоты Clock-алгоритма — частный (абелев) случай.

### B. Coset-центричная (универсальность через приближённые смежные классы)

Здесь единицей описания служит не частота, а смежный класс `gH`. Различающая
машинерия — понятие **приближённого coset**: нейрон активен на подмножестве
входов, близком к сдвигу подгруппы, и это подмножество, а не отдельная частота,
объясняет вычисление. Сила трактовки в универсальности: один и тот же
coset-язык покрывает и модульное сложение (циклические группы), и композицию
перестановок (coset circuits), связывая «incredibly different groups» единым
абстрактным алгоритмом \[[1.1](#ref-1-1)\]. Отличие от версии A — coset-описание
не требует, чтобы решение было континуально-фурье; оно охватывает и дискретные
неабелевы случаи, где A неполна.

### Оспаривают

2603.05228 оспаривает нюанс об **универсальности континуального
геометрического приора**. Различающий источник — контраст между циклической
арифметикой (где ограничение на сферу `L2`-нормировкой убирает задержку
генерализации, потому что решение фурье-непрерывно) и неабелевой `S5` (где то же
ограничение полностью проваливается, потому что успешные решения опираются на
**дискретные** coset-структуры, несовместимые со сферической геометрией). Вывод:
обход [фазы запоминания](memorization-phase.md) зависит от согласования
архитектурного приора с внутренней симметрией задачи, а не является общим
эффектом ограничения ёмкости \[[2.1](#ref-2-1)\].

### Поддерживают

2604.00316 присоединяется к представление-центричной трактовке со стороны
kernel-методов (RFM — Recursive Feature Machine, итеративно строящая матрицу
признаков через AGOP, средний внешнее произведение градиентов). Различающий
тестируемый тезис — генерализация наступает только при **нарушении** некоторой
симметрии обучающей выборки, и эмпирически RFM обобщает именно за счёт
**восстановления** действия скрытой группы симметрии, а обученные матрицы
признаков кодируют её конкретные элементы \[[3.1](#ref-3-1)\]. Это переносит
представление-теоретическое объяснение гроккинга за пределы нейросетей на [SGD](optimizer-adam-adamw-sgd.md).

## Ссылки

###### ref-1-1
**\[1.1\]** 2505.18266 — McCracken et al. 2025, «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"we introduce approximate cosets and show that neurons activate exclusively on them"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2). *«[мы вводим приближённые смежные классы и показываем, что нейроны возбуждаются исключительно на них](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p1-2)»*\
Доп.: [`"all ReLU neurons learning sinusoidal functions activate only on approximate cosets or linear combinations of them"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-1) — *«[все ReLU-нейроны, выучивающие синусоидальные функции, возбуждаются лишь на приближённых смежных классах или их линейных сочетаниях](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-1)»*; [`"work finding coset circuits in networks trained on permuting lists (permutation groups) [9] aligns with our results"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-1) — *«[работа, нашедшая контуры на смежных классах в сетях, обучаемых перестановкам списков (группы перестановок) [9], согласуется с нашими итогами](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-1)»*.

###### ref-1-2
**\[1.2\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». [`"For Abelian group, all complex irreps are 1d (i.e., Fourier bases)"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p6-4). *«[Для абелевой группы все комплексные неприводимые представления одномерны (то есть это базисы Фурье)](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p6-4)»*\
Доп.: [`"group representations often take the form of matrices (e.g., permutation / rotation matrix)"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p6-1) — *«[представления групп часто имеют вид матриц (например, матрицы перестановки или поворота)](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p6-1)»*.

###### ref-1-4
**\[1.4\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: первоисточник алгоритма GCR — вложить $a,b$ в матрицы представлений, перемножить их нелинейностью и считать логиты как характеры; рамка описывает не одно решение, а семейство контуров по числу неприводимых представлений. [`"We are not aware of this algorithm existing in any prior literature."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-7). *«[Нам не известно о существовании этого алгоритма в какой-либо прежней литературе](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-7)»*\
Доп.: [`"In general, a single network may choose any subset of these $k$ circuits to implement"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-12) — *«[В общем случае отдельная сеть может выбрать для реализации любое подмножество этих $k$ контуров](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-12)»*.

###### ref-1-5
**\[1.5\]** 2312.06581 — Stander et al. 2024, «Grokking Group Multiplication with Cosets». Нюанс: первоисточник контуров на смежных классах; сеть опознаёт попадание произведения в ОБЩИЙ смежный класс пары сопряжённых подгрупп по обращению суммы предактиваций в ноль. [`"The model implements the full group multiplication by picking out the shared cosets of conjugate subgroups."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p5-1). *«[Модель осуществляет полное групповое умножение, выбирая общие смежные классы сопряжённых подгрупп](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p5-1)»*\
Доп.: [`"almost every neuron had this property of only producing a discrete number of values that corresponded directly to the cosets of one of the subgroups of $S_{5}$ or $S_{6}$"`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p5-8) — *«[почти каждый нейрон обладает этим свойством — принимать лишь дискретное число значений, отвечающих прямо смежным классам одной из подгрупп $S_{5}$ или $S_{6}$](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p5-8)»*; [`"over 99.2% of the neurons in the linear layer had $\min_{H\in\operatorname{sub}(G)}C_{H}(f)<1.0$"`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p6-3) — *«[свыше 99.2% нейронов линейного слоя имели $\min_{H\in\operatorname{sub}(G)}C_{H}(f)<1.0$](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p6-3)»*; [`"This same construction works for every subgroup of $S_{n}$ except for $A_{n}$."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p5-7) — *«[То же построение работает для всякой подгруппы $S_{n}$, кроме $A_{n}$](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p5-7)»*.

###### ref-1-6
**\[1.6\]** 2311.07568 — Morwani et al. 2024, «Feature emergence via margin maximization: case studies in algebraic tasks». Выводит из максимума зазора то, что Chughtai et al. 2023 нашли опытно: веса каждого нейрона натянуты ровно на одно **нетривиальное** неприводимое представление, каждое такое представление занято хотя бы одним нейроном, а лемма 20 предъявляет конструкцию из $2{d_{R}}^{3}$ нейронов, считающую $\mathrm{tr}(R(a)R(b)R(c)^{-1})$. Нюанс: cosets в работе нет — слово не встречается, разложения по смежным классам не строится, а класс сопряжённости входит в разбор только как область постоянства характера. [`"For every neuron, there exists a non-trivial representation such that the input and output weight vectors are spanned only by that representation."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p13-4). *«[Для каждого нейрона существует нетривиальное представление, такое что входные и выходной векторы весов натянуты только на это представление.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p13-4)»*\
Доп. (что наблюдали до этого): [`"It was previously observed by Chughtai et al. 2023 that one-layer ReLU MLPs and transformers learn the task by mapping inputs $a,b$ to their respective matrices $R(a),R(b)$ for some irreducible representation $R$"`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p12-4) — *«[Ранее Chughtai et al. 2023 наблюдали, что однослойные ReLU-MLP и трансформеры выучивают задачу, отображая входы $a,b$ в соответствующие им матрицы $R(a),R(b)$ для некоторого неприводимого представления $R$](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p12-4)»*;
Доп. (конструкция): [`"there exists a construction of the network weights such that given inputs $a,b\in G$, the output at $c$ is $\mathrm{tr}(R(a)R(b)R(c)^{-1})$ using $2{d_{R}}^{3}$ neurons"`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p39-1) — *«[существует построение весов сети, такое что для данных входов $a,b\in G$ выход в $c$ равен $\mathrm{tr}(R(a)R(b)R(c)^{-1})$, при $2{d_{R}}^{3}$ нейронах](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p39-1)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Оспаривает: универсальность континуального фурье/сферического приора — неабелева `S5` опирается на дискретные coset-структуры, несовместимые со сферической геометрией. [`"successful $S_{5}$ solutions rely on discrete coset structures (Stander et al., 2024) rather than the continuous Fourier features"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-5). *«[успешные решения на $S_{5}$ опираются на дискретные структуры смежных классов (Stander et al., 2024), а не на непрерывные фурье-признаки](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p2-5)»*\
Доп.: [`"bypassing the memorization phase depends on alignment between architectural priors and the task’s intrinsic symmetry"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-5) — *«[обход фазы запоминания зависит от согласия архитектурных предпочтений с внутренней симметрией задачи](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p2-5)»*.

###### ref-2-2
**\[2.2\]** 2312.06581 — Stander et al. 2024, «Grokking Group Multiplication with Cosets». Оспаривает: эквивалентность GCR-рамки и coset-рамки — соответствия между смежными классами и неприводимыми представлениями нет, подгрупп асимптотически много больше. [`"The GCR algorithm and coset circuit cannot be *equivalent* because there is not, in fact, a one-to-one relationship between cosets and irreps."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p9-2). *«[Алгоритм GCR и контур на смежных классах не могут быть *эквивалентны*, потому что взаимно однозначного соответствия между смежными классами и неприводимыми представлениями на деле нет](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p9-2)»*\
Доп.: [`"any evidence that the linear layer implements matrix multiplication, again excluding scalar multiplication of the sign irrep"`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p8-8) — *«[каких-либо свидетельств того, что линейный слой осуществляет матричное умножение, снова за исключением скалярного умножения для знакового неприводимого представления](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p8-8)»*; хедж авторов существенен: [`"the GCR algorithm that [Chughtai et al. 2023] detail could indeed solve the problem of group multiplication"`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p9-5) — *«[алгоритм GCR, который подробно излагают [Chughtai et al. 2023], действительно мог бы решить задачу группового умножения](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p9-5)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2604.00316 — Tomàs et al., «Breaking Data Symmetry is Needed For Generalization in Feature Learning Kernels». Нюанс: обобщение = восстановление действия скрытой группы симметрии; матрицы признаков кодируют её элементы. [`"recovering the underlying symmetry group action inherent in the data"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4). *«[восстанавливая подлежащее действие группы симметрий, присущее данным](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p1-4)»*\
Доп.: [`"the learned feature matrices encode specific elements of the symmetry group"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4) — *«[выученные признаковые матрицы кодируют определённые элементы группы симметрий](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p1-4)»*; [`"generalization occurs only when a certain symmetry in the training set is broken"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4) — *«[генерализация наступает лишь тогда, когда нарушена определённая симметрия обучающего набора](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p1-4)»*.\
Доп. (группа симметрий сложения): [`"The symmetry group of modular addition $f(a,b)=a+b\,\mathrm{mod}\,p$ is isomorphic to the dihedral group $D_{2p}$"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p4-5) — *«[Группа симметрий модульного сложения $f(a,b)=a+b\,\mathrm{mod}\,p$ изоморфна диэдральной группе $D_{2p}$](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p4-5)»*

###### ref-3-2
**\[3.2\]** 2405.16658 — Park, Kim, Kim, «Acceleration of Grokking in Learning Arithmetic Operations via Kolmogorov-Arnold Representation». Нюанс: окружность модульного сложения выведена здесь из теории представлений — конечная абелева группа раскладывается в произведение циклических, каждая имеет одномерное неприводимое представление, вложение определяется как $\phi=\log\rho$, и характер $\exp(2\pi ix/p)$ получается следствием, — а PCA обученных вложений лишь подтверждает предсказанное. Практический вывод, ради которого всё строится: вложение не зависит от числа операндов, поэтому его можно переносить с бинарной операции на её композицию. Смежных классов, неабелевых групп и разбора того, какую частоту $k$ выбирает обученная сеть, в работе нет. [`"Since each $C_{q_{j}}$ is abelian, by Lemma 2, we have an irreducible representation for $C_{q_{j}}$"`](../papers/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation.card.md#p24-2). *«[Поскольку каждая $C_{q_{j}}$ абелева, по лемме 2 мы имеем неприводимое представление для $C_{q_{j}}$](../papers/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation.card.md#p24-2)»*\
Доп.: [`"we emphasize that the embedding and outer functions are independent of $n$, which denotes the number of inputs"`](../papers/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation.card.md#p7-6) — *«[мы обращаем внимание, что вложение и внешняя функция не зависят от $n$, обозначающего число входов](../papers/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation/2405.16658.acceleration-of-grokking-in-learning-arithmetic-operations-via-kolmogorov-arnold-representation.card.md#p7-6)»*.

###### ref-3-3
**\[3.3\]** 2604.06256 — Xu, «Spectral Edge Dynamics Reveal Functional Modes of Learning». Нюанс: догадка сформулирована через характеры, но испытаны только $\mathbb{Z}/p\mathbb{Z}$ и $(\mathbb{Z}/p\mathbb{Z})^{*}$; $S_{5}$ и групповая композиция не пробовались. [`"For tasks that factor through a group homomorphism $\phi:\mathcal{X}\to G$, these eigenmodes are the irreducible representations (characters) of $G$."`](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/original/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.md#p5-9). *«[Для задач, пропускающихся через групповой гомоморфизм $\phi:\mathcal{X}\to G$, эти собственные моды суть неприводимые представления (характеры) группы $G$.](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.card.md#p5-9)»*

###### ref-3-4
**\[3.4\]** 2606.12966 — Sivasankar, «Circuit Synchronization Precedes Generalization: A Causal Precursor to Grokking». [`"the analogue of the DFT is the Peter–Weyl transform"`](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p14-1). *«[аналогом DFT служит преобразование Питера–Вейля](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p14-1)»* — мощность нейрона в неприводимом представлении задаётся весом Планшереля, а упорядочение ведётся по $P_{j}(\rho)/d_{\rho}^{2}$, чтобы снять смещение в пользу наибольшего неприводимого представления.\
Доп. (сверка с частным случаем): [`"For $G=\mathbb{Z}_{p}$ every $d_{\rho}=1$, Eq. 4 is the DFT, and $\mathrm{FSD}_{\mathrm{gen}}$ *reduces exactly to FSD* (verified numerically)."`](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p14-2) — *«[Для $G=\mathbb{Z}_{p}$ все $d_{\rho}=1$, ур. 4 есть DFT, и $\mathrm{FSD}_{\mathrm{gen}}$ *сводится в точности к FSD* (проверено численно).](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p14-2)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2506.05718 — Notsawo et al., «Grokking Beyond the Euclidean Norm of Model Parameters». [`"neural networks trained on tasks like modular addition and sparse parity naturally converge to solutions based on Fourier features and group-theoretic representations"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p17-2). *«[нейронные сети, обучаемые задачам вроде остаточного сложения и разрежённой чётности, естественно сходятся к решениям на основе фурье-признаков и теоретико-групповых представлений](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.card.md#p17-2)»*

**\[4.2\]** 2306.17844 — Zhong et al. 2023, «The Clock and the Pizza». Групповые структуры названы только в перечне математических задач; вся разбираемая задача абелева, разложения по неприводимым представлениям в работе нет. [`"machine learning has been applied to learn other mathematical structures, including geometry [20], knot theory [21] and group theory [22]"`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p9-3). *«[машинное обучение применяли и к обучению другим математическим структурам, включая геометрию [20], теорию узлов [21] и теорию групп [22]](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p9-3)»*


### Внешние работы

###### ref-5-1
**\[5.1\]** 2601.21150 — Внешняя работа (демотирована из корпуса): Kvinge et al., «Can Neural Networks Learn Small Algebraic Worlds? An Investigation Into the Group-theoretic Structures Learned By Narrow Models Trained To Predict Group Operations». [`"sophisticated algorithms that depend on the representation theory of the corresponding groups"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p2-1). *«[изощрённые алгоритмы, опирающиеся на теорию представлений соответствующих групп](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p2-1)»*\
Доп.: [`"to what extent we can detect basic group-theoretic concepts from such a network"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p2-2) — *«[в какой мере в такой сети можно распознать основные теоретико-групповые понятия](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p2-2)»*.
```
concept:
  category: 2                    # 2. Механизмы и представления (Mechanisms & representations)
  papers_linked: 12             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-25
```
