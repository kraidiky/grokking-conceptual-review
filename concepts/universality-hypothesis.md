# Гипотеза универсальности (universality hypothesis)

[Ортогональность градиента](orthogonal-gradient-perp-grad.md) ← предыдущая карточка, следующая → [approximate-chinese-remainder-theorem](approximate-chinese-remainder-theorem.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Гипотеза универсальности** — предположение, что сети, обученные на схожих задачах, приходят к схожему устройству: *«гипотеза универсальности (Olah et al. 2020; Li et al. 2016) утверждает, что модели выучивают схожие признаки и контуры в разных моделях, когда обучаются на схожих задачах»* \[[1.1](#ref-1-1)\]. Для корпуса [гроккинга](grokking.md) она — не побочный вопрос, а условие осмысленности всего предприятия: если решения произвольны, разбор одной грокнувшей сети ничего не говорит о следующей, и [механистическая интерпретируемость](mechanistic-interpretability.md) теряет предмет.

Слово в корпусе несёт и второй, не связанный с первым смысл — **класс универсальности** из статистической физики: набор критических показателей, общий для разных систем с одинаковым переходом. В этом смысле [двойной спуск](double-descent.md) *«помещается в иной класс универсальности, нежели изучаемый здесь переход гроккинга»* \[[1.4](#ref-1-4)\].

## Детализация

**Как гипотезу проверяют.** Не спором, а полигоном: берётся семейство родственных задач, где верное решение известно математически, и смотрят, сходятся ли сети к одному алгоритму. [Композиция элементов конечных групп](group-composition-non-commutative-s5.md) выбрана именно поэтому — она *«задаёт большое семейство родственных задач, образующее алгоритмический полигон для исследования универсальности»* \[[1.2](#ref-1-2)\]. [Модульная арифметика](modular-arithmetic.md) оказывается частным случаем, а не отдельной историей.

**Чем гипотеза была опровергнута в сильной форме.** Двумя ударами с разных сторон. Первый: на закреплённом обучающем наборе *«малые изменения гиперпараметров модели и инициализаций способны вызвать открытие качественно разных алгоритмов»*, и в одной сети алгоритмы уживаются параллельно \[[2.1](#ref-2-1)\] — отсюда [Clock и Pizza](clock-vs-pizza.md). Второй: заявленное объединение механизмов через теорию представлений не устояло, потому что сети на $S_5$ и $S_6$ *«открывают подлинное подгрупповое строение полной группы и сходятся на нейронных контурах, раскладывающих групповую арифметику при помощи подгрупп»* — [схемы на смежных классах](group-representations-cosets.md), а не композицию представлений \[[2.2](#ref-2-2)\].

**Как гипотезу переформулировали, чтобы она пережила опровержение.** Ключ — уровень описания. Утверждение переносится с нейронов на отвлечённый алгоритм: *«кажущиеся несхожими решения нейронных сетей, наблюдаемые в простой задаче остаточного сложения, объединены общим отвлечённым действием»* \[[1.3](#ref-1-3)\], которым объявляется [приближённая китайская теорема об остатках](approximate-chinese-remainder-theorem.md). В этой постановке coset-схемы на перестановках не опровержение, а частный случай: *«наши приближённые смежные классы обобщают смежные классы»*, и потому найденное на перестановках *«согласуется с нашими итогами»* \[[3.1](#ref-3-1)\]. Цена переформулировки — падение проверяемости: чем отвлечённее описан алгоритм, тем труднее указать наблюдение, которое его опровергнет.

**Что от этого остаётся практике.** Различение уровней: универсальность на уровне нейронов опровергнута, на уровне [фурье-признаков](fourier-features-circuits.md) держится с оговорками, на уровне отвлечённого алгоритма заявлена и обсуждается. Разбирая новую грокнувшую сеть, стоит сразу называть уровень, на котором ожидается совпадение, — иначе спор о «том же самом алгоритме» неразрешим. Сюда же относится и требование к [разбросу по семенам](seed-variance-reproducibility.md): совпадение решений между прогонами — это и есть измерение универсальности в самой узкой форме.

## Альтернативные определения и нюансы

### A. Универсальность признаков и контуров

Исходная форма из механистической интерпретируемости \[[1.1](#ref-1-1)\]. Различающая черта — проверяется сопоставлением частей: находят ли в двух сетях одинаковые нейроны и подсхемы. В корпусе гроккинга эта форма опровергнута прямым опытом \[[2.1](#ref-2-1)\], и именно её опровержение сделало вопрос живым.

### B. Универсальность отвлечённого алгоритма

Ослабленная форма: совпадать должны не части, а вычисляемая функция и её разложение \[[1.3](#ref-1-3)\]. Источник различия — уровень описания; следствие — работа переносится на доказательство, что все наблюдавшиеся схемы суть воплощения одного отвлечённого действия \[[3.1](#ref-3-1)\], а спор смещается к тому, не стало ли описание настолько общим, что покрывает и то, что различать хотелось; историю этого смещения работа излагает сама \[[3.2](#ref-3-2)\].

### C. Класс универсальности

Омоним из статистической физики: две системы принадлежат одному классу, если у их переходов совпадают критические показатели \[[1.4](#ref-1-4)\]. Различающая черта — предмет сравнения не устройство сети, а форма расходимости вблизи перехода; и требования к заявке иные — нужны конечноразмерные проверки, а не разбор весов. Заявка о классе для гроккинга в корпусе прямо объявлена неустановленной: *«не установлены окончательный род перехода и устойчивый класс всеобщности»* \[[1.5](#ref-1-5)\]. Смешивать эти два смысла нельзя: совпадение показателей ничего не говорит об одинаковости алгоритмов, а одинаковость алгоритмов — о показателях.

## Ссылки

###### ref-1-1
**\[1.1\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». [`"The *universality hypothesis* (Olah et al. 2020; Li et al. 2016) asserts that models learn similar features and circuits across different models when trained on similar tasks."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p1-3). *«[*Гипотеза универсальности* (Olah et al. 2020; Li et al. 2016) утверждает, что модели выучивают схожие признаки и контуры в разных моделях, когда обучаются на схожих задачах.](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p1-3)»*

###### ref-1-2
**\[1.2\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». [`"as this defines a large family of related tasks, forming an algorithmic test bed for investigating universality"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p1-4). *«[поскольку она задаёт большое семейство родственных задач, образующее алгоритмический полигон для исследования универсальности](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p1-4)»*

###### ref-1-3
**\[1.3\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"seemingly disparate neural network solutions observed in the simple task of modular addition are unified under a common abstract algorithm"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2). *«[кажущиеся несхожими решения нейронных сетей, наблюдаемые в простой задаче остаточного сложения, объединены общим отвлечённым действием](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p1-2)»*

###### ref-1-4
**\[1.4\]** 2606.13753 — Truong et al., «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». [`"placing it in a different universality class from the grokking transition studied here"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-3). *«[что помещает его в иной класс универсальности, нежели изучаемый здесь переход гроккинга](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-3)»*

###### ref-1-5
**\[1.5\]** 2603.24746 — Bi et al., «Grokking as a Falsifiable Finite-Size Transition». [`"What is not established is the final transition order or a stable universality class."`](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p6-5). *«[Не установлены окончательный род перехода и устойчивый класс всеобщности.](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p6-5)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2306.17844 — Zhong et al., «The Clock and the Pizza: Two Stories in Mechanistic Explanation of Neural Networks». Оспаривает: универсальность на уровне алгоритма опровергнута на закреплённом обучающем наборе — различие вносят только настройки. [`"Small changes to model hyperparameters and initializations can induce discovery of qualitatively different algorithms from a fixed training set"`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p1-2). *«[Малые изменения гиперпараметров модели и инициализаций способны вызвать открытие качественно разных алгоритмов на одном и том же обучающем наборе](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p1-2)»*

###### ref-2-2
**\[2.2\]** 2312.06581 — Stander et al., «Grokking Group Multiplication with Cosets». Оспаривает: на перестановках сети выучивают не композицию представлений, а разложение по подгруппам, чем снимается заявка об объединении механизмов. [`"The models discover the true subgroup structure of the full group and converge on neural circuits that decompose the group arithmetic using the permutation group’s subgroups."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p1-2). *«[Модели открывают подлинное подгрупповое строение полной группы и сходятся на нейронных контурах, раскладывающих групповую арифметику при помощи подгрупп группы перестановок.](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p1-2)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». Нюанс: опровержение обращается в частный случай — приближённые смежные классы объемлют обычные, и разные группы попадают под одно описание. [`"As our approximate cosets generalize cosets, work finding coset circuits in networks trained on permuting lists (permutation groups) [9] aligns with our results."`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-1). *«[Поскольку наши приближённые смежные классы обобщают смежные классы, работа, нашедшая контуры на смежных классах в сетях, обучаемых перестановкам списков (группы перестановок) [9], согласуется с нашими итогами.](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-1)»*

###### ref-3-2
**\[3.2\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». Нюанс: история спора изложена самой работой — из неё и выводится требование формулировать гипотезу на уровне отвлечённого действия. [`"later work by Stander et al. [9] reverse-engineered models trained on $S_{n}$ under identical conditions and found that networks instead learn *coset*-based circuits, refuting the GCR universality claim"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-4). *«[более поздняя работа Stander et al. [9] разобрала на части модели, обученные на $S_{n}$ в тех же условиях, и нашла, что сети вместо этого выучивают контуры на *смежных классах*, опровергнув заявку GCR о всеобщности](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-4)»*
