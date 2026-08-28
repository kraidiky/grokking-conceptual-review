# Сингулярная теория обучения (singular learning theory, SLT)

[Информационное узкое место](information-bottleneck.md) ← предыдущая карточка, следующая → —

[Индекс карточек понятий](index.md), категория: [7. Теория и формальные результаты](index.md#cat-7)\
→ Следующая категория: [1. Явления](grokking.md)\
← Предыдущая категория: [6. Аналитические инструменты и метрики](progress-measures.md)

## Определение

**Сингулярная теория обучения** — рамка, исходящая из того, что нейросети принадлежат к *сингулярным* статистическим моделям: отображение параметров в функции не взаимно однозначно, и множество минимумов не изолированные точки, а многообразия с особенностями. Мотив прямой: классической статистики недостаточно, PAC-границы пусты и не объясняют обобщающей силы сетей \[[1.1](#ref-1-1)\]. Центральная величина рамки — **местный коэффициент обучения** (LLC), входящий в асимптотику свободной энергии наравне с эмпирическим правдоподобием \[[1.2](#ref-1-2)\].

## Детализация

**Что рамка даёт [гроккингу](grokking.md).** Она объясняет, почему задержка вообще возможна без изменения потери: если решения лежат на многообразии, движение по нему не меняет ошибку, но меняет сложность решения, а значит и обобщение. Отсюда и связь с [плато](memorization-phase.md): долгие плато обучающей ошибки возникают близ особых областей параметров, порождаемых перестановочными симметриями и избыточностями \[[3.1](#ref-3-1)\] — и это наблюдение старше самого термина «гроккинг».

**Как её проверяют.** Не выводом, а измерением: LLC оценивают по ходу обучения и смотрят, отмечает ли он переходы. Сводка такой проверки честна в оценке: свидетельство в пользу аррениусовской связи между изменением свободной энергии и скоростью перехода получено *смешанное* \[[1.3](#ref-1-3)\]. То есть рамка в корпусе — работающий инструмент с частичным подтверждением, а не установленная теория гроккинга.

**Отношение к соседним линиям.** SLT смыкается с [эффективной теорией и статистической механикой](effective-theory-statistical-mechanics.md) через свободную энергию, а с [выбором бассейна](loss-landscape-basins.md) — через утверждение, что переход есть смена области с иным коэффициентом. Отличие от энтропийной линии в том, что там объём считают перебором (сэмплированием), а здесь он входит в асимптотику как показатель.

**Слабое место.** Оценка LLC требует сэмплирования и чувствительна к его настройкам, а перенос теоремы, доказанной для байесовской асимптотики при растущей выборке, на обучение с закреплённой выборкой требует оговорок — что в корпусе и делается явно.

## Альтернативные определения и нюансы

### A. Сингулярность как свойство модели

Исходная форма: сеть — сингулярная статистическая модель, и потому классические асимптотики неприменимы \[[1.1](#ref-1-1)\]. Различающая черта — утверждение о геометрии множества решений, а не о динамике; из него ещё не следует, что переход между областями произойдёт.

### B. LLC как измеримый показатель

Прикладная форма: коэффициент оценивают численно и следят за ним по ходу обучения \[[1.2](#ref-1-2)\], \[[1.3](#ref-1-3)\]. Источник различия — здесь рамка становится [мерой прогресса](progress-measures.md) и подчиняется тем же требованиям: устойчивость по семенам, опережение перехода, воспроизводимость оценки.

### C. Плато как след особых областей

Историческая форма, предшествующая корпусу: долгие плато объясняются близостью к особым областям параметров \[[3.1](#ref-3-1)\]. Различающая черта — предмет объяснения: не отложенное обобщение, а застой обучающей ошибки; связь с гроккингом здесь — предположение, требующее показать, что застой и задержка вызваны одним и тем же.

## Ссылки

###### ref-1-1
**\[1.1\]** 2512.00686 — Lakkapragada, «Using physics-inspired Singular Learning Theory to understand grokking & other phase transitions in modern neural networks». [`"neural networks are *s"`](../papers/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks/original/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks.md#p1-3). *«[нейронные сети — *сингулярные*](../papers/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks.card.md#p1-3)»*

###### ref-1-2
**\[1.2\]** 2512.00686 — Lakkapragada, «Using physics-inspired Singular Learning Theory to understand grokking & other phase transitions in modern neural networks». [`"$\lambda_{\alpha}$ is the local learning coefficient (LLC[^3]) (Lau et al., 2023)"`](../papers/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks/original/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks.md#p2-1). *«[$\lambda_{\alpha}$ — местный коэффициент обучения (LLC[^3]) (Lau et al., 2023)](../papers/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks.card.md#p2-1)»*

###### ref-1-3
**\[1.3\]** 2512.00686 — Lakkapragada, «Using physics-inspired Singular Learning Theory to understand grokking & other phase transitions in modern neural networks». [`"we obtain mixed evidence for an Arrhenius-style reaction-rate relationship"`](../papers/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks/original/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks.md#p7-4). *«[мы получаем смешанное свидетельство в пользу аррениусовской связи скорости реакции](../papers/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks/2512.00686.using-physics-inspired-singular-learning-theory-to-understand-grokking-and-other-phase-transitions-in-modern-neural-networks.card.md#p7-4)»*

## Ссылки на присоединившиеся работы

### Поддерживают

###### ref-3-1
**\[3.1\]** 2510.04930 — Saheb Pasand et al., «Egalitarian Gradient Descent: A Simple Approach to Accelerated Grokking». Нюанс: наблюдение старше термина — плато связывали с особыми областями параметров задолго до работ о гроккинге. [`"Plateaus arise near *singular* parameter regions—caused by permutation symmetries and redundancies"`](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/original/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.md#p3-1). *«[Плато возникают близ *особых* областей параметров, порождаемых перестановочными симметриями и избыточностями](../papers/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking/2510.04930.egalitarian-gradient-descent-a-simple-approach-to-accelerated-grokking.card.md#p3-1)»*

###### ref-3-2
**\[3.2\]** 2603.01192 — Cullen et al., «A Basin-Selection Perspective on Grokking via Singular Learning Theory». Нюанс: LLC применён к гроккингу напрямую — как мера местного вырождения поверхности потерь, различающая соперничающие котловины. [`"The key measure is the local learning coefficient (LLC)"`](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2). *«[Ключевая мера — местный коэффициент обучения (LLC)](../papers/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory/2603.01192.a-basin-selection-perspective-on-grokking-via-singular-learning-theory.card.md#p1-2)»*
