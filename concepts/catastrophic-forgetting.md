# Катастрофическое забывание (catastrophic forgetting)

[Коллапс softmax](softmax-collapse.md) ← предыдущая карточка, следующая → [Унгрокинг](ungrokking.md)

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

**Catastrophic forgetting (катастрофическое забывание)** — резкая утрата
нейросетью ранее усвоенного знания при продолжении обучения на новых данных или
изменённом распределении (классическое явление, McCloskey & Cohen, 1989). В
исследованиях [гроккинга](grokking.md) им объясняют потерю уже достигнутого
обобщающего решения при дальнейшем обучении \[[1.1](#ref-1-1)\].

## Детализация

Varma et al. прямо связывают забывание с грокингом: [унгрокинг](ungrokking.md)
(регрессия грокнутой сети к плохой тестовой точности) — **частный случай**
катастрофического забывания, но с оговоркой: унгрокинг возникает даже при простом
удалении примеров из обучающего набора, тогда как классическое забывание обычно
предполагает *дообучение на новых* примерах \[[1.1](#ref-1-1)\]. Prakash & Martin
показывают конкретный механизм в грокинге: **[Correlation Traps](correlation-traps.md)** (аномально
большие элементы весовых матриц) вызывают забывание и сопровождают
[анти-грокинг](anti-grokking.md) \[[3.1](#ref-3-1)\]. Singh et al. в режиме
continual pretraining (грокнутая модель последовательно переходит с одной задачи
на другую) показывают, что **дистилляция знаний** (knowledge distillation, KD —
обучение модели на предсказаниях другой модели) ускоряет генерализацию и
**смягчает** забывание \[[3.2](#ref-3-2)\].

## Альтернативные определения и нюансы

### A. Классическое забывание при смене задачи (McCloskey & Cohen)

Каноническое определение: сеть, дообучаемая на новой задаче или распределении,
резко теряет прежние навыки \[[1.1](#ref-1-1)\]. Источник различия: движущий
фактор — **смена обучающего распределения**.

### B. Забывание внутри грокинга (унгрокинг как частный случай)

В грокинге забывание наступает даже без новой задачи — при сокращении датасета
(унгрокинг) или при спектральных аномалиях весов (Correlation Traps,
анти-грокинг) \[[1.1](#ref-1-1)\]\[[3.1](#ref-3-1)\]. Источник различия: потеря
обобщения происходит на той же задаче, по внутренним причинам динамики.

### Поддерживают

- **Забывание через Correlation Traps** \[[3.1](#ref-3-1)\]: спектральные
  аномалии весовых матриц индуцируют катастрофическое забывание в фазе
  анти-грокинга. Источник различия: конкретный весовой механизм забывания в
  грокинге.
- **Смягчение дистилляцией** \[[3.2](#ref-3-2)\]: при continual pretraining
  грокнутой модели дистилляция знаний ослабляет забывание. Источник различия:
  забывание рассматривается как устранимый дефект переноса.

## Ссылки

###### ref-1-1
**\[1.1\]** 2309.02390 — Varma et al., «Explaining grokking through circuit efficiency». [`"Ungrokking can be seen as a special case of catastrophic forgetting"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p6-6). *«[Унгрокинг можно рассматривать как частный случай катастрофического забывания](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p6-6)»*\
Доп. (отличие): [`"ungrokking would arise even if we only remove examples from the training dataset, whereas catastrophic forgetting typically involves training on new examples as well"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p6-6) — *«[унгрокинг возникнет, даже если мы только удаляем примеры из обучающего набора, тогда как катастрофическое забывание обычно предполагает и обучение на новых примерах](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p6-6)»*.

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2602.02859 — Prakash & Martin, «Late-Stage Generalization Collapse in Grokking: Detecting anti-grokking with WeightWatcher». Нюанс: в грокинге забывание индуцируется Correlation Traps. [`"Correlation Traps can induce catastrophic forgetting and/or prototype memorization"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p1-5). *«[ловушки корреляции способны вызывать катастрофическое забывание и (или) запоминание образцов-прообразов](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.card.md#p1-5)»*

###### ref-3-2
**\[3.2\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». Нюанс: дистилляция знаний смягчает забывание при continual pretraining грокнутой модели. [`"KD both accelerates generalization and mitigates catastrophic forgetting, achieving strong performance"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p1-3). *«[KD и ускоряет генерализацию, и смягчает катастрофическое забывание, достигая сильных итогов](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p1-3)»*


### Выходят за пределы

###### ref-3-3
**\[3.3\]** 2507.20057 — Lyle et al., «What Can Grokking Teach Us About Learning Under Nonstationarity?». Приносит в корпус соседнее явление непрерывного обучения — *смещение первенства* (primacy bias): порчу обобщения на поздних задачах ранними данными; под этим же именем авторы сводят «критические периоды» Achille et al. 2017. Это не забывание: сеть теряет не выученное, а способность хорошо выучить новое. Нюанс: единство устройства со гроккингом объявлено догадкой — совпадает лишь то, что одно вмешательство помогает в обеих постановках, и ни одна мера выучивания признаков не приложена к обеим постановкам разом. [`"the *same fundamental process* by which a network replaces randomly initialized *memorizing* features with *generalizing* ones during grokking can be leveraged in continual learning problems to overwrite previously-learned features with new ones"`](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/original/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.md#p2-1). *«[тот же самый основополагающий процесс, которым сеть заменяет случайно инициализированные запоминающие признаки на обобщающие в ходе гроккинга, может быть употреблён в задачах непрерывного обучения, чтобы переписывать ранее выученные признаки новыми](../papers/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity/2507.20057.what-can-grokking-teach-us-about-learning-under-nonstationarity.card.md#p2-1)»*
## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits». [`"To prevent catastrophic forgetting of the original knowledge, we also retain a subset of 8,000 atomic facts"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p4-3). *«[Чтобы не допустить катастрофического забывания исходного знания, мы сохраняем также подмножество из 8000 атомарных фактов](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p4-3)»*

**\[4.2\]** 2512.03437 — Liang & Li, «Grokked Models are Better Unlearners». Нюанс: плоские минимумы смягчают разрушительное забывание, но сверка с SAM показывает, что одной плоскости недостаточно. [`"confirming that flatter minima help buffer against catastrophic forgetting"`](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p19-3). *«[что подтверждает: плоские минимумы и впрямь смягчают разрушительное забывание](../papers/2512.03437.grokked-models-are-better-unlearners/2512.03437.grokked-models-are-better-unlearners.card.md#p19-3)»*

**\[4.3\]** 2308.15594 — Charton, «Learning the greatest common divisor: explaining transformer predictions». Нюанс: катастрофическое забывание названо тем, чего избегает лог-равномерная выборка операндов, — распределение неизменно в ходе обучения, в отличие от учебного расписания; опыта с меняющимся расписанием и измерения забывания в работе нет. [`"This is related to curriculum learning, but avoids catastrophic forgetting, because the training distribution never changes."`](../papers/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions/original/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions.md#p9-8). *«[Это родственно учебному расписанию (curriculum learning), но избегает катастрофического забывания, потому что обучающее распределение никогда не меняется](../papers/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions/2308.15594.learning-the-greatest-common-divisor-explaining-transformer-predictions.card.md#p9-8)»*
