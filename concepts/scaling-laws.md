# Законы масштабирования (scaling laws)

[Колмогоровская сложность](kolmogorov-complexity.md) ← предыдущая карточка, следующая → [criticality-critical-point](criticality-critical-point.md)

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Законы масштабирования** — степенные зависимости качества модели от размера модели, объёма данных и вычислительного бюджета, установленные для больших языковых моделей и перенесённые в разговор о [гроккинге](grokking.md) как рамка, в которую отложенная генерализация должна укладываться или которой она противоречит. В корпусе они появляются двояко: как одно из «поразительных открытий» о генерализации рядом с [двойным спуском](double-descent.md), гроккингом и [эмерджентностью](emergence.md) \[[1.1](#ref-1-1)\], и как предмет вывода — закон, описывающий границу между запоминанием и обобщением, доказывается из динамики обучения \[[1.2](#ref-1-2)\].

## Детализация

Разница между двумя употреблениями существеннее, чем кажется. Классический закон масштабирования — эмпирическая подгонка: потеря убывает степенным образом по числу параметров и по объёму данных, монотонно и гладко \[[3.2](#ref-3-2)\]. Закон, о котором говорит корпус гроккинга, — это утверждение о **пороге**: при каком объёме данных выученные признаки перестают быть обобщаемыми и остаются запоминающими \[[1.2](#ref-1-2)\].

**Где рамка ломается.** Гладкий степенной закон плохо совмещается с резким переходом: способности, выглядящие возникающими скачком, ставят под вопрос саму монотонность \[[3.3](#ref-3-3)\], а в упрощённой постановке модульной арифметики привычное «большие модели лучше при тех же данных» просто не выполняется — часть меньших моделей обгоняет чуть больших \[[3.1](#ref-3-1)\]. Это и есть точка, ради которой понятие держат в корпусе: гроккинг служит контрпримером к наивному прочтению законов.

**Спор о резкости.** Отдельная линия оспаривает саму резкость: ступенчатые кривые могут порождаться дискретной мерой оценивания, а не внутренней перестройкой сети \[[2.1](#ref-2-1)\]. Для законов масштабирования это критично — если ступенька есть артефакт меры, то никакого нарушения гладкого закона нет, и спорить не о чем; если ступенька настоящая, то закон описывает не всё.

**Что даёт вывод закона из динамики.** Когда закон получают не подгонкой, а из уравнений обучения, у него появляются проверяемые следствия: он предсказывает, какой объём выборки нужен, чтобы обобщающие признаки оставались устойчивыми, и как это зависит от [weight decay](weight-decay.md), [скорости обучения](learning-rate.md) и структуры задачи \[[1.2](#ref-1-2)\], \[[3.4](#ref-3-4)\]. Такой закон говорит о границе, которая на [фазовой диаграмме](phase-diagram.md) видна как линия, — и потому проверяется в точках, а не по хвосту кривой.

## Альтернативные определения и нюансы

### A. Эмпирический степенной закон

Исходная форма: потеря как степенная функция масштаба, подогнанная по множеству прогонов \[[3.2](#ref-3-2)\], \[[3.3](#ref-3-3)\]. Управляющие величины — число параметров, объём данных, бюджет вычислений; предсказание монотонно и не знает о внутренних перестройках. Именно к этой форме предъявляют претензию работы о гроккинге: она не запрещает отложенной генерализации, но и не описывает её.

### B. Закон границы запоминание/обобщение

Здесь величина не потеря, а положение порога: сколько нужно данных, чтобы выученные локальные максимумы оставались обобщаемыми \[[1.2](#ref-1-2)\]. Различающая черта — закон выводится из динамики и проверяется совпадением предсказанной границы с измеренной, а не качеством подгонки хвоста \[[3.4](#ref-3-4)\]. Это делает его ближе к критическому размеру выборки из [линии данных](data-fraction-critical-dataset-size.md), чем к законам Kaplan.

### C. Спор о том, что вообще масштабируется

Третья позиция: наблюдаемая ступенька — свойство меры, а не модели \[[2.1](#ref-2-1)\]. Источник различия здесь методический: непрерывная мера (потеря, вероятность верного ответа) может расти гладко там, где дискретная (точность) даёт скачок. Для карточки это означает, что всякое утверждение «закон масштабирования нарушается при гроккинге» требует указания, какой мерой оно получено.

## Ссылки

###### ref-1-1
**\[1.1\]** 2401.10463 — Zhu et al., «Critical Data Size of Language Models from a Grokking Perspective». [`"researchers have made a series of striking discoveries across generalization abilities, including neural scaling laws [4], double descent [11], grokking [15] and emergent abilities [23]"`](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p1-3). *«[исследователи сделали ряд поразительных открытий, касающихся способностей к генерализации, включая нейросетевые законы масштабирования [4], двойной спуск [11], гроккинг [15] и эмерджентные способности [23]](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p1-3)»*

###### ref-1-2
**\[1.2\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». [`"*Scaling Laws of Feature Emergence, Generalization and Memorization* can be derived by inspecting how the landscape changes with the data distribution."`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p2-3). *«[*Законы масштабирования возникновения признаков, генерализации и запоминания* выводятся из того, как ландшафт меняется вместе с распределением данных.](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p2-3)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2511.12768 — Hong et al., «Evidence of Phase Transitions in Small Transformer-Based Language Models». Оспаривает: ступенчатые кривые могут порождаться дискретной мерой оценивания, а не внутренней перестройкой сети — тогда нарушения гладкого закона нет. [`"They argued mathematically and empirically that many step-like curves reported in Wei et al. arise from discrete, non-linear evaluation metrics rather than abrupt internal reorganizations."`](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/original/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.md#p4-2). *«[Они доказали математически и опытно, что многие ступенчатые кривые из Wei et al. происходят от дискретных нелинейных мер оценивания, а не от резких внутренних перестроек.](../papers/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models/2511.12768.evidence-of-phase-transitions-in-small-transformer-based-language-models.card.md#p4-2)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2401.10463 — Zhu et al., «Critical Data Size of Language Models from a Grokking Perspective». Нюанс: в упрощённой постановке привычное «большие модели лучше при тех же данных» не выполняется. [`"As shown in Figure 4A, some smaller models achieve better performance than slightly la"`](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p8-1). *«[Как показано на рисунке 4A, некоторые меньшие модели достигают лучшего качества, чем чуть бо](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p8-1)»*

###### ref-3-2
**\[3.2\]** 2412.09810 — DeMoss et al., «The Complexity Dynamics of Grokking». Нюанс: монотонность «больше — лучше» названа нынешним взглядом, к которому гроккинг и предъявляет счёт. [`"the contemporary view in deep learning is that “larger models are better” [12], with empirically observed scaling laws across a range of modalities and architectures"`](../papers/2412.09810.the-complexity-dynamics-of-grokking/original/2412.09810.the-complexity-dynamics-of-grokking.md#p5-1). *«[нынешний взгляд в глубоком обучении состоит в том, что «бо́льшие модели лучше» [12], с эмпирически наблюдаемыми законами масштабирования на разных модальностях и архитектурах](../papers/2412.09810.the-complexity-dynamics-of-grokking/2412.09810.the-complexity-dynamics-of-grokking.card.md#p5-1)»*

###### ref-3-3
**\[3.3\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Paving the Way to Induce Emergence by Inhibiting Monosemantic Neurons on Pre-trained Models». Нюанс: закон обычно мягко-степенной, и именно на этом фоне резкие способности выглядят исключением. [`"Studies on Scaling Laws (Henighan et al., 2020; Kaplan et al., 2020) have analyzed the relationship between scale and performance, which typically follows a"`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-3). *«[Работы о законах масштабирования (Henighan et al., 2020; Kaplan et al., 2020) разобрали связь масштаба с качеством, обыкновенно следующую мягкому степенному закону.](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p1-3)»*

###### ref-3-4
**\[3.4\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». Нюанс: доказанный закон проверяется совпадением предсказанной границы с измеренной, а не качеством подгонки. [`"the proved scaling laws about the generalization/memorization boundary (Thm. 4) fits well with the experiments"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#fig-1). *«[доказанные законы масштабирования границы генерализации и запоминания (теорема 4) хорошо согласуются с экспериментами](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#fig-1)»*
