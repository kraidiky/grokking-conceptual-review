# Критичность / критическая точка (criticality / critical point)

[Законы масштабирования](scaling-laws.md) ← предыдущая карточка, следующая → [information-bottleneck](information-bottleneck.md)

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Критическая точка** в этой линии — значение управляющего параметра, вблизи которого динамика обучения замедляется настолько, что обобщение приходит с задержкой: *«гроккинг случается вблизи критической точки — подобно "критическому замедлению" в физической литературе»* \[[1.1](#ref-1-1)\]. В решаемой постановке бинарной логистической классификации это доказывается: задержка возникает и усиливается при приближении доли одного класса к $1/2$, и именно $\lambda=1/2$ оказывается критической точкой \[[1.2](#ref-1-2)\].

## Детализация

Понятие приходит из статистической физики, где вблизи критической точки время релаксации расходится, а система становится чувствительной к малым возмущениям. Перенос в обучение делает [гроккинг](grokking.md) не аномалией, а ожидаемым поведением: если траектория проходит вблизи критической точки, долгое плато — норма, а не загадка.

**Чем это отличается от [фазового перехода](phase-transition.md).** Переход — событие (пересечение границы), критичность — свойство окрестности: замедление, расходимость масштабов, повышенная чувствительность. Работа может наблюдать переход и не утверждать критичности; здесь же утверждается именно второе, и потому проверяется формой зависимости времени от расстояния до точки, а не самим фактом скачка.

**Насколько это общий закон.** Заявка сформулирована осторожно: авторы прямо пишут, что строго показать этого не могут, и предполагают, что связь с критическими точками есть и в других постановках, ссылаясь на решаемые модели, где это показано напрямую \[[3.1](#ref-3-1)\]. То есть критичность в корпусе — рабочая гипотеза с одним доказанным случаем, а не установленный механизм гроккинга.

**Где критичность вычисляется.** В решаемых моделях показатели не постулируются, а выводятся: переход оказывается фазовым переходом второго рода с критическим показателем тестовой ошибки, равным единице, тогда как параметры регуляризации меняют лишь префактор \[[3.3](#ref-3-3)\]. Родственная картина без слова «критичность» — метастабильный режим, из которого траектория выходит, и именно этот выход виден как обобщение \[[3.4](#ref-3-4)\].

**Смежные употребления.** Слово «критический» в корпусе носят и другие величины — прежде всего критический размер выборки, порог по данным \[[3.2](#ref-3-2)\]. Это не критическая точка в физическом смысле: там нет утверждения о замедлении и расходимости, есть граница между режимами. Различать их стоит, потому что предсказания у них разные: критичность предсказывает **как** растёт задержка при подходе к точке, порог — **где** проходит граница.

## Альтернативные определения и нюансы

### A. Критическая точка управляющего параметра

Строгая форма: параметр, при котором динамика замедляется, а задержка растёт \[[1.2](#ref-1-2)\]. Отличительная черта — доказуемость в решаемой модели и проверяемая форма зависимости; ограничение — постановка минимальна (бинарная логистическая классификация с гауссовыми классами), и перенос на трансформер остаётся догадкой \[[3.1](#ref-3-1)\].

### B. Критическое замедление как объяснение задержки

Прочтение, в котором долгое плато — не «сеть ничего не делает», а «сеть движется в области, где всё медленно». Различающая черта против рамки [скрытого прогресса](sparse-solutions-hidden-progress.md): там под плато идёт монотонное продвижение к решению, здесь замедление — свойство ландшафта вблизи точки, и предсказывается не рост меры, а масштаб времени.

### C. «Критический» как порог, а не точка

Употребление, которое стоит держать отдельно: критический размер выборки \[[3.2](#ref-3-2)\] и подобные пороги отмечают границу режимов, но не влекут ни замедления, ни расходимости. Смешение двух смыслов даёт ложное впечатление, будто всякий порог в корпусе физически критичен.

## Ссылки

###### ref-1-1
**\[1.1\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». [`"*grokking happens near a critical point*, similar to “critical slowing down” in the physics literature"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-1). *«[*гроккинг случается вблизи критической точки* — подобно «критическому замедлению» в физической литературе](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p2-1)»*

###### ref-1-2
**\[1.2\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». [`"We show that this happens because $\lambda=1/2$ is a *critical point*."`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-9). *«[Мы показываем, что так происходит потому, что $\lambda=1/2$ есть *критическая точка*.](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-9)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2410.04489 — Beck et al., «Grokking at the Edge of Linear Separability». Нюанс: перенос на другие постановки заявлен как догадка, а не как результат. [`"While we cannot show it rigorously, we conjecture that grokking is intimately related to such critical points also in different settings."`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p9-3). *«[Хотя строго показать этого мы не можем, мы предполагаем, что гроккинг тесно связан с такими критическими точками и в других постановках.](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p9-3)»*

###### ref-3-2
**\[3.2\]** 2401.10463 — Zhu et al., «Critical Data Size of Language Models from a Grokking Perspective». Нюанс: «критический размер данных» — порог между режимами, а не критическая точка в физическом смысле. [`"We explore the critical data size in language models, a threshold that marks a fundamental shift from quick memorization to slow generalization."`](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p1-2). *«[Мы исследуем критический размер данных в языковых моделях — порог, отмечающий основополагающий сдвиг от быстрого запоминания к медленной генерализации.](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p1-2)»*

###### ref-3-3
**\[3.3\]** 2210.15435 — Žunkovič et al., «Grokking phase transitions in learning local rules with gradient descent». Нюанс: в решаемой модели критичность не постулируется, а вычисляется — переход второго рода с показателем тестовой ошибки, равным единице. [`"Grokking in the considered 1D exponential model is a second-order phase transition with the test-error critical exponent equal to one."`](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p7-10). *«[Гроккинг в рассматриваемой одномерной показательной модели есть фазовый переход второго рода с критическим показателем тестовой ошибки, равным единице.](../papers/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent/2210.15435.grokking-phase-transitions-in-learning-local-rules-with-gradient-descent.card.md#p7-10)»*

###### ref-3-4
**\[3.4\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». Нюанс: родственная картина без слова «критичность» — задержка как удержание в метастабильном режиме, из которого траектория выходит. [`"generalization emerges as the trajectory exits this metastable regime"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p3-2). *«[генерализация возникает, когда траектория выходит из этого метастабильного режима](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p3-2)»*
