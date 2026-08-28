# Полисемантичность и суперпозиция (polysemanticity / superposition)

[activation-sparsity](activation-sparsity.md) ← предыдущая карточка, следующая → [execution-manifold](execution-manifold.md)

[Индекс карточек понятий](index.md), категория: [2. Механизмы и представления](index.md#cat-2)\
→ Следующая категория: [3. Задачи и наборы данных](modular-arithmetic.md)\
← Предыдущая категория: [1. Явления](grokking.md)

## Определение

**Полисемантичность** — свойство нейрона откликаться на несколько несвязанных признаков сразу; **суперпозиция** — объяснение этому: сеть размещает больше признаков, чем у неё направлений, накладывая их друг на друга в одном подпространстве. Противоположный полюс — **моносемантический** нейрон, возбуждаемый ровно одним признаком \[[1.2](#ref-1-2)\]. Для корпуса это важно потому, что все инструменты разбора грокнувших сетей — [зондирование](linear-sparse-probing.md), абляции, чтение [фурье-контуров](fourier-features-circuits.md) — молча предполагают, что найденное направление отвечает одному понятию.

Признание суперпозиции меняет постановку: часть составляющих модели *«представляют признаки в суперпозиции»*, и потому чтение по нейронам в стандартном базисе даёт лишь приближение \[[1.1](#ref-1-1)\].

## Детализация

**Почему это касается гроккинга.** Механистические разборы устроены так: найти направления, показать, что они несут алгоритм, проверить абляцией. Если направления полисемантичны, каждый шаг ослабевает — абляция задевает не одну функцию, а несколько, и вывод «эта часть считает сложение» перестаёт быть точным. В корпусе это проявляется как разрыв между «контур найден» и «контур достаточен»: сильные работы проверяют достаточность фильтрацией представления, а не только присутствием направления.

**Связь с масштабом.** Наблюдение, вокруг которого выстроена целая линия: моносемантические нейроны *«постепенно исчезают с ростом масштаба модели»* \[[1.2](#ref-1-2)\]. Отсюда неожиданный ход — не бороться с полисемантичностью, а использовать её как рычаг: подавление моносемантичности предлагается как приём, ускоряющий появление способностей \[[3.1](#ref-3-1)\]. Для карточки существенно, что здесь полисемантичность из помехи интерпретируемости превращается в управляемую величину.

**Что даёт модульная арифметика.** В задачах, где алгоритм известен, полисемантичность проверяема напрямую: можно спросить, несёт ли одно и то же направление признаки нескольких операций. Работы о переносе между операциями показывают, что грокнувшие модели приобретают общие признаки, переносимые между схожими задачами \[[3.2](#ref-3-2)\], — то есть направления делятся между операциями, что и есть полисемантичность в её мягкой форме.

**Оговорка.** Суперпозиция в корпусе почти всегда привлекается как объяснение, а не измеряется: работ, которые считали бы число признаков на направление в грокнувшей сети, нет. Поэтому утверждения вида «признаки лежат в суперпозиции» стоит читать как рамку, наследованную из интерпретируемости больших моделей, а не как результат, полученный на гроккинге.

## Альтернативные определения и нюансы

### A. Полисемантичность как свойство нейрона

Наблюдательная форма: нейрон откликается на несколько несвязанных признаков \[[1.2](#ref-1-2)\]. Различающая черта — определение операционально (статистика откликов) и не требует гипотезы о том, почему так вышло. Цена — оно привязано к базису нейронов и потому не переносится на представления, которые сеть держит в произвольных направлениях.

### B. Суперпозиция как объяснение

Теоретическая форма: признаков больше, чем направлений, и они накладываются \[[1.1](#ref-1-1)\]. Источник различия — предсказание: суперпозиция обещает, что признаки восстановимы разрежённым разложением, тогда как простая полисемантичность ничего об этом не говорит. В корпусе гроккинга это предсказание не проверялось.

### C. Моносемантичность как рычаг, а не как цель

Прикладное прочтение: если моносемантичность убывает с масштабом, то её подавление можно применять как приём обучения \[[3.1](#ref-3-1)\]. Различающая черта — направление вывода: не «сделаем сеть понятнее», а «сделаем её быстрее», и интерпретируемость здесь побочна. Оговорка: связь подавления с ускорением показана на своей постановке и не проверялась на алгоритмических задачах корпуса.

## Ссылки

###### ref-1-1
**\[1.1\]** 2405.12755 — Golechha, «Progress Measures for Grokking on Real-world Tasks». [`"While some model components represent features in superpo"`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p4-2). *«[И хотя некоторые составляющие модели представляют признаки в суперпозиции](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p4-2)»*

###### ref-1-2
**\[1.2\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». [`"The literature has observed that monosemantic neurons in neural networks gradually diminish as the model scale increases."`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2). *«[В литературе замечено, что моносемантические нейроны в нейронных сетях постепенно исчезают с ростом масштаба модели.](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». Нюанс: моносемантичность определяется операционально — по статистике откликов на один признак, через разрежённое щупание. [`"The left figure shows the output statistics of a monosemantic neuron, which is activated only by the feature “Python”."`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#fig-1). *«[Слева — статистика выходов моносемантического нейрона, возбуждаемого лишь признаком «Python».](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#fig-1)»*

###### ref-3-2
**\[3.2\]** 2402.16726 — Furuta et al., «Towards Empirical Interpretation of Internal Circuits and Properties in Grokked Transformers on Modular Polynomials». Нюанс: перенос признаков между схожими операциями — мягкая форма полисемантичности, проверяемая там, где алгоритм известен. [`"*grokked models obtain common features transferable among similar op"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-3). *«[*грокнувшие модели приобретают общие признаки, переносимые между схож](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.card.md#p1-3)»*
