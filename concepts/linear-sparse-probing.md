# Линейное зондирование (linear / sparse probing)

[Тяжёлохвостовая саморегуляризация](heavy-tailed-self-regularization-htsr.md) ← предыдущая карточка, следующая → [Корреляционные ловушки](correlation-traps.md)

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Линейное зондирование** — метод анализа внутренних представлений сети: к
замороженным скрытым активациям (hidden activations) обученной модели
присоединяют лёгкий линейный классификатор («зонд», probe) и по его точности
судят о том, линейно ли декодируемо интересующее свойство из этих активаций;
высокая точность зонда трактуется как признак качества и структурированности
внутреннего представления \[[1.1](#ref-1-1)\]. **Разреженное зондирование**
(sparse probing) — вариант того же приёма, отбирающий не всё представление, а
разреженное подмножество отдельных нейронов для детекции конкретного признака
\[[2.1](#ref-2-1)\]. В контексте [гроккинга](grokking.md) как инструмент
покадрового наблюдения за представлениями его вводят Fan et al.
\[[1.1](#ref-1-1)\] и Kvinge et al. \[[5.1](#ref-5-1)\]↗.

## Детализация

Каноническая процедура: к каждому внутреннему слою сети присоединяется
случайно инициализированная линейная «голова», которую дообучают на обучающей
выборке фиксированное число шагов и затем оценивают на тесте; точность такого
зонда измеряет, достаточно ли хороши выученные признаки, чтобы быть **линейно
разделимыми** (linearly separable — то есть чтобы классы можно было отделить
гиперплоскостью) в высокоразмерном пространстве \[[1.1](#ref-1-1)\]. Тем самым
точность зонда работает как [параметр порядка](order-parameter.md) (order parameter — скалярная
величина, отслеживающая переход) качества представления по слоям и по шагам
обучения. Целевой вариант зондирует не «качество вообще», а декодируемость
конкретного концепта: Kvinge et al. обучают линейный зонд предсказывать
принадлежность пары элементов к подгруппе по скрытым активациям и калибруют
его случайной разметкой, чтобы отделить подлинную алгебраическую структуру от
артефакта \[[5.1](#ref-5-1)\]↗. Разреженное зондирование (введено Gurnee et al.,
2023; в этом корпусе применяется Wang et al.) отбирает разреженный набор
нейронов и по F1-оценке или интерпретируемости выявляет **монофункциональные
нейроны** (monosemantic neurons — активирующиеся ровно на один признак)
\[[2.1](#ref-2-1)\]. У метода есть спорная сторона: зондирование
вычислительно дорого и на малых моделях (70M параметров) классификатор-зонд
может не улавливать целевые нейроны \[[2.1](#ref-2-1)\], а точность зонда
заметно варьируется между запусками — иногда модель верно решает задачу, но
зонд угадывает на уровне случайности \[[5.1](#ref-5-1)\]↗. С другой стороны,
покадровое линейное зондирование фиксирует, что декодируемость целевой
информации из скрытых состояний нарастает по ходу гроккинга и «дорастает» до
идеальной уже после перехода \[[3.1](#ref-3-1)\], а точность подгруппового
зонда отслеживает глобальную точность \[[5.1](#ref-5-1)\]↗ — это делает зонд
[мерой прогресса](progress-measures.md) представления. Родственный, но иной по природе приём —
интервенционный (каузальный) зонд: Truong et al. (2604.13123) вмешиваются в
само представление (representation-mixing probe), а не обучают классификатор
поверх него; это не линейное/разреженное зондирование, а причинная проверка. Метод востребован в исследованиях [эмерджентности](emergence.md),
где по зонду судят о появлении внутренних признаков раньше внешней метрики.

## Альтернативные определения и нюансы

### A. Зонд качества представления по слоям (Fan et al.)

Здесь зондирование определяется операционально: линейная голова на каждом
внутреннем слое, обученная фиксированное число шагов, а её тестовая точность —
скалярная оценка того, насколько внутренние признаки линейно разделимы
\[[1.1](#ref-1-1)\]. Отличительная машинерия — послойный профиль точности
зонда как индикатор того, на какой глубине и в какой момент обучения
формируется «хорошее» представление; зонд здесь агностичен к содержанию задачи
и меряет качество, а не конкретный концепт.

### B. Целевой зонд конкретного концепта с калибровкой (Kvinge et al.)

Определение через декодируемость заранее заданного свойства: активации метятся
бинарной меткой (принадлежит ли пара элементов подгруппе H) и линейный зонд
учат предсказывать эту метку \[[5.1](#ref-5-1)\]↗. Ключевое отличие — наличие
**калибровочного базлайна**: параллельно зонд обучают на случайном подмножестве
элементов, не образующем подгруппу; вывод о том, что структура действительно
закодирована, делается только если зонд на истинной подгруппе бьёт зонд на
случайной разметке \[[5.1](#ref-5-1)\]↗. Источник различия с трактовкой A —
зонд проверяет присутствие конкретной алгебраической структуры, а не общее
качество.

### C. Разреженное зондирование нейронов (sparse probing, Gurnee et al. via Wang et al.)

Определение через отбор нейронов, а не через полное представление: зонд
выбирает разреженное подмножество нейронов, наиболее информативных о заданном
признаке, и по их статистике (F1, автоинтерпретируемость) детектирует
монофункциональные нейроны \[[2.1](#ref-2-1)\]. Отличие от A и B — единица
анализа не гиперплоскость в полном пространстве активаций, а малый набор
отдельных координат-нейронов; это делает зонд инструментом
механистической атрибуции признака конкретным нейронам.

### Оспаривают

Wang et al. (2503.23298) присоединяются к разреженному зондированию как к
эталону, но оспаривают его практическую пригодность как рабочего детектора:
эксперименты по зондированию затратны по времени и требуют более дешёвых
альтернатив, а на малой модели зонд-классификатор оказывается неэффективным —
поэтому предлагается прокси-метрика (Monosemanticity Score) \[[2.1](#ref-2-1)\].

### Поддерживают

Wang et al. (2405.15071) присоединяются к трактовке зонда как меры прогресса
представления: линейное зондирование скрытого состояния показывает, что целевая
информация становится идеально декодируемой после гроккинга, а декодируемость
монотонно улучшается на протяжении всего перехода \[[3.1](#ref-3-1)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2405.19454 — Fan, Pascanu, Jaggi 2024, «Deep Grokking: Would Deep Neural Networks Generalize Better?». [`"For linear probing, we attach a randomly initialized linear classifier head to each internal layer $l$ of the neural network. We train the linear head only for a fixed number of steps on the training set, then evaluate it on the test set."`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p2-2). *«[Для линейного щупания мы приставляем к каждому внутреннему слою $l$ сети линейный распознаватель со случайным начальным приближением. Мы обучаем только эту линейную голову заданное число шагов на обучающем наборе, а затем оцениваем её на тестовом.](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p2-2)»*\
Доп.: [`"The linear probing accuracy measures whether the learnt internal features are good enough to be linearly separable in the high-dimensional space."`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p2-2) — *«[Точность линейного щупа меряет, довольно ли хороши выученные внутренние признаки, чтобы быть линейно разделимыми в многомерном пространстве.](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p2-2)»*

## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2503.23298 — Wang et al., «Learning Towards Emergence: Inducing Emergence by Inhibiting Monosemantic Neurons». Оспаривает: зондирование затратно и ненадёжно на малых моделях, нужен более дешёвый прокси. [`"However, probing experiments are time-consuming, making it crucial to develop alternative methods to boost the study of monosemanticity."`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p3-5). *«[Однако опыты со щупанием затратны по времени, отчего важно развивать иные приёмы, ускоряющие изучение моносемантичности](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p3-5)»*\
Доп.: [`"probing classifier may not be effective in detecting monosemantic neurons in the 70M model."`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p13-6) — *«[щупающий распознаватель, возможно, не годится для обнаружения моносемантических нейронов в модели 70M](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#p13-6)»*\
Доп. (сам приём как эталон): [`"We use sparse probing (Gurnee et al., 2023) on Pythia models (Biderman et al., 2023) to detect monosemantic neurons."`](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#fig-1) — *«[Мы берём разрежённое щупание (Gurnee et al., 2023) на моделях Pythia (Biderman et al., 2023), чтобы обнаруживать моносемантические нейроны](../papers/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models/2503.23298.learning-towards-emergence-inhibiting-monosemantic-neurons-on-pre-trained-models.card.md#fig-1)»*

### Поддерживают

###### ref-3-1
**\[3.1\]** 2405.15071 — Wang et al., «Grokked Transformers are Implicit Reasoners». Нюанс: зонд как мера прогресса — декодируемость нарастает по ходу гроккинга. [`"We also run linear probing on S[5, r1] throughout training to predict the second token of the bridge entity b"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p18-1). *«[Мы также прогоняем линейное зондирование $S[5,r_{1}]$ на протяжении обучения, чтобы предсказать второй токен мостовой сущности $b$.](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p18-1)»*\
Доп.: [`"the second token of $b$ can be perfectly decoded from $S[5,r_{1}]$ after grokking, and the decodability improves throughout grokking"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p18-1) — *«[после гроккинга второй токен $b$ идеально декодируется из $S[5,r_{1}]$, и декодируемость улучшается на протяжении гроккинга](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p18-1)»*

###### ref-3-2
**\[3.2\]** 2604.06256 — Xu, «Spectral Edge Dynamics Reveal Functional Modes of Learning». Нюанс: опорой объявлена не величина $R^{2}$ (для $x^{2}+y^{2}$ она $0.15$–$0.16$), а качественная избирательность перекрёстных членов; причинных вмешательств нет. [`"We test this by probing the perturbation with three feature sets via ridge regression"`](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/original/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.md#p11-6). *«[Мы проверяем это, зондируя возмущение тремя наборами признаков через гребневую регрессию](../papers/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning/2604.06256.spectral-edge-dynamics-reveal-functional-modes-of-learning.card.md#p11-6)»*

###### ref-3-3
**\[3.3\]** 2604.13082 — Gomezjurado Gonzalez, «The Long Delay to Arithmetic Generalization…». [`"Each probe is an L2-regularized logistic regression fit on standardized mean-pooled encoder states."`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p14-3). *«[Каждый зонд есть логистическая регрессия с L2-регуляризацией, подогнанная на стандартизованных усреднённых по позициям состояниях энкодера.](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p14-3)»*\
Доп. (крайний случай расхождения зонда и поведения): у 12-слойного decoder-only базового уровня [`"Exact-match at the final checkpoint is $0.000$ in every base"`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p25-2) — *«[Точное совпадение на итоговой контрольной точке равно $0.000$ в каждом основании](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p25-2)»* — при зондах $\geq 0.999$ в ранних слоях.
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation». [`"We provide interventional evidence via a representation-mixing probe, with an extended norm-matched control ($n=30$) disentangling the roles of parameter norm and representation entropy"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p2-3) — *«[Мы приводим свидетельство вмешательством через щуп с перемешиванием представлений, с расширенной сверкой при уравненной норме ($n=30$), разделяющей роли нормы параметров и энтропии представлений](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p2-3)»* (Иной по природе — интервенционный, каузальный зонд по активациям, а не обучаемый линейный/разреженный классификатор поверх них.)


### Внешние работы

###### ref-5-1
**\[5.1\]** 2601.21150 — Внешняя работа (демотирована из корпуса): Kvinge et al., «Can Neural Networks Learn Small Algebraic Worlds?». [`"We can test whether $f$ captures a distinct representation of $H$ by probing for subgroup membership on the hidden activations of $f$"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p7-2). *«[проверить, строит ли $f$ отдельное представление $H$, можно, прощупывая принадлежность к подгруппе по скрытым возбуждениям $f$](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p7-2)»*\
Доп.: [`"we find substantial evidence that performant models sometimes capture subgroup structure within their internal representations which we can access via the linear probing"`](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p7-6) — *«[мы находим весомое свидетельство того, что добротные модели иной раз всё же схватывают строение подгрупп во внутренних представлениях, до которых мы добираемся линейным прощупыванием](../externals/2601.21150.can-neural-networks-learn-small-algebraic-worlds/2601.21150.can-neural-networks-learn-small-algebraic-worlds.card.md#p7-6)»*
