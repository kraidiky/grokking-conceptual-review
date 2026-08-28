# Систематическая генерализация (systematic / hierarchical generalization)

[Сдвиг распределения](distribution-shift.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [3. Задачи и наборы данных](index.md#cat-3)\
→ Следующая категория: [4. Факторы обучения и оптимизации](weight-decay.md)\
← Предыдущая категория: [2. Механизмы и представления](structured-representation-learning.md)

## Определение

**Систематическая генерализация** — способность применить выученное правило к входам, устроенным иначе, чем всё виденное: не к новым примерам того же вида, а к новым сочетаниям. В корпусе она приходит через язык: наборы данных строятся так, что *«обучающую выборку одинаково хорошо объясняет и неиерархическое, и иерархическое правило, но обобщается на структурно новые входы только иерархическое»* \[[1.1](#ref-1-1)\]. Это делает задачу почти идеальной проверкой на [гроккинг](grokking.md): подгонка возможна двумя способами, и вопрос лишь в том, какой из них сеть выберет и когда.

Отсюда имя для явления в этой области — **структурный гроккинг**: обобщение на структурно новые входы улучшается уже после того, как внутридоменное качество вышло на насыщение \[[1.2](#ref-1-2)\].

## Детализация

**Почему это не просто ещё одна задача.** В [модульной арифметике](modular-arithmetic.md) обобщающее решение единственно и известно; здесь их два, и они различаются не эффективностью, а видом правила — линейным или иерархическим. Это позволяет спрашивать не «когда сеть обобщит», а «какое правило она предпочтёт», и делает предпочтение измеримым на структурно новых входах, а не на отложенной части той же таблицы.

**Немонотонность по глубине.** Структурный гроккинг зависит от [глубины](overparameterization-depth.md) перевёрнутой U-образной кривой: иерархическая генерализация сперва улучшается, затем ухудшается по мере углубления модели \[[1.2](#ref-1-2)\]. Это перекликается с немонотонностью, найденной в модульной арифметике, но получено на другом семействе задач, и потому служит независимым свидетельством.

**Роль цели обучения.** Отдельная находка линии: склонность к иерархическому обобщению наводится самой целью языкового моделирования, а не архитектурой — при смене цели поведение меняется, и результат переносится даже на рекуррентные сети \[[3.1](#ref-3-1)\]. Для корпуса это важно тем, что переносит вопрос с устройства сети на устройство обучающего сигнала.

**Методологическая ловушка.** Ранние работы объявляли, что обычные трансформеры на таких проверках проваливаются; объяснение оказалось процедурным — ранняя остановка по внутридоменному качеству, которая обрывает обучение ровно до того, как приходит структурное обобщение \[[3.2](#ref-3-2)\]. Это тот же класс ошибок, что и слишком короткий бюджет шагов в [измерении времени гроккинга](grokking-time.md): отрицательный результат оказывается свойством протокола.

## Альтернативные определения и нюансы

### A. Обобщение на структурно новые входы

Определение через данные: обучающая выборка допускает два правила, проверочная различает их \[[1.1](#ref-1-1)\]. Различающая черта — критерий успеха не «высокая точность», а «выбрано то правило», и потому проверка требует специально построенного набора, а не случайного разбиения.

### B. Структурный гроккинг

Определение через динамику: интересен не факт, а срок — обобщение продолжает улучшаться после насыщения внутридоменного качества \[[1.2](#ref-1-2)\]. Источник различия: здесь измеряют разрыв между двумя кривыми во времени, а не итоговое значение, и потому результат чувствителен к длительности обучения и к правилу остановки \[[3.2](#ref-3-2)\].

### C. Смещение, наводимое целью обучения

Третья позиция объясняет предпочтение правила не архитектурой и не данными, а обучающей целью \[[3.1](#ref-3-1)\]. Управляющая величина — вид цели (языковое моделирование против seq2seq); практическое следствие: спор «нужна ли рекуррентность для иерархии» оказывается плохо поставленным, пока цель не закреплена.

## Ссылки

###### ref-1-1
**\[1.1\]** 2305.18741 — Murty et al., «Grokking of Hierarchical Structure in Vanilla Transformers». [`"These datasets are constructed so that both a non-hierarchical as well as a hierarchical rule can perfectly fit the training set, but only the hierarchical rule generalizes to structurally novel inputs."`](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/original/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.md#fig-1). *«[Эти наборы построены так, что обучающую выборку одинаково хорошо объясняет и неиерархическое, и иерархическое правило, но обобщается на структурно новые входы только иерархическое.](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.card.md#fig-1)»*

###### ref-1-2
**\[1.2\]** 2305.18741 — Murty et al., «Grokking of Hierarchical Structure in Vanilla Transformers». [`"we show that structural grokking exhibits inverted U-shaped scaling behavior as a function of model depth: hierarchical generalization improves, then declines, as we train deeper models"`](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/original/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.md#p1-4). *«[мы показываем, что структурный гроккинг обнаруживает перевёрнутую U-образную зависимость от масштаба как функцию глубины модели: иерархическая генерализация сперва улучшается, затем ухудшается по мере того, как мы обучаем всё более глубокие модели](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.card.md#p1-4)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2404.16367 — Ahuja et al., «Learning Syntax Without Planting Trees: Understanding Hierarchical Generalization in Transformers». Нюанс: предпочтение иерархического правила наводится целью обучения, а не архитектурой, и переносится на рекуррентные сети. [`"Our results above suggest that the language modeling objective imposes bias towards hierarchical generalization in transformers."`](../papers/2404.16367.learning-syntax-without-planting-trees-understanding-hierarchical-generalization-in-transformers/original/2404.16367.learning-syntax-without-planting-trees-understanding-hierarchical-generalization-in-transformers.md#p6-7). *«[Наши результаты выше говорят, что цель языкового моделирования налагает склонность к иерархическому обобщению в трансформерах.](../papers/2404.16367.learning-syntax-without-planting-trees-understanding-hierarchical-generalization-in-transformers/2404.16367.learning-syntax-without-planting-trees-understanding-hierarchical-generalization-in-transformers.card.md#p6-7)»*

###### ref-3-2
**\[3.2\]** 2305.18741 — Murty et al., «Grokking of Hierarchical Structure in Vanilla Transformers». Нюанс: прежние отрицательные результаты объяснились протоколом — ранней остановкой по внутридоменному качеству. [`"We attribute these failures to early stopping based on in-domain validation performance"`](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/original/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.md#p1-5). *«[Мы объясняем эти провалы ранней остановкой по внутридоменному проверочному качеству](../papers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers/2305.18741.grokking-of-hierarchical-structure-in-vanilla-transformers.card.md#p1-5)»*
