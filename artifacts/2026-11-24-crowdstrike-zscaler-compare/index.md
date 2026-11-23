# CrowdStrike 与 Zscaler 全业务对比：业务线、部署、优劣与采购注意

> **不构成投资建议。** 本页做产品与业务对比，并分析一笔已签约的并购。CRWD、ZS 都是上市公司，请自行判断，不要据此买卖。
>
> 资料截至 2026-11-24。官方事实标注来源；我的判断单独放在「我的判断」一节。

## 一句话结论

CrowdStrike 从**端点、身份、云工作负载、SIEM** 这些地方"管人和设备在边界里做什么"；Zscaler 从**网络流量与零信任访问**这一层"管人和应用之间的连接"。两家直接卖同一件东西的地方不多，所以这笔全股票收购的逻辑是把「端点控制点」和「网络控制点」合到一个平台上，而不是两家同类公司合并。

收购**已签约**，有双方联合公告；**尚未交割**，要等 Zscaler 股东表决和监管审批。

## 先核实：收购是真的吗

- **2026-11-20**：Bloomberg 报道两家在洽谈（消息来自知情人士，属于「正在谈」，不是签约）。
- **2026-11-23**：双方发布联合新闻稿，宣布**已达成确定协议**。CrowdStrike 官网与 Zscaler 官网都有这份稿件。
- 条款（来自新闻稿）：
  - 全股票交易，企业价值约 **439 亿美元**（按 2026-11-20 CRWD 收盘价计算）。
  - 每股 ZS 换 **1.135 股 CRWD**，隐含约 **260.41 美元/股**，较 11-20 收盘溢价约 **34%**。
  - 合并后 CRWD 原股东约 **84.1%**，ZS 原股东约 **15.9%**。
  - Jay Chaudhry 及关联方（约 **34%** 的 ZS 股份）签署了支持交易的投票协议。
  - 双方董事会一致批准；Chaudhry 交割后继续领导 Zscaler 业务，并加入 CrowdStrike 董事会。
  - 预计在 CrowdStrike **FY2028 下半年**交割（2027-08-01 至 2028-01-31），需 ZS 股东批准和监管批准。
- **结论**：「已签约」属实。「已完成收购」不属实，目前是待交割状态。

## 业务总览

| 领域 | CrowdStrike（Falcon 平台） | Zscaler（Zero Trust Exchange） | 关系 |
|---|---|---|---|
| 终端 EDR / XDR | Falcon Endpoint | 基本不做 | CrowdStrike 独有 |
| 云安全 | Falcon Cloud Security（CDR，agent + agentless） | 云工作负载方面未核实 | 以 CrowdStrike 为主 |
| 身份安全 | Falcon Next-Gen Identity（ITDR，已宣布收购 SGNL） | ZPA 基于身份的用户到应用访问控制 | **部分重叠**（检测响应 vs 访问控制） |
| SIEM / SOC 自动化 | Next-Gen SIEM + Falcon Onum + agentic SOC | 日志与分析方面未核实 | 以 CrowdStrike 为主 |
| 威胁情报 | Threat Intelligence & Hunting | 不做 | CrowdStrike 独有 |
| 暴露面管理 | Falcon Exposure Management | 不做 | CrowdStrike 独有 |
| SWG / SSE / SASE | 不做 | Zscaler Internet Access（ZIA） | Zscaler 独有（收购后 CRWD 将提供） |
| ZTNA | 未在本次核实 | Zscaler Private Access（ZPA） | Zscaler 主导 |
| CASB | 未在本次核实 | Zscaler CASB（多模式） | Zscaler 独有（已核实） |
| DLP / 数据安全 | 官方 DLP 产品页本次未核实 | Zscaler Data Security（DLP + DSPM） | **有交叉**，需进一步核实 |
| 分支机构零信任 | 不做 | Zero Trust Branch | Zscaler 独有 |
| 自动化 SOAR | Falcon Fusion SOAR（页面未取到，未核实） | 未核实 | 未核实 |

## 逐项说明

以下每项包括：解决什么问题、部署与控制点、官方宣称的优势、局限、适用场景、运维复杂度、采购注意。性能与价格数据只引用官方原文，本页不下结论。

### 1. 终端 EDR / XDR（CrowdStrike Falcon Endpoint）
- **解决的问题**：恶意程序、勒索软件、供应链投毒、AI 智能体执行的命令。
- **部署与控制点**：在终端和工作负载上装轻量 Falcon 传感器，云端统一管理。
- **官方宣称**：2025 MITRE ATT&CK Enterprise 评测 100% 检出（厂商自报）；AI 智能体所在的端点是新的控制点；可在 npm、PyPI 包执行前拦截。
- **局限（我的判断）**：依赖装 agent；对纯网络流量与云访问的可见性不如网络类产品。
- **适用场景**：有终端规模、需要事件响应的企业。
- **运维复杂度**：低到中。agent 部署与策略管理成熟，主要成本在告警分诊。
- **采购注意**：问清 MDR（托管检测响应）是否包含；价格与合同条款请直接向厂商索取，本页不做判断。

### 2. 云安全（Falcon Cloud Security）
- **解决的问题**：云上工作负载的运行时威胁和代码到运行的风险。
- **部署与控制点**：agent + agentless 两种方式结合。
- **官方宣称**：MITRE 首次云评测 100% 检出（自报）；CDR 响应快 89%（自报）。
- **局限（我的判断）**：agentless 覆盖面与 agent 深度之间有取舍，需要按自己的云账号结构验证。
- **适用场景**：多云、容器化、需要运行时检测的团队。
- **运维复杂度**：中。
- **采购注意**：先在测试账号里验证 agentless 能看到什么。

### 3. 身份安全（Falcon Next-Gen Identity）
- **解决的问题**：身份被滥用、横向移动、账号接管。
- **部署与控制点**：ITDR（身份威胁检测与响应），页面称可实时检测并自动强制 MFA 或重置密码。支持的身份源清单本次未核实。
- **官方宣称**：IDC MarketScape 2026 领导者（厂商引用）。CrowdStrike 已宣布收购 SGNL（身份授权）。
- **局限（我的判断）**：偏检测与响应，不是 ZPA 那种访问控制的替代品。
- **适用场景**：身份是主要攻击路径的组织。
- **运维复杂度**：中。
- **采购注意**：与现有 IdP（身份提供方）的集成范围要核对清楚。

### 4. SIEM 与 SOC 自动化（Next-Gen SIEM、Onum、agentic SOC）
- **解决的问题**：海量日志的检索、关联、告警降噪。
- **部署与控制点**：云端 SIEM，Falcon Onum 负责数据管道。
- **官方宣称**：相比传统 SIEM 三年成本节省 80%、搜索快 150 倍、误报减少 95%（均为厂商自报）。
- **局限（我的判断）**：这些数字是厂商对比，需要用你自己的数据量验证。
- **适用场景**：已经用 CrowdStrike 端点、希望统一数据的团队。
- **运维复杂度**：中到高，取决于接入的数据源数量。
- **采购注意**：按日志量与保留期计价，提前算清。

### 5. 威胁情报与猎杀（Threat Intelligence & Hunting）
- **解决的问题**：了解攻击者，主动寻找威胁。
- **官方宣称**：追踪 281+ 个对手（来自云安全页面）；管理 3 亿多条实时指标（厂商自报）。
- **局限（我的判断）**：情报质量需要用真实事件检验；对小团队价值取决于是否有人分析。
- **适用场景**：有安全分析团队的企业。
- **采购注意**：问清是否包含 MDR 专家服务。

### 6. 暴露面管理（Falcon Exposure Management）
- **解决的问题**：找出哪些漏洞真的可被利用。
- **官方宣称**：页面称漏洞从暴露到被利用的时间正在从数周缩短到数小时（厂商观点）。
- **局限（我的判断）**：优先级排序依赖厂商的利用情报。
- **适用场景**：漏洞数量多、修补资源紧张的团队。

### 7. SWG / SSE / SASE（Zscaler ZIA）
- **解决的问题**：用户上网、访问 SaaS 时的威胁与数据泄露，替代传统防火墙与 Web 代理。
- **部署与控制点**：用户设备或网络流量经过 Zscaler 的云端代理，对 TLS 流量做检查。
- **官方宣称**：检查 100% TLS 流量；可替代老旧网络安全硬件。Zscaler 称保护了约 50% 的《财富》500 强（自报）。
- **局限（我的判断）**：所有流量都经过云端代理，延迟、可用性和隐私合规需要评估；需要部署客户端或隧道。
- **适用场景**：分布式办公、SaaS 多、要替代 VPN 与防火墙的组织。
- **运维复杂度**：中。策略多但集中。
- **采购注意**：先在试点用户上看真实延迟；核对 TLS 解密的合规要求。

### 8. ZTNA（Zscaler ZPA）
- **解决的问题**：替代 VPN，让用户只访问被授权的具体应用，而不是整个内网。
- **部署与控制点**：连接器部署在内网侧，用户通过客户端访问应用；一对一连接。
- **官方宣称**：可在数小时内替代 VPN 与 VDI；应用不暴露在公网（官方原文）。
- **局限（我的判断）**：和身份系统、设备状态的集成深度决定效果；老旧协议应用需要单独评估。
- **适用场景**：用 VPN 的远程办公组织，尤其有大量内部应用。
- **运维复杂度**：中。
- **采购注意**：盘点要迁移的应用清单，包括非 Web 协议的应用。

### 9. CASB（Zscaler CASB）
- **解决的问题**：SaaS 与云应用中的数据与配置风险，影子 IT 的可见性。
- **官方宣称**：多模式 CASB，统一策略，减少点产品。
- **局限（我的判断）**：仍需检查 SaaS 的 API 集成深度，而不只看代理。
- **适用场景**：SaaS 密集的企业。
- **采购注意**：核对支持的 SaaS 清单。

### 10. DLP 与数据安全（Zscaler Data Security；CrowdStrike 数据防泄露）
- **解决的问题**：敏感数据经网页、GenAI、邮件、端点外泄。
- **官方宣称**：Zscaler 称统一了分类、DSPM 与内联 DLP，使用 LLM 分类（官方原文）。
- **局限（我的判断）**：CrowdStrike 的数据防泄露产品线本次未核实，因此无法确认两家在这块是否直接重叠。
- **适用场景**：需要统一管控网页、GenAI、端点数据通道的组织。
- **采购注意**：问清 DLP 覆盖哪些通道，是否要装额外 agent。

### 11. 分支机构零信任（Zscaler Zero Trust Branch）
- **解决的问题**：分支、园区、工厂的网络连接，减少站点间 VPN 与防火墙。
- **官方宣称**：可减少 50% 的基础设施与防火墙支出（厂商自报）。
- **局限（我的判断）**：工厂 OT 网络场景要单独评估。
- **适用场景**：多分支、多工厂的组织。

## 收购合并后的意义（我的判断）

> **不构成投资建议。** 以下是对业务逻辑的看法，不涉及股价判断。

1. **控制点合并**：CrowdStrike 有端点与云，Zscaler 有流量路径。合并后的故事是「攻击路径上两端都控制」，这是业务逻辑而不是价格结论。
2. **真正的整合难点**：两家的数据模型、策略模型、销售渠道都不同。新闻稿说交割后"一个平台、一份数据层、一份合同"，但这是目标，不是已发生的事。
3. **对采购方的影响**：短期内两家产品仍然独立运营（新闻稿原文："Until closing, both companies will continue to operate independently"）。现在可以正常采购，但合同条款里要留意控制权变更后的条款。
4. **还没法判断的**：整合会不会按计划进行、客户会不会因此换供应商，都取决于交割后的执行，目前无法从公开资料判断。

## 未核实 / 存疑

- Falcon Fusion SOAR、Zscaler 云安全态势管理（CSPM）、Zscaler 数字体验（DEM）、Zscaler 防火墙页面，本次抓取失败，未核实。
- Bloomberg 与 CNBC 报道只看到标题和摘要，未读全文。
- 所有「MITRE 100%」「节省 80%」「ROI」等数字都是厂商自报，本页未独立复现。
- 收购的对价、协同效应（10 亿美元收入协同、5 亿美元成本协同）均为公司预期，不是已实现数字。

## 资料来源

- 2026-11-23 · CrowdStrike 与 Zscaler 联合新闻稿（Zscaler 版）：https://www.zscaler.com/press/crowdstrike-acquire-zscaler-bringing-network-security-and-zero-trust-falcon-platform
- 2026-11-23 · CrowdStrike 新闻稿：https://crowdstrike.com/en-us/press-releases/crowdstrike-to-acquire-zscaler-bringing-network-security-and-zero-trust-to-the-falcon-platform
- 2026-11-20 · Bloomberg「CrowdStrike is in talks to buy Zscaler」（仅标题与摘要）：https://bloomberg.com/news/articles/2026-11-20/crowdstrike-is-in-talks-to-buy-cybersecurity-firm-zscaler
- 2026-11-23 · CNBC「CrowdStrike bets on network security with $43.9 billion Zscaler deal」（仅摘要）：https://www.cnbc.com/2026/11/23/crowdstrike-to-buy-zscaler.html
- 2026-11-24 抓取 · CrowdStrike 产品页：
  - https://www.crowdstrike.com/en-us/platform/endpoint-security/
  - https://www.crowdstrike.com/en-us/platform/cloud-security/
  - https://www.crowdstrike.com/en-us/platform/identity-protection/
  - https://www.crowdstrike.com/en-us/platform/next-gen-siem/
  - https://www.crowdstrike.com/en-us/platform/exposure-management/
  - https://www.crowdstrike.com/en-us/platform/threat-intelligence/
- 2026-11-24 抓取 · Zscaler 产品页：
  - https://www.zscaler.com/products-and-solutions/zero-trust-exchange
  - https://www.zscaler.com/products-and-solutions/zscaler-internet-access
  - https://www.zscaler.com/products-and-solutions/zscaler-private-access
  - https://www.zscaler.com/products-and-solutions/data-protection
  - https://www.zscaler.com/products-and-solutions/cloud-access-security-broker-casb
  - https://www.zscaler.com/products-and-solutions/zero-trust-branch

> **不构成投资建议。** 本页只做产品与业务对比，不对股价或并购结果做判断。

[← 返回首页](../../)
