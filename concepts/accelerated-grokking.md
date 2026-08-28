# Ускорение гроккинга (accelerated grokking)

[Сферическое ограничение нормы](spherical-weight-norm-constraint.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [5. Интервенции и методы](index.md#cat-5)\
→ Следующая категория: [6. Аналитические инструменты и метрики](progress-measures.md)\
← Предыдущая категория: [4. Факторы обучения и оптимизации](weight-decay.md)

## Определение

**Ускорение гроккинга** — семейство вмешательств, цель которых сократить задержку между подгонкой обучающей выборки и обобщением, не меняя самой задачи. Мотив назван прямо: задержка *«вредит предсказуемости и эффективности»* \[[1.2](#ref-1-2)\], и потому *«мы ставим целью ускорить генерализацию модели, находящейся в явлении гроккинга»* \[[1.1](#ref-1-1)\]. Мерой успеха служит [время гроккинга](grokking-time.md), а спор идёт о том, за счёт чего именно сокращение достигается.

## Детализация

Приёмы корпуса распадаются на четыре семейства по тому, куда они вмешиваются.

**В градиент.** Если задержка приписывается медленно меняющейся составляющей обновлений, то её можно усилить фильтром: гроккинг связывается с низкочастотной частью двойственного представления обновлений \[[3.1](#ref-3-1)\], и отсюда [фильтрация градиента](gradient-low-pass-filtering.md). Родственные приёмы преобразуют градиент иначе — ортогонализацией или обучаемым преобразованием.

**В представление на входе.** Другая линия объясняет задержку неинформативным вложением и предлагает перенести готовое: содержательное вложение обеспечивает непрерывное продвижение вместо плато \[[3.2](#ref-3-2)\]. Здесь ускорение достигается не изменением динамики, а изменением начальной точки.

**В параметры по ходу.** Третье семейство толкает сеть из застойного состояния: обмен значений внутри слоя сокращает срок примерно впятеро \[[3.3](#ref-3-3)\]; сюда же относятся крупный гауссов шум и перезапуски.

**В регуляризацию и норму.** Четвёртое — управление нормой весов и [weight decay](weight-decay.md): удержание нормы в «зоне Златовласки» ускоряет обобщение, но ценой введения дополнительного ограничения \[[3.4](#ref-3-4)\].

**Общая оговорка.** Почти все заявки об ускорении меряются в шагах оптимизатора, а не в затраченном времени, и сравниваются с опорной кривой, подобранной под кратчайший гроккинг; при пересчёте в секунды часть выигрыша исчезает — это разобрано в карточке [времени гроккинга](grokking-time.md). Вторая общая слабость — [разброс по семенам](seed-variance-reproducibility.md): ускорение, показанное на одном семени, неотличимо от удачи.

## Альтернативные определения и нюансы

### A. Ускорение как изменение динамики

Вмешательство работает с обновлениями: усиливает медленную составляющую, ортогонализует, преобразует \[[3.1](#ref-3-1)\]. Различающая черта — задача решается той же сетью из той же начальной точки, меняется только путь; проверяется сравнением траекторий при прочих равных.

### B. Ускорение как изменение начальной точки

Вмешательство работает до обучения: переносится вложение, полученное дешёвой моделью \[[3.2](#ref-3-2)\]. Источник различия — здесь нельзя говорить «та же задача решается быстрее», потому что часть работы вынесена за пределы измеряемого прогона; честное сравнение требует учитывать стоимость получения вложения.

### C. Ускорение как выталкивание из застоя

Вмешательство работает по ходу и разрушает текущее состояние: обмен параметров, крупный шум \[[3.3](#ref-3-3)\]. Различающая черта — противоречащее чутью условие: разрушение части выученного ускоряет обобщение, что и делает эту ветвь свидетельством в пользу картины кинетического застоя, а не только приёмом.

### D. Ускорение как ограничение

Вмешательство сужает область поиска: норма закрепляется на сфере подходящего радиуса \[[3.4](#ref-3-4)\]. Оговорка, звучащая в самом корпусе: приём вводит дополнительный гиперпараметр, и выигрыш надо считать с учётом его подбора.

## Ссылки

###### ref-1-1
**\[1.1\]** 2405.20233 — Lee et al., «Grokfast: Accelerated Grokking by Amplifying Slow Gradients». [`"our goal is to accelerate generalization of a model under grokking phen"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p1-2). *«[мы ставим целью ускорить генерализацию модели, находящейся в явлении гроккинга](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p1-2)»*

###### ref-1-2
**\[1.2\]** 2504.13292 — Xu et al., «Let Me Grok for You: Accelerating Grokking via Embedding Transfer from a Weaker Model». [`"this delayed generalization phenomenon compromises predictability and efficiency"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-2). *«[это явление отложенной генерализации вредит предсказуемости и эффективности](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p1-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2405.20233 — Lee et al., «Grokfast: Accelerated Grokking by Amplifying Slow Gradients». Нюанс: ускорение выводится из гипотезы о том, что задержку несёт медленно меняющаяся часть обновлений. [`"the grokking phenomenon is directly related to the low-frequency part of the dual representatio"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p2-5). *«[явление гроккинга напрямую связано с низкочастотной частью двойственного представления](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p2-5)»*

###### ref-3-2
**\[3.2\]** 2504.13292 — Xu et al., «Let Me Grok for You: Accelerating Grokking via Embedding Transfer from a Weaker Model». Нюанс: ускорение достигается не изменением динамики, а заменой начальной точки — переносом содержательного вложения. [`"an informative embedding enables continuous progress during training"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-7). *«[содержательное вложение обеспечивает непрерывное продвижение в ходе обучения](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p1-7)»*

###### ref-3-3
**\[3.3\]** 2608.01833 — Chan et al., «Tunneling the Loss Landscape: Bypassing Memorization with Monte Carlo Parameter Swapping». Нюанс: разрушение части выученного ускоряет обобщение — довод в пользу картины кинетического застоя. [`"the swap intervention substantially reduces the grokking time"`](../papers/2608.01833.tunneling-the-loss-landscape-bypassing-memorization-with-monte-carlo-parameter-swapping/original/2608.01833.tunneling-the-loss-landscape-bypassing-memorization-with-monte-carlo-parameter-swapping.md#p4-9). *«[обменное вмешательство существенно сокращает время гроккинга](../papers/2608.01833.tunneling-the-loss-landscape-bypassing-memorization-with-monte-carlo-parameter-swapping/2608.01833.tunneling-the-loss-landscape-bypassing-memorization-with-monte-carlo-parameter-swapping.card.md#p4-9)»*

###### ref-3-4
**\[3.4\]** 2504.13292 — Xu et al., «Let Me Grok for You: Accelerating Grokking via Embedding Transfer from a Weaker Model». Нюанс: удержание нормы на сфере ускоряет обобщение, но вводит дополнительный гиперпараметр. [`"found that restricting the weight norm to a sphere of the appropriate radius during training can accelerate generalization"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p3-1). *«[обнаружили, что удержание нормы весов на сфере подходящего радиуса в ходе обучения способно ускорить генерализацию](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p3-1)»*

###### ref-3-5
**\[3.5\]** 2504.17243 — Zhou et al., «NeuralGrok: Accelerate Grokking by Neural Gradient Transformation». Нюанс: преобразование градиента не задаётся вручную, а выучивается вспомогательным блоком совместно с моделью. [`"learns an optimal gradient transformation to accelerate"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p1-2). *«[выучивающий оптимальное преобразование градиента ради ускорения](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.card.md#p1-2)»*
