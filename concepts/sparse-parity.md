# Разрежённая чётность (sparse parity)

[Реальные данные и зрение](vision-real-world-data-mnist-cifar.md) ← предыдущая карточка, следующая → [Шум в метках / случайные метки](label-noise-random-labels.md)

[Индекс карточек понятий](index.md), категория: [3. Задачи и наборы данных](index.md#cat-3)\
→ Следующая категория: [4. Факторы обучения и оптимизации](weight-decay.md)\
← Предыдущая категория: [2. Механизмы и представления](structured-representation-learning.md)

## Определение

**Разрежённая чётность** (sparse parity) — синтетическая задача бинарной
классификации, ставшая одним из канонических полигонов для изучения
[гроккинга](grokking.md): на вход подаётся строка из n случайных битов, а
метка равна *чётности* (произведению по модулю 2, то есть XOR) k «скрытых»
битов из фиксированного секретного подмножества S размера |S| = k, причём
k много меньше n \[[1.1](#ref-1-1)\]\[[1.2](#ref-1-2)\]. Формально (n, k)-чётность
отображает строку в чётность k релевантных координат; задачу называют
*разрежённой* именно потому, что число значимых битов k много меньше
размерности входа n \[[1.2](#ref-1-2)\]. Задача введена в контекст гроккинга
как модель «скрытого прогресса» ([Barak et al., 2022](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md)), а её систематический разбор в терминах гроккинга дан Merrill
et al. \[[1.1](#ref-1-1)\].

![Обучение (40,3)-чётности: точность, потеря и эффективная разрежённость — скачок совпадает с выделением разрежённой подсети (рис. 2 Merrill et al.)](assets/sparse-parity-training.png)

## Детализация

Разрежённая чётность привлекательна как полигон гроккинга по двум причинам.
Во-первых, она вычислительно трудна: (n, k)-чётность печально известна своей
трудностью по *статистическим запросам* (SQ-hardness — нижняя граница на число
запросов/нейронов/шагов, экспоненциальная по k), поэтому
обобщающее решение долго «прячется» за плато, и переход к нему выглядит
резким. Во-вторых, у задачи есть точная «правильная» подсеть (произведение k
битов), что делает удобным механистический анализ. Merrill et al. показали,
что на этой задаче [фаза запоминания](memorization-phase.md) соответствует
плотной сети, а последующий скачок обобщения — это [фазовый
переход](phase-transition.md), при котором управление предсказаниями
перехватывает возникшая [разрежённая подсеть](sparse-subnetwork-lottery-ticket.md) \[[1.1](#ref-1-1)\]. Вокруг этого
драйвера сложились конкурирующие трактовки: частотное рассогласование между
обучающим и тестовым спектром (F-принцип — склонность сетей учить частоты от
низких к высоким) \[[3.1](#ref-3-1)\]; обнаружение «хорошей подсети» в духе
гипотезы лотерейного билета (lottery ticket — разрежённый подграф, обучаемый до
той же точности) \[[3.2](#ref-3-2)\]; численная устойчивость и
[softmax-коллапс](softmax-collapse.md) как условие позднего обобщения
\[[3.3](#ref-3-3)\]. Что переход на разрежённой чётности реален, косвенно
подтверждают методы его ускорения — перенос эмбеддингов от слабой модели
\[[3.4](#ref-3-4)\] и «эгалитарный» градиентный спуск, снимающий плато
\[[3.5](#ref-3-5)\]. Мера нелинейной сложности LMN ([нелинейная
сложность](nlm.md)) отдельно фиксирует, как XOR-сеть (частный случай 2-чётности)
переключается между двумя генерализующими решениями. При
этом сам статус разрежённой чётности как «грокающей» задачи оспаривается: при
обучении MLP на обильных данных обобщение наступает одновременно с запоминанием,
без задержки и без разделения норм \[[2.1](#ref-2-1)\].

## Альтернативные определения и нюансы

### A. (n, k)-чётность как supervised-задача

Базовая формулировка: метка примера — это XOR (чётность) k битов, попадающих в
фиксированное секретное подмножество S ⊂ {1, …, n}, а остальные n − k битов
нерелевантны и играют роль шумовых признаков \[[1.1](#ref-1-1)\]. Ключевой
управляющий параметр — разреженность k/n: при k много меньшем n число возможных
секретных подмножеств огромно, а любой отдельный бит статистически
некоррелирован с меткой, поэтому «жадные» признаковые эвристики не работают и
модель вынуждена выучивать точное произведение k координат \[[1.2](#ref-1-2)\].

### B. Канонический бенчмарк «скрытого прогресса» и SQ-трудности

Альтернативная трактовка смотрит на задачу не как на конкретную функцию, а как
на *класс трудности*: (n, k)-чётность — стандартный пример SQ-трудной задачи,
на котором демонстрируют вычислительные препятствия обучения.
Именно эта трудность делает её удобной моделью гроккинга: видимого прогресса
на тесте долго нет («скрытый прогресс»), пока внутренняя структура постепенно
формируется, а затем обобщение возникает скачком. В этой трактовке
разрежённая чётность интересна не сама по себе, а как контролируемый источник
резкого отложенного перехода \[[3.1](#ref-3-1)\]\[[3.5](#ref-3-5)\].

### Оспаривают

Truong et al. показывают, что «грокаемость» разрежённой чётности не
универсальна и зависит от постановки: для 3-разрежённой чётности на MLP с
обильными данными их [закон задержки](norm-separation-delay-law.md) предсказывает *отсутствие* гроккинга, и
это подтверждается эмпирически — 0 из 15 запусков демонстрируют задержку, а
генерализация наступает одновременно с запоминанием \[[2.1](#ref-2-1)\].
Управляющая величина здесь — отношение норм между «запоминающим» и
«обобщающим» решениями (Vfinal против Vmem): на разрежённой чётности оно
*инвертировано* (Vfinal > Vmem), то есть нет разделения норм, и потому нет фазы
таблицы соответствий, которую пришлось бы вытеснять; [неявное смещение](margin-maximization-implicit-bias.md) MLP
находит функцию чётности напрямую, без задержки \[[2.1](#ref-2-1)\].

### Поддерживают

Присоединяющиеся работы трактуют разрежённую чётность как подлинно грокающую
задачу и расходятся в назывании драйвера. Zhou et al. объясняют переход
частотным рассогласованием: сеть сперва подхватывает ложные низкочастотные
компоненты, возникающие из-за недосэмплирования, и лишь позже выравнивается с
истинным спектром чётности \[[3.1](#ref-3-1)\]. Minegishi et al. связывают
снятие задержки с обнаружением хорошей подсети (грокнутого лотерейного билета),
а не со снижением нормы или ростом разреженности как таковых \[[3.2](#ref-3-2)\].
Prieto et al. переносят на разрежённую чётность своё объяснение через численную
устойчивость и softmax-коллапс \[[3.3](#ref-3-3)\]. Xu et al. и Saheb Pasand
et al. подтверждают реальность перехода косвенно — целенаправленно устраняя
плато гроккинга (переносом эмбеддингов и «эгалитарным» градиентным спуском
соответственно) \[[3.4](#ref-3-4)\]\[[3.5](#ref-3-5)\].

## Ссылки

###### ref-1-1
**\[1.1\]** 2303.11873 — Merrill et al., «A Tale of Two Circuits: Grokking as Competition of Sparse and Dense Subnetworks». [`"of networks undergoing grokking on the sparse parity task, and ﬁnd that the"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p1-2). *«[сетей, проходящих гроккинг на задаче разрежённой чётности, и обнаруживаем](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p1-2)»*\
Доп.: [`"We focus on analyzing grokking in the problem of learning a sparse $(n,k)$-parity function"`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p2-2) — *«[Мы сосредоточиваемся на анализе гроккинга в задаче выучивания разрежённой $(n,k)$-функции чётности](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p2-2)»*; [`"our sparse parity task indeed displays grokking, both in accuracy and loss."`](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/original/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.md#p3-2) — *«[наша задача разрежённой чётности действительно обнаруживает гроккинг — и по точности, и по потере](../papers/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks/2303.11873.a-tale-of-two-circuits-grokking-as-competition-of-sparse-and-dense-subnetworks.card.md#p3-2)»*.

###### ref-1-2
**\[1.2\]** 2301.02679 — Gromov, «Grokking modular arithmetic». [`"the authors of [1] studied online learning of the (k, n) sparse parity problem where the network function is asked to compute parity of k bits in a length-n string of random bits"`](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p2-3). *«[авторы \[1\] изучали *онлайн*-обучение задаче разрежённой $(k,n)$-чётности, где функция сети должна вычислить чётность $k$ битов в строке случайных битов длины $n$](../papers/2301.02679.grokking-modular-arithmetic/2301.02679.grokking-modular-arithmetic.card.md#p2-3)»*


###### ref-1-3
**\[1.3\]** 2207.08799 — Barak et al., «Hidden Progress in Deep Learning: SGD Learns Parities Near the Computational Limit». Нюанс: вводит задачу в корпус как полигон с доказанным вычислительным пределом (нижняя граница SQ), а не с эмпирическим порогом. [`"a learner who guesses indices $S^{\prime}$ cannot use correlations (equivalently, the accuracy of the hypothesis $\chi_{S^{\prime}}$) as feedback to reveal which indices in $S^{\prime}$ are correct, unless $S^{\prime}$ is *exactly* the correct subset"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p4-4). *«[учащийся, угадывающий индексы $S^{\prime}$, не может использовать корреляции (равносильно — точность гипотезы $\chi_{S^{\prime}}$) как обратную связь, обнажающую, какие индексы в $S^{\prime}$ верны, если только $S^{\prime}$ не есть *в точности* верное подмножество](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p4-4)»*\
Доп.: [`"SGD solved the parity problem (with $100\%$ accuracy, validated on a batch of $2^{13}$ samples) in at least $20\%$ of $25$ random trials"`](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p6-1) — *«[SGD решал задачу чётности (со $100\%$ точностью, проверенной на партии из $2^{13}$ примеров) по крайней мере в $20\%$ из $25$ случайных испытаний](../papers/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit/2207.08799.hidden-progress-in-deep-learning-sgd-learns-parities-near-the-computational-limit.card.md#p6-1)»*.

###### ref-1-4
**\[1.4\]** 2311.07568 — Morwani et al. 2024, «Feature emergence via margin maximization: case studies in algebraic tasks». Даёт замкнутый вид максимального зазора для $(n,k)$-чётности при возбуждении $x^{k}$: $\gamma^{*}=k!\sqrt{2(k+1)^{-(k+1)}}$ при $m\geq 2^{k-1}$, — и показывает, что у всякого решения этого зазора носитель каждого ненулевого $u_{i}$ равен ровно релевантному множеству $S$, все $|u_{i}[j]|$ равны $\|w_{i}\|$, а выходной вектор лежит вдоль $[1,-1]$. Нюанс: случай проще двух других именно потому, что задача бинарная ($g^{\prime}=g$, условие C.1 выполняется даром); о вычислительной трудности чётности и о числе шагов до решения работа не говорит. [`"Consider a single hidden layer neural network of width $m$ with the activation function given by $x^{k}$, i.e, $f(x)=\sum_{i=1}^{m}(u_{i}^{\top}x)^{k}w_{i}$, where $u_{i}\in\mathbb{R}^{n}$ and $w_{i}\in\mathbb{R}^{2}$, trained on the $(n,k)-$sparse parity task."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p11-2). *«[Рассмотрим нейросеть с одним скрытым слоем ширины $m$ с функцией возбуждения $x^{k}$, то есть $f(x)=\sum_{i=1}^{m}(u_{i}^{\top}x)^{k}w_{i}$, где $u_{i}\in\mathbb{R}^{n}$ и $w_{i}\in\mathbb{R}^{2}$, обученную на задаче $(n,k)-$разрежённой чётности.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#p11-2)»*\
Доп. (постановка опыта): [`"Final neurons with highest norm and the evolution of normalized $L_{2,5}$ margin over training of a 1-hidden layer quartic network (activation $x^{4}$) on $(10,4)$ sparse parity dataset with $L_{2,5}$ regularization."`](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#fig-5) — *«[Итоговые нейроны с наибольшей нормой и изменение нормированного $L_{2,5}$-зазора по ходу обучения квартичной сети с одним скрытым слоем (возбуждение $x^{4}$) на наборе $(10,4)$-разрежённой чётности с $L_{2,5}$-регуляризацией.](../papers/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks/2311.07568.feature-emergence-via-margin-maximization-case-studies-in-algebraic-tasks.card.md#fig-5)»*.

###### ref-1-5
**\[1.5\]** 2211.12316 — Bhattamishra et al., «Simplicity Bias in Transformers and their Ability to Learn Sparse Boolean Functions». Нюанс: разрежённая чётность выступает разделяющей задачей — на полной чётности обобщает LSTM, на разрежённой трансформер, — и трансформер держит почти идеальное обобщение при 5-20 % перевёрнутых меток. [`"on $\text{Parity}\text{-}(40,4)$, we find that while Transformers generalize well, LSTMs severely overfit and achieve poor validation accuracy"`](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p7-6). *«[на $\text{Parity}\text{-}(40,4)$ мы обнаруживаем, что трансформеры обобщают хорошо, а LSTM жестоко переобучаются и достигают плохой проверочной точности](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p7-6)»*\
Доп.: [`"Learning $\text{Parity}\text{-}(n,k)$ with gradient-based methods has well-known hardness results $-$ requiring at least $n^{\Omega(k)}$ computational steps to find the correct target function (Kearns 1998)"`](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p7-1) — *«[У обучения $\text{Parity}\text{-}(n,k)$ градиентными методами есть хорошо известные результаты о трудности $-$ требуется по меньшей мере $n^{\Omega(k)}$ вычислительных шагов, чтобы найти верную целевую функцию (Kearns 1998)](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p7-1)»*.
## Ссылки на присоединившиеся работы

### Оспаривают

###### ref-2-1
**\[2.1\]** 2603.13331 — Truong et al., «The Norm-Separation Delay Law of Grokking». Оспаривает: на разрежённой чётности гроккинга нет — генерализация наступает одновременно с запоминанием. [`"The theory also correctly predicts when grokking does not occur : sparse parity tasks"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p2-10). *«[Теория верно предсказывает и то, *когда гроккинга не происходит*: задачи разрежённой чётности](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p2-10)»*\
Доп.: [`"sparse parity tasks exhibit an inverted norm ratio ($V_{\mathrm{final}}>V_{\mathrm{mem}}$) and zero grokking delay in 15/15 runs"`](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p2-10) — *«[задачи разрежённой чётности обнаруживают перевёрнутое отношение норм ($V_{\mathrm{final}}>V_{\mathrm{mem}}$) и нулевую задержку гроккинга в 15 прогонах из 15](../papers/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization/2603.13331.the-norm-separation-delay-law-of-grokking-a-first-principles-theory-of-delayed-generalization.card.md#p2-10)»*.


###### ref-2-2
**\[2.2\]** 2606.13753 — Truong et al. 2026, «The Weight Norm Sets the Grokking Timescale: A Causal Delay Law». Употребляет разрежённую чётность задачей-проверкой на то, не есть ли критическая норма след фурье-строения, и получает перенос ровно наполовину. Переносится сосредоточенная, задаваемая weight decay, не зависящая от скорости обучения норма гроккинга (четырёхкратный размах скорости обучения меняет норму примерно на $3\%$ при примерно четырёхкратном изменении времени). Не переносится главное: норма гроккинга здесь зависит от объёма данных, а зажим ложится на границу weight decay в обе стороны, так что недостижимого для регуляризации состояния с высокой нормой нет. Нюанс: авторы сами разводят два утверждения по охвату — общим объявлена только сосредоточенная норма; объяснение расхождения через число обобщающих решений подано как догадка. [`"What does *not* transfer is the regularization-unreachable above-norm delay. The grokking norm here drifts with the amount of training data (it is not data-invariant)"`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p11-2). *«[Что *не* переносится — это недостижимая для регуляризации задержка выше нормы. Норма гроккинга здесь дрейфует с объёмом обучающих данных (она не независима от данных)](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p11-2)»*\
Доп. (что объявлено общим): [`"The *concentrated, regularization-set grokking norm* is task-general (modular arithmetic and sparse parity; MLP and, on the functional norm, the Transformer)."`](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p11-3) — *«[*Сосредоточенная, задаваемая регуляризацией норма гроккинга* обща по задачам (модульная арифметика и разрежённая чётность; MLP и, на функциональной норме, трансформер).](../papers/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law/2606.13753.the-weight-norm-sets-the-grokking-timescale-a-causal-delay-law.card.md#p11-3)»*.
### Поддерживают

###### ref-3-1
**\[3.1\]** 2405.17479 — Zhou et al., «A rationale from frequency perspective for grokking in training neural network». Нюанс: гроккинг на чётности объясняется частотным рассогласованием (F-принцип). [`"we consider one-dimensional synthetic data and high-dimensional parity"`](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-6). *«[мы рассматриваем одномерные искусственные данные и высокоразмерную функцию чётности](../papers/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network/2405.17479.a-rationale-from-frequency-perspective-for-grokking-in-training-neural-network.card.md#p1-6)»*

###### ref-3-2
**\[3.2\]** 2310.19470 — Minegishi et al., «Bridging Lottery Ticket and Grokking». Нюанс: отложенная генерализация на разрежённой чётности снимается «грокнутым билетом» (хорошей подсетью), а не снижением нормы. [`"we also demonstrate that delayed generalization is reduced by the grokked ticket in both the polynomial regression and sparse parity tasks"`](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/original/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.md#p5-3). *«[мы показываем, что грокнувший билет сокращает отложенную генерализацию и в задачах многочленной регрессии, и на разрежённой чётности](../papers/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks/2310.19470.bridging-lottery-ticket-and-grokking-understanding-grokking-from-inner-structure-of-networks.card.md#p5-3)»*

###### ref-3-3
**\[3.3\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». Нюанс: объяснение через численную устойчивость и softmax-коллапс подтверждается на разрежённой чётности. [`"We also validate some of our results on the Sparse Parity task outlined in Barak et al. (2022)"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p3-3). *«[Мы проверяем часть наших результатов и на задаче разрежённой чётности, изложенной в Barak et al. (2022)](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p3-3)»*

###### ref-3-4
**\[3.4\]** 2504.13292 — Xu et al., «Let Me Grok For You: Accelerating Grokking via Embedding Transfer from a Weaker Model». Нюанс: перенос эмбеддингов (GrokTransfer) устраняет отложенную генерализацию на разрежённой чётности. [`"the sparse parity task. Our experiments verify that GrokTransfer effectively reshapes the training dynamics and eliminate delayed generalization"`](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/original/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.md#p8-5). *«[задаче разрежённой чётности. Наши опыты подтверждают, что GrokTransfer действенно перекраивает динамику обучения и устраняет отложенную генерализацию](../papers/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model/2504.13292.let-me-grok-for-you-accelerating-grokking-via-embedding-transfer-from-a-weaker-model.card.md#p8-5)»*

###### ref-3-5
**\[3.5\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». Нюанс: «эгалитарный» градиентный спуск (EGD) убирает плато гроккинга на разрежённой чётности. [`"we empirically show that on classical arithmetic problems like modular addition and sparse parity"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p1-2). *«[мы опытно показываем на классических арифметических задачах вроде модульного сложения и разрежённой чётности](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p1-2)»*\
Доп.: [`"works have shown that this problem induces grokking in (stochastic) gradient descent"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p9-5) — *«[работы показали, что эта задача вызывает гроккинг при (стохастическом) градиентном спуске](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p9-5)»*.

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2310.05918 — Liu et al., «Grokking as Compression: A Nonlinear Complexity Perspective». [`"intriguing phenomenon of the XOR network switching between two generalization solutions, while $L_{2}$ does not"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2). *«[любопытное явление: сеть на задаче XOR перескакивает между двумя решениями-обобщениями, чего $L_{2}$ не показывает](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p1-2)»*

**\[4.2\]** 2402.09469 — Li, Liang, Shi, Song, Zhou 2024, «Fourier Circuits in Neural Networks and Transformers: A Case Study of Modular Arithmetic with Multiple Inputs». Нюанс: при $p=2$ задача вырождается в чётность, и оценка числа нейронов согласуется с известной нижней границей. [`"$(n,k)$-sparse parity problem is notorious hard to learn, i.e., Statistical Query (SQ) hardness"`](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/original/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.md#p14-2). *«[задача разрежённой чётности $(n,k)$ печально трудна для обучения, то есть трудна для статистических запросов (SQ)](../papers/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs/2402.09469.fourier-circuits-in-neural-networks-and-transformers-a-case-study-of-modular-arithmetic-with-multiple-inputs.card.md#p14-2)»*

**\[4.3\]** 2407.12332 — Mohamadi et al., «A Theoretical Analysis of Grokking Modular Addition». [`"grokking can happen in tasks beyond modular arithmetic: in learning sparse parities (Barak et al., 2022; Bhattamishra et al., 2023)"`](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/original/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.md#p1-3). *«[гроккинг может происходить и в задачах за пределами модульной арифметики: при обучении разрежённым чётностям [5, 6]](../papers/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition/2407.12332.why-do-you-grok-a-theoretical-analysis-of-grokking-modular-addition.card.md#p1-3)»*

**\[4.4\]** 2410.03569 — Saxena et al. 2024, «Making Hard Problems Easier with Custom Data Distributions and Loss Regularization: A Case Study in Modular Arithmetic». [`"Our techniques also help ML models learn other well-studied problems better, including copy, associative recall, and parity"`](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/original/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.md#p1-3). *«[Наши приёмы помогают моделям лучше выучивать и другие хорошо изученные задачи, включая копирование, ассоциативное припоминание и чётность](../papers/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization/2410.03569.making-hard-problems-easier-with-custom-data-distributions-and-loss-regularization.card.md#p1-3)»*

**\[4.5\]** 2604.00316 — Tomàs, Mallinar, Belkin, «Breaking Data Symmetry is Needed For Generalization in Feature Learning Kernels». Нюанс: разбиение, инвариантное относительно подгруппы симметрий, запирает модель и не даёт обобщать. [`"RFM fails to generalize only when using train-test partitions that are invariant under the action of a non-singleton subgroup"`](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/original/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.md#p4-2) — *«[RFM не обобщает лишь тогда, когда разбиение на обучение и тест инвариантно относительно действия неодноэлементной подгруппы](../papers/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels/2604.00316.breaking-data-symmetry-is-needed-for-generalization-in-feature-learning-kernels.card.md#p4-2)»*

**\[4.6\]** 2310.02541 — Xu et al. 2023. Нюанс: разрежённая чётность названа вариантом того же семейства, но собственных опытов на чётностях нет; XOR здесь — смесь четырёх гауссиан в $\mathbb{R}^{p}$, а не булева функция на гиперкубе. [`"or its variants like the sparse parity problem"`](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/original/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.md#p3-3). *«[или на его вариантах вроде задачи разрежённой чётности](../papers/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data/2310.02541.benign-overfitting-and-grokking-in-relu-networks-for-xor-cluster-data.card.md#p3-3)»*

**\[4.7\]** 2504.16041 — Tveit, Remseth, Skogvold, «Muon Optimizer Accelerates Grokking». Нюанс: чётность здесь **полная** — все десять битов десятибитной строки, — а не $k$-разрежённая чётность корпуса; ни скрытого подмножества, ни параметров $(n,k)$, ни вычислительной трудности в постановке нет, и относить работу к полигону разрежённой чётности нельзя. [`"The parity task involves predicting the parity bit for 10-bit binary strings, with 1024 possible combinations."`](../papers/2504.16041.muon-optimizer-accelerates-grokking/original/2504.16041.muon-optimizer-accelerates-grokking.md#p2-4). *«[Задача чётности состоит в предсказании бита чётности для 10-битных двоичных строк, при 1024 возможных сочетаниях.](../papers/2504.16041.muon-optimizer-accelerates-grokking/2504.16041.muon-optimizer-accelerates-grokking.card.md#p2-4)»*

**\[4.8\]** 2606.12966 — Sivasankar, «Circuit Synchronization Precedes Generalization: A Causal Precursor to Grokking». О «скрытом прогрессе» Barak et al. 2022 сказано: [`"an observation directly analogous to our FSD precursor"`](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p16-2) — *«[наблюдение, прямо созвучное нашему предвестнику FSD](../papers/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking/2606.12966.circuit-synchronization-precedes-generalization-a-causal-precursor-to-grokking.card.md#p16-2)»*.
