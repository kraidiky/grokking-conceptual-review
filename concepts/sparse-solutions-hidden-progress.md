# Разрежённые решения и скрытый прогресс (sparse solutions / hidden progress)

[model-complexity-error-tradeoff](model-complexity-error-tradeoff.md) ← предыдущая карточка, следующая → [polysemanticity-superposition](polysemanticity-superposition.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Скрытый прогресс** — продвижение внутри сети, которое идёт постепенно и монотонно, пока внешние меры (потеря, точность) стоят на плато: снаружи видно долгое плато и резкий скачок, а внутри — плавное усиление нужного признака \[[1.1](#ref-1-1)\]. Понятие введено на выучивании чётностей, где показано, что *«потери и точности, видимые снаружи, обнаруживают долгое плато и резкий фазовый переход, скрывая постепенное продвижение в итерациях SGD»* \[[1.1](#ref-1-1)\], и оттуда перенесено на [гроккинг](grokking.md).

Вторая половина понятия — **разрежённость решения**: то, к чему этот прогресс ведёт. Обобщающее решение оказывается сосредоточенным на немногих направлениях (частотах, нейронах, подсети), тогда как запоминающее размазано, и именно переход к разрежённому решению внешне выглядит как скачок.

## Детализация

Связка двух половин делает понятие рабочим: если решение разрежено, то у него есть измеримая координата (какая доля мощности сидит на нужных направлениях), и эта координата растёт задолго до скачка точности — то есть служит **скрытой мерой прогресса**. Nanda et al. прямо ставят задачу так: находить меры, которые *«предшествуют»* резкому переходу и позволяют его предсказывать \[[1.2](#ref-1-2)\], и получают их не подгонкой, а из механистического разбора выученного алгоритма \[[3.2](#ref-3-2)\].

**Чем скрытый прогресс отличается от [мер прогресса](progress-measures.md).** Мера прогресса — инструмент; скрытый прогресс — утверждение о том, что измерять есть что: под плато идёт монотонное движение, а не топтание на месте. Разница проверяема: работа о чётностях показывает фурье-зазор, растущий во время плато \[[3.1](#ref-3-1)\], тогда как разбор эмбеддингов на ранних шагах гроккинга не находит структуры ни в PCA, ни в t-SNE \[[3.3](#ref-3-3)\] — то есть скрытый прогресс виден не всякой мерой и не во всякой постановке.

**Где разрежённость становится причиной.** Если обобщающее решение — разрежённая подсеть, конкурирующая с плотной запоминающей, то переход есть смена победителя, а не появление нового умения; тогда скрытый прогресс — это накопление веса разрежённой ветви. Эта линия смыкается с [разрежёнными подсетями](sparse-subnetwork-lottery-ticket.md) и с [действенностью схем](circuit-efficiency.md), где обобщающая схема выигрывает у запоминающей по норме на единицу качества.

**Оговорка о постановке.** Исходный результат получен в онлайновом обучении со свежими партиями, что снимает переобучение, но связывает время обучения и число независимых примеров — авторы это оговаривают прямо \[[2.1](#ref-2-1)\]. Перенос на гроккинг, где выборка закреплена и переобучение существенно, поэтому не автоматичен.

## Альтернативные определения и нюансы

### A. Скрытый прогресс как измеримая величина под плато

Сильная форма: существует величина, монотонно растущая во время плато и предсказывающая скачок \[[1.1](#ref-1-1)\], \[[3.1](#ref-3-1)\]. Различающая черта — предсказательность: величина обязана опережать переход, а не совпадать с ним. Отсюда требование к любой такой мере — устойчивое опережение по семенам, иначе она бесполезна как предвестник.

### B. Скрытый прогресс как следствие механизма

Слабее по форме, сильнее по обоснованию: меру не ищут перебором, а выводят из разобранного алгоритма — зная, что сеть считает суммы через дискретное преобразование, можно смотреть на долю мощности в нужных частотах \[[1.2](#ref-1-2)\], \[[3.2](#ref-3-2)\]. Источник различия: такая мера привязана к конкретной задаче и не переносится на постановку, где алгоритм неизвестен.

### C. Разрежённость как свойство решения, а не пути

Третье прочтение смотрит не на путь, а на итог: обобщающее решение занимает мало направлений, запоминающее — много. Тогда «скрытый прогресс» есть постепенное перетекание мощности между двумя решениями. Оговорка: разрежённость итога сама по себе не доказывает, что переход был постепенным внутри, — она совместима и с резкой перестройкой, и потому требует отдельного измерения по ходу обучения \[[3.3](#ref-3-3)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». [`"Black-box losses and accuracies exhibit a long plateau and sharp phase transition (top), hiding gradual progress in the SGD iterates (bottom)."`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#fig-3). *«[Потери и точности, видимые снаружи, обнаруживают долгое плато и резкий фазовый переход (сверху), скрывая постепенное продвижение в итерациях SGD (снизу).](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#fig-3)»*

###### ref-1-2
**\[1.2\]** 2301.05217 — Nanda et al., «Progress measures for grokking via mechanistic interpretability». [`"We could better understand and predict these phase transitions by finding *hidden progress measures* (Barak et al., 2022): metrics that precede and ar"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-4). *«[Мы могли бы лучше понимать и предсказывать эти фазовые переходы, находя *скрытые меры прогресса*](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-4)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Ограничивает: исходный результат получен в онлайновом обучении, которое снимает переобучение и потому не совпадает с постановкой гроккинга на закреплённой выборке. [`"While this mitigates the confounding factor of overfitting, it couples the resources of training time and independent samples in a suboptimal way"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p10-3). *«[Хотя это смягчает мешающую причину переобучения, оно неоптимальным образом связывает ресурсы времени обучения и независимых примеров](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p10-3)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2210.01117 — Liu et al., «Omnigrok: Grokking Beyond Algorithmic Data». Нюанс: скрытый прогресс здесь назван одним из частичных ответов на вопрос о причине гроккинга, наряду с медленным образованием представлений. [`"Barak et al. [2022] uses Fourier gap to describe hidden progress"`](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/original/2210.01117.omnigrok-grokking-beyond-algorithmic-data.md#p1-8). *«[Barak et al. [2022] описывают скрытый прогресс через фурье-зазор](../papers/2210.01117.omnigrok-grokking-beyond-algorithmic-data/2210.01117.omnigrok-grokking-beyond-algorithmic-data.card.md#p1-8)»*

###### ref-3-2
**\[3.2\]** 2301.05217 — Nanda et al., «Progress measures for grokking via mechanistic interpretability». Нюанс: меру не ищут перебором, а выводят из механистического разбора выученного алгоритма. [`"we introduce a different approach to uncovering hidden progress measures: via *mechanistic explanations*"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-5). *«[мы предлагаем иной подход к обнаружению скрытых мер прогресса: через *механистические объяснения*](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-5)»*

###### ref-3-3
**\[3.3\]** 2205.10343 — Liu et al., «Towards Understanding Grokking: An Effective Theory of Representation Learning». Нюанс: скрытый прогресс виден не всякой мерой — на ранних шагах ни PCA, ни t-SNE структуры в эмбеддингах не находят. [`"We study the embeddings at different training times and find that neither PCA (shown in Figure 1) nor t-SNE (not shown here) reveal any structure."`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p8-8). *«[Мы изучаем эмбеддинги в разные моменты обучения и обнаруживаем, что ни PCA (показан на рисунке 1), ни t-SNE (здесь не показан) не выявляют никакой структуры.](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#p8-8)»*
