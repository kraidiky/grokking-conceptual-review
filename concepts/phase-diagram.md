# Фазовая диаграмма (phase diagram)

[Линейное зондирование](linear-sparse-probing.md) ← предыдущая карточка, следующая → [Корреляционные ловушки](correlation-traps.md)

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Фазовая диаграмма** — карта режимов обучения в координатах гиперпараметров: плоскость (или срез многомерного объёма), на которой размечено, где сеть запоминает, где грокает, а где не обучается вовсе. В корпус её вводит макроскопический разбор Liu et al.: наряду с микроскопической эффективной теорией строится *«макроскопический анализ фазовых диаграмм, описывающих качество обучения по гиперпараметрам»* \[[1.1](#ref-1-1)\]. Смысл приёма в том, что [гроккинг](grokking.md) перестаёт быть свойством одной настройки и становится областью на карте, у которой есть границы.

Вторая распространённая форма — раскрашивать карту не режимом, а величиной: контурный график [времени гроккинга](grokking-time.md) по двум гиперпараметрам, где белым помечены области, в которых гроккинга нет вовсе \[[1.2](#ref-1-2)\].

## Детализация

Диаграмма требует трёх вещей: осей (управляющих параметров), признака режима и разрешения по сетке. Оси в корпусе почти всегда берутся из настроек обучения — [weight decay](weight-decay.md), размер обучающей выборки, [скорость обучения](learning-rate.md), ширина слоя, — а признаком служит либо двоичный исход («грокнул / не грокнул»), либо непрерывная величина вроде срока или конечной точности.

**Что даёт разметка.** Она превращает разрозненные наблюдения в утверждение о границе: гроккинг наблюдается лишь в определённом диапазоне гиперпараметров \[[3.3](#ref-3-3)\], и вопрос смещается с «бывает ли» на «где проходит край». Отсюда же берётся понятие критического размера обучающей выборки как наименьшего объёма данных, при котором обобщение ещё достижимо \[[3.1](#ref-3-1)\] — это координата границы, а не отдельное явление.

**Чем диаграмма отличается от [фазового перехода](phase-transition.md).** Переход — событие во времени обучения, диаграмма — карта в пространстве настроек; на ней переход виден как линия, разделяющая области. Различие важно, потому что одно и то же слово «фаза» в корпусе носят обе вещи, и работа, сообщающая «фазовую структуру», может иметь в виду и режимы траектории, и области настроек \[[3.4](#ref-3-4)\].

**Где границы аналитические.** В решаемых моделях диаграмму не размечают по сетке прогонов, а получают решением уравнения: численное решение вытекающего уравнения даёт полную фазовую диаграмму, подтверждающую предположения теоремы \[[3.2](#ref-3-2)\]. Это меняет статус карты — из сводки опытов она становится предсказанием, которое можно проверить в точках.

**Что диаграмма не говорит.** Область на карте молчит о механизме: две точки внутри одной области могут реализовывать разные алгоритмы. Работа о часах и пицце размечает границу именно между алгоритмами — по силе внимания и ширине слоя — и находит её почти линейной \[[3.5](#ref-3-5)\]; такая карта не совпадает с картой «грокнул / не грокнул» и строится по другим признакам. И всякая диаграмма ограничена сеткой, на которой её считали: перенос на большие модели и реальные задачи авторы оставляют открытым \[[2.1](#ref-2-1)\].

## Альтернативные определения и нюансы

### A. Двоичная карта режимов

Простейшая форма: каждая точка сетки помечается исходом прогона, и области разделяются линией. Управляющие параметры — обычно размер выборки и сила регуляризации \[[1.1](#ref-1-1)\]. Достоинство — прямая читаемость; цена — исход зависит от определения гроккинга (порог, бюджет шагов) и от [разброса по семенам](seed-variance-reproducibility.md): у границы одна и та же точка сетки может дать разный ответ на разных семенах.

### B. Карта величины

Вместо режима на карту наносят число — чаще всего срок: контурный график времени гроккинга по двум гиперпараметрам, с явно помеченными областями, где гроккинг не наступает \[[1.2](#ref-1-2)\]. Различающая черта: такая карта показывает не только где проходит граница, но и как быстро величина растёт при подходе к ней, и потому пригодна для проверки предсказанной формы зависимости — например, экспоненциальной по норме весов.

### C. Аналитическая диаграмма

Границу получают не сеткой прогонов, а решением уравнения на параметр порядка \[[3.2](#ref-3-2)\]. Источник различия — статус результата: эмпирическая карта описывает то, что уже наблюдалось, аналитическая предсказывает границу в точках, где никто не считал, и потому фальсифицируема. Оговорка: аналитические диаграммы получены в моделях, гораздо более простых, чем трансформер, и перенос на него — самостоятельный вопрос.

### D. Карта алгоритмов, а не режимов

Особый случай: оси те же, но области размечены по тому, *какое* решение выучено, а не по тому, состоялось ли обобщение. Такая диаграмма показывает переход между двумя механизмами — например, от одного алгоритма модульного сложения к другому — и её граница почти линейна по силе внимания и ширине слоя \[[3.5](#ref-3-5)\]. Признак режима здесь не точность, а поведенческие меры, различающие алгоритмы, что делает карту несравнимой с картой «грокнул / не грокнул».

## Ссылки

###### ref-1-1
**\[1.1\]** 2205.10343 — Liu et al., «Towards Understanding Grokking: An Effective Theory of Representation Learning». [`"a *macroscopic* analysis of phase diagrams describing learning performance across hyperparameters"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p1-2). *«[*макроскопический* анализ фазовых диаграмм, описывающих качество обучения по гиперпараметрам](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#p1-2)»*

###### ref-1-2
**\[1.2\]** 2310.16441 — Levi et al., «Grokking in Linear Estimators – A Solvable Model that Groks without Understanding». [`"White regions indicate no grokking, as generalization accuracy does not converge to $95\%$."`](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/original/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.md#fig-4). *«[Белые области означают отсутствие гроккинга, поскольку обобщающая точность не сходится к $95\%$.](../papers/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding/2310.16441.grokking-in-linear-estimators-a-solvable-model-that-groks-without-understanding.card.md#fig-4)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Ограничивает: диаграмма построена на 2–3 задачах при одном модуле и малых моделях, и перенос её на большие модели авторы оставляют открытым. [`"Whether the scaling of manifold rank with task count, the holographic incompressibility, and the WD phase diagram generalize to larger models and real-world tasks remains open."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p30-7). *«[Обобщаются ли на бо́льшие модели и реальные задачи зависимость ранга многообразия от числа задач, голографическая несжимаемость и фазовая диаграмма по WD — остаётся открытым.](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p30-7)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2205.10343 — Liu et al., «Towards Understanding Grokking: An Effective Theory of Representation Learning». Нюанс: критический размер обучающей выборки — это координата границы на диаграмме, а не отдельное явление. [`"The critical training set size corresponds to the least amount of training"`](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/original/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.md#p1-6). *«[Критический размер обучающей выборки соответствует наименьшему количеству обучающих данных](../papers/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning/2205.10343.towards-understanding-grokking-an-effective-theory-of-representation-learning.card.md#p1-6)»*

###### ref-3-2
**\[3.2\]** 2310.03789 — Rubin et al., «Grokking as a First Order Phase Transition in Two Layer Networks». Нюанс: диаграмма получена решением уравнения, а не сеткой прогонов, и потому предсказывает границу там, где опытов не было. [`"Solving the implied equation for $a$ numerically yields the full phase diagram here"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p8-6). *«[Численное решение вытекающего уравнения на $a$ даёт полную фазовую диаграмму](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.card.md#p8-6)»*

###### ref-3-3
**\[3.3\]** 2306.13253 — Notsawo et al., «Predicting Grokking Long Before it Happens». Нюанс: наблюдение, ради которого диаграмма и нужна, — гроккинг живёт лишь в диапазоне настроек. [`"Recent work has shown that grokking is observed only with a certain range of hyperparameters"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p2-1). *«[Недавние работы показали, что гроккинг наблюдается лишь в определённом диапазоне гиперпараметров](../papers/2306.13253.predicting-grokking-long-before-it-happens/2306.13253.predicting-grokking-long-before-it-happens.card.md#p2-1)»*

###### ref-3-4
**\[3.4\]** 2602.18649 — Xu, «Global Low-Rank, Local Full-Rank: The Holographic Encoding of Learned Algorithms». Нюанс: «фазовая структура» здесь относится к режимам траектории, а не к областям настроек — то же слово в другом смысле. [`"A detailed analysis of the training dynamics, phase structure, and transverse instabilities of these models"`](../papers/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms/original/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms.md#p3-6). *«[Подробный анализ динамики обучения, фазовой структуры и поперечных неустойчивостей этих моделей](../papers/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms/2602.18649.global-low-rank-local-full-rank-the-holographic-encoding-of-learned-algorithms.card.md#p3-6)»*

###### ref-3-5
**\[3.5\]** 2306.17844 — Zhong et al., «The Clock and the Pizza: Two Stories in Mechanistic Explanation of Neural Networks». Нюанс: области размечены по выученному алгоритму, а не по факту обобщения, и граница оказывается почти линейной. [`"We also observe an almost linear phase boundary with regards to both attention rate and layer width."`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p8-5). *«[Мы наблюдаем также почти линейную фазовую границу по обеим величинам — силе внимания и ширине слоя.](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p8-5)»*

## Цитирования

Работы, лишь упоминающие понятие (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». [`"Liu et al. 2022b construct further small examples of grokking, which they use to compute phase diagrams"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-1). *«[Liu et al. 2022b строят дальнейшие малые примеры гроккинга, которыми пользуются для вычисления фазовых диаграмм](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p3-1)»*
