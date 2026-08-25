# Наивная минимизация потерь (Naive Loss Minimization)

[Анти-грокинг / коллапс генерализации](anti-grokking.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [1. Явления](index.md#cat-1)\
→ Следующая категория: [2. Механизмы и представления](structured-representation-learning.md)\
← Предыдущая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)

## Определение

**Naive Loss Minimization (NLM, «наивная минимизация потерь»)** — компонента
градиента, которая **не меняет предсказаний** модели, а снижает потерю
перекрёстной энтропии, просто **масштабируя логиты** (предсофтмаксные выходы)
вверх; за точкой переобучения градиент почти целиком уходит в это направление
неконтролируемого роста логитов. Введено Prieto et al. (2025) как причина
отсрочки генерализации при [гроккинге](grokking.md) и коллапса
[Softmax Collapse](softmax-collapse.md) \[[1.1](#ref-1-1)\].

## Детализация

Почему NLM вообще возникает: при потере перекрёстной энтропии даже уже верно
классифицированный пример продолжает давать ненулевой градиент — его можно
уменьшать почти бесконечно, лишь **раздувая масштаб логитов** (обычно через рост
норм весов вдоль текущего направления), не меняя того, какой класс предсказан
\[[1.1](#ref-1-1)\]. Prieto et al. показывают, что именно NLM ответственна за
**отсрочку генерализации** и в пределе приводит к Softmax Collapse; чтобы это
подтвердить, они вводят [оптимизатор](optimizer-adam-adamw-sgd.md) **[⊥Grad](orthogonal-gradient-perp-grad.md)**, сохраняющий только ортогональную к
направлению NLM часть градиента, — и он грокает без начальной фазы переобучения
\[[1.1](#ref-1-1)\]. Yıldırım берёт NLM за причину роста логитов и потому
ограничивает норму выходного слоя (bounded output) \[[3.1](#ref-3-1)\].

## Альтернативные определения и нюансы

### A. NLM как компонента градиента (по определению)

Определение по геометрии обучения: направление в градиенте, не меняющее
предсказаний, но снижающее потерю масштабированием логитов \[[1.1](#ref-1-1)\].
Источник различия: понятие задано формой градиента.

### B. NLM как причина коллапса и отсрочки (по роли)

Определение через последствия: неконтролируемый рост логитов откладывает
генерализацию и в пределе вызывает Softmax Collapse; устраняется удалением этой
компоненты (⊥Grad) или ограничением выхода
\[[1.1](#ref-1-1)\]\[[3.1](#ref-3-1)\]. Источник различия: акцент на
функциональной роли, а не на определении.

### Поддерживают

- **NLM как обоснование ограниченного выхода** \[[3.1](#ref-3-1)\]: Yıldırım берёт
  NLM за причину роста логитов и потому ограничивает норму выходной (unembedding)
  матрицы, чтобы не доводить до Softmax Collapse. Источник различия: то же понятие
  применяется как аргумент для архитектурного ограничения.

## Ссылки

###### ref-1-1
**\[1.1\]** 2501.04697 — Prieto et al., «Grokking at the Edge of Numerical Stability». [`"overfitting and cross-entropy loss push the model in a direction of uncontrolled logit growth"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-4). *«[переобучение и кросс-энтропийная потеря толкают модель в направлении неконтролируемого роста логитов](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-4)»*\
Доп. (природа): [`"does not change the predictions of the model but decreases the loss by scaling the logits"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2) — *«[не меняет предсказаний модели, но уменьшает потерю за счёт масштабирования логитов](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p1-2)»*.\
Доп. (роль): [`"we validate that NLM is responsible for delaying generalization (Fig. 1, a to b) and leading to SC"`](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-2) — *«[мы проверяем, что за задержку генерализации (рис. 1, a к b) и за приход к SC отвечает NLM](../papers/2501.04697.grokking-at-the-edge-of-numerical-stability/2501.04697.grokking-at-the-edge-of-numerical-stability.card.md#p2-2)»*.

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2603.05228 — Yıldırım, «The Geometric Inductive Bias of Grokking: Bypassing Phase Transitions via Architectural Topology». Нюанс: NLM берётся за причину роста логитов и мотивирует ограниченный выходной слой. [`"This behavior—known as Naïve Loss Minimization—leads to numerical instability and Softmax Collapse"`](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/original/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.md#p6-5). *«[Это поведение, известное как наивная минимизация потерь, ведёт к вычислительной неустойчивости и коллапсу softmax](../papers/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology/2603.05228.the-geometric-inductive-bias-of-grokking-bypassing-phase-transitions-via-architectural-topology.card.md#p6-5)»*

```
concept:
  category: 1                    # 1. Явления (Phenomena)
  papers_linked: 2             # различных статей в разделах ссылок карточки
  counted_at: 2026-08-19
```
