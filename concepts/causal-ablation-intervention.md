# Каузальная абляция (causal ablation / intervention)

[Спектральный анализ / FVE](spectral-analysis-svd-esd-fve.md) ← предыдущая карточка, следующая → [Время гроккинга](grokking-time.md)

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Causal ablation / intervention (каузальная абляция / интервенция)** —
экспериментальная методология, устанавливающая **каузальную** (а не просто
корреляционную) роль внутренних компонентов сети в [гроккинге](grokking.md):
выбранный компонент (частоту Фурье, контур/подсеть, направление градиента,
признак) целенаправленно **удаляют** (абляция = обнуление/вырезание) или
**подменяют** (интервенция), а затем измеряют эффект на обобщении. Если после
абляции обобщение рушится — компонент **необходим**; если, оставив только его,
обобщение сохраняется — он **достаточен**. В контекст гроккинга приём ввели
Nanda et al. (2023), абляцией ключевых частот в Фурье-пространстве подтвердив
каузальность выученного алгоритма \[[1.1](#ref-1-1)\].

![Абляция каждой частоты по отдельности: удаление пяти ключевых рушит качество, удаление остальных 95 % частот слегка его улучшает (рис. 6 Nanda et al.)](assets/causal-ablation-frequencies.png)

## Детализация

Логика метода двусторонняя. **Абляция на необходимость**: обнуляют «ключевые»
компоненты и смотрят, падает ли тестовая точность до случайной. **Абляция на
достаточность**: наоборот, оставляют только их и проверяют, сохраняется ли
качество. У Nanda et al. это реализовано в Фурье-пространстве: сеть, грокнувшая
[модульное сложение](modular-arithmetic.md) (сложение по простому модулю — каноническая игрушечная задача
гроккинга), опирается на 3–8 дискретных частот; обнуление именно этих частот
роняет точность до уровня угадывания, а обнуление остальных 95% частот её даже
слегка улучшает \[[1.1](#ref-1-1)\]. На той же операции построены две
[прогресс-меры](progress-measures.md) (непрерывные индикаторы скрытого прогресса, идущие раньше
внешнего скачка точности): restricted loss
(оставить только ключевые частоты) и excluded loss (убрать ключевые частоты)
\[[1.1](#ref-1-1)\]. Тем же приёмом был подтверждён и тригонометрический
«алгоритм-часы» ([Clock](clock-vs-pizza.md)) — схема сложения углов на
окружности.

Присоединившиеся работы переносят каузальную абляцию/интервенцию с обнуления
частот на другие мишени. Yildirim показывает **достаточность** малого набора
Фурье-частот: оставив лишь топ-5 частот, сеть сохраняет >99.75% точности
\[[3.3](#ref-3-3)\]. Xu переносит интервенцию с выученного признака на
**обучающую динамику** — подавляет специальное направление градиентного потока и
получает монотонную дозозависимость (чем сильнее подавление, тем хуже
обобщение), устанавливая необходимость \[[3.2](#ref-3-2)\]. Zhang et al.
применяют **каузальный медиационный анализ** (CMA) через активационный патчинг
(подмену промежуточных активаций между двумя входными контекстами), локализуя
каузальность до отдельных голов внимания \[[3.4](#ref-3-4)\]. Clauw et al.
связывают каузальность с подсетями, возникающими на эмерджентной фазе
([эмерджентность](emergence.md)) \[[3.1](#ref-3-1)\], а Truong et al. дают
интервенционное свидетельство каузальной роли энтропии представлений через
зонд, смешивающий представления, — с явной оговоркой, что это ещё не полностью
каузальное утверждение \[[3.5](#ref-3-5)\]. С другой стороны, Furuta et al.
используют абляцию не для подтверждения чистого универсального контура, а чтобы
**вскрыть его пределы**: перенос грокнутых эмбеддингов между операциями
ограничен, а некоторые операции мешают друг другу — то есть выявленные абляцией
контуры не универсальны \[[2.1](#ref-2-1)\]. Каузальная абляция тем самым служит
не только подтверждению [фазового перехода](phase-transition.md) после
[фазы меморизации](memorization-phase.md), но и его критическому разбору.

## Альтернативные определения и нюансы

### A. Абляция признаков: необходимость и достаточность (Nanda; Yildirim)

Каноническая трактовка: мишень интервенции — отдельные **частоты Фурье** в
выученном представлении, а наблюдаемая величина — тестовая точность при (i)
удалении ключевых частот и (ii) сохранении только их. Nanda: обнуление ключевых
частот → случайная точность, обнуление прочих 95% → лёгкое улучшение
(необходимость) \[[1.1](#ref-1-1)\]; Yildirim: оставить лишь топ-5 частот →
>99.75% (достаточность) \[[3.3](#ref-3-3)\]. Источник различия от B и C:
интервенция идёт по **статичному выученному признаку**, а не по обучающей
динамике или активациям в середине прямого прохода.

### B. Интервенция на обучающую динамику (Xu)

Здесь каузальная рукоятка — не выученный признак, а **направление в градиентном
потоке** (ортогональный градиентный поток) во время обучения. Необходимость
устанавливается монотонной дозозависимостью: подавление направления
предотвращает генерализацию, тогда как искусственное усиление кривизны эффекта
не даёт \[[3.2](#ref-3-2)\]. Источник различия: каузальный фактор — динамическая
величина времени обучения, отсюда и вывод «необходимо, но не достаточно».

### C. Каузальный медиационный анализ активаций (Zhang)

Мишень — отдельные **головы внимания**, проверяемые активационным патчингом
(подменой промежуточных активаций между двумя входными контекстами) и оценкой
каузального медиационного эффекта (CMS) \[[3.4](#ref-3-4)\]. Источник различия:
интервенция выполняется «на лету» внутри одного прямого прохода и локализует
каузальность до конкретных голов, а не до частот или направлений градиента.

### Оспаривают

- **Абляция вскрывает пределы, а не универсальный контур** \[[2.1](#ref-2-1)\]:
  на предварительно грокнутых моделях абляция показывает, что переносимость
  грокнутых эмбеддингов ограничена отдельными комбинациями операций, а часть
  операций интерферирует, не достигая оптимума. Источник различия: тот же
  инструмент используется не для подтверждения чистого каузального контура, а
  для демонстрации его неуниверсальности — вывод об общности механизма
  ослабляется.

### Поддерживают

- **Каузальная роль подсетей эмерджентной фазы** \[[3.1](#ref-3-1)\]: подсети,
  возникающие на эмерджентной фазе (по анализу синергии/избыточности между
  нейронами), каузально связаны с отложенной генерализацией. Источник различия:
  мишень каузальности — подсеть, а не частота.
- **Необходимость ортогонального градиентного потока** \[[3.2](#ref-3-2)\]:
  интервенция на обучающую динамику даёт монотонную дозозависимость. Источник
  различия: каузальный фактор — направление градиента, а не признак.
- **Достаточность топ-частот** \[[3.3](#ref-3-3)\]: абляция, оставляющая лишь
  топ-5 частот, сохраняет >99.75% точности. Источник различия: акцент на
  достаточности малого частотного ядра.
- **Медиационный анализ голов внимания** \[[3.4](#ref-3-4)\]: CMA через
  активационный патчинг определяет, какие головы необходимы. Источник различия:
  локализация каузальности до отдельных голов внутри прямого прохода.
- **Интервенция на энтропию представлений** \[[3.5](#ref-3-5)\]: зонд,
  смешивающий представления, с контролем, уравненным по норме, разделяет вклад
  нормы параметров и энтропии. Источник различия: интервенция на
  информационную величину представления, с явной оговоркой о неполноте
  каузального утверждения.

## Ссылки

###### ref-1-1
**\[1.1\]** 2301.05217 — Nanda et al., «Progress measures for grokking via mechanistic interpretability». [`"ablating key frequencies used by the model reduces performance to chance, while ablating the other 95% of frequencies slightly improves performance"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-2). *«[абляция ключевых частот, используемых моделью, снижает качество до случайного, тогда как абляция остальных 95 % частот качество слегка улучшает](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-2)»*\
Доп. (метод): [`"performing ablations in Fourier space"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-2) — *«[выполняя абляции в фурье-пространстве](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p1-2)»*.\
Доп. (прогресс-меры): [`"restricted loss, where we ablate every non-key frequency, and excluded loss, where we instead ablate all key frequencies"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-3) — *«[ограниченную потерю (restricted loss), где мы удаляем абляцией все не-ключевые частоты, и исключающую потерю (excluded loss), где мы, наоборот, удаляем все ключевые частоты](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-3)»*.


###### ref-1-2
**\[1.2\]** 2306.17844 — Zhong et al. 2023, «The Clock and the Pizza». Вводит приём *изоляции круга*: замену матрицы вложений рядом ранг-2 приближений по парам главных компонент. Нюанс: вмешательство только после обучения; ансамблирование выведено из падения точности при удалении, обучающего вмешательства нет. [`"We call this procedure *circle isolation*."`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p5-1). *«[Мы называем эту процедуру *изоляцией круга*.](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p5-1)»*\
Доп.: [`"Circle isolation thus reveals an *error correction* mechanism achieved via ensembling"`](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/original/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.md#p5-1) — *«[Тем самым изоляция круга вскрывает механизм *исправления ошибок*, достигаемого ансамблированием](../papers/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks/2306.17844.the-clock-and-the-pizza-two-stories-in-mechanistic-explanation-of-neural-networks.card.md#p5-1)»*.

###### ref-1-3
**\[1.3\]** 2302.03025 — Chughtai et al., «A Toy Model of Universality: Reverse Engineering how Networks Learn Group Operations». Нюанс: двусторонняя схема — исключить предсказанное алгоритмом и ограничиться им; исключение обоих ключевых представлений в логитах даёт потерю хуже случайной, а абляция предсказанного шумом качество улучшает. [`"to 7.60 excluding both, significantly worse than random. Ablating other directions improves performance."`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p6-9). *«[до 7.60 при исключении обоих — существенно хуже случайного. Абляция прочих направлений качество улучшает](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p6-9)»*\
Доп.: [`"ablating the components of weights and activations predicted by our algorithm destroys performance, while ablating parts we predict are noise does not affect loss, and often *improves* it"`](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p2-2) — *«[Абляция компонент весов и активаций, предсказанных нашим алгоритмом, разрушает качество, тогда как абляция частей, которые мы предсказываем шумом, на потерю не влияет и часто её *улучшает*](../papers/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations/2302.03025.a-toy-model-of-universality-reverse-engineering-how-networks-learn-group-operations.card.md#p2-2)»*.

###### ref-1-4
**\[1.4\]** 2312.06581 — Stander et al. 2024, «Grokking Group Multiplication with Cosets». Нюанс: наблюдения объявлены недостаточными до опыта, контур ломается нацеленно; замена ReLU на модуль даёт 100% точности и потерю 3.69e-13 против 1.97e-6. [`"Neural circuits are complex enough that observational evidence is not enough."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p7-2). *«[Нейронные контуры достаточно сложны, чтобы наблюдательных свидетельств было недостаточно](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p7-2)»*\
Доп.: [`"The property that the loss goes down when we replace the ReLU activation function with absolute value is a very strange property that GCR does not predict."`](../papers/2312.06581.grokking-group-multiplication-with-cosets/original/2312.06581.grokking-group-multiplication-with-cosets.md#p9-4) — *«[То, что потеря снижается, когда мы заменяем функцию активации ReLU модулем, есть весьма странное свойство, которого GCR не предсказывает](../papers/2312.06581.grokking-group-multiplication-with-cosets/2312.06581.grokking-group-multiplication-with-cosets.card.md#p9-4)»*.

###### ref-1-5
**\[1.5\]** 2407.20199 — Mallinar et al., «Emergence in non-neural models: grokking modular arithmetic via average gradient outer product». Нюанс: приём обратный абляции — на вход подставляется случайная циркулянтная матрица, и отсрочка исчезает и у обычного ядра, и у обычной сети; тем самым показано, что отсрочка есть время добывания признака. [`"a transformation with a *generic* block-circulant matrix enables kernels machines to learn modular arithmetic"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p9-4). *«[преобразование *типичной* блочно-циркулянтной матрицей позволяет ядерным машинам выучивать модульную арифметику](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p9-4)»*\
Доп. (сетевое плечо того же вмешательства): [`"networks trained on transformed integers achieved $100\%$ test accuracy within several hundred epochs and exhibit little delayed generalization while networks trained on non-transformed integers do not achieve $100\%$ test accuracy even within $3000$ epochs"`](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/original/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.md#p12-1) — *«[сети, обучаемые на преобразованных числах, достигали $100\%$ тестовой точности за несколько сотен эпох и почти не обнаруживают отложенной генерализации, тогда как сети, обучаемые на непреобразованных числах, не достигают $100\%$ тестовой точности даже за $3000$ эпох](../papers/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product/2407.20199.emergence-in-non-neural-models-grokking-modular-arithmetic-via-average-gradient-outer-product.card.md#p12-1)»*.

###### ref-1-6
**\[1.6\]** 2602.18523 — Xu, «The Geometry of Multi-Task Grokking: Transverse Instability, Superposition, and Weight Decay Phase Structure». Нюанс: удаление ортогональной составляющей градиента дозируется от 1 % до 50 %, задержка растёт до +203 %, а при 10 % гроккинг обрывается — обрыв воспроизведён и при двух, и при трёх задачах, при разном числе параметров и разных временны́х масштабах. Контроль на случайную проекцию той же силы поставлен и даёт ноль. Ограничение авторы называют сами: одно семя (42) и одно значение weight decay (1.0). [`"At 10% deletion and above, grokking fails entirely within 60k steps—a sharp cliff between 7% and 10%."`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p15-1). *«[При 10 % удаления и выше гроккинг проваливается целиком в пределах 60 тыс. шагов — резкий обрыв между 7 % и 10 %](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p15-1)»*\
Доп. (контроль на случайную проекцию): [`"PCA projection at strength $s=0.25$ delays grokking by 70–140% ($13.9$k $\to$ $23.7$k steps at seed 42), while random projection at the same strength has no effect"`](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/original/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.md#p14-3) — *«[Проецирование PCA при силе $s=0.25$ задерживает гроккинг на 70–140 % ($13.9$ тыс. $\to$ $23.7$ тыс. шагов при семени 42), тогда как случайное проецирование той же силы не даёт ничего](../papers/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure/2602.18523.the-geometry-of-multi-task-grokking-transverse-instability-superposition-and-weight-decay-phase-structure.card.md#p14-3)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2402.16726 — Furuta et al., «Towards Empirical Interpretation of Internal Circuits and Properties in Grokked Transformers on Modular Polynomials». Оспаривает: абляция выявляет пределы контуров, а не их универсальность. [`"the ablation study with pre-grokked models reveals that the transferability of grokked embeddings and models is limited to specific combinations"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-3). *«[абляционное исследование с предгрокнутыми моделями обнаруживает, что переносимость грокнувших вложений и моделей ограничена отдельными сочетаниями](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.card.md#p1-3)»*\
Доп. (интерференция): [`"others may interfere with each other, not reaching optimal solutions"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-3) — *«[другие могут мешать друг другу, не достигая оптимальных решений](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.card.md#p1-3)»*.

### Поддерживают

###### ref-3-1
**\[3.1\]** 2408.08944 — Clauw, Stramaglia, Marinazzo 2024, «Information-Theoretic Progress Measures reveal Grokking is an Emergent Phase Transition». Нюанс: абляция подтверждает каузальную роль подсетей эмерджентной фазы. [`"sub-networks at the emergent phase may be causally related to delayed generalization"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p1-7). *«[подсети на поре возникновения могут быть причинно связаны с отложенной генерализацией](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.card.md#p1-7)»*\
Доп. (насколько это показано): [`"the contrast between the synergistic sub-networks and its inverse is not that large"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p4-9) — *«[различие между соработающей подсетью и обратной к ней не столь велико](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.card.md#p4-9)»*.

###### ref-3-2
**\[3.2\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». Нюанс: интервенция на градиентный поток задаёт необходимость. [`"Causal intervention experiments establish that orthogonal gradient flow is necessary but not sufficient for grokking: suppressing it prevents generalization with a monotonic dose–response across four operations"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2). *«[Опыты с причинным вмешательством устанавливают, что ортогональный поток градиента необходим, но не достаточен для гроккинга: его подавление предотвращает генерализацию с монотонным откликом на дозу по четырём операциям](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p1-2)»*

###### ref-3-3
**\[3.3\]** 2603.05228 — Yildirim, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Нюанс: абляция подтверждает достаточность малого набора частот. [`"causal ablation retaining only the top 5 frequencies preserves >99.75% accuracy across all bounded sphere seeds"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p11-2). *«[причинное упрощение, оставляющее лишь пять главных частот, сохраняет $>$99,75 % точности на всех сидах ограниченной сферы](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p11-2)»*

###### ref-3-4
**\[3.4\]** 2603.29262 — Zhang et al., «Grokking: From Abstraction to Intelligence». Нюанс: каузальный медиационный анализ локализует необходимые головы внимания. [`"Causal Mediation Analysis (CMA). This framework allows us to identify which attention heads are necessary for the modular arithmetic operations"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p3-2). *«[причинный разбор посредничества (CMA). Эта рамка позволяет указать, какие головы внимания необходимы для действий модульной арифметики](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p3-2)»*\
Доп. (порядок подстановки): [`"patching is applied at the answer-token position (the final token where the model predicts $y$) and is evaluated by the induced logit-difference improvement for the correct answer"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p3-6) — *«[подстановка прилагается в положении знака ответа (последний знак, где модель предсказывает $y$) и оценивается по вызванному улучшению разности логитов для верного ответа](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p3-6)»*

###### ref-3-5
**\[3.5\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation: An Interventional and Predictive Framework for Grokking». Нюанс: интервенционное свидетельство каузальной роли энтропии представлений, с оговоркой о неполноте каузальности. [`"We provide interventional evidence via a representation-mixing probe, with an extended norm-matched control ($n=30$) disentangling the roles of parameter norm and representation entropy"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p2-3). *«[Мы приводим свидетельство вмешательством через щуп с перемешиванием представлений, с расширенной сверкой при уравненной норме ($n=30$), разделяющей роли нормы параметров и энтропии представлений](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p2-3)»*



###### ref-3-7
**\[3.7\]** 2408.11804 — Yunis et al., «Approaching Deep Learning through the Spectral Dynamics of Weights». Единственное вмешательство работы поставлено чисто: случайные нормальные возмущения весов растущей силы разрушают линейную связность мод и совпадение верхних сингулярных векторов одновременно (выходной слой и входной слой трансформера намеренно не возмущаются). Нюанс: вмешательство касается связности, а не гроккинга — на модульном сложении ни одного вмешательства не поставлено. [`"In Figure 13, when increasingly large random perturbations are applied, the barrier between final checkpoints increases and the LMC behavior disappears."`](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/original/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.md#p13-2). *«[На рисунке 13 при всё бо́льших случайных возмущениях барьер между итоговыми контрольными точками растёт, и поведение LMC исчезает](../papers/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights/2408.11804.approaching-deep-learning-through-the-spectral-dynamics-of-weights.card.md#p13-2)»*

###### ref-3-8
**\[3.8\]** 2602.16967 — Xu, «Early-Warning Signals of Grokking via
Loss-Landscape Geometry». Нюанс: схема 1A-kick / 1A-noise / 1B-project /
1B-penalty перенесена без изменений, но достаточность разошлась по задачам —
на модульной арифметике усиление не даёт ничего, на SCAN мягкое ускоряет
(${\sim}32\%$), а жёсткое расшатывает, на Dyck ускоряют оба (${\sim}50\%$ и
${\sim}40\%$). Числа прогонов на условие не указаны, величины силы
воздействия не названы. [`"The three tasks form a *spectrum* of causal sensitivity to curvature interventions"`](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/original/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.md#p13-4). *«[Три задачи образуют *спектр* причинной чувствительности к вмешательствам в кривизну](../papers/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry/2602.16967.early-warning-signals-of-grokking-via-loss-landscape-geometry.card.md#p13-4)»*

###### ref-3-9
**\[3.9\]** 2604.13082 — Gomezjurado Gonzalez, «The Long Delay to Arithmetic Generalization…». [`"projecting out the rank-$15$ probe subspace for $n\bmod 16$ at layer $4$ drops accuracy to $0.043$, a fall of $0.802$, whereas projecting out a random subspace of the same rank costs $0.005$"`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p8-1). *«[проекция вон подпространства зонда ранга $15$ для $n\bmod 16$ на слое $4$ роняет точность до $0.043$, то есть на $0.802$, тогда как проекция вон случайного подпространства того же ранга стоит $0.005$](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p8-1)»*\
Доп. (иерархия при сходимости): [`"Only magnitude is load-bearing, at $0.141$"`](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/original/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.md#p28-4) — *«[Несущей является одна лишь величина, на $0.141$](../papers/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior/2604.13082.the-long-delay-to-arithmetic-generalization-when-learned-representations-outrun-behavior.card.md#p28-4)»*, тогда как все подпространства остатков избыточны (просадка $\leq 0.001$ при зонде $\geq 0.99$).

###### ref-3-10
**\[3.10\]** 2604.20923 — Golwala, «ILDR: Geometric Early Detection of Grokking». [`"This is consistent with ILDR tracking a condition that underlies the transition, rather than merely being correlated with it."`](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/original/2604.20923.ildr-geometric-early-detection-of-grokking.md#p12-3). *«[Это согласуется с тем, что ILDR отслеживает условие, лежащее в основе перехода, а не просто с ним коррелирует.](../papers/2604.20923.ildr-geometric-early-detection-of-grokking/2604.20923.ildr-geometric-early-detection-of-grokking.card.md#p12-3)»* Свидетельство хеджировано и в аннотации: `mechanistically suggestive evidence`.

###### ref-3-11
**\[3.11\]** 2605.08237 — Wang, Ying, Kanamori 2026, «Distributional Spectral Diagnostics for Localizing Grokking Transitions». Нюанс: возмущения меряют чувствительность, а не механизм; пулы малы — 4 и 5 прогонов на масштаб. [`"Monitoring does not directly imply beneficial control."`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p29-4). *«[Мониторинг не влечёт напрямую полезного управления.](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p29-4)»*\
Доп. (возмущения): [`"at scale $0.01$, mean short-horizon deviation is $0.090$ (high-RR) vs. $0.029$ (low-RR), giving high/low ratio $\approx 3.1\times$"`](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p7-1) — *«[на масштабе $0.01$ среднее краткосрочное отклонение составляет $0.090$ (высокая RR) против $0.029$ (низкая RR), что даёт отношение высокая/низкая $\approx 3.1\times$](../papers/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions/2605.08237.distributional-spectral-diagnostics-for-localizing-grokking-transitions.card.md#p7-1)»*

###### ref-3-12
**\[3.12\]** 2605.20441 — Verma 2026, «Weight Decay Regimes in Grokking Transformers: Cheap Online Diagnostics». Парный опыт с тремя группами и заранее заявленными гипотезами, разводящий два обычно смешиваемых объяснения: переинициализация 2 голов из 8 в пору пика $\sigma_{H}$ снижает этот пик, а согласованное обрезание нормы весов до той же медианы в ту же пору — нет. Нюанс: меняется только амплитуда фазы 2 — все три группы грокают со стопроцентной долей, эпоха гроккинга и конечная точность не сдвигаются, а перенос на другие архитектуры, задачи и значения $\lambda$ вне $\{0.015,0.05\}$ автором прямо не заявляется. [`"Group C (weight clipping) yields no significant peak $\sigma_{H}$ change relative to group A at either $\lambda$ (paired $d{=}-0.406$, $p_{t}{=}0.094$ pooled), localising the effect to head-pattern structure rather than weight norm."`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p11-4). *«[Группа C (обрезание весов) не даёт значимого изменения пикового $\sigma_{H}$ относительно группы A ни при одном $\lambda$ (парное $d{=}-0.406$, $p_{t}{=}0.094$ сводно), что локализует эффект в строении образцов голов, а не в норме весов.](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p11-4)»*\
Доп. (что вмешательство не меняет): [`"All three groups grok at $100\%$ rate"`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p11-2) — *«[Все три группы грокают с долей $100\%$](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p11-2)»*.

###### ref-3-13
**\[3.13\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Образец уклада, которому в корпусе почти нет равных: семя выводится только из базовой настройки, исключая вмешательство, так что инициализация, разбиение данных и доинтервенционная траектория у сверки и у всех зажимов общие. Четыре встречных объяснения названы вслух и закрыты каждое своей отрицательной сверкой: не проекция (рука $\rho=1.00$), не регуляризация (граница weight decay), не толчок, а удержание ($1.2\times$ против $7.3\times$), не межслойное строение (послойные доли дрейфуют как при свободном обучении). Нюанс: вмешательство сильное и непрерывное, а не тонкое возмущение, и авторы это оговаривают; связь с фурье-признаками вмешательством не проверяется и остаётся временно́й. [`"the random seed is derived from the base configuration only, *excluding* the intervention, so the control and every intervention share identical initialization, data split, and pre-intervention trajectory"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-10). *«[случайное семя выводится только из базовой настройки, *исключая* вмешательство, так что у сверки и у всякого вмешательства одинаковы инициализация, разбиение данных и доинтервенционная траектория](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p4-10)»*\
Доп. (толчок против удержания): [`"a one-shot global rescale to $\rho\,\|W\|_{c}$ that is then released leaves the grokking time nearly flat across $\rho\in[0.70,1.30]$ (median $T_{\mathrm{grok}}$ moves only $1.2\times$, staying within $\sim\!15\%$ of the free control), whereas a *sustained* hold of the same norm spans $7.3\times$ over the narrower range $\rho\in[0.90,1.15]$"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p7-2) — *«[одноразовое общее перемасштабирование до $\rho\,\|W\|_{c}$ с последующим отпусканием оставляет время гроккинга почти плоским на $\rho\in[0.70,1.30]$ (срединное $T_{\mathrm{grok}}$ сдвигается лишь в $1.2\times$, оставаясь в пределах $\sim\!15\%$ от свободной сверки), тогда как *непрерывное* удержание той же нормы охватывает $7.3\times$ на более узком промежутке $\rho\in[0.90,1.15]$](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p7-2)»*.

###### ref-3-14
**\[3.14\]** 2606.12966 — Sivasankar, «Circuit Synchronization Precedes Generalization: A Causal Precursor to Grokking». [`"we load the add_mod97_s42 checkpoint at step 1,000 (FSD $=0.84$, val acc $=10.6\%$) and fork into six independent branches"`](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p6-5). *«[мы загружаем контрольную точку add_mod97_s42 на шаге 1000 (FSD $=0.84$, проверочная точность $=10.6\%$) и разветвляем её на шесть независимых ветвей](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p6-5)»*\
Доп. (что именно объявлено причинно подтверждённым): [`"The strict monotone ordering for $\lambda\in\{1,2,3\}$ causally confirms Phase 2 is a regularisation phase"`](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p7-1) — *«[Строгий монотонный порядок при $\lambda\in\{1,2,3\}$ причинно подтверждает, что фаза 2 есть фаза регуляризации](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p7-1)»*.\
Доп. (для координатора): цитирующая работа `2607.04333` относит настоящую работу к **наблюдательным** объяснениям и прямо пишет, что ни одно из них не вмешивается в сроки, — то есть корпус уже прочёл заявку о причинности мягче, чем она сформулирована.

###### ref-3-15
**\[3.15\]** 2606.18465 — Truong 2026, «What Does the Weight Norm Control in Grokking? Logit-Scale Mediation under Cross-Entropy». Два образцовых вмешательства: трёхусловное температурное опосредование — необучаемая $\tau$ вне $\|W\|$ даёт независимую ручку функционально-пространственной переменной при физически зажатой норме — и ветвление четырёх рукавов из одного сохранённого состояния, где тождественная операция зажима при разных удерживаемых значениях даёт $3.3$–$3.4\times$ разницу задержки во всех 12 семенах обоих модулей, закрывая возражение об артефакте операции. Нюанс авторской аккуратности: доля возврата названа величиной уровня вмешательства, а не регрессионной оценкой естественного косвенного эффекта. [`"If the norm effect runs through the logit scale, setting $\tau$ so that the effective logit scale matches the baseline should recover the baseline delay"`](../papers/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy.card.md#p4-2). *«[Если действие нормы идёт через масштаб логитов, установка $\tau$ так, чтобы действенный масштаб логитов совпал с базовым, должна вернуть базовую задержку](../papers/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy.card.md#p4-2)»*\
Доп. (значение против операции): [`"Since the operation is identical in the two arms, the difference is not an artifact of the operation: the delay follows the held value"`](../papers/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy.card.md#p8-2). *«[Раз операция в двух рукавах тождественна, различие — не артефакт операции: задержка следует за удерживаемым значением](../papers/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy/2606.18465.what-does-the-weight-norm-control-in-grokking-logit-scale-mediation-under-cross-entropy.card.md#p8-2)»*
###### ref-3-16
**\[3.16\]** 2607.06628 — Truong Xuan Khanh 2026, «Cross-Trajectory Chimera Interventions Reveal Dissociable Roles of Weight Magnitude and Direction in Grokking». Три переносимых методических приёма для причинных вмешательств: сверка matched_random — случайное направление на точно том же угловом расстоянии, что и донорское (в высокой размерности случайное направление почти ортогонально, а два обученных прогона — нет, так что без уравнивания угла «подействовал донор» неотличимо от «подействовало возмущение такой величины»); адаптивное деление пополам, локализующее порог вмешательства до $\pm 1/64$ за ~три продолжения на пару; и абляция состояния оптимизатора — повтор при моментах Adam от принимающего и от донора вместо сброса. Нюанс: устойчивость повторных продолжений, на которой держится «одно продолжение на точку сетки», принята, а не измерена; объявленная сверка random_u нигде не отчитана; совместная вероятность $6.3\times 10^{-12}$ перемножает шесть разделений как независимые при трёх условиях на одних и тех же парах. [`"we repeat the experiment under three sources for the continuation optimizer’s moments—reset (freshly initialized, as reported above), recipient (transplant $A$’s moments), and donor (transplant $B$’s moments)—and ask whether the reported effect survives all three."`](../papers/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking.card.md#p8-1). *«[мы повторяем эксперимент при трёх источниках моментов оптимизатора продолжения — reset (свежеинициализированные, как сообщено выше), recipient (пересадка моментов $A$) и donor (пересадка моментов $B$) — и спрашиваем, выживает ли сообщённый эффект при всех трёх.](../papers/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking/2607.06628.cross-trajectory-chimera-interventions-reveal-dissociable-roles-of-weight-magnitude-and-direction-in-grokking.card.md#p8-1)»*

### Наследуют

###### ref-3-6
**\[3.6\]** 2405.12755 — Golechha 2024, «Progress Measures for Grokking on Real-world Tasks». Даёт корпусу приём, разводящий норму весов и генерализацию в разные стороны: добавленный к потере член с отрицательным знаком гонит норму вверх, а гроккинг всё равно наступает, — то есть проверка причинности вмешательством, а не очередное наблюдение совпадения кривых. Оговорка сделана верно: под AdamW этот член идёт через адаптивный градиент и к «weight decay с обратным знаком» не сводится. Нюанс: по трём собственным мерам вмешательства нет вовсе, и их статус остаётся ровно тем, в котором обвинена норма, — совпадение по времени. [`"We employ a clever trick to elicit grokking while the $L_{2}$ norm of the weights increases during generalization."`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p2-5). *«[Мы применяем ловкий приём, чтобы вызвать гроккинг при росте $L_{2}$-нормы весов в ходе генерализации.](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p2-5)»*\
Доп. (почему это не обратный weight decay): [`"Note that for the AdamW optimizer, this is implemented differently than weight decay."`](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/original/2405.12755.progress-measures-for-grokking-on-real-world-tasks.md#p3-1) — *«[Заметим, что для оптимизатора AdamW это реализуется иначе, чем weight decay.](../papers/2405.12755.progress-measures-for-grokking-on-real-world-tasks/2405.12755.progress-measures-for-grokking-on-real-world-tasks.card.md#p3-1)»*.
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2505.18266 — McCracken et al. 2025, «Uncovering a Universal Abstract Algorithm for Modular Addition in Neural Networks». Нюанс: отсечением подтверждено «действие фурье-умножения», позже названное часами. [`"This was termed the *Fourier Multiplication Algorithm* (later *Clock* [5]), and validated through ablation experiments"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p2-3). *«[Приём назвали *действием фурье-умножения* (позже *часами* [5]) и подтвердили опытами с отсечением](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.card.md#p2-3)»*
