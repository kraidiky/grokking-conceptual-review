# Приближённая китайская теорема об остатках (approximate Chinese Remainder Theorem, aCRT)

[Гипотеза универсальности](universality-hypothesis.md) ← предыдущая карточка, следующая → [generalization-circuit](generalization-circuit.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Приближённая китайская теорема об остатках** — предлагаемое отвлечённое описание того, что сеть вычисляет, грокнув [модульную арифметику](modular-arithmetic.md): задача разбивается на несколько независимых подсистем по остаткам, каждая решается отдельно, ответы пересекаются. Заявка сильна тем, что относится не к одной архитектуре: *«многослойные перцептроны и трансформеры всеобще воплощают отвлечённое действие, которое мы называем приближённой китайской теоремой об остатках»* \[[1.1](#ref-1-1)\]. Классическая КТО опирается на смежные классы; здесь их приходится ослабить, потому что сети выучивают частоты, не делящие модуль: *«теперь мы вводим приближённые смежные классы (приближённые классы равнозначности) через наименьшее расстояние по путям между вершинами в $\Gamma$»*, где $\Gamma$ — граф Кэли \[[1.2](#ref-1-2)\]. Итог этого ослабления работа формулирует прямо: *«тем самым приближённые смежные классы общее смежных»*.

## Детализация

**Что здесь измеримо.** Утверждение о нейронах: *«простые нейроны в слое 1 возбуждаются (ReLU > 0) на приближённом смежном классе, содержащем верный ответ $c$… все нейроны в более поздних скрытых слоях возбуждаются на линейных сочетаниях приближённых смежных классов»* \[[1.3](#ref-1-3)\]. Это проверяемо прямо: берётся обученная сеть, для каждого нейрона считается, на каком множестве входов он молчит. Тем самым отвлечённый алгоритм получает наблюдаемое следствие, чего [гипотезе универсальности](universality-hypothesis.md) в её общей форме как раз не хватало.

**Числовое предсказание.** Из аналогии с КТО следует счёт: *«CRT пользуется $\mathcal{O}(\log(n))$ остаточными подсистемами, а следствие 4.8 даёт, что DNN нужно $\mathcal{O}(\log(n))$ различных частот»* \[[1.4](#ref-1-4)\]. Это отличает рамку от пересказа: она предсказывает, сколько [частот](fourier-features-circuits.md) должно быть выучено, и предсказание сверяется с опытом при разных модулях.

**Отношение к предшественникам.** Рамка выстроена как обобщение двух разборов, до того считавшихся соперничающими. Первый — [алгоритм фурье-умножения](fourier-features-circuits.md), где *«слои внимания и MLP затем комбинируют их с помощью тригонометрических тождеств»* \[[3.1](#ref-3-1)\]; второй — [схемы на смежных классах](group-representations-cosets.md), найденные на группах перестановок \[[3.2](#ref-3-2)\]. Приближённые смежные классы объемлют обычные, поэтому оба разбора оказываются воплощениями одного действия. Туда же попадает и [Pizza](clock-vs-pizza.md): различие с Clock объявляется низкоуровневым, а не алгоритмическим \[[3.3](#ref-3-3)\].

**Чего рамка не покрывает — по признанию авторов.** Проверка поставлена на двух крайностях группового строения — циклических группах и группах перестановок: *«существенное ограничение нашей работы в том, что мы не исследуем группы между этими двумя краями»* \[[1.5](#ref-1-5)\]. Промежуточные случаи и есть место, где обобщение может сломаться, и оговорка сохранена дословно, потому что без неё заявка выглядит шире, чем проверена.

**Как читать заявку.** «Приближённая» здесь не смягчение, а существенная часть: строгая КТО требует, чтобы частоты делили модуль, а сети выучивают частоты, которые его не делят. Всё содержание рамки в том, чем заменяется точное деление — расстоянием на графе Кэли; на этом же держится и её уязвимость, поскольку достаточно широкое понятие близости подойдёт к слишком многому.

## Альтернативные определения и нюансы

### A. aCRT как описание вычисления

Основная форма: сеть раскладывает задачу на подсистемы по остаткам и пересекает их ответы \[[1.1](#ref-1-1)\], \[[1.4](#ref-1-4)\]. Различающая черта — уровень описания: говорится о вычисляемой функции и её разложении, а не о том, какие нейроны его несут; отсюда и совместимость с несовпадающими разборами на уровне нейронов.

### B. Приближённые смежные классы как самостоятельное понятие

Инструментальная форма: главное не КТО, а ослабление смежного класса до множества, близкого по графу Кэли \[[1.2](#ref-1-2)\], \[[1.3](#ref-1-3)\]. Источник различия — это понятие переживает отказ от аналогии с теоремой: даже если разложение по остаткам окажется натяжкой, утверждение «нейрон возбуждается на связном куске графа Кэли» проверяется само по себе.

### C. aCRT как объединяющая рамка

Историческая форма: ценность не в новом механизме, а в том, что прежние несовместимые разборы сведены к одному \[[3.1](#ref-3-1)\], \[[3.2](#ref-3-2)\], \[[3.3](#ref-3-3)\]. Различающая черта — проверяется не опытом, а перечислением: показывается, что каждое известное описание получается частным случаем. Слабость ровно там же: объединение достигается расширением определения, и вопрос, не потеряно ли при этом различение, остаётся открытым.

## Ссылки

###### ref-1-1
**\[1.1\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"multilayer perceptrons and transformers universally implement the abstract algorithm we call the approximate Chinese Remainder Theorem"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2). *«[многослойные перцептроны и трансформеры всеобще воплощают отвлечённое действие, которое мы называем приближённой китайской теоремой об остатках](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p1-2)»*

###### ref-1-2
**\[1.2\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"We now introduce **approximate cosets** (approximate equivalence classes) using the minimum path distance between vertices in $\Gamma$."`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p4-5). *«[Теперь мы вводим **приближённые смежные классы** (приближённые классы равнозначности) через наименьшее расстояние по путям между вершинами в $\Gamma$.](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p4-5)»*

###### ref-1-3
**\[1.3\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"*Simple neurons in layer 1 activate (ReLU $>0$) on an approximate coset containing the correct answer $c$, by concentrating their preactivations on approximate cosets that contain $a$ and $b$; all neurons in later hidden layers activate on linear combinations of approximate cosets.*"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p4-7). *«[*Простые нейроны в слое 1 возбуждаются (ReLU $>0$) на приближённом смежном классе, содержащем верный ответ $c$, сосредоточивая свои предвозбуждения на приближённых смежных классах, содержащих $a$ и $b$; все нейроны в более поздних скрытых слоях возбуждаются на линейных сочетаниях приближённых смежных классов.*](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p4-7)»*

###### ref-1-4
**\[1.4\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"The CRT uses $\mathcal{O}(\log(n))$ modular subsystems, and Corollary 4.8 gives that DNNs need $\mathcal{O}(\log(n))$ unique frequencies"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p9-1). *«[CRT пользуется $\mathcal{O}(\log(n))$ остаточными подсистемами, а следствие 4.8 даёт, что DNN нужно $\mathcal{O}(\log(n))$ различных частот](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p9-1)»*

###### ref-1-5
**\[1.5\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». [`"A core limitation of our work is that we do not explore the groups between these two extremes."`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p9-4). *«[Существенное ограничение нашей работы в том, что мы не исследуем группы между этими двумя краями.](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p9-4)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2301.05217 — Nanda et al., «Progress measures for grokking via mechanistic interpretability». Нюанс: предшествующий разбор, который aCRT объявляет своим частным случаем, — сложение углов через тригонометрические тождества. [`"The attention and MLP layers then combine these using trigonometric identities to compute the sine and cosine of $w_{k}(a+b)$"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-1). *«[Слои внимания и MLP затем комбинируют их с помощью тригонометрических тождеств, чтобы вычислить синус и косинус $w_{k}(a+b)$](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-1)»*

###### ref-3-2
**\[3.2\]** 2312.06581 — Stander et al., «Grokking Group Multiplication with Cosets». Нюанс: второй частный случай — схемы на смежных классах, найденные на группах перестановок; приближённые классы объемлют обычные. [`"The models discover the true subgroup structure of the full group and converge on neural circuits that decompose the group arithmetic using the permutation group’s subgroups."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p1-2). *«[Модели открывают подлинное подгрупповое строение полной группы и сходятся на нейронных контурах, раскладывающих групповую арифметику при помощи подгрупп группы перестановок.](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p1-2)»*

###### ref-3-3
**\[3.3\]** 2306.17844 — Zhong et al., «The Clock and the Pizza: Two Stories in Mechanistic Explanation of Neural Networks». Нюанс: та самая пара соперничающих схем, различие между которыми aCRT объявляет низкоуровневым. [`"Some networks trained to perform modular addition implement a familiar *Clock* algorithm (previously described by Nanda et al. [1]); others implement a previously undescribed, less intuitive, but comprehensible procedure we term the *Pizza* algorithm"`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p1-2). *«[Часть сетей, обученных выполнять модульное сложение, осуществляет знакомый алгоритм *Clock* (описанный ранее у Nanda et al. [1]); другие осуществляют ранее не описанную, менее умозрительную, но постижимую процедуру, которую мы называем алгоритмом *Pizza*](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p1-2)»*
