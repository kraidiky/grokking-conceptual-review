# Параметр порядка (order parameter)

[Время гроккинга](grokking-time.md) ← предыдущая карточка, следующая → [Тяжёлохвостовая саморегуляризация](heavy-tailed-self-regularization-htsr.md)

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Параметр порядка** (order parameter) — макроскопическая скалярная (реже
векторная) величина, характеризующая состояние системы и резко меняющаяся,
когда управляющий параметр (варьируемая величина — размер выборки, сила weight
decay, число шагов обучения) пересекает критический порог; в статистической
физике это канонический маркер [фазового перехода](phase-transition.md)
\[[1.2](#ref-1-2)\]. В теориях [гроккинга](grokking.md) параметром порядка
называют внутреннюю величину, чей скачок (или пересечение порога) отмечает
переход от запоминания к генерализации; впервые его формализовали как перекрытие
весов нейрона с «учительским» направлением (целевым вектором в teacher–student
постановке) \[[1.1](#ref-1-1)\].

![Качество представления (RQI) как скрытый параметр порядка: точность, предсказанная по представлению, меняется вместе с измеренной (рис. 3 Liu et al.)](assets/order-parameter-rqi.png)

## Детализация

Понятие пришло из [статистической механики](effective-theory-statistical-mechanics.md): при непрерывном переходе (второго
рода) параметр порядка плавно вырастает от нуля с расходящимися флуктуациями, а
при переходе первого рода он меняется скачком, отражая сосуществование и обмен
фаз \[[1.2](#ref-1-2)\]. Его главная роль — свести многомерное состояние сети к
одному скаляру: у Rubin et al. апостериорное распределение по всем весам
редуцируется к вероятности по единственной величине Φ (перекрытие весов нейрона
с учительским вектором), и именно скачок Φ между сёдлами действия и есть фазовый
переход первого рода \[[1.1](#ref-1-1)\]. Разные работы предлагают собственные
параметры порядка для гроккинга: у Hong et al. это лексическая когерентность
(согласованность на уровне слов), играющая роль параметра порядка для
[эмерджентных](emergence.md) языковых способностей (навыков, внезапно
возникающих при росте масштаба) \[[1.2](#ref-1-2)\]; у Truong et al. —
спектральная энтропия ковариации представлений \[[3.1](#ref-3-1)\]; у Bi et al. —
спектральный контраст «голова–хвост» собственного спектра представлений
\[[2.1](#ref-2-1)\]. Сама пригодность параметра порядка как доказательства
перехода оспаривается: Bi et al. настаивают, что подгонки сигмоиды к одному
прогону недостаточно, и требуют конечно-размерного масштабирования — finite-size
scaling: проверки пересечения биндеровских кумулянтов (безразмерных комбинаций
моментов параметра порядка) и степенного роста восприимчивости
\[[2.1](#ref-2-1)\], — тогда как Truong et al., поддерживая рамку, оговаривают,
что один скалярный параметр порядка не описывает полностью разброс между
случайными инициализациями \[[3.1](#ref-3-1)\].

## Альтернативные определения и нюансы

### A. Формальный статфизический параметр порядка: перекрытие с учителем

Самая строгая трактовка — буквальный статфизический параметр порядка. Rubin et
al. показывают, что в подходящем масштабном пределе всё апостериорное
распределение по весам сети сводится к скалярной вероятности по единственной
величине Φ = w·w* (перекрытие вектора весов нейрона с учительским)
\[[1.1](#ref-1-1)\]. Управляющий параметр u (эффективное взаимодействие,
задаваемое размером выборки, [шумом](gradient-noise.md) и шириной сети) при критическом u_c порождает
новые сёдла действия, и среднее Φ меняется скачком — это делает гроккинг фазовым
переходом первого рода. Ключевое отличие: параметр порядка выводится из
первопринципной статфизической модели, а его разрыв — предсказание теории, а не
просто наблюдаемая на кривой ступенька.

### B. Каноническое определение Ландау и лексический параметр порядка

Hong et al. берут учебниковое определение Ландау–Лифшица: параметр порядка —
макроскопическая величина, характеризующая состояние системы и резко меняющаяся,
когда управляющий параметр пересекает критический порог; при непрерывном переходе
она плавно растёт от нуля, при переходе первого рода — скачет
\[[1.2](#ref-1-2)\]. Прикладывая это к малым трансформерным языковым моделям, они
назначают конкретный параметр порядка — лексическую когерентность (структурность
на уровне слов), синхронные разрывы которой во время обучения отмечают появление
связной речи. Ключевое отличие от трактовки A: параметр порядка здесь не выводится
из модели, а выбирается как измеримая наблюдаемая, и переход диагностируется по её
эмпирическому разрыву прямо в линейном (а не логарифмическом) масштабе обучения.

### Оспаривают

**Параметр порядка сам по себе ничего не доказывает** \[[2.1](#ref-2-1)\]: Bi et
al. возражают против принятой в машинном обучении практики «подогнать сигмоиду к одному
прогону и объявить переход». Они принимают спектральный контраст «голова–хвост»
как параметр порядка уровня представлений, но требуют полной конечно-размерной
цепочки диагностики (пересечение биндеровских кумулянтов по размеру группы p,
степенной рост восприимчивости), которая отвергает гладкий кроссовер (разница по
AIC = 16.8 в пользу перехода), — при этом порядок самого перехода остаётся
неопределённым. Источник различия: параметр порядка легитимен лишь внутри
фальсифицируемого FSS-протокола, а не как вольная аналогия.

### Поддерживают

**Спектральная энтропия как феноменологический параметр порядка**
\[[3.1](#ref-3-1)\]: Truong et al. присоединяются к рамке, вводя параметр порядка
для геометрии представлений — спектральную энтропию ковариации активаций,
инвариантную к перемасштабированию весов (в отличие от мер сжатия/сложности,
живущих в пространстве параметров). Оговорка-нюанс: один скалярный параметр
порядка не может полностью описать стохастику между сидами — случайными посевами (разные инициализации
достигают энтропийного порога с разной скоростью), поэтому степенной закон
предсказания времени гроккинга подаётся как вероятностный прогноз, а не
детерминированный. Источник различия: та же рамка параметра порядка, но
перенесённая в пространство активаций и честно ограниченная одним скаляром.

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.03789 — Rubin et al., «Grokking as a First Order Phase Transition in Two Layer Networks». [`"can be approximately marginalized to track a single random variable called an order parameter"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p4-2). *«[оставив одну случайную величину, называемую параметром порядка](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p4-2)»*\
Доп.: [`"Notably, this expression reduces the high-dimensional network posterior into a scalar probability"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p6-4) — *«[Примечательно, что это выражение сводит высокоразмерное апостериорное распределение сети к скалярной вероятности](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p6-4)»*.

###### ref-1-2
**\[1.2\]** 2511.12768 — Hong et al., «Evidence of Phase Transitions in Small Transformer-Based Language Models». [`"Landau and Lifshitz’s canonical treatment establishes the foundational concepts: an order parameter—a macroscopic quantity characterizing the state of the system—changes abruptly as a control parameter crosses a critical threshold [8]"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p2-3). *«[Узаконенное изложение Ландау и Лифшица закладывает основные понятия: параметр порядка — макроскопическая величина, описывающая состояние системы, — резко меняется, когда управляющий параметр переходит решающий порог \[8\]](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p2-3)»*\
Доп.: [`"We argue that lexical-level coherence functions as an order parameter for emergent linguistic ability"`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p1-9) — *«[Мы доказываем, что связность на уровне слов работает как параметр порядка возникающей языковой способности](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p1-9)»*.


###### ref-1-3
**\[1.3\]** 2505.11411 — Zhang, Shang, Yang, Zhang, «Is Grokking a Computational Glass Relaxation?». Назначает параметром порядка тестовую точность явно и по аналогии с плотностью при переходе жидкость—газ, и на её непрерывности строит отрицание перехода первого рода. Нюанс: та же величина служит второй координатой сэмплирования и ради градиента по параметрам сглажена сигмоидой с наклоном $s=7$ — то есть непрерывна по построению; чувствительность выводов к наклону не показана. [`"Given this, we identify the test accuracy as the order parameter for characterizing grokking."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p6-1). *«[Ввиду этого мы определяем тестовую точность как параметр порядка для описания гроккинга.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p6-1)»*\
Доп. (двойная роль величины): [`"We choose $ln(L_{train})$ as the energy in a broad sense $x_{WL}$, and $A_{test}$ as the order parameter $y_{WL}$."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p5-4) — *«[Мы выбираем $ln(L_{train})$ как энергию в широком смысле $x_{WL}$, а $A_{test}$ — как параметр порядка $y_{WL}$.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p5-4)»*\
Доп. (сглаживание ради градиента): [`"Since $A_{test}$ is discrete, we approximate its gradient using the sigmoid function, following the approach in yang2025high."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p5-5) — *«[Поскольку $A_{test}$ дискретна, мы приближаем её градиент сигмоидой, следуя подходу yang2025high.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p5-5)»*.

###### ref-1-4
**\[1.4\]** 2503.10483 — Pomarico et al., «Grokking as an entanglement transition in tensor network machine learning». Нюанс: переход наблюдается у всех 20 генных сообществ, действенный гроккинг — почти в 60%; в искусственной вырожденной сверке «кроссовок против кроссовка» переход виден там, где классифицировать нечего. [`"The observation of a gain in independent sets classification implies that an entanglement transition occurs, but the viceversa does not hold true"`](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p27-4). *«[Наблюдение выигрыша в классификации независимых наборов влечёт, что переход запутанности происходит, но обратное неверно](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p27-4)»*\
Доп. (что именно измеряется): [`"The training dynamics of each gene community in Table 1 undergoes an entanglement transition, with an effective grokking in independent sets classification observed in almost $60\%$ of cases"`](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p13-1) — *«[Динамика обучения каждого генного сообщества из Таблицы 1 претерпевает переход запутанности, причём действенный гроккинг в классификации независимых наборов наблюдается почти в $60\%$ случаев](../papers/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning/2503.10483.grokking-as-an-entanglement-transition-in-tensor-network-machine-learning.card.md#p13-1)»*.

###### ref-1-5
**\[1.5\]** 2605.20441 — Verma 2026, «Weight Decay Regimes in Grokking Transformers: Cheap Online Diagnostics». Вводит два параметра порядка, отличающиеся от прежних ценой и местом замера: средняя попарная косинусная близость голов и разброс энтропий по головам считаются из активаций прямого прохода, стоят около 3 % времени по часам при замере раз в 10 шагов и не требуют ни контрольных точек, ни цепей выборки. Нюанс: величины определены только для архитектур с головами; связи с нарушаемой симметрией работа не проводит; и по собственному заранее объявленному критерию (AUC $\geq 0.85$) диагностики признаны корреляционными, а не предсказательными. [`"We define two scalar quantities computable at every training step: mean pairwise cosine similarity $\bar{s}(t)$ and entropy standard deviation $\sigma_{H}(t)$."`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p2-2). *«[Мы определяем две скалярные величины, вычислимые на каждом шаге обучения: среднюю попарную косинусную близость $\bar{s}(t)$ и стандартное отклонение энтропии $\sigma_{H}(t)$.](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p2-2)»*\
Доп. (собственный отрицательный вердикт): [`"the order parameters at epoch 1 K carry retention signal but do not reach the predictor threshold on out-of-distribution WDs"`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p13-4) — *«[параметры порядка на эпохе 1 тыс. несут сигнал об удержании, но не достигают порога предсказателя на WD вне распределения](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p13-4)»*.

###### ref-1-6
**\[1.6\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Обращается с нормой весов как с управляющим параметром в рабочем, а не в лексическом смысле: величину удерживают на заданном значении пошаговой проекцией и строят по ней дозовый отклик, а вторую ось (скорость обучения) прогоняют той же сеткой, чтобы показать разделимость. Нюанс: «параметр порядка» в статфизическом смысле работа не вводит — нарушаемой симметрии не называет, конечноразмерного схлопывания по критической точке не делает, и от отнесения к классам универсальности по показателям прямо отказывается; сворачивание здесь идёт по размеру задачи при одном общем показателе, а не по расстоянию до порога. [`"we identify a scalar control parameter — the weight norm — and measure its causal, quantitative effect on the timescale of the transition"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-2). *«[мы указываем скалярный управляющий параметр — норму весов — и меряем его причинное количественное действие на временной масштаб перехода](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-2)»*\
Доп. (разделение двух осей): [`"The norm sets the *timescale* of grokking; the learning rate sets the rate within it."`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p6-2) — *«[Норма задаёт *временной масштаб* гроккинга; скорость обучения задаёт скорость внутри него.](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p6-2)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2603.24746 — Bi et al., «Grokking as a Falsifiable Finite-Size Transition». Оспаривает: параметр порядка легитимен лишь внутри фальсифицируемого конечно-размерного протокола, а не как аналогия. [`"phase-transition language, but that claim has lacked falsifiable finite-size inputs"`](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-2). *«[на языке фазовых переходов, но этой заявке недоставало опровержимых конечноразмерных входных величин](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-2)»*\
Доп.: [`"requires two inputs that are not automatic: an extensive size variable and a representation-level order parameter"`](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-5) — *«[требует двух входных величин, которые сами собой не даются: экстенсивной переменной размера и параметра порядка на уровне представлений](../papers/2603.24746.grokking-as-a-falsifiable-finite-size-transition/2603.24746.grokking-as-a-falsifiable-finite-size-transition.card.md#p1-5)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation». Нюанс: присоединяется к рамке, вводя спектральную энтропию представлений как параметр порядка, но оговаривает недостаточность одного скаляра. [`"analogous but distinct phase-transition-like signature in the small-scale grokking setting, characterised by a single scalar order parameter"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p4-1). *«[подобный, но отличный признак фазового перехода в малой постановке гроккинга, описываемый одним скалярным параметром порядка](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p4-1)»*\
Доп.: [`"A single scalar order parameter cannot fully capture this stochasticity"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p9-4) — *«[Один скалярный параметр порядка этой случайности вполне не схватывает](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p9-4)»*.\
Доп. (измеренный порог): [`"there exists an empirically stable threshold $\tilde{H}^{*}=0.609$"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p5-10) — *«[существует опытно устойчивый порог $\tilde{H}^{*}=0.609$](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p5-10)»*

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». [`"and the curvature defect serving as an order parameter"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p18-6). *«[а дефект кривизны служит параметром порядка](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p18-6)»*

**\[4.2\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». Нюанс: LLC предложен как величина, считаемая только по обучающим данным и повторяющая ход потери на проверке. [`"Despite the LLC being calculated exclusively from the training data, its evolution closely mirrors that of the validation loss"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p8-8) — *«[Хотя LLC вычисляется исключительно по обучающим данным, его развитие тесно вторит развитию потери на проверке](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p8-8)»*

**\[4.3\]** 2306.17844 — Zhong et al. 2023, «The Clock and the Pizza». Строит двумерную фазовую диаграмму с резкой границей и две скалярные меры, различающие фазы, но термином «параметр порядка» не пользуется и особенности мер в точке перехода не показывает: рисунок 6 — совместное распределение по совокупности прогонов. [`"These model-internal phase transitions are harder to study, but closer to corresponding phenomena in physical systems [24]."`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p9-4). *«[Такие внутримодельные фазовые переходы изучать труднее, но они ближе к соответствующим явлениям в физических системах [24].](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p9-4)»*

**\[4.4\]** 2210.15435 — Žunkovič, Ilievski, «Grokking phase transitions in learning local rules with gradient descent». Нюанс: параметра порядка работа не вводит и симметрии не называет — неаналитичной величиной служит сама тестовая ошибка, управляющим параметром время обучения; ландшафтную постановку Ландау приписывать авторам нельзя. Ценно другое: разделены роли величин — вероятность и время гроккинга зависят от начальных условий и хода обучения, а показатель только от плотности данных у границы области. [`"Obtained critical exponent $\nu=\frac{D+1}{2}$ is universal for isotropic probability densities that do not vanish at the ball boundary and is consistent with the result in Eq. 23."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p17-5). *«[Полученный критический показатель $\nu=\frac{D+1}{2}$ универсален для изотропных плотностей вероятности, не обращающихся в нуль на границе шара, и согласуется с результатом Ур. 23](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p17-5)»*\
Доп.: [`"the critical exponent $\nu$ depends only on the data distribution at the boundary of the domain"`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p18-1) — *«[критический показатель $\nu$ зависит только от распределения данных на границе области](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p18-1)»*.
