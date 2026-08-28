# Отрицательные результаты (negative results)

[Корреляционные ловушки](correlation-traps.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [6. Аналитические инструменты и метрики](index.md#cat-6)\
→ Следующая категория: [7. Теория и формальные результаты](effective-theory-statistical-mechanics.md)\
← Предыдущая категория: [5. Интервенции и методы](gradient-low-pass-filtering.md)

## Определение

**Отрицательный результат** — измерение, показывающее, что ожидаемого действия нет: вмешательство не ускоряет, признак не предсказывает, механизм не воспроизводится. В корпусе [гроккинга](grokking.md) они встречаются реже положительных, но именно они ограничивают объяснения, и часть работ подаёт их как самостоятельный вклад: *«Мы подчёркиваем режимы отказа как самостоятельный вклад»* \[[1.2](#ref-1-2)\]. Прямая формулировка встречается и в отдельном виде: *«Мы также сообщаем ясный отрицательный результат»* \[[1.1](#ref-1-1)\].

## Детализация

**Чем отрицательный результат отличается от отсутствия результата.** Он требует того же, что и положительный: заранее объявленного ожидания, меры и порога значимости. В корпусе порогом служит [разброс по семенам](seed-variance-reproducibility.md) — толчки вдоль коммутатора не ускоряют обобщение, потому что все 27 прогонов укладываются *«в пределах разброса между сидами»* \[[3.1](#ref-3-1)\]. Без этого сравнения «не ускорило» неотличимо от «шум съел эффект».

**Что они закрывают.** Отрицательные результаты — единственный способ отделить сопутствующее от действующего. Механизм, который сопровождает гроккинг, но не меняет его при вмешательстве, теряет статус причины; именно так снимаются заявки о накоплении дефектов, о достаточности одной меры и о переносимости признаков между постановками. Их же обратная сторона — уточнение границ: отрицательный результат в одной постановке не переносится на другую, и потому карточка требует называть постановку вместе с итогом.

**Почему их мало.** Причина названа в самом корпусе: проверка отрицательных случаев дорога — явление требует крайне долгого обучения, и всеохватного обследования не получается \[[3.2](#ref-3-2)\]. Отсюда перекос: неудавшиеся вмешательства чаще остаются в приложениях или не публикуются вовсе.

**Как их читать.** Отрицательный результат с одним семенем не отрицательный: он неотличим от неудачного розыгрыша. Поэтому надёжные примеры в корпусе всегда сопровождаются числом прогонов и разбросом, а слабые — оговоркой авторов о неполноте проверки \[[3.2](#ref-3-2)\].

## Альтернативные определения и нюансы

### A. Отрицательный результат как вклад

Работа объявляет режимы отказа частью своего вклада и разбирает их наравне с успехами \[[1.2](#ref-1-2)\]. Различающая черта — отказ описан с той же полнотой, что и удача: названы условия, при которых он наступает, и мера, по которой он зафиксирован.

### B. Отрицательный результат как ограничение объяснения

Здесь предмет — не приём, а механизм: показывается, что предполагаемая причина недостаточна \[[3.1](#ref-3-1)\]. Источник различия — вывод адресован не практику, а теории; и требование к нему строже, потому что «недостаточно» проверяется только вмешательством, а не наблюдением.

### C. Отрицательный результат внутри положительной работы

Наиболее частая форма в корпусе: основной итог положителен, а рядом сообщается, что ожидавшееся объяснение не подтвердилось \[[1.1](#ref-1-1)\]. Различающая черта — такие результаты почти не цитируются и теряются первыми, хотя именно они несут ограничения переносимости.

## Ссылки

###### ref-1-1
**\[1.1\]** 2607.05104 — Ootani, «Grokking Is Conditional and Fragile: A Fully-Tractable, Multi-Seed Study at 12K Parameters». [`"We also report a clear negative result."`](../papers/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters/original/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters.md#p10-1). *«[Мы также сообщаем ясный отрицательный результат.](../papers/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters/2607.05104.grokking-is-conditional-and-fragile-a-fully-tractable-multi-seed-study-at-12k-parameters.card.md#p10-1)»*

###### ref-1-2
**\[1.2\]** 2607.20552 — Pandey, «Thermodynamic Weight Decay: Exploring Grokking Acceleration via Attention Specific Heat». [`"We emphasize the failure modes as a contribution in their own right."`](../papers/2607.20552.thermodynamic-weight-decay-exploring-grokking-acceleration-via-attention-specific-heat/original/2607.20552.thermodynamic-weight-decay-exploring-grokking-acceleration-via-attention-specific-heat.md#p7-2). *«[Мы подчёркиваем режимы отказа как самостоятельный вклад.](../papers/2607.20552.thermodynamic-weight-decay-exploring-grokking-acceleration-via-attention-specific-heat/2607.20552.thermodynamic-weight-decay-exploring-grokking-acceleration-via-attention-specific-heat.card.md#p7-2)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2602.16746 — Xu, «Low-Dimensional and Transversely Curved Optimization Dynamics in Grokking». Нюанс: отрицательный результат сформулирован через порог значимости — разброс по семенам, — а не через отсутствие видимого эффекта. [`"within seed-to-seed variability). This negative result demonstrates that defect accumulation alone is insufficient to induce grokking"`](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/original/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.md#p12-7). *«[в пределах разброса между сидами). Этот отрицательный результат показывает, что одного накопления дефекта недостаточно, чтобы вызвать гроккинг](../papers/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking/2602.16746.low-dimensional-and-transversely-curved-optimization-dynamics-in-grokking.card.md#p12-7)»*

###### ref-3-2
**\[3.2\]** 2601.09049 — He et al., «Is Grokking Worthwhile? Functional Analysis and Transferability of Generalization Circuits in Transformers». Нюанс: причина, по которой отрицательных проверок мало, названа прямо — они дороги, и обследование выходит неполным. [`"our investigation into “fake grokking” (behavioral grokking without circuit formation) is not exhaustive"`](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/original/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.md#p5-2). *«[наше исследование «поддельного гроккинга» (поведенческого гроккинга без складывания контура) не всеохватно](../papers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers/2601.09049.is-grokking-worthwhile-functional-analysis-and-transferability-of-generalization-circuits-in-transformers.card.md#p5-2)»*
