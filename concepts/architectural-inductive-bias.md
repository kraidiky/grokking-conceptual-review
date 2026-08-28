# Архитектурное индуктивное смещение (architectural inductive bias)

[Переопараметризация и глубина](overparameterization-depth.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [4. Факторы обучения и оптимизации](index.md#cat-4)\
→ Следующая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)\
← Предыдущая категория: [3. Задачи и наборы данных](modular-arithmetic.md)

## Определение

**Архитектурное индуктивное смещение** — предпочтение решений, заложенное самим устройством сети, до всякого обучения: топологией остаточного потока, наличием нормировки, способом смешивания токенов. В корпусе [гроккинга](grokking.md) понятие ставится как управляемый множитель, а не как фон: *«индуктивные смещения, выраженные через устройство, модулируют гроккинг»*, и проверяется это архитектурным зондом — положением слойной нормировки, дающим *«разительно разные скорости гроккинга»* при схождении к одинаково устроенным решениям \[[1.3](#ref-1-3)\].

Главный итог линии — условный: обойти задержку архитектурой можно, *«но строго при условии согласия архитектурных предпочтений с симметрией задачи»* \[[1.1](#ref-1-1)\].

## Детализация

**Как ставится опыт.** Не постфактумным разбором обученной сети, а изменением топологии *до* обучения, с двумя независимыми вмешательствами: ограничением величины остаточного потока ($L_2$-нормированием, [сферическим ограничением](spherical-weight-norm-constraint.md)) и заменой выученного внимания равномерным сложением. Каждое по отдельности устраняет [фазу запоминания](memorization-phase.md) на модульном сложении и умножении: точность на обучении и на тесте растут вместе с самой инициализации \[[1.1](#ref-1-1)\].

**Отрицательный контроль — самое ценное здесь.** То же сферическое ограничение *«совершенно не работает»* на [некоммутативной композиции перестановок $S_5$](group-composition-non-commutative-s5.md): модели не обобщают ни на одном семени, хотя безупречной точности на обучении достигают \[[1.2](#ref-1-2)\]. Объяснение опирается на чужой разбор: обобщающие решения на $S_5$ держатся на дискретных [смежных классах](group-representations-cosets.md), а не на непрерывных [фурье-признаках](fourier-features-circuits.md), с которыми согласуется сферическая геометрия. Отсюда и вывод: дело не в ограничении вместимости самом по себе, а в согласии с [внутренней симметрией задачи](intrinsic-task-symmetry.md).

**Почему это делает понятие инструментом, а не оговоркой.** Связь работает в обе стороны: *«испытание архитектурных ограничений и наблюдение за тем, какие из них ускоряют или затрудняют генерализацию, способно обнаружить свойства внутреннего устройства задачи»* \[[3.1](#ref-3-1)\]. Провал ограничения становится измерением: он говорит, что обобщающее представление не лежит на непрерывном круговом многообразии. Порядок «истолковать контуры, затем устроить архитектуру» превращает [механистическую интерпретируемость](mechanistic-interpretability.md) в проектный приём.

**Где смещение остаётся необъяснённым остатком.** В работах, объясняющих гроккинг одной величиной, архитектура всплывает как то, что мешает объяснению замкнуться: схлопывание спектральной энтропии наблюдается и без гроккинга, а значит оно *«необходимо, но недостаточно»*, и *«своё дело делает и устройственное предпочтение»* \[[3.2](#ref-3-2)\]. Такое употребление — честное признание границы, но и напоминание, что «архитектурное смещение» в этой роли не измеряется, а называется.

**Чего понятие не объясняет.** Оно говорит о сроках и о том, наступит ли обобщение вообще, но не обязано менять сам выученный алгоритм. Это различение существенно и отделяет линию от [гипотезы универсальности](universality-hypothesis.md): архитектура двигает путь, а не непременно место назначения.

## Альтернативные определения и нюансы

### A. Смещение как топология

Сильная форма: степени свободы архитектуры продлевают запоминание, и их ограничение снимает задержку \[[1.1](#ref-1-1)\], \[[1.2](#ref-1-2)\]. Различающая черта — вмешательство ставится до обучения и потому даёт причинное, а не сопутственное свидетельство; управляющая величина здесь дискретна (топология есть или нет), в отличие от непрерывных множителей вроде [weight decay](weight-decay.md).

### B. Смещение как расположение нормировки

Мягкая форма: те же слои, иной порядок — и срок обобщения меняется на порядки, хотя решение получается то же \[[1.3](#ref-1-3)\]. Источник различия — предмет измерения: скорость, а не достижимость. Отсюда и роль в объяснении: архитектура задаёт, сколько идти, а не куда придёшь.

### C. Смещение как необъяснённый остаток

Служебная форма: понятие называется там, где единая мера не сходится с опытом \[[3.2](#ref-3-2)\]. Различающая черта — здесь оно не измеряется и работает как имя для расхождения; ценность такой записи в том, что она честно очерчивает область применимости объяснения, а не в том, что она что-то объясняет.

### D. Возражение: архитектура не меняет выучиваемого действия

Против сильной формы говорит наблюдение с другого уровня описания: *«многослойные перцептроны и трансформеры всеобще воплощают отвлечённое действие»* — [приближённую китайскую теорему об остатках](approximate-chinese-remainder-theorem.md) \[[2.1](#ref-2-1)\]. Если это верно, архитектурное смещение управляет сроками и достижимостью, но не содержанием решения, и тогда сильная форма A должна быть переформулирована как утверждение о пути, а не об алгоритме.

## Ссылки

###### ref-1-1
**\[1.1\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». [`"bypassing the generalization delay is possible—but strictly depends on alignment between architectural priors and task symmetry"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2). *«[обойти задержку генерализации возможно, но строго при условии согласия архитектурных предпочтений с симметрией задачи](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p1-2)»*

###### ref-1-2
**\[1.2\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». [`"the same spherical constraint fails entirely: models cannot generalize on any seed within the training window, despite reaching perfect training accuracy"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p2-5). *«[то же сферическое ограничение совершенно не работает: модели не могут обобщить ни на одном сиде в пределах окна обучения, несмотря на достижение безупречной точности на обучении](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p2-5)»*

###### ref-1-3
**\[1.3\]** 2602.06702 — Singh et al., «Explaining Grokking in Transformers through the Lens of Inductive Bias». [`"*Inductive biases expressed through architecture modulate grokking.*"`](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p2-1). *«[*Индуктивные смещения, выраженные через устройство, модулируют гроккинг.*](../papers/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias/2602.06702.explaining-grokking-in-transformers-through-the-lens-of-inductive-bias.card.md#p2-1)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». Оспаривает: если разные архитектуры воплощают одно и то же отвлечённое действие, архитектурное смещение управляет сроком, а не содержанием решения. [`"multilayer perceptrons and transformers universally implement the abstract algorithm"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2). *«[многослойные перцептроны и трансформеры всеобще воплощают отвлечённое действие](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p1-2)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Нюанс: связь обращается — по тому, какое ограничение помогает, а какое мешает, распознаётся строение самой задачи. [`"testing architectural constraints and observing which accelerate or impede generalization can reveal properties of the task’s intrinsic structure"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p16-2). *«[испытание архитектурных ограничений и наблюдение за тем, какие из них ускоряют или затрудняют генерализацию, способно обнаружить свойства внутреннего устройства задачи](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p16-2)»*

###### ref-3-2
**\[3.2\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation». Нюанс: понятие названо как остаток, не покрываемый единой мерой, — схлопывание энтропии случается и без гроккинга. [`"Entropy collapse is therefore **necessary but not sufficient** for generalisation in our setting; architectural inductive bias plays a role."`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p5-1). *«[Оттого схлопывание энтропии в нашей постановке **необходимо, но недостаточно** для генерализации; своё дело делает и устройственное предпочтение.](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p5-1)»*
