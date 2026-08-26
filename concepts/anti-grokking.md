# Анти-грокинг / коллапс генерализации (anti-grokking / generalization collapse)

[Полу-грокинг](semi-grokking.md) ← предыдущая карточка, следующая → [Наивная минимизация потерь](nlm.md)

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

**Anti-grokking (анти-грокинг), или поздний коллапс генерализации** — третья фаза
грокинга: после того как сеть уже грокнула, при очень долгом продолжении обучения
её тестовая точность **обрушивается** обратно к низкой, тогда как обучающая
точность остаётся насыщенной. Введено Prakash & Martin (2025) — на исходном
датасете без [weight decay](weight-decay.md) \[[1.1](#ref-1-1)\]\[[1.2](#ref-1-2)\].

![Фазы длинного обучения: за запоминанием и гроккингом при продолжении оптимизации следует поздний коллапс генерализации — анти-грокинг (рис. 1 Prakash & Martin)](assets/anti-grokking-phases.png)

## Детализация

Prakash & Martin выделяют анти-грокинг как ранее не отмеченную **третью фазу** —
после запоминания и [гроккинга](grokking.md): train accuracy насыщена, а test
accuracy обрушивается \[[1.1](#ref-1-1)\]. Коллапс наступает на **исходном**
датасете, **без** weight decay, после порядка 10⁷ шагов \[[1.2](#ref-1-2)\].
Механизм — **[Correlation Traps](correlation-traps.md)** (аномально большие элементы весовых матриц,
выявляемые спектральными метриками [HTSR](heavy-tailed-self-regularization-htsr.md) / WeightWatcher), которые вызывают
[катастрофическое забывание](catastrophic-forgetting.md) \[[1.1](#ref-1-1)\]; регуляризация weight decay
подавляет их появление.

У явления есть и предвестник 2023 года в третьем режиме: Varma et al.
увидели необъяснённый поздний рост тестовой потери в отобранном запуске
полу-грокинга с weight decay и предположили многофазную динамику смесей
контуров \[[3.3](#ref-3-3)\] — термина и систематики это наблюдение не
получило.

Отсюда ключевое отличие от [унгрокинга](ungrokking.md): унгрокинг — регрессия
генерализации при уменьшении датасета *в режиме weight decay*, тогда как
анти-грокинг возникает на *исходных* данных *без* weight decay и потому **не
предсказывается** теорией [эффективности контуров](circuit-efficiency.md), на которой построен унгрокинг
\[[1.2](#ref-1-2)\].

## Альтернативные определения и нюансы

### A. Третья фаза грокинга (WeightWatcher)

Определение как фазы динамики: после грокинга наступает поздний коллапс
генерализации, детектируемый спектральными метриками весов (Correlation Traps)
\[[1.1](#ref-1-1)\]. Источник различия: явление привязано к фазе обучения и к
спектральным аномалиям весовых матриц.

### B. Коллапс вне предположений унгрокинга (HTSR)

Определение по условиям: поздний коллапс на исходном датасете без weight decay,
который механизм унгрокинга не покрывает \[[1.2](#ref-1-2)\]. Источник различия:
подчёркивается независимость от малого датасета и weight decay.

### Поддерживают

- **Поздний режим, улавливаемый спектральной метрикой** \[[3.1](#ref-3-1)\]:
  анти-грокинг как поздний коллапс тестовой точности (порядка 10⁷ шагов)
  распознаётся спектральной метрикой там, где конкурирующие метрики его
  пропускают — независимое подтверждение реальности режима. Источник различия:
  явление выделяется отдельной диагностической мерой.

## Ссылки

###### ref-1-1
**\[1.1\]** 2602.02859 — Prakash & Martin, «Late-Stage Generalization Collapse in Grokking: Detecting anti-grokking with WeightWatcher». [`"a previously unreported third phase of grokking in this training regime"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p1-2). *«[прежде не описанную третью фазу гроккинга в этом режиме обучения](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.card.md#p1-2)»*\
Доп. (суть): [`"a late-stage collapse of generalization"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p1-2) — *«[позднее обрушение генерализации](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.card.md#p1-2)»*.\
Доп. (механизм): [`"Correlation Traps can induce catastrophic forgetting and/or prototype memorization"`](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/original/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.md#p1-5) — *«[ловушки корреляции способны вызывать катастрофическое забывание и (или) запоминание образцов-прообразов](../papers/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher/2602.02859.late-stage-generalization-collapse-in-grokking-detecting-anti-grokking-with-weightwatcher.card.md#p1-5)»*.

###### ref-1-2
**\[1.2\]** 2506.04434 — Prakash & Martin 2025, «Grokking and Generalization Collapse: Insights from HTSR theory». [`"**late-stage generalization collapse** (’anti-grokking’) occurring on the *original* dataset after prolonged training (~$10^{7}$ steps) *without* WD (WD=0)"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p3-1). *«[**поздний обвал генерализации** («антигроккинг») на *исходном* наборе данных после долгого обучения (~$10^{7}$ шагов) *без* WD (WD=0)](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p3-1)»*\
Доп.: [`"This distinct phenomenon is not predicted by varma2023explaining as it falls outside of the crucial weight decay assumption on which it relies"`](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/original/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.md#p3-1) — *«[Это отдельное явление не предсказывается varma2023explaining, ибо выпадает из решающего допущения об ослаблении весов, на котором та работа держится](../papers/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory/2506.04434.grokking-and-generalization-collapse-insights-from-htsr-theory.card.md#p3-1)»*.

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2604.13123 — Truong et al., «Spectral Entropy Collapse as a Phase Transition in Delayed Generalisation: An Interventional and Predictive Framework for Grokking». Нюанс: анти-грокинг подтверждается как поздний режим, улавливаемый спектральной метрикой. [`"identify an *anti-grokking* regime — a late-stage test-accuracy collapse after $\sim\!10^{7}$ steps — not captured by competing metrics"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-5). *«[выделяют режим *антигроккинга* — позднее обрушение точности на тесте после $\sim\!10^{7}$ шагов, — не схватываемый соперничающими мерами](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p3-5)»*\
Доп. (проверяемое предсказание): [`"any rebound of $\tilde{H}$ upward at $\sim\!10^{7}$ steps would be a natural candidate signature of the transition into the anti-grokking regime"`](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p18-7) — *«[всякий откат $\tilde{H}$ вверх на $\sim\!10^{7}$ шагах был бы естественным кандидатом на признак вступления в режим антигроккинга](../papers/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation/2604.13123.spectral-entropy-collapse-as-a-phase-transition-in-delayed-generalisation.card.md#p18-7)»*


###### ref-3-2
**\[3.2\]** 2605.20441 — Verma 2026, «Weight Decay Regimes in Grokking Transformers: Cheap Online Diagnostics». Первое независимое наблюдение позднего коллапса при условиях, противоположных исходным: не на исходном наборе без weight decay после $\sim\!10^{7}$ шагов, а при $\lambda=1.0$ на 20 тыс. эпох, причём по когорте удержания коллапс сгущается при **большем** weight decay. Даёт понятию две поправки: цикл объявлен семя-зависимой хрупкостью (удержание $5/5$, $4/5$, $3/5$, $4/5$), а спектральный признак «третьей фазы» по времени не совпадает — тяжёлый хвост складывается при входе в гроккинг. Нюанс: механизма нет — ни ловушек корреляции, ни катастрофического забывания; рисунок со следом $\alpha$ построен на одном семени, и межсеменной расчёт прямо объявлен отложенным. [`"confirms the five-stage pattern is a *seed-dependent fragility* rather than a universal cycle"`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p6-3). *«[подтверждает, что пятиступенчатая картина есть *семя-зависимая хрупкость*, а не всеобщий цикл](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p6-3)»*\
Доп. (расхождение по времени с Prakash & Martin): [`"Heavy-tail structure therefore *forms during grokking onset, not during late-stage collapse*"`](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/original/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.md#p8-2) — *«[Тяжелохвостовая структура, следовательно, *складывается при входе в гроккинг, а не при позднем коллапсе*](../papers/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics/2605.20441.weight-decay-regimes-in-grokking-transformers-cheap-online-diagnostics.card.md#p8-2)»*.
###### ref-3-3
**\[3.3\]** 2309.02390 — Varma et al. 2023, «Explaining Grokking Through Circuit Efficiency». Самое раннее наблюдение класса в корпусе — предвестник в другом режиме, а не первенство: в специально отобранном одиночном запуске полу-грокинга (D=1532, с weight decay) поздний рост тестовой потери увиден и оставлен необъяснённым за два с лишним года до того, как Prakash & Martin назвали и систематически описали третью фазу. [`"At epoch $3.2\times 10^{7}$ we see test loss *rise*, we do not know why"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#fig-6). *«[На эпохе $3.2\times 10^{7}$ мы видим, что тестовая потеря *растёт*, и не знаем почему](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#fig-6)»*\
Доп. (многофазная гипотеза там же): [`"There seem to be multiple phases, perhaps corresponding to the network transitioning between mixtures of multiple circuits with increasing efficiencies, but further investigation is needed"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#fig-6) — *«[По-видимому, фаз несколько — возможно, они соответствуют переходам сети между смесями нескольких контуров с растущей эффективностью, — но нужны дальнейшие исследования](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#fig-6)»*.

```
concept:
  category: 1                    # 1. Явления (Phenomena)
  papers_linked: 5             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-24
```
