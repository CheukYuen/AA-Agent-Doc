Zhuoyuan (Leon) Lin
Target Roles: FinTech AI Product / Tech Lead · LLM Application Engineer · Agentic Systems Engineer · AI-Native Full-Stack
Location: [Taipei / Remote] · Phone: [xxx] · Email: [xxx] · LinkedIn: [link] · GitHub: [link] · Portfolio: [link]

SUMMARY
AI product engineer who transitioned from front-end engineering to LLM systems in 2025. Personally led a finance-vertical AI Investment Advisor from 0→1, delivering an institution-ready asset allocation workflow: SAA (constraint-based allocation) + TAA (state-driven tilts) + transaction-friction-aware rebalancing (min-change trades). Built an agentic architecture with dynamic context assembly and governance. Supporting capability: fine-tuned Qwen3-4B-Instruct-2507 on 100k labeled samples as an Intent Router for intent recognition + dynamic prompt-template routing, validated online.

KEY HIGHLIGHTS (Asset Allocation + Agentic Systems)

* Institution-ready asset allocation engine: SAA + TAA + executable rebalancing under trading frictions to avoid “paper optimal, non-tradable” outputs.
* Transaction friction + min-change rebalancing: enforce trading costs, turnover caps, and tradability constraints; trigger rebalancing by risk thresholds / drift bands.
* Strategy selector (not just “more strategies”): strategy–indicator mapping matrix to retrieve only relevant signals per strategy; reduces noisy context injection and drift.
* Profitability proof loop with controls: end-to-end backtest (no look-ahead + realistic costs + turnover constraints) + Monte Carlo preserving state/serial structure + forward validation (shadow/small live). Includes random switching vs single-strategy controls to eliminate “selection illusion.”
* Agentic roadmap (kept as first-class capability):

  * Skills engineering: optimizer + vector math modules (convex optimization for weights, utility maximization, configurable constraints, auditable outputs)
  * MCP external information layer: pluggable ingestion and structuring of market news, index quotes, earnings, and government/regulatory reports, with source traceability and timestamp alignment
  * Dynamic prompt templates: fragment-based decomposition of persona/task/rules/data/tools/output, assembled by a dispatcher per scenario
* Governance: regression sets, drift monitoring, staged rollout and rollback, audit trails for compliance and internal/external reviews.

CORE SKILLS

* Asset Allocation Engineering: constraint-based allocation, risk guardrails, TAA tilt bands, transaction-cost-aware rebalancing, SAA/TAA/friction attribution
* Agentic Systems: Decision/Execution/Explanation pipelines, dynamic context orchestration, prompt fragments, tool/function calling, routing and fallbacks
* LLM Engineering: structured generation (JSON/CSV), schema contracts, evaluation + regression, online A/B validation + monitoring
* Model Fine-Tuning: Qwen3-4B fine-tuning, intent taxonomy, 100k labeling pipeline, confidence gating, safe iteration with versioning
* Data Pipeline: frequency alignment, missing values, definition checks, timestamp alignment, source traceability, standardized outputs
* Full-Stack: web engineering, API design, observability, performance optimization

EXPERIENCE

[Ping An Bank / FinTech Company] — Software Engineer / AI Product Engineer (title placeholder)
Dec 2017 – Present | [City, Country]

AI Investment Advisor (Finance Vertical LLM Application) — Personal Lead
Jan 2025 – Present

* Built an institution-ready asset allocation workflow: SAA (constraint-based) + TAA (macro state-driven) + rebalancing under transaction frictions, producing executable allocation ranges and rebalancing actions with explainable narratives.
* Implemented transaction friction + min-change rebalancing: triggered by risk thresholds / drift bands; executed minimum-change trades under trading costs, turnover caps, and tradability constraints.
* Designed a strategy selector driven by a strategy–indicator mapping matrix:

  * Strategy library: 20+ portfolio strategies (Risk Parity / Black–Litterman / Mean–Variance Optimization, etc.)
  * Indicator pool: 30+ macro/market/fundamental indicators
  * Selection: retrieve only relevant signals per strategy instead of dumping all data into the model
* Built a “quality lock” for high-signal outputs: expert persona anchoring + cleaned structured feature injection (JSON/CSV) to force reasoning over features rather than generic text completion.
* Built a profitability proof & validation loop with controls:

  * End-to-end backtests (no look-ahead; realistic costs [placeholder]; turnover limits; tradability constraints)
  * Controls: single-strategy vs random switching vs the strategy selector to eliminate “selection illusion”
  * Monte Carlo preserving serial correlation/state structure to estimate win probability, outperformance probability, and left-tail risk
  * Forward validation via shadow accounts / small live allocations to verify signal reproducibility and execution realism
* Agentic system design and production roadmap:

  * Skills engineering: optimizer + vector math modules (convex optimization for weights, utility maximization, configurable constraints, auditable outputs)
  * MCP external information layer: pluggable ingestion/structuring of market news, index quotes, earnings, government/regulatory reports; source traceability and timestamp alignment
  * Dynamic prompt templates: fragment-based persona/task/rules/data/tools/output, assembled by a dispatcher per scenario
* Governance and operations: drift monitoring, regression suites, staged releases with rollback, suitability constraints, hard risk limits, full audit trails; attribution for SAA/TAA/friction to guide iteration.

Impact (fill later)

* Backtest window: [YYYY–YYYY]
* CAGR / Sharpe / Max Drawdown: [X / X / X]
* Turnover: [X] · Cost model: [X] · p95 latency: [X ms]
* Forward validation: [X weeks/months] · win/outperform rate: [X%]
* #Intents: [N] · #Templates: [N] · #Strategies: [20+] · #Indicators: [30+]

Front-End Engineering (Product/Platform)
Dec 2017 – Dec 2024

* Delivered and maintained production web applications with modular architecture, performance optimization, and reliable integrations.
* Owned features end-to-end from UI delivery to production support and cross-team collaboration.
* Tech stack (edit): TypeScript/JavaScript, [React/Angular], [Node.js], [SQL/NoSQL], CI/CD, monitoring.

Google (Sunnyvale, CA) — Web Developer (Contract)
Mar 2015 – Feb 2017

* Shipped production web features using AngularJS / Closure Library.
* Built async UX patterns: search/filter/navigation/pagination for large datasets.
* Integrated data-backed views (e.g., BigQuery-driven) and improved performance and compatibility.

Cinequest, Inc. (San Jose, CA) — Web Developer Intern
Jun 2014 – Dec 2014

* Built event websites with HTML5/CSS3/jQuery and backend endpoints with PHP + MySQL.
* Supported mobile-friendly experiences and basic operational tooling.

SUPPORTING CAPABILITY (Model Training / Routing)

Intent Router Fine-Tuning — Qwen3-4B-Instruct-2507 (100k labeled + online validation)

* Fine-tuned Qwen3-4B-Instruct-2507 for intent recognition + dynamic prompt-template routing.
* Built intent taxonomy, 100k labeling pipeline, offline evaluation ([Accuracy/F1 placeholder]) and regression suites.
* Validated online via A/B + monitoring; deployed with confidence thresholds and fallbacks as the routing gate for downstream execution.

EDUCATION
Santa Clara University — M.S., Computer Science & Engineering | Dec 2014
Guangzhou University — B.S., Mathematics & Applied Mathematics | Jul 2011

ADDITIONAL TRAINING
Stanford CS336 — Large Language Models (Completed)

TECHNOLOGIES (edit to match reality)
Languages: TypeScript/JavaScript, Python, [Java/Go/…]
Frontend: [React/Angular], SPA architecture, performance optimization
Backend/Data: REST APIs, [Node/FastAPI], [PostgreSQL/MySQL/MongoDB], data pipelines & QA
LLM/Agents: prompt/schema, structured generation, dynamic prompt fragments, tool calling, routing/orchestration, fine-tuning (Qwen3-4B), eval & regression, monitoring
Infra: [Docker], [CI/CD], [Cloud], observability ([Prometheus/Grafana/…])

