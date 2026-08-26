# Колмогоровская сложность (Kolmogorov complexity)

[Закон задержки через норм-сепарацию](norm-separation-delay-law.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Колмогоровская сложность** объекта (в нашем случае — обученной модели) — это
длина минимальной программы, которая на универсальной машине Тьюринга порождает
описание этого объекта \[[1.2](#ref-1-2)\]. В исследованиях [гроккинга](grokking.md)
её вводят как меру сложности, под которой обобщающее решение оказывается «проще»
запоминающего, так что отложенная генерализация трактуется как спуск модели к
меньшей колмогоровской сложности; поскольку сама KC невычислима, для нейросетей
предлагают её вычислимые прокси (например, число линейных отображений LMN)
\[[1.1](#ref-1-1)\].

![Число линейных отображений (LMN) как практическая мера сложности: ReLU-сеть разбивает пространство входов на линейные области (рис. 1 Liu, Zhong, Tegmark)](assets/kolmogorov-lmn-illustration.png)

## Детализация

Понятие пришло в литературу о гроккинге из двух работ 2023 года. Миллер и соавт.
используют колмогоровскую сложность как объединяющий формализм «сложности модели»:
выбор модели описывается как минимизация суммы ошибки и сложности, а KC задаёт саму
меру сложности как длину кратчайшей порождающей программы \[[1.2](#ref-1-2)\].
Поскольку точное вычисление KC невозможно (мера невычислима — не существует
алгоритма, дающего её значение для произвольной строки), на практике берут её
приближения; в частности, при допущении о нормальном распределении весов
приближённая KC оказывается пропорциональна квадрату весов, так что её минимизация
совпадает с привычным L2-штрафом — то есть с [weight decay](weight-decay.md), регуляризацией,
штрафующей норму весов \[[1.2](#ref-1-2)\]. Это прямо связывает колмогоровскую
сложность с [фазой меморизации](memorization-phase.md) и последующей генерализацией:
высокосложное запоминающее решение постепенно вытесняется низкосложным обобщающим.

Лю и соавт. переносят ту же идею на нейросети напрямую, называя предложенный ими
linear mapping number (LMN — число локальных линейных отображений, на которые сеть
разбивает входное пространство) «нейросетевой версией колмогоровской сложности»,
поскольку, в отличие от L2-нормы, LMN допускает естественную трактовку как
количество информации/вычисления \[[1.1](#ref-1-1)\]. В их постановке гроккинг — это
компрессия: обобщающее решение эффективнее (короче по описанию), поэтому появляется
позже запоминающего; на задаче XOR они наблюдают даже [двойной спуск](double-descent.md)
(немонотонную динамику — повторное снижение величины после кратковременного роста)
у LMN \[[1.1](#ref-1-1)\]. Чжан и соавт. доводят линию до аналитической формулировки:
они выражают KC через принцип минимальной длины описания (MDL — minimum description
length, длина кратчайшего сообщения, кодирующего модель) и показывают, что
наступление гроккинга эквивалентно «сбрасыванию» алгоритмических бит — непрерывному
снижению колмогоровской сложности модели по ходу обучения \[[3.1](#ref-3-1)\]. Через
ту же оптику они связывают гроккинг с [эмерджентностью](emergence.md) (внезапным
появлением способности) как с проявлением бритвы Оккама.

## Альтернативные определения и нюансы

### A. Длина минимальной порождающей программы

Классическая теоретико-информационная трактовка (Миллер и соавт.): сложность модели
`f` — это длина `l(p)` кратчайшей программы `p`, которая на универсальном компьютере
`U` выводит строковое представление модели \[[1.2](#ref-1-2)\]. Отличительный признак
этой формулировки — она операционализирует принцип экономии (парсимонии, «бритвы
Оккама») через теорию индуктивного вывода Соломонова: из двух гипотез, порождающих
одно и то же наблюдение, вероятнее та, у которой колмогоровская сложность меньше.
На практике невычислимую KC заменяют суррогатом — длиной описания модели (model
description length), а при нормальном приоре на весах приближённая KC сводится к
L2-норме, что и делает weight decay частным случаем минимизации сложности
\[[1.2](#ref-1-2)\].

### B. Нейросетевой прокси: число линейных отображений (LMN)

Вычислительная трактовка (Лю и соавт.): вместо невычислимой KC берётся конкретная
измеримая величина — LMN, обобщение числа линейных областей ReLU-сети на сети с
любыми (в том числе гладкими) активациями \[[1.1](#ref-1-1)\]. Отличительный признак
здесь — источник различия с конкурирующей мерой L2: LMN считает локальные/условные
линейные вычисления и потому напрямую интерпретируется как объём информации/вычисления,
а в фазе компрессии линейно коррелирует с тестовой ошибкой, тогда как L2 связана с ней
сложным нелинейным образом. Именно эта интерпретируемость «как информации» делает LMN
кандидатом на роль нейросетевой версии колмогоровской сложности \[[1.1](#ref-1-1)\].

### Поддерживают

Чжан и соавт. присоединяются к трактовке гроккинга как минимизации колмогоровской
сложности, но выводят её аналитически: KC модуля формализуется через MDL как
битовая стоимость кодирования топологии плюс точности весов, а наступление гроккинга
доказывается как «сбрасывание» алгоритмических бит — переход к решению с меньшей KC
\[[3.1](#ref-3-1)\]. Отличие от группы 1 — не эмпирический прокси, а замкнутая
формула, связывающая геометрическую сложность решения с его алгоритмической длиной.

## Ссылки

###### ref-1-1
**\[1.1\]** 2310.05918 — Liu et al., «Grokking as Compression: A Nonlinear Complexity
Perspective». [`"we argue that LMN is a promising candidate as the neural network version of the Kolmogorov complexity, since it explicitly considers local or conditioned linear computations aligned with the nature of modern artificial neural networks"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2). *«[мы доказываем, что LMN — многообещающий кандидат на роль нейросетевого варианта колмогоровской сложности, поскольку он явно учитывает локальные, или обусловленные, линейные вычисления, отвечающие природе современных искусственных нейронных сетей](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p1-2)»*\
Доп.: [`"(1) LMN can be naturally interpreted as information/computation, while $L_{2}$ cannot"`](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/original/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.md#p1-2) — *«[(1) LMN естественно трактуется как информация или вычисление, а $L_{2}$ — нет](../papers/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective/2310.05918.grokking-as-compression-a-nonlinear-complexity-perspective.card.md#p1-2)»*.

###### ref-1-2
**\[1.2\]** 2310.17247 — Miller, O'Neill, Bui 2024, «Grokking Beyond Neural
Networks: An Empirical Exploration with Model Complexity». [`"we measure the complexity as the length of the minimal program required to generate a given model"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p2-3). *«[сложность меряется длиной наименьшей программы, нужной для порождения данной модели](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p2-3)»*\
Доп.: [`"Essentially, the Kolmogorov complexity is the length of the minimal program required to produce a string representation of a model"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p16-3) — *«[По существу колмогоровская сложность есть длина наименьшей программы, нужной для порождения строчной записи модели](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p16-3)»*; [`"by minimising the approximate Kolmogorov complexity under the assumption of normality, we also minimise the $L_2$ norm of the weights"`](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p17-5) — *«[уменьшая приближённую колмогоровскую сложность при допущении нормальности, мы вместе с тем уменьшаем норму $L_2$ весов](../papers/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity/2310.17247.grokking-beyond-neural-networks-an-empirical-exploration-with-model-complexity.card.md#p17-5)»*.


###### ref-1-3
**\[1.3\]** 2211.12316 — Bhattamishra et al., «Simplicity Bias in Transformers and their Ability to Learn Sparse Boolean Functions». Нюанс: колмогоровская сложность служит лишь ориентиром — работает измеримая замена (средняя чувствительность), а её связь с SOP, энтропией меток и долей критических примеров проверена на 200k случайных моделей; связь односторонняя (высокая чувствительность влечёт высокую энтропию, но не наоборот). [`"While measures such as Kolmogorov complexity are uncomputable, sensitivity can be tractably estimated"`](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p1-6). *«[меры вроде колмогоровской сложности невычислимы, чувствительность можно оценивать разрешимым образом](../papers/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions/2211.12316.simplicity-bias-in-transformers-and-their-ability-to-learn-sparse-boolean-functions.card.md#p1-6)»*

###### ref-1-4
**\[1.4\]** 2605.09724 — Song & Ye, «Model Capacity Determines Grokking through Competing Memorisation and Generalisation Speeds». Нюанс: $K_{\text{alg}}$ введена словесно и не вычисляется ни разу; единственное косвенное свидетельство о ней — модели с $P$ чуть ниже $P_{\text{mem}}$, уже достигающие почти идеальной точности. [`"We treat modular division as a structured task that admits a much more compact algorithmic description of complexity $K_{\text{alg}}(p)$, although we do not attempt to compute $K_{\text{alg}}$ explicitly here."`](../papers/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds/original/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds.md#p4-4). *«[Мы рассматриваем модульное деление как структурированную задачу, допускающую гораздо более компактное алгоритмическое описание сложности $K_{\text{alg}}(p)$, хотя здесь мы не пытаемся вычислить $K_{\text{alg}}$ явно.](../papers/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds/2605.09724.model-capacity-determines-grokking-through-competing-memorisation-and-generalisation-speeds.card.md#p4-4)»*
## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.29262 — Zhang et al., «Grokking: From Abstraction to Intelligence».
Нюанс: гроккинг как минимизация аналитической колмогоровской сложности через MDL.
[`"We formalize KC via the Minimum Description Length (MDL) principle"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p7-8). *«[Мы записываем KC через начало наименьшей длины описания (MDL)](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p7-8)»*\
Доп.: [`"The emergence of grokking is thus rigorously characterized as the system shedding algorithmic bits"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p8-1) — *«[Возникновение гроккинга тем самым строго описывается как сбрасывание системой алгоритмических битов](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p8-1)»*; [`"even in the early stages, when training and test accuracy remain constant, the model internal structure still evolves block-cyclic features, corresponding to a continuous decrease in the model’s KC"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-5) — *«[даже на ранних порах, когда точности на обучении и на тесте неизменны, внутреннее строение модели всё же развивает блочно-циклические признаки, чему отвечает непрерывное убывание KC модели](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p1-5)»*.\
Доп. (замена BDM): [`"BDM approximates the global complexity of a large tensor $X$ by partitioning it into a set of smaller, non-overlapping sub-blocks"`](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p2-8) — *«[BDM приближает общую сложность большого тензора $X$, разбивая его на набор меньших непересекающихся подблоков](../papers/2603.29262.grokking-from-abstraction-to-intelligence/2603.29262.grokking-from-abstraction-to-intelligence.card.md#p2-8)»*

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition
in Delayed Generalisation». [`"DeMoss et al. (2024) develop a rate–distortion and Kolmogorov complexity framework for grokking and introduce a regulariser based on the *spectral entropy of weight matrices*"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-4) — *«[DeMoss et al. (2024) развивают рамку «скорость — искажение» и колмогоровской сложности для гроккинга и вводят регуляризатор на основе *спектральной энтропии весовых матриц*](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-4)»*\
Доп. (размежевание по предмету измерения): [`"Both works use the term “spectral entropy”, but applied to different objects"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-4) — *«[Обе работы пользуются словами «спектральная энтропия», но прилагают их к разным предметам](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-4)»*

```
concept:
  category: 7                    # 7. Теория и формальные результаты (Theory & formal results)
  papers_linked: 6             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-20
```
