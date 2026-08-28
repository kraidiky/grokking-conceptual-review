# Поведенческий против истинного грокинга (behavioral vs true grokking)

[Полугроккинг](semi-grokking.md) ← предыдущая карточка, следующая → [comprehension-confusion-phases](comprehension-confusion-phases.md)

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

Различение **поведенческого** и **истинного** грокинга — вопрос о том, что именно засчитывается за [гроккинг](grokking.md): скачок тестовой точности сам по себе или скачок вместе со складыванием обобщающего механизма внутри сети. Постановка вопроса принадлежит работам, которые механистически перепроверяют гроккнувшие модели и находят, что *«гроккнувшие и негроккнувшие модели следуют тождественным путям рассуждения»*, а сам гроккинг лишь встраивает запомненное \[[1.1](#ref-1-1)\]. Отсюда рабочее имя для случая, когда поведение есть, а механизма нет, — «поддельный гроккинг»: *«поведенческий гроккинг без складывания контура»* \[[1.2](#ref-1-2)\].

## Детализация

Различение опирается на то, что у гроккинга есть две независимо измеримые стороны. Внешняя — кривые точности, порог, [время гроккинга](grokking-time.md). Внутренняя — наличие [контура](fourier-features-circuits.md), который эту точность производит, и его достаточность в отрыве от остальной сети. Пока обе стороны совпадают, различать нечего; работы этой линии показывают, что они расходятся, причём в обе стороны.

**Механизм есть, а скачка ещё нет.** Обобщающая схема складывается раньше видимого перехода — это установлено линией [мер прогресса](progress-measures.md) и лежит в основе всей идеи скрытого продвижения под плато.

**Скачок есть, а перестройка не закончена.** Обратный случай измерен прямо: половина послегроккинговой перестройки схемы приходится в медиане на 350 шагов *после* перехода тестовой точности, а девяносто процентов — на 1250–1850 шагов, и так во всех 32 семенах на задачу \[[2.1](#ref-2-1)\]. То есть момент, засчитываемый за гроккинг по кривой, не совпадает с моментом, когда механизм готов.

**Почему это не спор о словах.** От ответа зависит, что считать доказанным: если гроккинг определяется поведенчески, то заявка «модель обрела способность рассуждать» не следует из скачка \[[1.1](#ref-1-1)\]. Рамка [действенности схем](circuit-efficiency.md) даёт этому естественное объяснение: задача допускает и обобщающее, и запоминающее решение, и переход есть смена победителя, а не появление умения из ниоткуда \[[3.1](#ref-3-1)\]. В крайних режимах это различение делается наблюдаемым: у [полугроккинга](semi-grokking.md) сеть застревает между решениями, что и показывает, что «грокнул» — не двоичное свойство \[[3.2](#ref-3-2)\].

**Чего не хватает.** Проверка «поддельного» случая ограничена вычислительно: авторы прямо оговаривают, что не смогли обследовать его исчерпывающе, потому что явление требует крайне долгого обучения \[[1.2](#ref-1-2)\].

## Альтернативные определения и нюансы

### A. Поведенческое определение

Гроккинг = скачок отложенной точности после плато; ничего о внутреннем устройстве не утверждается. Достоинство — определение операционально и переносимо на любую задачу; цена — под него попадают случаи, где обобщающего механизма нет, а рост точности объясняется интеграцией запомненного \[[1.1](#ref-1-1)\].

### B. Механистическое определение

Гроккинг = складывание контура, который в отрыве решает задачу. Различающая черта — проверяется абляцией и фильтрацией представления, а не кривой; следствие — момент гроккинга смещается относительно поведенческого, и смещение измерено: перестройка продолжается тысячи шагов после скачка \[[2.1](#ref-2-1)\].

### C. Определение через конкуренцию решений

Промежуточная позиция: гроккинг — момент, когда обобщающее решение становится действеннее запоминающего \[[3.1](#ref-3-1)\]. Управляющая величина здесь — размер выборки (критический размер данных), а не время; отсюда предсказание обратимых явлений вроде разгроккинга и режимов, где сеть остаётся между решениями \[[3.2](#ref-3-2)\].

### D. Двухузорная рамка и её пределы

Родственное прочтение: два узора, быстрый плохо обобщающий и медленный хорошо обобщающий \[[3.3](#ref-3-3)\]. Оговорка, звучащая в самом корпусе: без определения «узора» рамка описывает, но не предсказывает, и потому её приходится закреплять либо мерой действенности, либо механистической проверкой.

## Ссылки

###### ref-1-1
**\[1.1\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits in Transformers». [`"We show that grokked and non-grokked models follow identical reasoning paths"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-6). *«[Мы показываем, что гроккнувшие и негроккнувшие модели следуют тождественным путям рассуждения](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p4-6)»*

###### ref-1-2
**\[1.2\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits in Transformers». [`"our investigation into “fake grokking” (behavioral grokking without circuit formation) is not exhaustive"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p5-2). *«[наше исследование «поддельного гроккинга» (поведенческого гроккинга без складывания контура) не всеохватно](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p5-2)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2607.06628 — Truong Xuan Khanh, «Cross-Trajectory Chimera Interventions Reveal Dissociable Roles of Weight Magnitude and Direction in Grokking». Оспаривает поведенческое определение изнутри: перестройка схемы продолжается тысячи шагов после того, как кривая уже засчитала гроккинг. [`"$50\%$ of the post-grokking reorganization is reached at a median of $350$ steps after the test-accuracy transition"`](../papers/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking.card.md#p7-3). *«[$50\%$ послегроккинговой перестройки достигается в медиане через $350$ шагов после перехода тестовой точности](../papers/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking.card.md#p7-3)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2309.02390 — Varma et al., «Explaining grokking through circuit efficiency». Нюанс: переход объясняется сменой победителя между двумя решениями, а не появлением нового умения. [`"We propose that grokking occurs when the task admits a generalising solution and a memorising solution"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3). *«[Мы предлагаем объяснение: гроккинг возникает, когда задача допускает и обобщающее, и](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3)»*

###### ref-3-2
**\[3.2\]** 2309.02390 — Varma et al., «Explaining grokking through circuit efficiency». Нюанс: у границы предсказаны режимы, где «грокнул» перестаёт быть двоичным свойством. [`"which we call the critical dataset size $D_{\textrm{crit}}$"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p2-1). *«[мы называем её критическим размером набора данных $D_{\textrm{crit}}$](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p2-1)»*

###### ref-3-3
**\[3.3\]** 2311.18817 — Lyu et al., «Dichotomy of Early and Late Phase Implicit Biases Can Provably Induce Grokking». Нюанс: двухузорная рамка описывает различение, но сама нуждается в определении «узора», иначе не предсказывает. [`"Davies et al. 2022 hypothesized that both grokking and double descent are the result of the existence of two patterns: one pattern is faster to learn but generalizes poorly, and the other pattern is slower to learn but generalizes well."`](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/original/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.md#p16-1). *«[Davies et al. 2022 выдвинули гипотезу, что и гроккинг, и двойной спуск суть следствие существования двух узоров: один узор выучивается быстрее, но обобщает плохо, а другой выучивается медленнее, но обобщает хорошо.](../papers/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking/2311.18817.dichotomy-of-early-and-late-phase-implicit-biases-can-provably-induce-grokking.card.md#p16-1)»*
