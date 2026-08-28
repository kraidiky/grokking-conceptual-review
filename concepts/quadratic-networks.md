# Квадратичные сети (quadratic networks)

[Законы масштабирования](scaling-laws.md) ← предыдущая карточка, следующая → [Информационное узкое место](information-bottleneck.md)

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Квадратичные сети** — двухслойные сети с активацией $\sigma(x)=x^2$: *«мы обучаем двухслойную квадратичную сеть… где $d=2p$, а $\sigma(x)=x^{2}$ — квадратичная активация»* \[[1.2](#ref-1-2)\]. В корпусе [гроккинга](grokking.md) это не архитектура для практики, а разрешимая моделька: при квадратичной активации *«полная функция сети принимает ещё более простой вид»* \[[1.1](#ref-1-1)\] — многочлен третьей степени по параметрам, — и потому решение [модульного сложения](modular-arithmetic.md) удаётся выписать в замкнутом виде, а не восстановить обратной инженерией.

## Детализация

**Зачем они понадобились.** Всякое теоретическое утверждение о гроккинге упирается в нелинейность: у ReLU-сети нет замкнутых выражений, у бесконечно широкой — нет обучения признакам. Квадратичная активация — наименьшее отступление от линейности, при котором обучение признакам ещё есть, а выкладки ещё возможны. Отсюда её появление в трёх разных теоретических линиях сразу, с разными орудиями и одинаковой ролью подмостков.

**Первая линия: аналитическое решение.** Веса, вычисляющие сумму по модулю, выписываются формулами — косинусы с частотами $2\pi k/p$ и наборами фаз \[[1.1](#ref-1-1)\]. Это единственный в корпусе случай, когда обобщающее решение известно точно, а не разобрано постфактум; [фурье-признаки](fourier-features-circuits.md) здесь не находка, а следствие построения. Цена — выводы привязаны к MSE, двухслойному MLP и этой активации.

**Вторая линия: границы выборочной сложности.** Утверждение о том, что задача трудна в [ядровом режиме](neural-tangent-kernel-ntk.md), дополняется положительной половиной: *«двухслойные квадратичные сети, достигающие нулевой потери на обучении при ограниченной $\ell_{\infty}$-норме, хорошо генерализуют при существенно меньшем числе обучающих точек»*, и такие сети находимы градиентным спуском с малой $\ell_{\infty}$-регуляризацией \[[1.3](#ref-1-3)\]. Здесь квадратичность — условие теоремы, а не удобство изложения: именно она позволяет описать множество решений с нулевой ошибкой.

**Третья линия: геометрия ландшафта.** [Сингулярная теория обучения](singular-learning-theory.md) приложена к гроккингу тоже на квадратичных сетях — как *«взгляд на гроккинг как на отбор котловины»*, где местный коэффициент обучения упорядочивает соперничающие котловины с почти нулевой потерей \[[1.4](#ref-1-4)\]. Причина выбора та же: для квадратичной сети множество решений — известное алгебраическое многообразие, и вырожденность считается, а не оценивается наугад.

**Что даёт эта общность.** Три несвязанные рамки — точное решение, граница выборочной сложности, байесовская геометрия — сходятся на одной модельке, и потому их выводы сравнимы между собой. Это редкий случай в корпусе, где расхождение объяснений нельзя списать на разные постановки. Обратная сторона — общая слепая зона: всё, что зависит от глубины, softmax-потери или внимания, в этих результатах отсутствует по построению, и переносимость на трансформеры остаётся допущением, которое проверяется отдельно \[[1.3](#ref-1-3)\].

**Оговорка о том, чем они не являются.** Квадратичная сеть — не приближение практической архитектуры и не предложение её заменить. Утверждение «в квадратичной сети гроккинг устроен так-то» само по себе ничего не говорит о ReLU-сети; перенос требует отдельного довода, и в корпусе он обычно даётся эмпирически — воспроизведением того же явления на обычной архитектуре.

## Альтернативные определения и нюансы

### A. Разрешимая модель

Форма, в которой квадратичность нужна ради замкнутого решения \[[1.1](#ref-1-1)\]. Различающая черта — предмет получаемого знания: не оценка, а тождество; отсюда и особая роль в спорах — конструкция служит контрпримером, показывающим, что обобщающее решение существует и достижимо без [регуляризации](regularization-necessity.md).

### B. Класс, для которого доказуемы границы

Форма, где квадратичность — посылка теоремы о выборочной сложности \[[1.3](#ref-1-3)\]. Источник различия — утверждение не о конкретной сети, а обо всём классе решений с нулевой ошибкой и ограниченной нормой; следствие — результат переживает смену семени и инициализации, чего эмпирические наблюдения не гарантируют.

### C. Алгебраическое многообразие решений

Форма, где сеть берётся ради строения множества её решений \[[1.4](#ref-1-4)\]. Различающая черта — предметом становится не траектория, а геометрия: вырожденность решения выражается через размерность секущего многообразия, и это единственная из трёх форм, где сравниваются котловины, а не решения.

### D. Отношение к прочим разрешимым постановкам

Квадратичная сеть — один из нескольких упрощённых носителей гроккинга наряду с [моделями вне нейросетей](grokking-in-non-neural-models.md); отличие в том, что признаки здесь всё же выучиваются, а не заданы. Именно поэтому конструкция \[[1.1](#ref-1-1)\] служит опорой и для более поздних работ, где она оказывается частным случаем более общей схемы \[[3.1](#ref-3-1)\], а постановка опыта нарочно очищается от постоянного смещения, *«вынуждая модель выучить строение задачи»* \[[3.2](#ref-3-2)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2301.02679 — Gromov, «Grokking modular arithmetic». [`"In passing, we note that, in the case of quadratic activation the full network function takes an even simpler form"`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p4-3). *«[Мимоходом отметим, что в случае квадратичной активации полная функция сети принимает ещё более простой вид](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p4-3)»*

###### ref-1-2
**\[1.2\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». [`"We train a 2-layer quadratic network $f_{\theta}$ with parameters $\theta=(W,V)$, hidden width $K$, and no bias terms"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p4-1). *«[Мы обучаем двухслойную квадратичную сеть $f_{\theta}$ с параметрами $\theta=(W,V)$, шириной скрытого слоя $K$ и без свободных членов](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p4-1)»*

###### ref-1-3
**\[1.3\]** 2407.12332 — Mohamadi et al., «Why Do You Grok? A Theoretical Analysis of Grokking Modular Addition». [`"two-layer quadratic networks that achieve zero training loss with bounded $\ell_{\infty}$ norm generalize well with substantially fewer training points"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-2). *«[двухслойные квадратичные сети, достигающие нулевой потери на обучении при ограниченной $\ell_{\infty}$-норме, хорошо генерализуют при существенно меньшем числе обучающих точек](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p1-2)»*

###### ref-1-4
**\[1.4\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». [`"we develop a basin-selection perspective on grokking in quadratic networks: LLC ranks competing near-zero-loss basins by statistical preference"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2). *«[мы развиваем взгляд на гроккинг как на отбор котловины в квадратичных сетях: LLC упорядочивает соперничающие котловины с почти нулевой потерей по статистическому предпочтению](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2311.07568 — Morwani et al., «Feature emergence via margin maximization: case studies in algebraic tasks». Нюанс: аналитическое построение оказывается схемой, частным случаем которой становятся более поздние доказательства. [`"Gromov 2023 provides an analytic construction of various two-layer quadratic networks that can solve the modular addition task."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p4-1). *«[Gromov 2023 даёт аналитическое построение различных двухслойных квадратичных сетей, способных решать задачу модульного сложения.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p4-1)»*

###### ref-3-2
**\[3.2\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». Нюанс: постановка нарочно очищена от тривиального пути к малой потере, чтобы обобщение шло только через выучивание признаков. [`"This projection eliminates trivial constant-bias fitting, forcing the model to learn the task’s structure"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p4-3). *«[Этот проектор устраняет пустое приближение постоянным смещением, вынуждая модель выучить строение задачи](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p4-3)»*
