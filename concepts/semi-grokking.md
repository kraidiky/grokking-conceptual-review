# Полу-грокинг (semi-grokking)

[Унгрокинг](ungrokking.md) ← предыдущая карточка, следующая → [Анти-грокинг / коллапс генерализации](anti-grokking.md)

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

**Semi-grokking (полу-грокинг)** — отложенная генерализация не до идеальной, а
лишь до **частичной** («средней») тестовой точности: сеть грокает, но выходит на
неполное качество. Открыто Varma et al. (2023) как родственник
[гроккинга](grokking.md) и [унгрокинга](ungrokking.md), следующий из той же теории
[эффективности контуров](circuit-efficiency.md) \[[1.1](#ref-1-1)\].

## Детализация

Semi-grokking привязан к **[критическому размеру данных](data-fraction-critical-dataset-size.md)** Dcrit — тому объёму
обучающего набора, при котором запоминающий и обобщающий контуры (подсети,
реализующие соответственно хранение примеров и общий алгоритм) одинаково
эффективны. Если для унгрокинга датасет берут *меньше* Dcrit (запоминание
выигрывает, генерализация теряется), то для semi-grokking выбирают размер *ровно
около* Dcrit: [фазовый переход](phase-transition.md) к обобщению всё же
происходит, но выводит лишь на промежуточную, «среднюю» тестовую точность
\[[1.1](#ref-1-1)\]. Тем самым semi-grokking — не откат и не полный грок, а
устойчивое **плато частичной генерализации** в точке равной эффективности
контуров.

## Альтернативные определения и нюансы

### A. Плато частичной генерализации у Dcrit (Varma)

Каноническая трактовка: при D ≈ Dcrit фазовый переход выводит сеть лишь на
неполную тестовую точность \[[1.1](#ref-1-1)\]. Источник различия: управляющий
параметр — размер набора точно у порога равной эффективности контуров.

### Поддерживают

- **Умеренные способности при критическом размере** \[[3.1](#ref-3-1)\]: у
  критического размера данных модель показывает semi-grokking — умеренную
  генерализацию; независимое воспроизведение в рамке конкуренции контуров.
  Источник различия: то же поведение получено в другой теоретической рамке.

## Ссылки

###### ref-1-1
**\[1.1\]** 2309.02390 — Varma et al., «Explaining grokking through circuit efficiency». [`"semi-grokking, in which a network shows delayed generalisation to partial rather than perfect test accuracy"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3). *«[полу-грокинг, при котором сеть демонстрирует отложенную генерализацию к частичной, а не идеальной тестовой точности](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p1-3)»*\
Доп. (механизм): [`"leading to a phase transition but only to middling test accuracy"`](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p2-1) — *«[что приводит к фазовому переходу, но лишь к средней тестовой точности](../papers/2309.02390.explaining-grokking-through-circuit-efficiency/2309.02390.explaining-grokking-through-circuit-efficiency.card.md#p2-1)»*.

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2402.15175 — Huang et al., «Unified View of Grokking, Double Descent and Emergent Abilities: A Perspective from Circuits Competition». Нюанс: semi-grokking воспроизводится в рамке конкуренции контуров. [`"named semi-grokking, characterized by moderate generalization capabilities"`](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/original/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.md#p2-2). *«[названное полу-грокингом, с умеренной способностью к генерализации](../papers/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities/2402.15175.unified-view-of-grokking-double-descent-and-emergent-abilities.card.md#p2-2)»*

## Цитирования

Работы, лишь упоминающие явление (обзор литературы, связанные работы, попутное цитирование) без его подробного разбора.

**\[4.1\]** 2511.04760 — Singh et al., «When Data Falls Short: Grokking Below the Critical Threshold». [`"Training below this threshold yields semi-grokking, and fine-tuning grokked models on such small data can cause “ungrokking.”"`](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/original/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.md#p2-4). *«[Обучение ниже этого порога даёт полугроккинг, а дообучение гроккнувших моделей на столь малых данных способно вызвать «разгроккивание»](../papers/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold/2511.04760.when-data-falls-short-grokking-below-the-critical-threshold.card.md#p2-4)»*

**\[4.2\]** 2509.21519 — Tian, «Provable Scaling Laws of Feature Emergence from Learning Dynamics of Grokking». [`"Boundary of generalization and memorization (semi-grokking (Varma et al., 2023))"`](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/original/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.md#p8-2). *«[Граница генерализации и запоминания (*полу-грокинг* (Varma et al., 2023))](../papers/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking/2509.21519.provable-scaling-laws-of-feature-emergence-from-learning-dynamics-of-grokking.card.md#p8-2)»*

**\[4.3\]** 2601.19791 — Xu et al., «To Grok Grokking: Provable Grokking in Ridge Regression». [`"Varma et al. (2023) interpreted grokking from the perspective of circuit efficiency, and discovered two related phenomena named “ungrokking” and “semi-grokking”"`](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/original/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.md#p3-2). *«[Varma et al. (2023) истолковали гроккинг со стороны действенности контуров и открыли два родственных явления, названных «разгроккиванием» и «полугроккингом»](../papers/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression/2601.19791.to-grok-grokking-provable-grokking-in-ridge-regression.card.md#p3-2)»*

**\[4.4\]** 2401.10463 — Zhu et al. 2024. Нюанс: полу-грокинг помянут только как содержание работы Varma et al., от постановки которой авторы себя отделяют; собственных опытов у критического размера с промежуточной точностью в работе нет. [`"Training with these data points will result in suboptimal test loss (i.e., semi-grokking)."`](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p12-2). *«[Обучение на таком числе точек приведёт к неоптимальной тестовой потере (то есть к полу-грокингу).](../papers/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective/2401.10463.critical-data-size-of-language-models-from-a-grokking-perspective.card.md#p12-2)»*

```
concept:
  category: 1                    # 1. Явления (Phenomena)
  papers_linked: 6             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-20
```
