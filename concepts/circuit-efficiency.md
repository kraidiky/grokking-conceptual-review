# Эффективность контуров (circuit efficiency)

[Переход lazy→rich](lazy-to-rich-kernel-to-feature-learning.md) ← предыдущая карточка, следующая → [Сжатие многообразия представлений](manifold-representation-compression.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Circuit efficiency (эффективность контуров)** — свойство контура (подсети,
реализующей некоторый под-алгоритм внутри сети) выдавать заданную величину
логитов (значений на выходе перед softmax) при **меньшей норме параметров**
(L2-длине весов). Когда несколько контуров одинаково хорошо решают обучающую
выборку, **[weight decay](weight-decay.md)** (L2-штраф на нормы весов) отбирает более эффективные из
них. Понятие введено Varma et al. (2023) для объяснения [гроккинга](grokking.md)
\[[1.1](#ref-1-1)\].

![Потеря, норма параметров и логит верного ответа для обобщающего и запоминающего контуров: гроккинг возникает потому, что обобщающий контур эффективнее (рис. 9 Varma et al.)](assets/circuit-efficiency-crossover.png)

## Детализация

Механизм строится на противоборстве двух сил у любого минимума потерь.
Кросс-энтропия поощряет **увеличивать** логиты (уверенные ответы = меньшая
потеря), а значит — наращивать веса; weight decay тянет веса **вниз**. У более
эффективного контура «рычаг» кросс-энтропии сильнее, а давление weight decay
слабее, поэтому такой контур доминирует \[[1.1](#ref-1-1)\]. Varma et al.
постулируют два семейства контуров: обобщающий `Cgen` и запоминающий `Cmem`
(последний хранит [заученные примеры](memorization-phase.md)). Гроккинг возникает
при трёх условиях: `Cgen` обобщает, `Cgen` **эффективнее** `Cmem`, но `Cgen`
учится **медленнее**; тогда норма параметров со временем «перетекает» от `Cmem` к
`Cgen`, вызывая [фазовый переход](phase-transition.md) тестовой точности
\[[1.1](#ref-1-1)\].

Мера, которой действенность оценивают без разбора самой схемы, называется **локальной сложностью контура** (local circuit complexity).

Ключевое следствие — зависимость от объёма данных. Обобщающий контур работает и
на новых точках, поэтому его эффективность от размера набора не зависит; а
запоминающий вынужден заучивать каждую добавленную точку и потому **теряет
эффективность** с ростом набора. Отсюда — **[критический размер данных](data-fraction-critical-dataset-size.md)** `Dcrit`,
при котором оба контура одинаково эффективны \[[1.1](#ref-1-1)\]. Этот кроссовер
порождает родственные явления: ниже `Dcrit` запоминание снова выгоднее, и
грокнутая сеть скатывается назад — [унгрокинг](ungrokking.md), частный случай
[катастрофического забывания](catastrophic-forgetting.md); ровно у `Dcrit`
переход выводит лишь на частичную точность — [semi-grokking](semi-grokking.md).

Присоединившиеся работы развивают и оспаривают этот механизм. Merrill et al.
дают родственную рамку **конкуренции подсетей**: до перехода доминирует плотная
подсеть, после — разрежённая \[[3.1](#ref-3-1)\]; Huang et al. сводят гроккинг,
[двойной спуск](double-descent.md) и родственные явления к «конкуренции контуров»
\[[3.2](#ref-3-2)\], а Wang et al. переносят относительную эффективность контуров
на задачи неявного рассуждения \[[3.3](#ref-3-3)\]. Tian **выводит** эмпирическую
гипотезу Varma из первопринципной градиентной динамики \[[3.4](#ref-3-4)\], Singh
et al. связывают эффективность контуров с критическим порогом данных
\[[3.5](#ref-3-5)\], а He et al. прямо используют «гипотезу эффективности
контуров» для настройки экспериментов \[[3.6](#ref-3-6)\]. С другой стороны,
механизм критикуют за опору на weight decay: Prakash et al. наблюдают поздний
коллапс генерализации ([анти-грокинг](anti-grokking.md)) **без** weight decay, вне
предположений теории \[[2.1](#ref-2-1)\], а Prieto et al. показывают гроккинг без
регуляризации вовсе — за счёт устранения [коллапса softmax](softmax-collapse.md),
связанного с [наивной минимизацией потерь](nlm.md) \[[2.2](#ref-2-2)\].

## Альтернативные определения и нюансы

### A. Норм-эффективность под weight decay (Varma)

Каноническая трактовка: эффективность = логиты на единицу нормы параметров, а
отбор эффективного контура выполняет именно weight decay \[[1.1](#ref-1-1)\].
Управляющий параметр — **размер обучающего набора** (он двигает `Dcrit`),
обязательная предпосылка — режим ненулевого weight decay. Именно эта предпосылка и
делает механизм уязвимым для критики (см. «Оспаривают»).

### B. Конкуренция плотной и разрежённой подсетей (Merrill; Huang)

Здесь эффективность операционализирована не через норму на логит, а через то,
**какая подсеть доминирует**: плотная запоминающая — до перехода, разрежённая
обобщающая — после \[[3.1](#ref-3-1)\]; Huang et al. обобщают это до «конкуренции
контуров», единой рамки для нескольких явлений \[[3.2](#ref-3-2)\]. Источник
различия: наблюдаемая величина — доля/плотность активной подсети, а не
арифметика «норма/логит».

### C. Эффективность из градиентной динамики (Tian)

У Varma «`Cgen` эффективнее, но медленнее» — **эмпирическая гипотеза**; Tian
получает то же утверждение как **следствие** первопринципной динамики: распределение
данных задаёт ландшафт оптимизации, а он — в какой локальный оптимум сойдутся веса
\[[3.4](#ref-3-4)\]. Источник различия: эффективность здесь не постулируется, а
**выводится**, и порядок «сначала запоминание, потом признаки» становится
необходимым, а не наблюдаемым.

### Оспаривают

- **Гроккинг вне weight decay** \[[2.1](#ref-2-1)\]: поздний коллапс генерализации
  («анти-грокинг») возникает на исходном датасете **без** weight decay после
  очень долгого обучения — то есть за пределами ключевого предположения теории
  эффективности контуров, и ею не предсказывается. Источник различия: сдвиг
  эффективностей под weight decay не описывает поведение, где weight decay
  отсутствует.
- **Гроккинг без регуляризации через устранение softmax-коллапса**
  \[[2.2](#ref-2-2)\]: если снять численную нестабильность softmax, гроккинг
  наступает и без регуляризации — значит, весовая эффективность обобщающих решений
  не является необходимым условием. Источник различия: движущий фактор задержки —
  масштабирование логитов (softmax-коллапс), а не отбор эффективного контура
  под weight decay.

- **Сосуществование вместо вытеснения** \[[2.3](#ref-2-3)\]: приложение A.4
  основополагающей работы вводит $k$ уравнений с перепутанными ответами и
  сообщает, что сеть всегда интерполирует все метки, а при малых $k$ (до 1000)
  генерализация почти не страдает — запомненные исключения и обобщающее решение
  живут в одних весах в том самом режиме (AdamW, weight decay 1), где вытеснение
  должно работать. Источник различия: картина «эффективный контур вытесняет
  запоминающий» не учитывает измеренное самой основополагающей статьёй мирное
  сосуществование.

### Поддерживают

- **Конкуренция плотной/разрежённой подсети** \[[3.1](#ref-3-1)\]: независимая
  рамка, где эффективность видна как смена доминирующей подсети (плотная до
  перехода, разрежённая после). Источник различия: то же противоборство контуров
  измеряется через плотность подсети.
- **Единый взгляд «конкуренции контуров»** \[[3.2](#ref-3-2)\]: гроккинг, двойной
  спуск и родственные явления объясняются конкуренцией запоминающих и обобщающих
  контуров. Источник различия: расширение области применения механизма на семейство
  явлений.
- **Относительная эффективность контуров в рассуждении** \[[3.3](#ref-3-3)\]:
  формирование обобщающего контура и его относительная эффективность объясняют
  неявное рассуждение у трансформеров. Источник различия: перенос механизма с
  [модульной арифметики](modular-arithmetic.md) на задачи рассуждения.
- **Первопринципный вывод** \[[3.4](#ref-3-4)\]: «эффективнее, но медленнее»
  выводится из градиентной динамики, а не постулируется. Источник различия:
  теоретическое обоснование эмпирической гипотезы Varma.
- **Критический порог данных** \[[3.5](#ref-3-5)\]: анализ эффективности контуров
  задаёт критический размер данных, ниже которого сеть запоминает, а не обобщает.
  Источник различия: акцент на пороговом поведении вокруг `Dcrit`.
- **Прямое применение гипотезы** \[[3.6](#ref-3-6)\]: отношение
  выведенных/атомарных фактов настраивается по гипотезе эффективности контуров,
  чтобы ускорить появление обобщающего контура. Источник различия: механизм
  используется как инженерный рычаг постановки эксперимента.

###### ref-3-10
**\[3.10\]** 2602.02859 — Prakash et al., «Late-Stage Generalization Collapse in Grokking: Detecting anti-grokking with WeightWatcher». Нюанс: приближённая местная сложность контуров взята как мера продвижения — попытка измерить действенность схемы, не разбирая её. [`"Approximate Local Circuit Complexity, which capture broader"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p2-2). *«[приближённую местную сложность контуров, схватывающие более широкие](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.card.md#p2-2)»*

## Ссылки

###### ref-1-1
**\[1.1\]** 2309.02390 — Varma et al., «Explaining grokking through circuit efficiency». [`"weight decay prefers circuits with high “efficiency”, that is, circuits that require less parameter norm to produce a given logit value."`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-6). *«[weight decay предпочитает контуры с высокой «эффективностью» — то есть такие, которым нужно меньше нормы параметров, чтобы выдать заданное значение логита](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-6)»*\
Доп. (суть): [`"the generalising solution is slower to learn but more efficient, producing larger logits with the same parameter norm"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3) — *«[обобщающее выучивается медленнее, но оказывается более эффективным — производит бо́льшие логиты при той же норме параметров](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3)»*.\
Доп. (операционализация): [`"the extent to which the circuit can convert relatively small parameters into relatively large logits"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p3-6) — *«[насколько контур способен превращать относительно малые параметры в относительно большие логиты](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p3-6)»*.\
Доп. (ингредиент): [`"it can produce equivalent cross-entropy loss on the training set with a lower parameter norm"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p3-9) — *«[способен дать ту же кросс-энтропийную потерю на обучающем наборе при меньшей норме параметров](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p3-9)»*.\
Доп. (Dcrit): [`"memorising circuits become more inefficient with larger training datasets while generalising circuits do not, suggesting there is a critical dataset size"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3) — *«[запоминающие контуры становятся менее эффективными с ростом обучающего набора, тогда как обобщающие — нет, а значит, существует критический размер набора данных](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3)»*.

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2506.04434 — Prakash & Martin 2025, «Grokking and Generalization Collapse: Insights from HTSR theory». Оспаривает опору устройства на ослабление весов: обвал генерализации возникает и без него. [`"grokking occurs even without weight decay (leading to an increasing norm), suggesting weight norm alone is not a complete explanation"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p2-1). *«[гроккинг случается и без ослабления весов (отчего норма растёт), а значит, одна норма весов не есть полное объяснение](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p2-1)»*\
Доп.: [`"attributing it to shifting circuit efficiencies under WD"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p3-1) — *«[приписав его смене действенности контуров при WD](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p3-1)»*; [`"This distinct phenomenon is not predicted by varma2023explaining as it falls outside of the crucial weight decay assumption on which it relies"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p3-1) — *«[Это отдельное явление не предсказывается varma2023explaining, ибо выпадает из решающего допущения об ослаблении весов, на котором та работа держится](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p3-1)»*.

###### ref-2-2
**\[2.2\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». Оспаривает необходимость весовой эффективности: гроккинг достижим без регуляризации. [`"mitigating SC leads to grokking without regularization"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2). *«[устранение SC ведёт к гроккингу без регуляризации](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2)»*\
Доп. (атрибуция): [`"highlighted weight efficiency of generalizing solutions"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-5) — *«[подчеркнули эффективность обобщающих решений по весам](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-5)»*.

###### ref-2-3
**\[2.3\]** 2201.02177 — Power et al., «Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets». Оспаривает вытеснение меморизации генерализацией: в приложении A.4 сеть при $k$ испорченных уравнениях всегда интерполирует все метки, и при малых $k$ запомненные исключения сосуществуют с обобщающим решением в одних весах. [`"all experiments reach 100% training accuracy at some point, and this point is not considerably affected by changing the number of outliers $k$"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p9-5). *«[все эксперименты в какой-то момент достигают 100 % точности на обучении, и этот момент существенно не зависит от изменения числа выбросов $k$](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p9-5)»*\
Доп.: [`"However the effect of introducing a small number of outliers (up to 1000) is not very pronounced"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p9-5) — *«[Однако эффект от введения небольшого числа выбросов (до 1000) выражен не очень сильно](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p9-5)»*.


###### ref-2-4
**\[2.4\]** 2407.20199 — Mallinar et al., «Emergence in non-neural models: grokking modular arithmetic via average gradient outer product». Оспаривает всеобщность: на носителе, где контуров нет вовсе, гроккинг есть; собственного разбора контуров, их вытеснения или эффективности работа не ведёт. [`"explanations for grokking that depend on the magnitude of the weights or neural circuit efficiency (e.g., [31, 42]) or other attributes of neural networks, such as specific optimization methods, cannot account for the phenomena described in our work"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p16-2). *«[объяснения гроккинга, зависящие от величины весов или от эффективности нейронных контуров (напр., [31, 42]) либо от иных свойств нейронных сетей, таких как определённые методы оптимизации, не в состоянии объяснить описанные в нашей работе явления](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p16-2)»*

###### ref-2-5
**\[2.5\]** 2505.11411 — Zhang, Shang, Yang, Zhang, «Is Grokking a Computational Glass Relaxation?». Предлагает разрешение спора об «эффективности»: расхождение Varma et al. с Kumar et al. отнесено на счёт определения эффективности через одну лишь норму параметров, а мерой предложена энтропия — объём решений с той же способностью к генерализации. Нюанс: ни постановка Varma et al., ни постановка Kumar et al. не воспроизведены, «эффективность» через энтропию не измерена ни на одном контуре, и дальше пересказа чужого утверждения слово «контур» в работе не идёт. [`"While Varma et al.varma2023explaining argue that grokking requires a transition to an “efficient” generalizing circuit characterized by a lower parameter norm, Kumar et al.kumar2023grokking challenges it as they identified generalizing solutions with higher norm than memorizing solutions."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p10-1). *«[Тогда как Varma et al.varma2023explaining утверждают, что гроккинг требует перехода к «эффективному» обобщающему контуру, отличающемуся меньшей нормой параметров, Kumar et al.kumar2023grokking оспаривают это, поскольку выявили обобщающие решения с более высокой нормой, чем у запоминающих.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p10-1)»*\
Доп. (предлагаемое разрешение): [`"Our framework suggests this conflict stems from using parameter norm alone to define efficiency, while entropy is actually a better indicator since the efficient solution will have more “free” parameters and therefore a larger entropy."`](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/original/2505.11411.is-grokking-a-computational-glass-relaxation.md#p10-1) — *«[Наша рамка указывает, что этот спор происходит из определения эффективности через одну лишь норму параметров, тогда как энтропия на деле лучший показатель, поскольку у эффективного решения будет больше «свободных» параметров и потому большая энтропия.](../papers/2505.11411.is-grokking-a-computational-glass-relaxation/2505.11411.is-grokking-a-computational-glass-relaxation.card.md#p10-1)»*.
###### ref-2-6
**\[2.6\]** 2502.01739 — Manning-Coe, Gliozzi, Stapleton, Hirst, De Tomasi, Bradlyn & Berman 2025, «Grokking vs. Learning: Same features, different encodings». Прямое возражение линии «обобщение как переход к более экономной кодировке»: если бы гроккинг был сжатием к эффективной схеме, грокнутые модели сжимались бы лучше — показано обратное, ровное обучение сжимает те же признаки до пяти раз эффективнее. [`"In our case, the features learned were the same, and the steady learning regime had higher compression"`](../papers/2502.01739.grokking-vs-learning-same-features-different-encodings/original/2502.01739.grokking-vs-learning-same-features-different-encodings.md#p8-5). *«[В нашем случае выученные признаки оказались теми же, а режим ровного обучения имел более высокое сжатие](../papers/2502.01739.grokking-vs-learning-same-features-different-encodings/2502.01739.grokking-vs-learning-same-features-different-encodings.card.md#p8-5)»*\
Доп. (масштаб разрыва): [`"we are able to *cut 95% of the weights* in the model for a 5% drop in accuracy, a compressibility 25x that of the original model"`](../papers/2502.01739.grokking-vs-learning-same-features-different-encodings/original/2502.01739.grokking-vs-learning-same-features-different-encodings.md#p5-6) — *«[мы способны *вырезать 95% весов* модели ценой 5% падения точности — сжимаемость в 25 раз к исходной модели](../papers/2502.01739.grokking-vs-learning-same-features-different-encodings/2502.01739.grokking-vs-learning-same-features-different-encodings.card.md#p5-6)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2303.11873 — Merrill et al., «A Tale of Two Circuits: Grokking as Competition of Sparse and Dense Subnetworks». Нюанс: эффективность как смена доминирующей подсети. [`"two largely distinct subnetworks: a dense one that dominates before the transition and generalizes poorly, and a sparse one that dominates afterwards"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2). *«[двух в значительной мере различных подсетей: плотной, доминирующей до перехода и плохо обобщающей, и разрежённой, доминирующей после него](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p1-2)»*

###### ref-3-2
**\[3.2\]** 2402.15175 — Huang et al., «Unified View of Grokking, Double Descent and Emergent Abilities: A Perspective from Circuits Competition». Нюанс: единая рамка конкуренции контуров. [`"from the perspective of competition between memorization circuits and generalization circuits in neural models"`](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/original/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.md#p1-3). *«[со стороны состязания между запоминающими и обобщающими контурами в нейросетевых моделях](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.card.md#p1-3)»*

###### ref-3-3
**\[3.3\]** 2405.15071 — Wang et al., «Grokked Transformers are Implicit Reasoners: A Mechanistic Journey to the Edge of Generalization». Нюанс: относительная эффективность контуров в задачах рассуждения. [`"its relation to the relative efficiency of generalizing and memorizing circuits"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2). *«[его связь с относительной эффективностью обобщающего и запоминающего контуров](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)»*

###### ref-3-4
**\[3.4\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». Нюанс: вывод гипотезы Varma из градиентной динамики. [`"generalization circuits $\mathcal{C}_{gen}$ is more efficient but learn slower than memorization circuits $\mathcal{C}_{mem}$"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p2-2). *«[обобщающие контуры $\mathcal{C}_{gen}$ эффективнее, но выучиваются медленнее запоминающих контуров $\mathcal{C}_{mem}$](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p2-2)»*

###### ref-3-5
**\[3.5\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». Нюанс: эффективность контуров задаёт критический размер данных. [`"Circuit-efficiency analysis shows that generalization is slower but more efficient"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p2-4). *«[Разбор действенности контуров показывает, что генерализация медленнее, но действеннее](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p2-4)»*

###### ref-3-6
**\[3.6\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits». Нюанс: гипотеза эффективности контуров как рычаг постановки эксперимента. [`"According to the circuit efficiency hypothesis, a high ratio like 18.0 increases the relative complexity of a memorizing circuit compared to a generalizing one"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p9-7). *«[Согласно догадке о действенности контуров, высокое отношение вроде $18.0$ увеличивает относительную сложность запоминающего контура по сравнению с обобщающим](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p9-7)»*


###### ref-3-7
**\[3.7\]** 2606.12966 — Sivasankar, «Circuit Synchronization Precedes Generalization: A Causal Precursor to Grokking». [`"We view our results as direct, controlled evidence for the mechanism that Varma et al. 2023 argue for on efficiency grounds."`](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p16-4). *«[Мы рассматриваем наши итоги как прямое управляемое свидетельство в пользу того механизма, за который Varma et al. 2023 доводят на основаниях эффективности.](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p16-4)»*
###### ref-3-8
**\[3.8\]** 2505.18266 — McCracken et al., «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». Нюанс: числовая мера экономности всеобщего решения — довод, что выигрывающий контур мал; с норм-эффективностью Varma работа напрямую не сверяется. [`"It predicts that universally learned solutions in DNNs with trainable embeddings or more than one hidden layer require only $\mathcal{O}(\log n)$ features, a result we empirically confirm"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2). *«[Она предсказывает, что всеобще выучиваемые решения в DNN с обучаемыми вложениями или более чем одним скрытым слоем требуют лишь $\mathcal{O}(\log n)$ признаков, и этот итог мы подтверждаем опытом](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p1-2)»*

###### ref-3-9
**\[3.9\]** 2606.26050 — Li, Sreedhar 2026, «Natural Ungrokking: Asymmetric Control of Which Rules Survive Pretraining». Соревнование цепей, переведённое с эффективности на измеримые статистики данных и проверенное причинно в обе стороны: переворот поддержки в противосвидетельство на месте (числа токенов сохранены) убивает правило с точной дозовой монотонностью ($\rho_{\mathrm{kill}}=-1.00$, $\mathrm{CM}$ от $+3.68$ до $-2.99$) и воспроизводится на втором правиле пятью дозами внутри одного корпуса ($0.96\to0.00$ строго монотонно); обратная инъекция при отношении свидетельств до $3\,565$ против $7.9$ выживающего уровня не возвращает поведение ни при равномерном, ни при раннем, ни при позднем расписаниях — при частичной пересборке несущей головы (в выживающих прогонах маржу несёт одна голова последнего слоя, $0.75$–$0.90$ атрибуции; в обвалившихся носителя нет). Нюанс: асимметрия сравнивает неравные правки — убийство меняет ровно те токены, что тестирует батарея, а восстановление нарочно вливает непересекающиеся имена, так что провал возврата может быть свойством конструкции опыта. [`"No dose produces a control-valid recovery in any seed"`](../papers/2606.26050.natural-ungrokking-asymmetric-control-of-which-rules-survive-pretraining/original/2606.26050.natural-ungrokking-asymmetric-control-of-which-rules-survive-pretraining.md#p5-6). *«[Никакая доза не производит control-valid восстановления ни в одном семени](../papers/2606.26050.natural-ungrokking-asymmetric-control-of-which-rules-survive-pretraining/2606.26050.natural-ungrokking-asymmetric-control-of-which-rules-survive-pretraining.card.md#p5-6)»*

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2512.03437 — Liang & Li, «Grokked Models are Better Unlearners». [`"This distributed-to-modular shift involves competition between dense memorizing and sparse generalizing circuits (Merrill et al., 2023; Varma et al., 2023)"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p4-1). *«[Этот сдвиг от распределённого к составному включает состязание плотных запоминающих и разрежённых обобщающих контуров (Merrill et al., 2023; Varma et al., 2023)](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p4-1)»*

**\[4.2\]** 2601.03162 — Jiang et al., «On the Convergence Behavior of Preconditioned Gradient Descent Toward the Rich Learning Regime». [`"explain the delay including regularization with weights, dynamics of adaptive optimizers, and circuit efficiencies"`](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p1-5). *«[объяснить задержку: упорядочение весами, динамика приспосабливающихся оптимизаторов, действенность контуров](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p1-5)»*

**\[4.3\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». [`"Varma et al. (2023) explained it through circuit efficiency"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p20-4). *«[Varma et al. (2023) объяснили его через эффективность контуров](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p20-4)»*

**\[4.4\]** 2603.01968 — Hwang et al., «Intrinsic Task Symmetry Drives Generalization in Algorithmic Tasks». [`"Prevailing explanations for grokking such as implicit biases (Lyu et al., 2024), regularization norms (Liu et al., 2023), and circuit efficiency (Varma et al., 2023) converge"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-4). *«[Ходовые объяснения гроккинга — неявные склонности (Lyu et al., 2024), нормы сглаживания (Liu et al., 2023) и действенность контуров (Varma et al., 2023) — сходятся](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.card.md#p1-4)»*

**\[4.5\]** 2311.18817 — Lyu et al., «Dichotomy of Early and Late Phase Implicit Biases Can Provably Induce Grokking». Нюанс: пересказывает действенность контуров через норму параметров и относит её к объяснениям без строгого разбора динамики обучения; ungrokking и semi-grokking только названы, опытом не проверяются. [`"Varma et al. 2023 hypothesized that grokking happens when the generalizable solutions have a smaller parameter norm than the overfitting solutions, but the former takes a longer time to learn"`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p16-1). *«[Varma et al. 2023 выдвинули гипотезу, что гроккинг случается, когда обобщающие решения обладают меньшей нормой параметров, чем переобучающие, но первые дольше выучиваются](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p16-1)»*

**\[4.6\]** 2310.02541 — Xu et al. 2023. Нюанс: понятие только помянуто; никаких контуров в собственном разборе нет — механизм здесь геометрический (выстроенность нейронов вдоль средних кластеров). [`"utilized circuit efficiency to interpret grokking and discovered two novel phenomena called ungrokking and semi-grokking"`](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/original/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.md#p3-2). *«[использовали эффективность контуров для толкования гроккинга и обнаружили два новых явления, названные ungrokking и semi-grokking](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.card.md#p3-2)»*

**\[4.7\]** 2401.10463 — Zhu et al. 2024. Нюанс: эффективность контуров изложена точно, но исключительно как содержание смежной работы, от которой авторы себя отделяют; механистической части в статье нет вовсе — единственные внутренние величины суть норма L2 и гистограммы весов последнего слоя. [`"They define ‘critical data size’ as the number of data points at which memorizing and generalizing circuits produce identical logits."`](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p12-2). *«[Они определяют «критический размер данных» как число точек данных, при котором запоминающий и обобщающий контуры дают одинаковые логиты.](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p12-2)»*

**\[4.8\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Лишь упоминает: эффективность контуров входит как одна из двух спорящих сторон (против «зоны Златовласки» Liu et al. 2022b), и работа объявляет, что «находит долю истины в обеих точках зрения», сдвигая объяснение на отношение норм шага и параметров. Ни контуров, ни их стоимости не измеряется; архитектура опыта при этом заимствована именно у Varma et al. 2023. [`"Varma et al. 2023 argue the converse: grokking occurs when the optimizer converges to an “efficient” circuit which generalizes well, from which point weight decay can reduce the parameter norm without harming accuracy"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#p3-2). *«[Varma et al. 2023 доказывают обратное: гроккинг происходит, когда оптимизатор сходится к «эффективному» контуру, который хорошо обобщает, и с этого мига weight decay может снижать норму параметров, не вредя точности](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#p3-2)»*

**\[4.9\]** 2605.09724 — Song & Ye, «Model Capacity Determines Grokking through Competing Memorisation and Generalisation Speeds». Нюанс: заявлено как уточнение, а не спор; ни норм контуров, ни их выделения работа не измеряет. [`"previous works identify which solution is preferred at convergence, while we give a more formal treatment on which solution gradient descent encounters first as a function of model capacity"`](../papers/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds/original/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds.md#p9-4). *«[прежние работы выделяют, какое решение предпочитается на сходимости, тогда как мы даём более формальное рассмотрение того, какое решение градиентный спуск встречает первым как функцию ёмкости модели](../papers/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds.card.md#p9-4)»*

**\[4.10\]** 2604.13082 — Gomezjurado Gonzalez, «The Long Delay to Arithmetic Generalization…». [`"Its step-for-step coincidence with the loss elbow is consistent with circuit-competition accounts of grokking (Varma et al., 2023; Merrill et al., 2023)"`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p20-2). *«[Её пошаговое совпадение с изломом потерь согласуется с объяснениями гроккинга через соревнование контуров (Varma et al., 2023; Merrill et al., 2023)](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p20-2)»*

**\[4.11\]** 2605.08237 — Wang, Ying, Kanamori 2026, «Distributional Spectral Diagnostics for Localizing Grokking Transitions». Нюанс: одно упоминание в обзорном приложении, никакого сопоставления с собственной величиной. [`"Fourier features for modular addition [28] and circuit-efficiency tradeoffs [43]"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p30-7). *«[фурье-признаков для модульного сложения [28] и компромиссов эффективности контуров [43]](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p30-7)»*

**\[4.12\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Упоминает действенность контуров в обзоре как одно из объяснений того, *что* сеть вычисляет, и прямо противопоставляет ему свой предмет — скалярную управляющую переменную и её причинную роль в запуске перехода. Нюанс: соотношения двух картин работа не строит — ни того, как удержание полной нормы сказывается на соотношении действенностей двух контуров, ни того, объясняет ли действенность величину показателя $\alpha$. [`"Varma et al. 2023 framed grokking as competition between a memorizing and a generalizing circuit of differing parameter efficiency, with weight decay tipping the balance."`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p2-2). *«[Varma et al. 2023 подали гроккинг как состязание между запоминающим и обобщающим контурами разной действенности по параметрам, где weight decay склоняет чашу весов.](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p2-2)»*

### Внешние работы

###### ref-5-1
**\[5.1\]** 2508.17689 — **Внешняя работа (выдержка): Buchanan et al., «On the Edge of Memorization in Diffusion Models».** Нюанс: та же форма объяснения — соперничество двух решений по цене, где роль цены играет взвешенная обучающая потеря, а порог назван точкой пересечения. [`"the weighted training loss of a fully generalizing model becomes greater than that of an underparameterized memorizing model at a critical value of model (under)parameterization"`](../externals/2508.17689.on-the-edge-of-memorization-in-diffusion-models/original/2508.17689.on-the-edge-of-memorization-in-diffusion-models.md#p1-2). *«[взвешенная обучающая потеря полностью обобщающей модели становится больше, чем у недопараметризованной запоминающей, при критическом значении (недо)параметризации модели](../externals/2508.17689.on-the-edge-of-memorization-in-diffusion-models/2508.17689.on-the-edge-of-memorization-in-diffusion-models.card.md#p1-2)»*
