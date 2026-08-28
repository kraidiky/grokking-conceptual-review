# Глобальный индекс понятий грокинга (Global Grokking Concept Index)

Построено инверсией по-статейных экстракций в индекс «понятие -> статьи» по 66 статьям arXiv о грокинге (исходно — по txt-экстракциям репозитория, из которого вики выделена; они здесь не хранятся). Каждая ссылка quote-anchored: точный английский фрагмент оригинала, связанный с хранимым текстом статьи в `papers/<папка>/original/` — якорем абзаца, а для заголовков и формул — ссылкой на файл. Синонимичные ярлыки (напр. frequency-principle / spectral-bias; lazy-to-rich / kernel-to-feature-learning; anti-grokking / generalization-collapse; weight-norm-minimization / zero-loss-manifold) сведены к одному каноническому понятию, слитые алиасы указаны в скобках.

Формат ссылки: `arxivid: ["verbatim English quote"](../papers/<папка>/original/<файл>.md#якорь)` — цитаты приведены дословно (verbatim), на английском, в сериализации хранимого оригинала; не изменяйте их. Дословность проверяет quote_check.py.

###### cat-1
## 1. Явления (Phenomena)

### [Грокинг / отложенная генерализация](grokking.md) (grokking / delayed generalization) — 162 статей

Феномен отложенной генерализации: сеть сначала почти идеально подгоняет обучающую выборку при низкой тестовой точности, а спустя на порядки более долгое обучение тестовая точность резко возрастает. Термин и явление введены Power et al. (2022).

### [Фаза меморизации / плато](memorization-phase.md) (memorization phase / plateau) — 80 статей

Начальный этап обучения при гроккинге: почти идеальная точность на обучающей выборке при низкой тестовой — затяжное плато, предшествующее резкому переходу к генерализации.

### [Фазовый переход](phase-transition.md) (phase transition) — 66 статей

Трактовка отложенной генерализации как резкой смены режима по образцу фазовых переходов в физике: долгое низкое плато тестовой точности и внезапный скачок.

### [Эффект рогатки](slingshot.md) (slingshot effect / mechanism) — 30 статей

Оптимизационная аномалия адаптивных оптимизаторов на поздних стадиях обучения: циклы из быстрого роста нормы весов последнего слоя, всплеска обучающей потери и полки нормы. Часто сопутствует гроккингу без явной регуляризации.

### [Эмерджентность / эмерджентные способности](emergence.md) (emergence / emergent abilities) — 29 статей

Резкое появление качественно новых способностей при пересечении критического порога масштаба — числа параметров, данных или вычислений. В работах о гроккинге — та же скачкообразность, но управляемая временем обучения.


### [Двойной спуск](double-descent.md) (double descent) — 27 статей

Немонотонность тестовой ошибки: спад, подъём к пику вблизи точки интерполяции и второй спад в переобученной области. С гроккингом делит вопрос, почему переобучение оказывается не финалом, а промежуточной стадией.


### [Коллапс softmax](softmax-collapse.md) (Softmax Collapse) — 11 статей

Численная нестабильность: без регуляризации обучение выталкивает модель на край численной стабильности, ошибки плавающей точки в softmax останавливают обучение и препятствуют гроккингу. Введено Prieto et al. (2025).

### [Анти-грокинг / коллапс генерализации](anti-grokking.md) (anti-grokking / generalization collapse) — 9 статей
Слитые алиасы: anti-grokking / generalization-collapse.

Третья фаза после гроккинга: при очень долгом продолжении обучения тестовая точность обрушивается обратно к низкой, тогда как обучающая остаётся насыщенной. Введено Prakash & Martin (2025).

### [Катастрофическое забывание](catastrophic-forgetting.md) (catastrophic forgetting) — 9 статей

Резкая утрата ранее усвоенного при продолжении обучения на новых данных или изменённом распределении; в контексте гроккинга — объяснение потери уже достигнутого обобщающего решения.

### [Унгрокинг](ungrokking.md) (ungrokking) — 9 статей

Переход, обратный гроккингу: сеть, уже достигшая идеальной тестовой точности, при дальнейшем обучении регрессирует к низкой — от генерализации назад к запоминанию. Предсказан теорией эффективности контуров (Varma et al., 2023).


### [Полу-грокинг](semi-grokking.md) (semi-grokking) — 6 статей

Отложенная генерализация лишь до частичной тестовой точности: сеть грокает, но выходит на неполное качество. Открыт Varma et al. (2023) как следствие той же теории эффективности контуров.

### [Поведенческий против истинного грокинга](behavioral-vs-true-grokking.md) (behavioral vs true grokking) — 4 статей

Что засчитывать за гроккинг: скачок точности или скачок вместе со складыванием обобщающего механизма. Различение измеримо — перестройка схемы продолжается тысячи шагов после того, как кривая уже засчитала переход.

### [Фазы: понимание, гроккинг, запоминание, путаница](comprehension-confusion-phases.md) (comprehension / grokking / memorization / confusion) — 3 статей

Разметка плоскости настроек на четыре области по соотношению сроков выхода на порог по двум кривым. Не путать с трёхчастной разметкой времени обучения: там фазы сменяются по ходу прогона, здесь это области в пространстве настроек.

###### cat-2
### [Наивная минимизация потерь](nlm.md) (Naive Loss Minimization) — 2 статей

Компонента градиента, снижающая кросс-энтропию простым масштабированием логитов вверх без изменения предсказаний; за точкой переобучения градиент почти целиком уходит в неё, откладывая генерализацию.

## 2. Механизмы и представления (Mechanisms & representations)

### [Обучение структурированных представлений](structured-representation-learning.md) (structured representation learning) — 53 статей

Объяснение гроккинга: генерализация возникает из постепенного формирования структурированных внутренних представлений, кодирующих устройство задачи, а не из самого запоминания примеров.

### [Фурье-признаки и контуры](fourier-features-circuits.md) (Fourier features / circuits) — 49 статей

Периодические представления входов на немногих ключевых частотах и подсети, складывающие числа через тригонометрические тождества, — язык описания решений модульной арифметики, идущий от разбора Nanda et al.


### [Возникновение признаков](feature-emergence-feature-learning.md) (feature emergence / feature learning) — 38 статей

Появление в ходе обучения признаков, кодирующих устройство задачи; генерализация совпадает с возникновением структуры в эмбеддингах, нарастающей всё время плато.

### [Ландшафт потерь / бассейны](loss-landscape-basins.md) (loss landscape / basins) — 32 статей

Геометрия функции потерь над пространством весов: отложенная генерализация описывается как перемещение оптимизационной траектории между бассейнами — из запоминающего минимума в обобщающий.


### [Переход lazy→rich](lazy-to-rich-kernel-to-feature-learning.md) (lazy-to-rich / kernel-to-feature-learning transition) — 31 статей
Слитые алиасы: lazy-to-rich / kernel-to-feature-learning / lazy-learning-stage / lazy-rich-regime-transition.

Трактовка гроккинга как смены режимов: сначала «ленивое» обучение около инициализации (ядерная регрессия с NTK, запоминание), затем переход в «богатый» режим обучения признаков, дающий генерализацию.

### [Эффективность контуров](circuit-efficiency.md) (circuit efficiency) — 28 статей

Свойство контура выдавать нужные логиты при меньшей норме параметров; когда несколько контуров одинаково решают обучающую выборку, weight decay отбирает более эффективный. Основа объяснений гроккинга, унгрокинга и полу-грокинга у Varma et al.


### [Сжатие многообразия представлений](manifold-representation-compression.md) (manifold / representation compression) — 25 статей

Трактовка, в которой генерализация наступает, когда представления или траектория весов сжимаются из раздутой запоминающей конфигурации в компактное низкоразмерное многообразие.

### [Разрежённая подсеть / lottery ticket](sparse-subnetwork-lottery-ticket.md) (sparse subnetwork / lottery ticket) — 24 статей

Связывает гроккинг с выделением внутри плотной сети малой разрежённой подсети, реализующей обобщающий алгоритм и вытесняющей плотную запоминающую.

### [Нейронное касательное ядро](neural-tangent-kernel-ntk.md) (neural tangent kernel, NTK) — 20 статей
Слитые алиасы: neural-tangent-kernel / NTK-task-kernel-alignment.

Ядро, к которому сводится обучение сети в ленивом (линеаризованном) режиме; в теории гроккинга задаёт начальную запоминающую фазу, из которой сеть выходит при переходе к обучению признаков.

### [Маршрутизация внимания](attention-routing-heads.md) (attention routing / heads) — 18 статей

Перераспределение информации между позициями последовательности головами внимания через обучаемые взаимодействия «запрос-ключ»; в разборах гроккинга — место, где читается выученный алгоритм.

### [Групповые представления и cosets](group-representations-cosets.md) (group representations / cosets) — 14 статей

Язык теории представлений групп для описания найденного сетью решения: после гроккинга сеть реализует структуру, диктуемую представлениями группы, а не произвольную подгонку.

### [Нейронный коллапс](neural-collapse.md) (neural collapse) — 13 статей

Структурное явление конечной фазы обучения: представления классов в предпоследнем слое стягиваются к классовым средним, выстроенным в симплексный равноугольный фрейм (ETF); в корпусе — линза для поздних стадий гроккинга.

### [Алгоритмы Clock и Pizza](clock-vs-pizza.md) (Clock vs Pizza) — 9 статей

Два качественно различных внутренних контура, к которым сеть сходится на модульном сложении, — «Часы» и «Пицца»; различие введено Zhong et al. как свидетельство неединственности выученного алгоритма.

### [Частотный принцип / спектральное смещение](frequency-principle-spectral-bias.md) (frequency principle / spectral bias) — 7 статей
Слитые алиасы: frequency-principle / spectral-bias / F-Principle / frequency-perspective.

Неявное индуктивное смещение нейросетей подгонять целевую функцию «от низких частот к высоким»: медленно меняющиеся компоненты выучиваются первыми, высокочастотные детали — существенно позже.

### [Ортогональность градиента](orthogonal-gradient-perp-grad.md) (orthogonal gradient / perp-Grad) — 7 статей
Слитые алиасы: gradient-orthogonality / orthogonal-gradient-flow / perp-Grad.

Компонента градиента, ортогональная направлению весов или многообразию нулевой потери, — та часть обновлений, что действительно меняет предсказания; одноимённый приём (perp-Grad) оставляет только её.

### [Контур генерализации](generalization-circuit.md) (generalization circuit) — 4 статей

Часть сети, реализующая обобщающее решение, против запоминающего контура. Различение операционально через пару мер прогресса, а граница между контурами получает число — критический размер данных.

### [Компромисс сложность–ошибка](model-complexity-error-tradeoff.md) (model complexity / error tradeoff) — 4 статей

Размен между сложностью выученной функции и ошибкой: сеть добивается нулевой ошибки сложным решением, а затем переходит к простому с той же ошибкой. Слабое место рамки названо в ней самой — единой меры сложности нет.

### [Разрежённые решения и скрытый прогресс](sparse-solutions-hidden-progress.md) (sparse solutions / hidden progress) — 4 статей

Монотонное продвижение внутри сети под внешним плато и разрежённость решения, к которому оно ведёт. Отсюда идея скрытых мер прогресса: величина, растущая во время плато и предсказывающая скачок.

### [Разрежённость](activation-sparsity.md) (activation and structural sparsity) — 3 статей

Три разных наблюдения под одним именем: разрежённость в фурье-области, в структуре подсети и в отклике. Проверка гипотезы устроена честно — модели с той же разрежённостью строят отдельно.

###### cat-3
### [Многообразие исполнения](execution-manifold.md) (execution manifold) — 3 статей

Малоразмерная поверхность в пространстве весов, на которой держится траектория обучения: первая главная компонента забирает 68–83% дисперсии. Задержка — удержание на ней, обобщение — выход.

### [Полисемантичность и суперпозиция](polysemanticity-superposition.md) (polysemanticity / superposition) — 3 статей

Нейрон, откликающийся на несколько несвязанных признаков, и объяснение этому через наложение большего числа признаков, чем есть направлений. Затрагивает все инструменты разбора грокнувших сетей, которые предполагают одно понятие на направление.

## 3. Задачи и наборы данных (Tasks & datasets)

### [Модульная арифметика](modular-arithmetic.md) (modular arithmetic) — 78 статей

Класс задач вида (a ∘ b) mod p, на котором гроккинг впервые наблюдали и который стал его каноническим полигоном; деление требует простого модуля — оно задаётся обращением умножения и однозначно только при простом p.


### [Эталонный полигон гроккинга](canonical-grokking-testbed.md) (canonical grokking testbed / Power et al. setup) — 51 статей

Экспериментальная постановка Power et al. (2022) — таблицы бинарных операций, малый трансформер, доля данных как управляющий параметр, — унаследованная большинством работ корпуса как стандартная площадка.

### [Реальные данные и зрение](vision-real-world-data-mnist-cifar.md) (vision / real-world data: MNIST, CIFAR, ...) — 33 статей

Собирательное понятие для наблюдений гроккинга за пределами синтетических алгоритмических задач — на классификации изображений (MNIST, CIFAR, ImageNet), тексте и молекулах.

### [Разрежённая чётность](sparse-parity.md) (sparse parity) — 22 статей

Синтетическая задача бинарной классификации: метка — чётность (XOR) k скрытых битов из фиксированного секретного подмножества n-битной строки при k много меньше n; один из канонических полигонов гроккинга.

### [Шум в метках / случайные метки](label-noise-random-labels.md) (label noise / random labels; у Power et al. — outliers) — 20 статей

Намеренная замена истинных обучающих меток случайными — у части примеров или у всех; введена в корпус Power et al. как «выбросы», числом которых измеряется влияние шума на наступление генерализации.

### [Композиция групп / некоммутативность](group-composition-non-commutative-s5.md) (group composition, non-commutative, S5) — 16 статей

Задачи предсказания результата групповой операции по паре элементов; ключевой водораздел — коммутативность: некоммутативные группы вроде S5 ведут себя иначе, чем модульная арифметика.

### [Рассуждение и графы знаний](reasoning-knowledge-graphs.md) (reasoning / knowledge graphs) — 5 статей

Установленная Wang et al. (2024) связь гроккинга со способностью трансформера к неявному рассуждению над параметрическим знанием, представленным графом знаний: вывод новых фактов без явных промежуточных шагов.

### [Гроккинг вне нейросетей](grokking-in-non-neural-models.md) (grokking in non-neural and solvable models) — 4 статей

Отложенная генерализация в моделях, которые не являются глубокими сетями или являются линейными и потому решаются аналитически: гребневая и логистическая регрессия, линейный «учитель — ученик», модель Изинга. Ценность — строгие определения запоминающего и обобщающего решений.

### [Сдвиг распределения](distribution-shift.md) (distribution shift) — 2 статей

Расхождение между обучающей и проверочной раздачей: и объяснение задержки (разрежённость данных порождает сдвиг), и условие, в котором гроккнувшую модель проверяют на прочность.

### [Систематическая генерализация](systematic-generalization.md) (systematic / hierarchical generalization) — 2 статей

Обобщение на структурно новые входы, где обучающую выборку одинаково объясняют два правила, а проверочная различает их. Отсюда структурный гроккинг и его немонотонная зависимость от глубины.

###### cat-4
## 4. Факторы обучения и оптимизации (Training / optimization factors)

### [Weight decay / L2-регуляризация](weight-decay.md) (weight decay) — 100 статей

Штраф, пропорциональный квадрату L2-нормы весов (эквивалент покомпонентного затухания весов на каждом шаге); главный регуляризатор корпуса — с ним связаны и скорость, и само наступление гроккинга в большинстве постановок.


### [Доля данных / критический размер набора](data-fraction-critical-dataset-size.md) (data fraction / critical dataset size) — 63 статей

Порог объёма обучающей выборки, разделяющий два режима: выше него сеть в конце концов обобщает, ниже — остаётся на запоминании; удобно выражается долей всех допустимых примеров задачи.

### [Оптимизатор](optimizer-adam-adamw-sgd.md) (optimizer: Adam / AdamW / SGD) — 54 статей

Алгоритм обновления весов по градиентам; в корпусе выбор оптимизатора — содержательный фактор: уже Power et al. показали, что детали оптимизации влияют на само наступление отложенной генерализации.

### [Минимизация нормы весов](weight-norm-minimization.md) (weight-norm minimization / zero-loss manifold) — 45 статей
Слитые алиасы: weight-norm minimization / norm minimization on the zero-loss manifold.

Трактовка гроккинга как движения по многообразию нулевой потери к решению с меньшей нормой весов: после полного запоминания обучение медленно сползает к обобщающей конфигурации минимальной нормы.

### [Масштаб инициализации](initialization-scale.md) (initialization scale) — 41 статей

Начальная норма весов сети, задаваемая множителем к стандартной схеме; Omnigrok установил, что большой масштаб инициализации вызывает гроккинг, а малый его устраняет.

### [Когда регуляризация необходима](regularization-necessity.md) (regularization necessity) — 33 статей

Открытый спор корпуса: является ли регуляризация необходимым условием гроккинга; собраны прямо противоположные экспериментальные ответы, каждый честно полученный в своей постановке.

### [Скорость обучения](learning-rate.md) (learning rate) — 30 статей

Масштаб шага обновления весов; один из управляющих параметров — наряду с weight decay, размером батча и долей данных, — от которых зависит, наступит ли отложенная генерализация и как быстро.

### [Варианты регуляризации](regularization-variants.md) (regularization variants) — 26 статей

Семейство приёмов регуляризации, отличных от канонического weight decay, которые также вызывают или ускоряют отложенный переход к генерализации.

### [Зона Златовласки](goldilocks-zone.md) (Goldilocks zone) — 22 статей

Узкая, «в самый раз» подобранная область параметров, внутри которой — и только внутри которой — происходит содержательное обучение представлений; введена Liu et al. (2022) применительно к гроккингу.

### [Роль шума градиента](gradient-noise.md) (gradient noise / full-batch training) — 19 статей

Вопрос, нужен ли гроккингу стохастический шум оптимизации (минибатчевый шум SGD, впрыснутый гауссов, «температура» lr/B); линия корпуса, где шумовая гипотеза Power et al. и полнобатчевые эксперименты натянуты друг против друга.

### [Край устойчивости](edge-of-stability.md) (edge of stability) — 11 статей

Режим градиентного спуска, при котором резкость ландшафта (максимальное собственное значение гессиана) в ходе прогрессирующего заострения дорастает до порога устойчивости 2/η и удерживается около него.

### [Направление влияния weight decay](weight-decay-direction.md) (weight decay direction / 1/gamma law) — 9 статей

Вопрос, в какую сторону и по какому закону сила weight decay сдвигает гроккинг, — одна из немногих точек корпуса, где эмпирические свидетельства формально противоречат друг другу.

### [Переопараметризация и глубина](overparameterization-depth.md) (overparameterization / depth) — 5 статей

Избыток параметров относительно данных и число слоёв как управляющие величины. Глубина действует немонотонно: провал на средней глубине лечится не слоями, а стабилизацией; переопараметризация — обычное условие опытов, а не необходимая часть явления.

###### cat-5
## 5. Интервенции и методы (Interventions & methods)

### [Grokfast / фильтрация градиента](gradient-low-pass-filtering.md) (gradient low-pass filtering) — 13 статей

Приём ускорения гроккинга: последовательность градиентов каждого параметра рассматривается как временной сигнал, и его медленная (низкочастотная) составляющая, связываемая с генерализацией, усиливается.

### [StableMax / perp-Grad](numerical-stability-fix.md) (numerical-stability fix) — 13 статей

Вмешательства Prieto et al. (2025), устраняющие коллапс softmax или порождающую его компоненту градиента: StableMax и perp-Grad включают гроккинг без регуляризации.

### [Замораживание подсети / edge-popup](freezing-subnetwork.md) (freezing subnetwork) — 11 статей

Веса сети фиксируются, оптимизируется только структура — какие связи оставить активными; генерализация достигается нахождением хорошей подсети внутри уже имеющейся сети (edge-popup).

### [Сферическое ограничение нормы](spherical-weight-norm-constraint.md) (spherical weight-norm constraint) — 11 статей

Интервенция, удерживающая L2-норму весов или активаций фиксированной: параметры принудительно кладутся на сферу, масштабная степень свободы устраняется, и информация кодируется только направлением.

### [Ускорение гроккинга](accelerated-grokking.md) (accelerated grokking) — 4 статей

Семейство вмешательств, сокращающих задержку: в градиент, в начальное вложение, в параметры по ходу, в норму. Общая слабость — выигрыш меряется в шагах, а не в секундах, и часто на одном семени.

###### cat-6
## 6. Аналитические инструменты и метрики (Analytical tools & metrics)

### [Меры прогресса](progress-measures.md) (progress measures) — 66 статей

Непрерывные метрики внутреннего состояния сети, которые предшествуют резкому скачку способности и причинно с ним связаны, — способ увидеть за внешней внезапностью гроккинга постепенный скрытый процесс (Nanda et al., 2023).

### [Механистическая интерпретируемость](mechanistic-interpretability.md) (mechanistic interpretability) — 48 статей

Обратная разработка выученного поведения: восстановление из весов и активаций человекочитаемого алгоритма, который сеть фактически исполняет; опорный метод корпуса после разбора модульного сложения Nanda et al.

### [Спектральный анализ / FVE](spectral-analysis-svd-esd-fve.md) (spectral analysis, SVD, ESD, FVE) — 39 статей

Семейство диагностик по спектрам внутренних матриц сети (SVD, ESD, FVE): отложенная генерализация отслеживается по перестройке сингулярных и собственных чисел весов, градиентов и представлений.

### [Каузальная абляция](causal-ablation-intervention.md) (causal ablation / intervention) — 24 статей

Методология установления каузальной, а не корреляционной роли компонентов: частоту, контур или признак целенаправленно удаляют или подменяют и измеряют эффект на поведение сети.

### [Время гроккинга](grokking-time.md) (grokking time) — 18 статей

Задержка между подгонкой обучающей выборки и началом обобщения — величина, по которой корпус сравнивает вмешательства. Определяется операционально через пороги, и от выбора определения (первое касание против устойчивого грока) и единиц (шаги, секунды, FLOPs) зависит, какой метод окажется быстрее.

### [Разброс по семенам и воспроизводимость](seed-variance-reproducibility.md) (seed variance / reproducibility) — 13 статей

Расхождение исходов между прогонами, отличающимися только случайным семенем, и вопрос о том, что из сообщённого переживает его смену. Разброс входит в отчёт, служит порогом значимости для отрицательных результатов и определяет устройство опыта: парные ветви из одного состояния против сравнения средних.

### [Параметр порядка](order-parameter.md) (order parameter) — 12 статей

Макроскопическая величина, резко меняющаяся при пересечении критического порога управляющим параметром, — канонический маркер фазового перехода, перенесённый в теории гроккинга из статистической физики.

### [Тяжёлохвостовая саморегуляризация](heavy-tailed-self-regularization-htsr.md) (heavy-tailed self-regularization, HTSR) — 9 статей

Теория Martin & Mahoney: спектральная плотность весовых матриц характеризуется степенным тяжёлохвостовым показателем α; в корпус её как аналитическую линзу вводят Prakash & Martin.

### [Линейное зондирование](linear-sparse-probing.md) (linear / sparse probing) — 8 статей

К замороженным активациям присоединяют лёгкий линейный классификатор-зонд; по его точности судят, линейно ли декодируемо интересующее свойство, то есть о структурированности представления.

### [Фазовая диаграмма](phase-diagram.md) (phase diagram) — 8 статей

Карта режимов обучения в координатах гиперпараметров: где сеть запоминает, где грокает, где не обучается вовсе. Бывает двоичной, раскрашенной величиной (сроком) и аналитической — полученной решением уравнения, а не сеткой прогонов.

### [Корреляционные ловушки](correlation-traps.md) (correlation traps) — 4 статей

Аномально большие собственные значения в спектре поэлементно перемешанной матрицы весов, выходящие далеко за верхний край «объёмной» части распределения Марченко-Пастура.

### [Отрицательные результаты](negative-results.md) (negative results) — 4 статей

Измерения, показывающие, что ожидаемого действия нет. Требуют того же, что и положительные: объявленного ожидания, меры и порога значимости — которым служит разброс по семенам.

###### cat-7
## 7. Теория и формальные результаты (Theory & formal results)

### [Эффективная теория / статистическая механика](effective-theory-statistical-mechanics.md) (effective theory / statistical mechanics) — 27 статей

Физически мотивированные описания гроккинга через немногие макроскопические величины — параметр порядка, управляющий параметр, фазы — по образцу физики фазовых переходов.

### [Максимизация зазора](margin-maximization-implicit-bias.md) (margin maximization / implicit bias) — 25 статей

Неявное предпочтение градиентного спуска на разделимых данных: среди интерполирующих решений выбирается решение с максимальным зазором; в корпусе этим объясняют вторую, медленную пору обучения при гроккинге.

### [Границы генерализации](generalization-bounds.md) (generalization bounds) — 18 статей

Формально доказуемые оценки обобщающей способности: на ошибку генерализации, выборочную сложность или — в приложении к гроккингу — на длительность задержки до обобщения.

### [Закон задержки через норм-сепарацию](norm-separation-delay-law.md) (Norm-Separation Delay Law) — 10 статей

Количественный закон Truong et al.: задержка гроккинга обратно пропорциональна эффективной скорости сжатия нормы и пропорциональна логарифму отношения норм запоминающего и обобщающего решений.

### [Колмогоровская сложность](kolmogorov-complexity.md) (Kolmogorov complexity) — 7 статей

Длина минимальной программы, порождающей объект; мера сложности, под которой обобщающее решение проще запоминающего, а отложенная генерализация трактуется как спуск модели к меньшей сложности.

### [Критичность / критическая точка](criticality-critical-point.md) (criticality / critical point) — 5 статей

Значение управляющего параметра, вблизи которого динамика замедляется настолько, что обобщение приходит с задержкой. Доказано в одной решаемой постановке, перенос на прочие заявлен как догадка; «критический размер данных» — порог другого рода.

### [Законы масштабирования](scaling-laws.md) (scaling laws) — 5 статей

Степенные зависимости качества от масштаба, перенесённые в разговор о гроккинге как рамка, которой отложенная генерализация противоречит или в которую укладывается. Отдельно стоит закон границы запоминания и обобщения, выводимый из динамики, и спор о том, не порождена ли ступенька мерой.

### [Информационное узкое место](information-bottleneck.md) (information bottleneck) — 3 статей

Рамка сжатия входа при сохранении сведений о цели, предсказывающая подгонку и затем сжатие. В корпусе держится как словарь, общий с двойным спуском и гроккингом, а её место как измеримой величины заняли вычислимые меры сложности.

###### cat-8
### [Сингулярная теория обучения](singular-learning-theory.md) (singular learning theory, SLT) — 3 статей

Рамка, исходящая из того, что сети — сингулярные статистические модели, а множество минимумов есть многообразие с особенностями. Центральная величина — местный коэффициент обучения; подтверждение в корпусе смешанное.

## 8. Заготовки понятий без карточек (Concept stubs — no card yet)

Записи этой категории — заготовки: название, английский термин и уже выписанные
цитаты; карточки у понятия нет. Счётчик «— N статей» здесь значит не то же, что
у карточек категорий 1–7: у карточки это `papers_linked`, число статей в её
блоках ссылок, а у заготовки ссылаться нечему, поэтому счётчик пересчитывается
по текстам — сколько статей корпуса содержат английский термин в теле
`original/` (библиография отрезана, как в `count_incorpus.py`). Число цитат под
записью обычно меньше счётчика: цитаты выписывают по мере импорта статей.
Пересчёт — `.claude/skills/check-corpus-integrity/scripts/count_stub_mentions.py`
(`--write` переписывает счётчики, `--check` только сверяет); он же держит
правило построения запроса. Порядок записей — по английскому термину.

### Абсолютная энтропия градиента (Absolute Gradient Entropy, AGE) — 1 статей

- 2504.17243: [`"a novel Absolute Gradient Entropy (AGE) metric"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p1-2)

### Контроль через нелинейность активации (activation nonlinearity control) — 0 статей

- 2411.05353: [`"can be controlled by modifying the profile of the activation function"`](../papers/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry/original/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry.md#p1-5)

### Адаптивное ядро для feature learning (adaptive kernel feature learning) — 0 статей

- 2310.03789: [`"the adaptive kernel approach, to two teacher-student models"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-2)

### Приближённая китайская теорема об остатках (approximate CRT) — 2 статей

- 2505.18266: [`"we call the approximate Chinese Remainder Theorem"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2)

### Архитектурное индуктивное смещение (architectural inductive bias) — 4 статей

- 2603.05228: [`"bypassing the generalization delay is possible—but strictly depends on alignment between architectural priors and task symmetry"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2)

### Аррениусовское масштабирование (Arrhenius scaling) — 1 статей

- 2606.17120: [`"with escape times following Arrhenius scaling"`](../papers/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking/original/2606.17120.noise-driven-escape-from-metastable-phases-explains-grokking.md#p1-2)

### Анализ каузальной роли (causal role analysis) — 0 статей

- 2509.17738: [`"the causal role of either"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2)

### Конкуренция плотной и разрежённой подсетей (dense vs sparse circuit competition) — 0 статей

- 2303.11873: [`"two largely distinct subnetworks: a dense one that dominates before the transition"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2)

### Метрика сложности контура (circuit complexity metric) — 0 статей

- 2506.04434: [`"Approximate Local Circuit Complexity"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p2-1)

### Очистка убирает меморизацию (cleanup removes memorization) — 0 статей

- 2301.05217: [`"removes the memorization components"`](../papers/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability/2301.05217.progress-measures-for-grokking-via-mechanistic-interpretability.card.md#p2-3)

### Со-грокинг в мультизадачном обучении (co-grokking) — 1 статей

- 2402.16726: [`"some multi-task mixtures may lead to co-grokking"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-1)

### Кривизна через коммутаторный дефект (commutator defect curvature) — 0 статей

- 2602.16746: [`"commutator defects—the non-commutativity of"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2)

### Межслойное разделение памяти (cross-layer memory sharing) — 0 статей

- 2405.15071: [`"encouraging cross-layer knowledge sharing"`](../papers/2405.15071.grokked-transformers-are-implicit-reasoners/2405.15071.grokked-transformers-are-implicit-reasoners.card.md#p1-2)

### Кросс-задачная переносимость (cross-task transferability) — 0 статей

- 2402.16726: [`"grokked models obtain common features transferable among similar operations"`](../papers/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials/original/2402.16726.towards-empirical-interpretation-of-internal-circuits-and-properties-in-grokked-transformers-on-modular-polynomials.md#p1-1)

### Куррикулум easy→hard (curriculum easy-hard data) — 0 статей

- 2410.03569: [`"exposes models to easy and harder versions of modular arithmetic"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p2-2)

### Анализ кривизны (curvature analysis) — 1 статей

- 2512.03437: [`"Analyses of features and curvature further suggest that post‑grokking models learn"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Нарушение симметрии данных (data symmetry breaking) — 0 статей

- 2604.00316: [`"Breaking Data Symmetry is Needed For Generalization in Feature Learning Kernels"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-1)

### Dropout устраняет грокинг (dropout eliminates grokking) — 0 статей

- 2510.25966: [`"dropout can eliminate grokking"`](../papers/2510.25966.grokking-in-the-ising-model/original/2510.25966.grokking-in-the-ising-model.md#p2-2)

### Кривая устойчивости к dropout (Dropout Robustness Curve, DRC) — 1 статей

- 2507.11645: [`"a Dropout Robustness Curve (DRC)"`](../papers/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation/original/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation.md#p1-5)

### Раннее предсказание грокинга (early grokking prediction) — 0 статей

- 2306.13253: [`"predict grokking without training for a large number"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p1-2)

### Степенной закон раннего предупреждения (early-warning power law) — 0 статей

- 2602.16746: [`"the lead time obeys a power law ∆t ∝ t α"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2)

### Край линейной разделимости (edge of linear separability) — 1 статей

- 2410.04489: [`"the training dataset is nearly linearly separable"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)

### Косинусная близость эмбеддингов (embedding cosine similarity) — 0 статей

- 2507.11645: [`"similarity between embeddings in a high-dimensional spaces"`](../papers/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation/original/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation.md#p2-3)

### Униформность эмбеддинг-пространства (embedding-space uniformity) — 0 статей

- 2504.03162: [`"the uniformity of the embedding space and the"`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p1-2)

### Ускорение через перенос эмбеддингов (embedding transfer acceleration) — 0 статей

- 2504.13292: [`"a simple and principled method for accelerating grokking"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-2)

### Градиентный подъём энергетической функции (energy-function gradient ascent) — 0 статей

- 2509.21519: [`"gradient ascent of an energy function E"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p1-2)

### Ядро feature learning / RFM (feature-learning kernel, RFM) — 0 статей

- 2604.00316: [`"via the Recursive Feature Machine (RFM)"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p1-4)

### Коллапс ранга признаков (feature rank collapse) — 0 статей

- 2405.19454: [`"the decreasing of feature ranks and the"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p1-3)

### Плоские минимумы и генерализация (flat minima generalization) — 0 статей

- 2603.01192: [`"“flatter” regions of the loss landscape generalise better"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-4)

### Регуляризация к плоскостности (flatness regularization) — 1 статей

- 2509.17738: [`"models regularized away from flat"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-2)

### Точность плавающей запятой (floating-point precision) — 1 статей

- 2605.06152: [`"of floating-point arithmetic precision limits"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-2)

### Фурье-голова (Fourier head) — 1 статей

- 2603.05228: [`"Fourier-constrained output heads"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p3-4)

### Фурье-инициализация (Fourier initialization) — 1 статей

- 2603.05228: [`"deterministically initialized with cosine and sine values at five key frequencies"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p6-8)

### Фурье-энтропия (Fourier / frequency entropy) — 1 статей

- 2310.19470: [`"we introduce Fourier Entropy (FE) as follows"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p10-5)

### Частотное рассогласование (frequency misalignment) — 0 статей

- 2405.17479: [`"a misalignment between the preferred frequency in the"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-5)

### Гауссово feature learning (Gaussian Feature Learning, GFL) — 1 статей

- 2310.03789: [`"that of Gaussian Feature Learning (GFL)"`](../papers/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks/original/2310.03789.grokking-as-a-first-order-phase-transition-in-two-layer-networks.md#p1-4)

### Геометрические сигнатуры генерализации (geometric signatures) — 1 статей

- 2509.17738: [`"geometric signatures of generalization"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p1-3)

### Выравнивание градиентов forget/retain (gradient alignment forget-retain) — 0 статей

- 2512.03437: [`"reduced gradient alignment between forget and retain subsets"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Спектральное разложение градиента (gradient spectral decomposition) — 0 статей

- 2405.20233: [`"we can spectrally decompose the parameter trajectories under"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p1-2)

### Графовые свойства подсети (graph properties of subnetwork) — 0 статей

- 2310.19470: [`"beneficial graph properties such as increased average"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2)

### Grokked tickets (grokked lottery tickets) — 0 статей

- 2310.19470: [`"lottery tickets obtained during the generalizing phase (termed grokked tickets)"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p1-2)

### Грокинг как сжатие (grokking as compression) — 1 статей

- 2310.05918: [`"many of them share a similar high-level idea which is"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-3)

### Переносимость грокнутых моделей (grokking transferability) — 0 статей

- 2601.09049: [`"the downstream transferability of “grokked” Transformers remains largely underexplored"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-1)

### Ослабление грокинга с ростом сложности (grokking weakens with complexity) — 0 статей

- 2402.09469: [`"as $k$ increases, the grokking phenomenon becomes weak"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#fig-4)

### Эвристическое ядро (heuristic core) — 1 статей

- 2403.03942: [`"evidence of a *heuristic core*: a set of attention heads that appear in all generalizing subnetworks but, on their own, do not generalize"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#fig-1)

### Настройка гиперпараметров (hyperparameter tuning) — 8 статей

- 2601.19791: [`"through proper hyperparameter tuning"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p1-2)

### Неявное смещение адаптивных оптимизаторов (implicit bias of adaptive optimizers) — 0 статей

- 2206.04817: [`"characterizing an implicit bias of such optimizers"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p2-8)

### Метрика неактивных нейронов (inactive neurons metric) — 0 статей

- 2507.11645: [`"percentage of inactive neurons decreases during generalization"`](../papers/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation/original/2507.11645.tracing-the-path-to-grokking-embeddings-dropout-and-network-activation.md#p1-5)

### Недостаточная выборка / алиасинг (insufficient sampling / aliasing) — 1 статей

- 2405.17479: [`"caused by insufficient sampling"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-6)

### Критичность порога интерполяции (interpolation threshold criticality) — 0 статей

- 2410.04489: [`"the interpolation threshold, reminiscent of critical"`](../papers/2410.04489.grokking-at-the-edge-of-linear-separability/2410.04489.grokking-at-the-edge-of-linear-separability.card.md#p1-2)

### Внутренняя симметрия задачи (intrinsic task symmetry) — 1 статей

- 2201.02177: [`"Some of the operations listed in Figure 2 (right) are symmetric with respect to the order of the"`](../papers/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets/2201.02177.grokking-generalization-beyond-overfitting-on-small-algorithmic-datasets.card.md#p3-4)
- 2603.01968: [`"we propose that intrinsic task symmetry is"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-5)

### Дистилляция знаний (knowledge distillation) — 2 статей

- 2511.04760: [`"Knowledge Distillation (KD) from a model that has already"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p1-3)

### L-бесконечность регуляризация (l-infinity regularization) — 0 статей

- 2407.12332: [`"can be found by gradient descent with small ℓ∞ regularization"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-2)

### Циклирование нормы последнего слоя (last-layer weight-norm cycling) — 0 статей

- 2206.04817: [`"cyclic behavior of the norm of the"`](../papers/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking/2206.04817.the-slingshot-mechanism-an-empirical-study-of-adaptive-optimizers-and-grokking.card.md#p1-2)

### Сначала менее заметные частоты (less-salient frequency first) — 0 статей

- 2405.17479: [`"initially learn the less salient"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-2)

### LLC-траектория как зонд (LLC trajectory probe) — 0 статей

- 2603.01192: [`"LLC trajectories estimated from training data track the onset of generalisation"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)

### Анализ локальной сложности (local complexity analysis) — 1 статей

- 2512.03437: [`"quantifies the density of linear regions in a neural network’s input space partition"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p9-8)

### Локальный асинхронный грокинг (local asynchronous grokking) — 0 статей

- 2506.21551: [`"enter their grokking stages asynchronously"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)

### Неограниченный рост логитов (logit scaling growth) — 0 статей

- 2501.04697: [`"a direction of uncontrolled logit growth"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-4)

### Низкочастотный фильтр градиента (low-pass gradient filter) — 0 статей

- 2405.20233: [`"low-pass filtered gradients which is added to the current"`](../papers/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients/2405.20233.grokfast-accelerated-grokking-by-amplifying-slow-gradients.card.md#p3-10)

### Низкоранговая факторизация матриц (low-rank matrix factorization) — 1 статей

- 2506.05718: [`"sparse recovery and low rank matrix factorization"`](../papers/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters/original/2506.05718.grokking-beyond-the-euclidean-norm-of-model-parameters.md#p4-3)

### Машинное разучивание (machine unlearning) — 1 статей

- 2512.03437: [`"*machine unlearning*, i.e., removing the influence of specified data without full retraining"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Матричная энтропия (matrix Renyi entropy metric) — 0 статей

- 2311.06597: [`"The $\alpha$-order (Rényi) entropy for matrix $\mathbf{R}$ is defined as follows"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p3-4)

### Снижение сложности модели (model complexity reduction) — 0 статей

- 2504.17243: [`"the intrinsic complexity of the model leveraging the absolute weight"`](../papers/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation/original/2504.17243.neuralgrok-accelerate-grokking-by-neural-gradient-transformation.md#p2-1)

### Модульные представления (modular representations) — 1 статей

- 2512.03437: [`"post‑grokking models learn *more modular representations*"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p1-2)

### Пути экспертов MoE (MoE expert pathways) — 0 статей

- 2506.21551: [`"expert choices across layers in MoE"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)

### Многостадийная генерализация (multi-stage generalization) — 1 статей

- 2405.19454: [`"We observe a multi-stage progress in generalization"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#p1-9)

### Мультизадачная эмерджентность (multi-task emergence) — 0 статей

- 2402.15175: [`"By extending our framework to multi-task learning"`](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/original/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.md#p3-2)

### Взаимная информация как мера прогресса (mutual information progress measure) — 0 статей

- 2408.08944: [`"higher-order mutual information to analyze the"`](../papers/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition/original/2408.08944.information-theoretic-progress-measures-reveal-grokking-is-an-emergent-phase-transition.md#p1-2)

### Натуральный градиентный спуск (natural gradient descent) — 1 статей

- 2510.04930: [`"formal links to natural gradient descent"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p1-6)

### nGPT / гиперсфера (normalized Transformer hypersphere) — 0 статей

- 2603.05228: [`"a normalized Transformer that constrains all vectors to the unit hypersphere"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p4-2)

### Численное раздувание признаков (Numerical Feature Inflation, NFI) — 1 статей

- 2605.06152: [`"this mechanism Numerical Feature Inflation (N FI)"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-2)

### Разрыв OOD-генерализации (OOD generalization gap) — 0 статей

- 2403.03942: [`"generalize very differently to adversarial out-of-domain (OOD) evaluation sets"`](../papers/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models/2403.03942.the-heuristic-core-understanding-subnetwork-generalization-in-pretrained-language-models.card.md#p1-5)

### Метрика похожести путей (pathway similarity metric) — 0 статей

- 2506.21551: [`"one computes the pathway similarity"`](../papers/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test/2506.21551.grokking-in-llm-pretraining-monitor-memorization-to-generalization-without-test.card.md#p1-2)

### PCA-факторизация симметрии (PCA symmetry factorization) — 0 статей

- 2411.05353: [`"can yield a factorization of the modulus"`](../papers/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry/original/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry.md#p2-4)

### PCA-анализ траекторий (PCA trajectory analysis) — 0 статей

- 2602.16746: [`"Using PCA on attention weight trajectories and commutator defect analysis"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p1-2)

### Перестановочная эквивариантность (permutation equivariance) — 1 статей

- 2407.12332: [`"no permutation-equivariant model can achieve small population error"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-2)

### Предобусловленный градиентный спуск (preconditioned gradient descent) — 1 статей

- 2601.03162: [`"the impact of preconditioned gradient descent"`](../papers/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime/2601.03162.on-the-convergence-behavior-of-preconditioned-gradient-descent-toward-the-rich-learning-regime.card.md#p1-2)

### Проблемно-специфичная регуляризация потерь (problem-specific loss regularization) — 1 статей

- 2410.03569: [`"Design a custom loss function with a penalty term specific to modular"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p2-3)

### Квадратичные сети (quadratic networks) — 4 статей

- 2603.01192: [`"a basin-selection perspective on grokking in quadratic networks"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)

### Относительная плоскостность (relative flatness) — 1 статей

- 2509.17738: [`"the Hessian trace normalized by the weight"`](../papers/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking/original/2509.17738.flatness-is-necessary-neural-collapse-is-not-rethinking-generalization-via-grokking.md#p2-2)

### Магнитуда residual stream (residual magnitude) — 1 статей

- 2603.05228: [`"unbounded residual magnitude and data-dependent attention routing"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p1-2)

### Спад нормы как достаточное условие (robustness sufficient condition) — 0 статей

- 2311.06597: [`"the decrease of weight norm usually happens before the grokking on the test dataset, making it seemingly a sufficient condition for grokking but not a necessary condition"`](../papers/2311.06597.understanding-grokking-through-a-robustness-viewpoint/2311.06597.understanding-grokking-through-a-robustness-viewpoint.card.md#p1-4)

### Упрощённые границы решений (simplified decision boundaries) — 1 статей

- 2510.25966: [`"simplified decision boundaries in the input space"`](../papers/2510.25966.grokking-in-the-ising-model/original/2510.25966.grokking-in-the-ising-model.md#p2-3)

### Разрежённая аугментация данных (sparse data augmentation) — 0 статей

- 2410.03569: [`"Sparse data elements are critical for learning"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p7-1)

### Коллапс спектральной энтропии (spectral entropy collapse) — 1 статей

- 2604.13123: [`"norm expansion followed by entropy collapse"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p1-2)

### Спектральный гейтинг (spectral gating) — 2 статей

- 2603.15492: [`"revealing a “Spectral Gating” mechanism that regulates the transition"`](../papers/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold/original/2603.15492.grokking-as-a-variance-limited-phase-transition-spectral-gating-and-the-epsilon-stability-threshold.md#p1-2)

### Спектральная сигнатура осцилляций как предиктор (spectral signature predictor) — 0 статей

- 2306.13253: [`"We propose spectral signature to quantify the oscilla-"`](../papers/2306.13253.predicting-grokking-long-before-it-happens/original/2306.13253.predicting-grokking-long-before-it-happens.md#p2-2)

### Ложные измерения индуцируют грокинг (spurious dimensions induce grokking) — 0 статей

- 2310.17247: [`"via the addition of dimensions containing spurious information"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p1-1)

### Ограниченный выход через температуру (temperature-bounded output) — 0 статей

- 2603.05228: [`"logit magnitudes are strictly bounded by the temperature parameter"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p6-7)

### Трёхстадийная динамика обучения (three-stage training dynamic) — 1 статей

- 2603.01968: [`"we identify a consistent three-stage training dynamic:"`](../papers/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks/original/2603.01968.intrinsic-task-symmetry-drives-generalization-in-algorithmic-tasks.md#p1-6)

### Разделение временных масштабов (time-scale separation) — 1 статей

- 2509.20829: [`"distinct time scales between fitting the training set"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)

### Униформность токенов (token uniformity) — 1 статей

- 2504.03162: [`"this optimization merely leads to token uniformity"`](../papers/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking/2504.03162.beyond-progress-measures-theoretical-insights-into-the-mechanism-of-grokking.card.md#p1-2)

### Эффект туннеля (tunnel effect) — 1 статей

- 2405.19454: [`"Emergence of *Tunnel* on various depth of models"`](../papers/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better/2405.19454.deep-grokking-would-deep-neural-networks-generalize-better.card.md#fig-3)

### Гипотеза универсальности (universality hypothesis) — 2 статей

- 2505.18266: [`"We propose a testable universality hypothesis"`](../papers/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks/original/2505.18266.uncovering-a-universal-abstract-algorithm-for-modular-addition-in-neural-networks.md#p1-2)

### Энтропия весов (weight entropy metric) — 0 статей

- 2411.05353: [`"a metric for the generalization ability of"`](../papers/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry/original/2411.05353.controlling-grokking-with-nonlinearity-and-data-symmetry.md#p1-5)

### Сжатие внутриклассовой дисперсии (within-class variance contraction) — 0 статей

- 2509.20829: [`"population within-class variance is a key factor underlying both grokking"`](../papers/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence/2509.20829.explaining-grokking-and-information-bottleneck-through-neural-collapse-emergence.card.md#p1-2)

### Задача XOR (XOR classification task) — 2 статей

- 2504.13292: [`"on a synthetic XOR task where delayed generalization"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p1-3)

### Ограничение нулевой суммы градиентов (zero-sum gradient constraint) — 0 статей

- 2605.06152: [`"This breaks the zero-sum constraint of gradients across classes"`](../papers/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes/original/2605.06152.grokking-or-glitching-how-low-precision-drives-slingshot-loss-spikes.md#p1-2)
