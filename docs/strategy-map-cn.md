下面是你上传的 **“Strategy Screening Map for 29 Asset-Allocation Strategies Using a 32-Indicator Macro Set”** 的中文翻译版（尽量保持原文结构与术语一致）。 

---

# 基于 32 个宏观指标集，对 29 种资产配置策略的「策略筛选地图」

## 在你的管线里，“策略筛选”到底是什么意思？

你的管线把资产配置系统里经常被混在一起的 3 类决策拆开了：

宏观数据 → 特征工程 → 宏观状态识别（LLM）→ 策略池筛选（规则 + LLM）

其中，“策略池筛选（strategy pool screening）”更适合被定义为**准入（gating）+ 优先级（prioritization）层**，而不是产出最终权重的那一层。实际落地里，筛选层主要回答这些问题：

* **这条策略现在适不适合跑？**（可用 / 不可用）
* **应该跑得多激进？**（risk-on 还是 risk-off 的仓位力度）
* **它应该做“主引擎”还是只做“卫星策略”？**（优先级排序）

这个区分很关键，因为你 29 条策略里有不少其实是**组合构建框架**：它们真正的输入是市场收益分布的统计量（期望收益、波动率、相关性），而不是宏观序列（例如 Markowitz 均值-方差、风险平价、动态风险预算）。宏观指标仍然有价值，但更多用于：**设约束、给先验/观点、触发风控覆盖**。尤其是均值-方差优化，本质上是围绕 μ/Σ（期望收益与协方差）工作的，而不是围绕宏观指标。 

---

## 有研究背书的“宏观/金融条件 → 策略选择”的驱动逻辑

一个可辩护的筛选地图，需要把每条策略锚定到**它已知敏感的经济/金融变量**，再把这些变量映射到你的 32 个指标。

### 1）优化框架（MVO、Black–Litterman）

* **Markowitz/MVO**：由收益的联合分布驱动（均值、方差、协方差）。 
* **Black–Litterman**：常被描述为把“均衡基线（反向优化）”与“主观观点（views）”结合，从而让 MVO 更稳健、更可解释。 
  在宏观驱动的体系里，views 很自然来自增长/通胀/流动性等状态，但优化器最终仍然消费的是 μ/Σ。

### 2）风险预算 / 风险平价 / 全天候

* **风险预算**：把组合风险分解为各资产的风险贡献；**风险平价**是其中一种特例（目标更偏“平衡风险贡献”）。 
* **All Weather（全天候）**：显式围绕不同经济环境（常见表述是增长/通胀四象限）来设计。 
  因此这些策略的筛选关键宏观状态会围绕：**增长、通胀、融资环境**（因为通胀与利率上行时，杠杆和久期暴露的行为会很不一样）。 

### 3）趋势/动量 与 波动率缩放

* **时间序列动量（trend following）**：跨股票、债券、外汇、商品期货，1–12 个月的过去收益对短期方向常被发现有预测性。 
* **波动率管理/波动率目标**：波动率高时降仓，证据显示波动率择时会显著改变夏普（通过在波动率飙升期降低风险）。 
  实务研究也指出：风险平价、波动率目标、趋势策略在波动上行时可能被迫去杠杆，所以“隐含波动率/压力指标”应该是筛选层的一等公民。 

### 4）Carry（套息/期限利差）与 Spread（利差）策略

“Carry”已被形式化为跨资产类别的事前收益来源（不止 FX）。Carry 溢价往往与衰退/流动性/波动率风险暴露有关——与你信用与风险情绪模块试图捕捉的风险高度一致。 

### 5）金融条件 / 金融压力

* **金融条件指数（FCI）**：总结融资可得性与融资成本（常混合利率、利差、价格、波动率），央行与机构常用它监控对未来增长与通胀的风险。 
* **金融压力指数（FSI）**：也会组合资金面、信用、估值、安全资产、波动率等类别——与你的 `DR007`、信用利差、`ERP_EP_10Y`、`IV_OPTION_INDEX` 很接近。 

### 6）信用利差作为宏观金融预警

研究显示公司信用利差（以及类似“excess bond premium”成分）含有关于信用供给冲击、后续经济活动与资产价格的信息。这支持把 `CREDIT_SPREAD_AAA_10Y` 放在“压力触发去风险/信用周期轮动”的中心。 

### 7）股权风险溢价 / 盈利收益率 vs 债券收益率

股权风险溢价（ERP）是资产配置核心风险定价变量。实务里的“盈利收益率减去 10 年期收益率”类指标（常与 “Fed model” 争论相关）在估值讨论里历史悠久，也常被用于 ERP 信号框架。支持你用 `ERP_EP_10Y` 作为权益-债券切换/估值筛选变量。 

### 8）黄金：通胀/尾部风险对冲与分散

机构研究支持黄金的长期通胀对冲属性（短期不稳定）。它也常被研究为分散器与潜在尾部风险对冲组件。这支持将黄金（`XAUUSD_AVG_MONTHLY`）映射到“通胀保护 / 尾部对冲”筛选逻辑。 

---

## 从 32 个原始指标，抽象成用于筛选的稳定“宏观状态”

生产级筛选系统通常不会直接用原始序列，而是用更稳定、可解释、变化更慢的**状态向量**（避免噪声与过度反应）。

下面给出一套常见且可辩护的映射：把你的 32 指标工程化为筛选状态（可用 z-score、滚动分位数、3–12 个月动量等）。

* **增长状态（G）**：8 个增长指标合成。PMI 常被视为领先指标。
  输入：`AUTO_RETAIL_SALES_YOY`, `INDUSTRY_YOY`, `INVESTMENT_FA_YOY`, `PMI_MANU`, `PMI_SERV`, `POWER_GEN_YOY`, `REAL_GDP_QOQ`, `RETAIL_SALES_YOY` 

* **通胀状态（π）**：通胀压力水平与方向。
  输入：`CPI_YOY`, `PPI_YOY` 

* **流动性/信用状态（L/C）**：货币与社融 vs 资金成本。TSF 在中国语境下常被视为“广义信用”。
  输入：`M2_YOY`, `M1_YOY`, `M1_M2_GAP`, `TSF_STOCK_YOY`, `DR007` 

* **利率与期限状态（R）**：利率水平与“曲线压力”的近似。可用 `(YIELD_10Y − DR007)` 或 `(YIELD_10Y − LPR_1Y)` 作为简化期限利差代理。
  输入：`LPR_1Y`, `YIELD_10Y` + 衍生利差 

* **风险溢价状态（RP）**：权益与信用风险的定价。
  输入：`CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y` 

* **金融条件/压力状态（FCI/FSI）**：资金、利差、估值、汇率压力、波动率的综合。
  输入：`DR007`, `LPR_1Y`, `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y`, `FX_CNY_MID_MONTHLY`, `IV_OPTION_INDEX` 

* **外需/贸易状态（X）**：进出口周期。
  输入：`EXPORT_CUMULATIVE_YOY`, `IMPORT_CUMULATIVE_YOY`, `TRADE_TOTAL_CUMULATIVE_YOY` 

* **汇率压力 / 对冲状态（FX/Gold）**：
  输入：`FX_CNY_MID_MONTHLY`, `XAUUSD_AVG_MONTHLY` 

* **内需拖累状态（地产/就业）**：中国周期细化与“政策宽松概率”判断。
  输入：`RESID_HOUSE_AREA_CUMULATIVE_YOY`, `RESID_HOUSE_SALES_CUMULATIVE_YOY`, `URBAN_SURVEY_UNEMPLOYMENT`, `URBAN_YOUTH_UNEMPLOYMENT` 

* **投资信心状态（I）**：总投资与民间投资动量差异。
  输入：`INVESTMENT_FA_PRIVATE_YOY`, `INVESTMENT_FA_YOY` 

* **风险情绪状态（V）**：隐含波动率（期权反映的“未来不确定性”）。
  输入：`IV_OPTION_INDEX` 

这些工程化状态，才是 LLM 应该“命名”的对象（例如“增长改善 / 流动性收紧 / 压力抬升”），也是策略筛选器应直接消费的输入。 

---

## 策略 ↔ 指标映射表（筛选所需的最小指标集）

解释：每行列出驱动筛选的**最小可用宏观指标集合**——用于决定
(a) 是否可用（eligible），(b) 优先级与力度（priority / aggressiveness）。
若策略的核心信号依赖市场价格（动量、回撤、组合波动等），这里列的是**宏观层面的 gating/覆盖**而非策略全部信号。

| 策略（你的 29 条）          | 决定筛选的宏观指标（来自 32）                                                                                                                                      |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| 60/40 股债（带区间约束）      | `ERP_EP_10Y`, `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `IV_OPTION_INDEX`, `DR007`                                                                        |
| 均值-方差优化（MVO）         | 用于参数稳定性/风控 gating：`IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `FX_CNY_MID_MONTHLY`（核心优化仍用 μ/Σ）                                               |
| Black–Litterman      | 用于宏观 views：增长块（8项）、`CPI_YOY`, `PPI_YOY`, `TSF_STOCK_YOY`, `M1_M2_GAP`, `DR007`, `YIELD_10Y`, `ERP_EP_10Y`, `CREDIT_SPREAD_AAA_10Y`                    |
| 风险预算框架（SAA）          | 风险状态切换筛选：`IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `YIELD_10Y`, `CPI_YOY`                                                                  |
| 风险平价（ERC 特例）         | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `CPI_YOY`, `YIELD_10Y`（通胀/利率压力影响久期风险）                                                            |
| 全天候（All Weather）     | 增长块（8）+ 通胀（2）+ 杠杆/压力 gating：`IV_OPTION_INDEX`, `DR007`, `YIELD_10Y`                                                                                   |
| 因子配置（长期倾斜）           | 状态背景：增长（8）、通胀（2）、流动性/信用（5）、`ERP_EP_10Y`（因子收益本身需额外数据）                                                                                                  |
| 目标日期基金（TDF 路径）       | 主要由生命周期驱动；宏观叠加去风险：`IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`                                                                                 |
| 核心-卫星（核心仓）           | 卫星启停与风险预算：`IV_OPTION_INDEX`, `ERP_EP_10Y`, `CREDIT_SPREAD_AAA_10Y`, `DR007`                                                                           |
| 宏观周期轮动               | 增长（8）、通胀（2）、流动性/信用（5）、利率（`YIELD_10Y`,`LPR_1Y`）、风险溢价（`ERP_EP_10Y`,`CREDIT_SPREAD_AAA_10Y`）                                                             |
| 政策周期轮动               | 资金 + 信用 + 财政：`DR007`, `TSF_STOCK_YOY`, `M1_M2_GAP`, `LPR_1Y`, `YIELD_10Y`, `GOV_EXPEND_CUMULATIVE_YOY`（政策文本/NLP 另算）                                   |
| 信用周期轮动               | `CREDIT_SPREAD_AAA_10Y`, `TSF_STOCK_YOY`, `DR007`, `M1_M2_GAP`, `YIELD_10Y`                                                                           |
| 趋势/动量（6–12M）         | 动量需价格；宏观 gating：`IV_OPTION_INDEX`, `DR007`, `FX_CNY_MID_MONTHLY`, `CREDIT_SPREAD_AAA_10Y`                                                             |
| 估值分位数调整              | `ERP_EP_10Y`, `YIELD_10Y`, `CPI_YOY`（真正估值分位需更多估值数据）                                                                                                   |
| 股债相对价值（ERP 切换）       | `ERP_EP_10Y`, `YIELD_10Y`, `CPI_YOY`（需要实质利率语境）                                                                                                        |
| 极端下逆向/均值回归           | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `FX_CNY_MID_MONTHLY`                                                                             |
| 因子择时                 | 增长（8）、通胀（2）、信用/流动性：`TSF_STOCK_YOY`, `M1_M2_GAP`, `DR007` + `IV_OPTION_INDEX`（因子 P&L 另需数据）                                                             |
| FCI 策略               | `DR007`, `LPR_1Y`, `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `ERP_EP_10Y`, `FX_CNY_MID_MONTHLY`, `IV_OPTION_INDEX`                                        |
| GTAA（全球战术配置）         | 现有偏国内宏观：`FX_CNY_MID_MONTHLY`, `XAUUSD_AVG_MONTHLY`, `EXPORT_CUMULATIVE_YOY`, `IMPORT_CUMULATIVE_YOY`, `YIELD_10Y`, `IV_OPTION_INDEX`（真正 GTAA 更需要全球宏观） |
| Carry / Spread 策略    | `YIELD_10Y`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `CPI_YOY`                                                                                              |
| FX 叠加层（外汇对冲/暴露）      | `FX_CNY_MID_MONTHLY`, `DR007`, `YIELD_10Y`, `IV_OPTION_INDEX`, `TRADE_TOTAL_CUMULATIVE_YOY`                                                           |
| 波动率目标（vol targeting） | `IV_OPTION_INDEX`, `DR007`, `CREDIT_SPREAD_AAA_10Y`（组合波动来自收益序列；宏观主要 gating）                                                                           |
| 动态风险预算               | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `ERP_EP_10Y`                                                                                     |
| CPPI / OBPI（组合保险）    | 压力 + 资金：`IV_OPTION_INDEX`, `DR007`, `CREDIT_SPREAD_AAA_10Y`, `YIELD_10Y`（机制依赖净值路径/期权）                                                                 |
| 最大回撤控制               | 宏观预警 + 危机过滤：`IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `FX_CNY_MID_MONTHLY`（回撤来自组合/价格序列）                                                    |
| 尾部风险对冲               | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `XAUUSD_AVG_MONTHLY`, `FX_CNY_MID_MONTHLY`                                                                |
| 金融压力触发去风险            | `IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`, `ERP_EP_10Y`, `FX_CNY_MID_MONTHLY`                                                               |
| 流动性敏感配置              | `DR007`, `M1_YOY`, `M2_YOY`, `M1_M2_GAP`, `TSF_STOCK_YOY`, `CREDIT_SPREAD_AAA_10Y`                                                                    |
| 宏观 × 趋势 × 风险预算（集成）   | 宏观：增长（8）+ 通胀（2）+ 信用/流动性（5）；执行风控：`IV_OPTION_INDEX`, `CREDIT_SPREAD_AAA_10Y`, `DR007`；外部冲击：`FX_CNY_MID_MONTHLY`                                         |



---

## 让筛选在生产环境更稳健的实操要点

你的 32 指标来自不同官方与市场来源，频率和发布时间滞后不同（宏观发布、金融市场日频/月频等）。筛选层如果忽略发布时间、修订、数据“陈旧度”，会很容易失真。所以有三条实践经验特别重要：

1. **区分“水平（level）”与“方向（direction）”**
   很多 regime 的变化不是“高/低”，而是“加速/减速”。增长和信用尤其如此：yoy 可能仍然很弱，但边际在改善。

2. **把少数“压力/金融”指标提升为一等 gating 变量**
   FCI/FSI 文献强调利差与波动率比慢宏观更能反映融资紧张与风险偏好。在你的集合里，这意味着：`CREDIT_SPREAD_AAA_10Y`、`DR007`、`IV_OPTION_INDEX` 几乎应该出现在所有风控筛选里，并且要对趋势、carry、杠杆型风险平价等策略的激进程度设置上限。 

3. **把 FCI 当作“综合分数”，不要只看单一变量**
   FCI 通常是多维金融变量的汇总。你已具备一个中国语境下紧凑的子集（利率、利差、汇率、波动率），用综合“收紧分数”会比用单项更稳定。 

---

## 局限性：后续可能需要补哪些数据

有些策略如果只用宏观筛选，会丢失其核心信号：

* **趋势/动量、回撤控制、波动率目标、CPPI/OBPI**：需要资产/组合价格路径或期权面；宏观更适合做覆盖而不是主信号。 
* **估值分位与因子择时**：`ERP_EP_10Y` 已是很好的起点，但完整估值分位通常需要更明确的估值序列（PE/PB/盈利收益率/利润率等）与因子收益历史。 
* **GTAA**：健壮的 GTAA 通常需要全球增长/通胀/利率及跨国相对估值/利差；仅用国内宏观 + 少量 FX/黄金只能算“部分版本”。 

即便如此，你当前的 32 指标集已经足以实现一个可生产的筛选层，只要你把它定位为：**宏观状态识别 + 金融条件/压力 gating + 策略优先级排序**，而把真正的 alpha 与仓位细节留给消费市场价格数据的策略引擎。 
