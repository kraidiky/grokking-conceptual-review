# Внешние работы

Статьи, несущие релевантные корпусу наблюдения, но сами не посвящённые
гроккингу. Конвенция — в [`papers/README.md`](../papers/README.md),
раздел «Внешние работы (externals/)»: хранимый `original/` +
карточка-выдержка (аннотация, введение, цитируемые абзацы); в индексах
и счётчиках корпуса эти работы не участвуют; упоминания в карточках
понятий стоят в группах «Внешние работы» и несут знак ↗.

## Реестр

Формат строки: `arxiv-id` · заголовок · зачем затянута · кто из корпуса
цитирует · дата сверки цитат.

- `2110.06914` · What Happens after SGD Reaches Zero Loss? — A Mathematical Framework (Li, Wang & Arora, ICLR 2022) · адресат генеалогического утверждения Mohamadi о до-терминологическом наблюдении отложенной генерализации (подраздел «Более ранние наблюдения» в grokking.md) · 2311.18817, 2407.12332, 2505.20172 · 2026-08-24
- `2212.07677` · Transformers Learn In-Context by Gradient Descent (von Oswald et al., ICML 2023) · демотирована из корпуса (task-019): предмет — механизм обучения в контексте, гроккинг — один хеджированный абзац приложения A.11; глобально самая цитируемая работа подборки (843) · см. записи в карточках понятий · 2026-08-25
- `2406.03999` · Unveiling the Dynamics of Information Interplay in Supervised Learning (Zhang et al.) · демотирована из корпуса (task-019): предмет — нейронный коллапс; гроккинг — один подраздел из пяти без теории · см. записи в карточках понятий · 2026-08-25
- `2406.05335` · Phase Transition in Large Language Models and the Criticality of Natural Languages (Nakaishi et al.) · демотирована из корпуса (task-019): не о гроккинге — критичность естественного языка; соседствует по существу · см. записи в карточках понятий · 2026-08-25
- `2502.20763` · Information-Theoretic Perspectives on Optimizers (Tan & Huang) · демотирована из корпуса (task-019): о гроккинге ни слова, попала в подборку по смежности приёма · см. записи в карточках понятий · 2026-08-25
- `2601.21150` · Can Neural Networks Learn Small Algebraic Worlds? (Kvinge et al.) · демотирована из корпуса (task-019): гроккинг появляется одной догадкой; корпусу ценен приём · см. записи в карточках понятий · 2026-08-25
- `2211.11052` · Convexifying Transformers (Ergen, Neyshabur & Mehta) · триаж task-021: предмет — выпуклый анализ внимания; гроккинг — один из двух опытов, привязка к невыпуклости — авторская догадка · пока никто · 2026-08-26
- `2212.04458` · General-Purpose In-Context Learning by Meta-Learning Transformers (Kirsch et al.) · триаж task-021: предмет — мета-обучение ICL; гроккинг — приложение A.6 с отрицательным итогом · пока никто · 2026-08-26
- `2302.09160` · Identifying Equivalent Training Dynamics (Redman et al.) · триаж task-021: купмановская эквивалентность динамик; гроккинг — наименьшее из пяти применений · 2605.08237 · 2026-08-26
- `2310.11282` · ChapGTP, ILLC's Attempt at Raising a BabyLM (Jumelet et al.) · триаж task-021: конкурсный отчёт BabyLM; гроккинг — одна из неудавшихся стратегий · пока никто · 2026-08-26
- `2310.12956` · Eureka-Moments in Transformers (Hoffmann et al.) · триаж task-021: статья сама отделяет своё явление от гроккинга (softmax-причина, нет зазора train/val) · пока никто (цитирует внешняя 2506.13688) · 2026-08-26
- `2311.04163` · Outliers with Opposing Signals… (Rosenfeld & Risteski) · триаж task-021: гроккингу отведены два хеджированных предложения без опытов · пока никто · 2026-08-26
- `2311.04354` · Uncovering Intermediate Variables in Transformers using Circuit Probing (Lepori et al.) · триаж task-021: инструмент интерпретируемости; гроккинг — одно из четырёх применений · пока никто (цитирует внешняя 2508.15841) · 2026-08-26
- `2402.10688` · Towards Uncovering How Large Language Model Works (Zhao et al.) · триаж task-021: обзор объяснимости LLM; гроккинг — полторы страницы пересказа · 2504.03162 · 2026-08-26
- `2403.06925` · Transformers Learn Low Sensitivity Functions (Vasudeva et al.) · триаж task-021: предмет — чувствительность как индуктивное смещение; гроккинг — третье из трёх следствий · пока никто · 2026-08-26
- `2409.16767` · Exploring Information-Theoretic Metrics Associated with Neural Collapse (Song et al.) · триаж task-021: расширенная версия 2406.03999, держать вместе; гроккинг — один подраздел · пока никто · 2026-08-26
- `2501.02436` · Network dynamics-based framework for understanding DNNs (Lin, Feng et al.) · триаж task-021: рамка OPT/NPT; гроккинг — показательный случай; ценно предупреждение о BatchNorm · пока никто · 2026-08-26
- `2501.12391` · Physics of Skill Learning (Liu et al.) · триаж task-021: физика освоения навыков; гроккинг — одна из арен; ценно «SignGD снимает задержку» · пока никто · 2026-08-26
- `2502.01628` · Harmonic Loss Trains Interpretable AI Models (Baek et al.) · триаж task-021: новая функция потерь; гроккинг — один из трёх устраняемых изъянов · пока никто · 2026-08-26
- `2502.21009` · Position: Solve Layerwise Linear Models First… (Nam et al.) · триаж task-021: позиционная статья; гроккинг — одно из четырёх подводимых явлений; ценна шкала Sigma0/S · 2603.01968 · 2026-08-26
- `2503.10065` · Do We Always Need the Simplicity Bias? (Teney et al.) · триаж task-021: мета-обучение активаций; гроккинг — одна из четырёх областей; ценно 10x ускорение сплайнами · пока никто · 2026-08-26
- `2504.12916` · Exact Learning Dynamics of In-Context Learning in Linear Transformers (Mainali & Teixeira) · триаж task-021: точная динамика ICL; гроккинг — одно из двух приложений · пока никто · 2026-08-26
- `2504.20571` · RL for Reasoning in LLMs with One Training Example (Wang et al.) · триаж task-021: 1-shot RLVR; гроккинг — блок цитат для отделения post-saturation generalization · пока никто · 2026-08-26
- `2506.13688` · What Happens During the Loss Plateau? (Gopalani & Hu) · триаж task-021: статья сама размежёвывается с гроккингом (плато до первого решения, нет зазора) · пока никто · 2026-08-26
- `2506.23679` · Learning Modular Exponentiation with Transformers (Africa et al.) · триаж task-021: мехинтерп модульного возведения в степень; гроккинга как явления нет · 2601.09049 · 2026-08-26
- `2508.04401` · Why are LLMs' abilities emergent? (Havlík) · триаж task-021: философская статья; гроккинг — раздел-контрпример к сведению эмерджентности к масштабу · пока никто · 2026-08-26
- `2508.15841` · A Review of Developmental Interpretability in LLMs (Kendiukhov) · триаж task-021: обзор; гроккинг — один абзац с одной ссылкой · пока никто · 2026-08-26
- `2510.00468` · Feature Identification via the Empirical NTK (Lin) · триаж task-021: eNTK как словарь признаков; гроккинг — обстановка двух игрушечных опытов · пока никто · 2026-08-26
- `2510.25791` · The Kinetics of Reasoning (Pengmei et al.) · триаж task-021, решение пользователя: CoT «через призму гроккинга», отграничение от позднего обучения слабое · пока никто · 2026-08-26
- `2512.04165` · Mitigating the Curse of Detail (Rubin, Davidovich & Ringel) · триаж task-021: скейлинговая эвристика FL; гроккинг — сноска с хеджем · пока никто · 2026-08-26
- `2512.13568` · Superposition as Lossy Compression (Bereska et al.) · триаж task-021: мера суперпозиции через SAE; гроккинг — стенд одного прогона · пока никто (цитирует внешняя 2605.16325) · 2026-08-26
- `2601.08316` · Deep Exploration of Epoch-wise Double Descent in Noisy Data (Kubo, Uda & Iida) · триаж task-021: предмет — двойной спуск при шуме меток; ценно для карточки double-descent · пока никто · 2026-08-26
- `2603.11161` · Algorithmic Task Capture… of Infinite Transformers (Davidovich & Ringel) · триаж task-021: критерий «захвата алгоритма» неприменим к канонической постановке гроккинга · пока никто · 2026-08-26
- `2604.19740` · Generalization at the Edge of Stability (Tuci et al.) · триаж task-021: случайные динамические системы; гроккинг — один из проверочных опытов · пока никто · 2026-08-26
- `2605.01172` · A Theory of Generalization in Deep Learning (Litman & Guo) · триаж task-021: теория через eNTK; гроккинг — одно из четырёх подводимых явлений; ценен SNR-предобуславливатель · пока никто · 2026-08-26
- `2605.04230` · Layerwise LQR for Geometry-Aware Optimization (Dufort-Labbé et al.) · триаж task-021: оптимизатор; гроккингу отведены двенадцать строк · пока никто · 2026-08-26
- `2605.06258` · The Weight Gram Matrix Captures Sequential Feature Linearization (Cha et al.) · триаж task-021: рамка обучения признаков; гроккинг-прогоны испытательные · пока никто · 2026-08-26
- `2605.15551` · Characterizing Learning in DNNs using Tractable Algorithmic Complexity (Bakhtiarifard et al.) · триаж task-021: оценка KCS-сложности QuBD; гроккинг — один из шести пунктов применений · пока никто · 2026-08-26
- `2605.16325` · Phase Transitions in Driven Informational Systems (Truong) · триаж task-021: программная perspective без опытов; ценно возражение картине фиксированного ландшафта · пока никто · 2026-08-26
- `2605.28975` · A Training-Time Diagnostic for Generalization via the Log-Alignment Ratio (Shehper & Vaswani) · триаж task-021: диагностика LAR; гроккинг — испытательный стенд · пока никто · 2026-08-26
- `2606.00045` · Universal Quantum Transformer for Exact Reasoning (Chung & Talebpour) · триаж task-021: квантовая архитектура; «кристаллизация» строже гроккинга при слабой проверке · пока никто · 2026-08-26
- `2606.20737` · Repeated Shared Access Enables Grokking, but Edit Propagation… (Niu) · триаж task-021: предмет — распространение фактовой правки; гроккинг — OOD-барьер · пока никто · 2026-08-26
- `2606.21158` · Dead-Direction Signatures (Shirodkar & Narayanan) · триаж task-021: дешёвая спектральная оценка LLC/RLCT; гроккинг — стенд · пока никто · 2026-08-26
- `2606.23044` · Prime Fourier Embeddings (Hwang, Bae & Lee) · триаж task-021: готовые фурье-вложения (сторона входа); о времени генерализации ничего · пока никто · 2026-08-26
