# Информационное узкое место (information bottleneck)

[quadratic-networks](quadratic-networks.md) ← предыдущая карточка, следующая → [Сингулярная теория обучения](singular-learning-theory.md)

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Информационное узкое место** — рамка, в которой обучение описывается как сжатие входа при сохранении сведений о цели, и которая предсказывает две поры: подгонку, а затем сжатие. Для [гроккинга](grokking.md) она привлекательна тем, что даёт готовое объяснение задержке — обобщение приходит во второй поре, — и в корпусе цитируется именно так: теория информационного узкого места *«предполагает фазу сжатия, следующую за фазой подгонки»*, с оговоркой, что сама эта картина чувствительна к техническим подробностям \[[1.1](#ref-1-1)\].

## Детализация

**Почему рамка не стала объяснением по умолчанию.** Оговорка, с которой она входит в корпус, существеннее самой рамки: спор о том, есть ли пора сжатия, идёт с 2018 года и зависит от того, как оценивать взаимную информацию в детерминированной сети. Поэтому в корпусе гроккинга её используют как метафору порядка событий, а не как измеряемую величину.

**Что заняло её место.** Практически ту же работу выполняют меры [сжатия представления](manifold-representation-compression.md) и [сложности](model-complexity-error-tradeoff.md): число линейных отображений, эффективный ранг, спектральная энтропия. Они вычислимы без оценки взаимной информации и потому проверяемы — в этом их преимущество перед узким местом \[[3.1](#ref-3-1)\].

**Как её держат в корпусе.** Не поодиночке, а в связке: гроккинг, [двойной спуск](double-descent.md) и узкое место рассматриваются как три рамки с общим лежащим в основе строением \[[3.2](#ref-3-2)\], и именно это делает её полезной — она даёт словарь, общий для трёх линий, а гроккинг служит чистой средой, где сжатие, обобщение и запоминание прослеживаются точно \[[1.2](#ref-1-2)\].

**Где рамка всё же работает.** Полезной она остаётся как язык постановки вопроса: что именно сеть выбрасывает при переходе и что сохраняет. В этом виде она смыкается с [гроккингом как сжатием](manifold-representation-compression.md) и с линией, где отложенная генерализация трактуется как спуск к решению меньшей сложности \[[3.1](#ref-3-1)\].

## Альтернативные определения и нюансы

### A. Две поры: подгонка и сжатие

Исходная форма \[[1.1](#ref-1-1)\]. Различающая черта — предсказывается порядок событий, а не их сроки; отсюда и слабость: наблюдение задержки согласуется с рамкой, но не подтверждает её, потому что тот же порядок предсказывают и другие картины.

### B. Сжатие, измеряемое без взаимной информации

Замещающая форма: вместо оценки $I(X;T)$ берут вычислимую меру сложности представления \[[3.1](#ref-3-1)\]. Источник различия — проверяемость: величина считается точно, и утверждение «сеть сжалась» перестаёт зависеть от способа оценки информации.

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.05918 — Liu et al., «Grokking as Compression: A Nonlinear Complexity Perspective». [`"The theory of information bottleneck Tishby et al. [2000] suggests a compression phase followed by a fitting phase, although the compression story is sensitive to technical details"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p5-1). *«[Теория информационного бутылочного горлышка [Tishby et al., 2000] предполагает фазу сжатия, следующую за фазой подгонки, хотя история со сжатием чувствительна к техническим подробностям](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p5-1)»*

###### ref-1-2
**\[1.2\]** 2509.20829 — Sakamoto et al., «Explaining Grokking and Information Bottleneck through Neural Collapse Emergence». [`"Two prominent examples are grokking, where test performance improves abruptly long after the training loss has plateaued, and the information bottleneck principle"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2). *«[Два приметных примера — гроккинг, при котором качество на тесте резко улучшается много позже того, как обучающая потеря вышла на плато, и начало информационного бутылочно](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-2
**\[3.2\]** 2504.12700 — de Mello Koch & Ghosh, «A Two-Phase Perspective on Deep Learning Dynamics». Нюанс: рамка входит в корпус не сама по себе, а как одна из трёх, у которых предполагается общее строение — с гроккингом и двойным спуском. [`"We propose that these frameworks are not merely analogous but reflect a common underlying structure"`](../papers/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics/original/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics.md#p3-2). *«[Мы предполагаем, что эти рамки не просто аналогичны, а отражают общее лежащее в основе строение](../papers/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics/2504.12700.a-two-phase-perspective-on-deep-learning-dynamics.card.md#p3-2)»*

###### ref-3-1
**\[3.1\]** 2310.05918 — Liu et al., «Grokking as Compression: A Nonlinear Complexity Perspective». Нюанс: место непроверяемой информационной меры занимает вычислимая мера сложности представления. [`"We define *linear mapping number* (LMN) to measure network complexity"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2). *«[Мы определяем *число линейных отображений* (linear mapping number, LMN), чтобы измерять сложность сети](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p1-2)»*
