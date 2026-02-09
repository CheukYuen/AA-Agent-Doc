下面是**完整英文简历（已把“AI 智能投资顾问产品”提升为主叙事，Qwen3-4B 微调作为支撑能力放在第二层）**。缺少信息我都用 **[PLACEHOLDER]** mock 了，你后续直接替换即可。口径已统一：**Qwen3-4B-Instruct-2507**。

---

## Zhuoyuan (Leon) Lin

**Target Roles:** AI Product Engineer (FinTech) · LLM Application Engineer · AI Tech Lead (AI-Native Full-Stack)
**Location:** [Taipei, Taiwan] · **Phone:** [xxx] · **Email:** [xxx] · **LinkedIn:** [link] · **GitHub:** [link] · **Portfolio:** [link]

---

### SUMMARY

AI product engineer who transitioned from front-end engineering to LLM systems in 2025. Personally led an **AI Investment Advisor** (finance vertical LLM application) end-to-end—from strategy framework and data pipeline to orchestration, evaluation, and governance. Specialized in **shrinking the solution space** of LLM outputs via **persona anchoring + structured data injection + strategy–indicator mapping**, producing controllable, auditable recommendations. Fine-tuned **Qwen3-4B-Instruct-2507** on **100k** labeled samples as an **Intent Router** to drive dynamic prompt-template assembly and stable downstream execution.

---

### PRODUCT HIGHLIGHTS (AI INVESTMENT ADVISOR)

* **Quality Lock Mechanism (“Why → How”):** aligned LLM latent space to professional finance personas and forced reasoning through **JSON/CSV structured features**, turning generic advice into constrained, high-signal outputs.
* **Strategy System:** a library of **20+ portfolio strategies** (e.g., Risk Parity, Black–Litterman, Mean–Variance Optimization) mapped to **30+ macro/market indicators** via a **strategy–indicator matrix** for precise context selection.
* **AI Agent Roadmap:** data pipeline + compute skills (optimizer/vector math) + external info ingestion (news/indices/earnings/regulatory) + pluggable prompt fragments assembled by a dispatcher.
* **Profitability Proof Framework:** end-to-end backtesting (no look-ahead, real costs, turnover constraints), Monte Carlo preserving state structure, and forward validation via shadow/live small-scale accounts.
* **Lifecycle Management:** drift/risk-triggered rebalancing with transaction constraints, attribution (SAA/TAA/friction), compliance audit trails, and version governance with safe rollback.

---

### CORE SKILLS

* **AI Product & Architecture:** AI-native workflow design, deterministic+LLM separation, release gates, version governance, auditability
* **LLM Engineering:** prompt/schema design, structured generation (JSON/CSV), dynamic context assembly (fragments), tool/function calling, routing/orchestration, multi-agent workflows
* **Evaluation & Governance:** regression sets, offline metrics, online validation (A/B + monitoring), fallbacks & confidence gating, traceability
* **FinTech / Quant Concepts:** MVO/SAA/TAA concepts, constraints/guardrails, risk attribution, backtest/Monte Carlo/forward validation thinking
* **Full-Stack Delivery:** web app engineering, API design, data pipelines & QA, performance optimization

---

## EXPERIENCE

### [Ping An Bank / FinTech Company] — Software Engineer / AI Product Engineer (Title placeholder)

**Dec 2017 – Present | [City, Country]**

#### AI Investment Advisor (Finance Vertical LLM Product) — Personal Lead

**Jan 2025 – Present**

* Led a **0→1 AI Investment Advisor** product: defined strategy framework, designed system architecture, implemented pipelines and orchestration, and set up evaluation + governance for safe iteration.
* Built a **“Quality Lock” output mechanism**:

  * Anchored the model with a **professional finance persona** (e.g., macro hedge fund PM) to constrain generation style and domain priors.
  * Injected **cleaned, structured inputs (JSON/CSV)** instead of narrative text to force reasoning over features rather than generic completion.
  * Implemented a **strategy–indicator mapping matrix** to retrieve only relevant signals per strategy (e.g., CPI/PMI/commodity vol for all-weather; valuation & cashflow features for value).
* Designed a scalable **strategy system**:

  * Maintained a library of **20+ strategies** and **30+ indicators** with explicit data definitions, frequency alignment, and feature contracts.
  * Standardized preprocessing: frequency normalization, missing-value handling, definition checks, timestamp alignment, and output standardization.
* Engineered an AI-agent roadmap for production:

  * **Skills layer:** optimizer + vector math (convex optimization, utility maximization, configurable & auditable constraints).
  * **External Info layer (MCP concept):** pluggable ingestion to structure news, index quotes, earnings, and government/regulatory documents with source traceability and timestamp alignment.
  * **Dynamic Prompt Fragments:** decomposed “persona/task/rules/data/tools/output” into reusable fragments assembled by a dispatcher per scenario.
* Built a **profitability proof & validation framework**:

  * **End-to-end backtests** with no look-ahead, real trading costs, turnover constraints, and tradability constraints; benchmarked against static baselines and “random switching” controls.
  * **Monte Carlo simulations** preserving serial correlation/state structure to estimate win probability, outperformance probability, and left-tail risk.
  * **Forward validation** via shadow accounts / small live allocation to verify signal reproducibility and execution realism.
* Implemented **lifecycle management & governance**:

  * Risk/drift-triggered **rebalancing** with “minimum-change” trades under cost/turnover/tradability constraints.
  * Attribution for **SAA vs TAA vs friction** to drive iterative rule tuning and audit evidence.
  * Compliance-ready **audit trails**, suitability constraints, hard risk limits, and version rollout with safe rollback.

**Impact (fill later):**

* [Routing accuracy ↑ X% / Misroute ↓ Y%] · [Latency p95 ≤ X ms] · [Cost per request ↓ X%] · [Shadow/live validation hit rate ↑ X%] · [#intents: X] [#templates: Y]

---

#### Front-End Engineering (Web Product/Platform) — [Scope placeholder]

**Dec 2017 – Dec 2024**

* Delivered and maintained production web applications with strong engineering discipline: modular architecture, performance optimization, and reliable integrations.
* Owned features end-to-end from UI implementation to production support and cross-team collaboration.
* **Tech stack (edit):** TypeScript/JavaScript, [React/Angular], [Node.js], [SQL/NoSQL], CI/CD, monitoring.

---

### Google (Sunnyvale, CA) — Web Developer (Contract)

**Mar 2015 – Feb 2017**

* Shipped production web features using **AngularJS / Closure Library**.
* Built asynchronous UX flows: search/filter/navigation/pagination for large datasets.
* Integrated data-backed views (e.g., BigQuery-driven) and improved performance & compatibility.

---

### Cinequest, Inc. (San Jose, CA) — Web Developer Intern

**Jun 2014 – Dec 2014**

* Built event websites with **HTML5/CSS3/jQuery** and backend endpoints with **PHP + MySQL**.
* Delivered mobile-friendly experiences and basic operational tooling.

---

## MODEL TRAINING (SUPPORTING CAPABILITY)

### Intent Router Fine-Tuning (Qwen3-4B-Instruct-2507)

* Fine-tuned **Qwen3-4B-Instruct-2507** on **100k** labeled samples for **intent recognition + dynamic prompt-template routing**.
* Built intent taxonomy, dataset QA, offline evaluation ([Accuracy/F1 placeholder]), and regression suites for stable releases.
* Validated online via **A/B + monitoring**, deployed with confidence thresholds and fallbacks as the routing gate for downstream execution.

---

## SELECTED PROJECTS

### Strategy–Indicator Mapping Matrix (Context Selection Engine)

* Created a mapping between portfolio strategies and macro/market indicators to **select only relevant signals** per scenario.
* Reduced noisy context injection and improved recommendation stability and explainability.

### Profitability Proof Pipeline (Backtest + Monte Carlo + Forward Validation)

* End-to-end backtesting with realistic constraints; Monte Carlo simulation for probabilistic robustness; forward validation to avoid backtest illusions.

### Dynamic Prompt Fragment System (Pluggable Prompt Packs)

* Modularized prompts into reusable fragments (persona/task/rules/data/tools/output) assembled dynamically by a dispatcher.

---

## EDUCATION

**Santa Clara University** — M.S., Computer Science & Engineering | **Dec 2014**
**Guangzhou University** — B.S., Mathematics & Applied Mathematics | **Jul 2011**

---

## ADDITIONAL TRAINING

* Stanford **CS336** — Large Language Models (Completed)

---

## TECHNOLOGIES (EDIT TO MATCH REALITY)

* **Languages:** TypeScript/JavaScript, Python, [Java/Go/…]
* **Frontend:** [React/Angular], SPA architecture, performance optimization
* **Backend/Data:** REST APIs, [Node.js/FastAPI], [PostgreSQL/MySQL/MongoDB], data pipelines, QA
* **LLM/AI:** prompt/schema, structured generation, routing/orchestration, fine-tuning (Qwen3-4B), eval & regression, monitoring
* **Infra:** [Docker], [CI/CD], [Cloud], observability ([Prometheus/Grafana/…])

---

## OPTIONAL

**Open Source / Writing:** [links] · **Patents/Awards:** [links] · **Languages:** Chinese (Native), English ([Professional])

---
