---
title: AI 时代的软件工程 / DevOps / SRE / Platform 岗位：名字怎么变、要求怎么变、活儿怎么变
---

# AI 时代的软件工程 / DevOps / SRE / Platform 岗位：名字怎么变、要求怎么变、活儿怎么变

> 写于 2026-11-20（悉尼时间）。起因：群里聊完 Ruby2Shell 补丁之后，有人问「软件工程师是不是都在变成 FDE」「DevOps 将来职位名称和工作内容会怎么变」「LinkedIn 上的要求有什么趋势」。只用公开来源，每条数据注明出处和日期；查不到的放在最后「未核实」。标「我的判断」的是个人观点。

## 一句话

**岗位没在消失，是在往上走、往外走：招聘回暖集中在资深岗和带 AI 的岗位，入门岗更难。DevOps/SRE 的名字大概率还在，只是会被加上 AI、Platform、Reliability 这类前缀；Terraform 和 K8s 成了门槛而不是卖点。新加的要求有三条：会用 AI 写代码，能把 LLM 和 agent 当作生产负载来运维，补丁要更快。真正爆发的新名字是 FDE（前置部署工程师）。「AI SRE」「Agent Ops」现在更多是产品品类，还不是一个成规模的职位。**

## 要点

1. **名字：FDE 是唯一有硬数据的「爆款新名字」。** Lightcast 的数据显示，2026 年 1–8 月 FDE 岗位同比涨了 **1000% 以上**，广告薪资中位数 **18.8 万美元以上**，普通软件工程师约 14.5 万（Fortune，2026-09-03）。Gartner 2026-11-18 的预测：到 2028 年 **70% 以上**的 AI 技术和服务商会用 FDE，现在是 10%。LinkedIn 的 Jobs on the Rise 2026（美国和澳洲版）第一名都是 **AI Engineer**。在我能读到的条目里没看到 FDE。
2. **要求：AI 从加分项变成门槛。** Dice 统计，美国科技岗里写了至少一项 AI 技能的，从 2024 年 1 月的 **15%** 涨到 2026 年 5 月的 **73%**。LinkedIn 说要求「AI 素养」的岗位一年涨了约 **65%**（2026-10-27）。真实 SRE 岗位 JD（Deloitte「Applied AI SRE III」）同时要 K8s、Terraform、ArgoCD 和 Bedrock、Azure OpenAI、LLM gateway、LangFuse/LangSmith。
3. **回暖是「资深 + AI」的回暖。** Indeed：从 Claude Code 发布（2025-02）算起，美国软件开发岗位涨了约 **15%**，但仍比疫情前低 **27.5%**。增量里 **71%** 是资深岗，**37%** 的职位名里带 AI（2026-07-08）。澳洲更冷：JSA 的 ICT 职位广告 2026 年 10 月只有 **5,743** 条，创历史新低（Information Age，2026-11-19）。
4. **工作重心：从「搭流水线」变成「管 AI 带来的变更、可观测和成本」。** Catchpoint SRE Report 2026：toil 中位数从 **20%** 升到 **34%**，只有 **13%** 的人很有把握监控 AI 组件的可靠性。DORA 2025：AI 让交付更快，但不稳定性也在升；**90%** 的组织已有内部平台。
5. **安全和补丁速度成了硬指标。** Mandiant M-Trends 2026 估算的平均「漏洞被利用时间」是 **−7 天**，即平均在补丁出来之前就已被利用（二手引用，见来源）。Google GTIG：2026 年 1–8 月已记录 **141** 个被利用的漏洞，超过 2025 全年的 127 个。
6. **培训网站那个「Gen AI 岗位涨 400%」要打折听。** 我找到的同款说法出自一家培训/外包网站，原文是「自 **2015 年**以来涨了 400% 以上」，不是同比，也没有方法说明。可信度低，不建议引用。

## 详解

### 1. 职位名称：哪些是真的在长，哪些还只是概念

**有硬数据的：**
- **AI Engineer**：LinkedIn Jobs on the Rise 2026（美国 25 强）连续第二年排第一，常见技能是 LangChain、RAG、PyTorch，转岗来源第一位是 Software Engineer。方法是看 2023-01-01 到 2025-07-31 LinkedIn 会员新入职的职位增速。澳洲 15 强第一名也是 AI Engineer，岗位集中在悉尼、墨尔本、布里斯班，转岗来源是 Software Engineer、Data Scientist、Data Engineer。LinkedIn 2026-10-27 又说，2026 年 7–9 月 AI Engineer 在 7 个主要经济体（含美、英、德、加、印）都排进增速前 10，美国 AI Engineer 的入门级招聘同比 +68%。
- **FDE（Forward-Deployed Engineer）**：这是 Palantir 的老模式，即把工程师派到客户那里把 AI 真正接进业务。Lightcast 的数据（经 Dice 总裁转述，Fortune 2026-09-03）：2026 年 1–8 月比去年同期 +1000% 以上，比 2023 年 +4,600%；同期科技岗整体 +13%。Microsoft、Meta、Google、OpenAI、Anthropic 都在招。Indeed 印度：FDE 岗位 2025-06 到 2026-06 涨了近 19 倍（IANS，2026-10-14）。Gartner 2026-11-18 把 FDE 列为软件工程六大趋势之一。
- **Platform Engineer**：Gartner 2023 年预测「到 2026 年 80% 的软件工程组织会有平台团队」。DORA 2025 的实测是 **90%** 的组织用内部平台，**76%** 有专门平台团队（InfoQ 2025-09-29 转述）。这个名字已经主流化了，代价是不再稀缺：platformengineering.org 的 2026 年调查（518 人）显示，北美平台工程薪资同比降了约 3.3 万美元，欧洲降约 2 万，原因是 0–2 年经验的新人几乎翻倍。另一个信号是约 **35%** 的组织有了专职的「平台产品经理」。

**有真实 JD、但还没有统计口径的：**
- **AI Reliability Engineer / Applied AI SRE**：有真实招聘，比如 Flatiron Health 的「Senior AI Reliability Engineer (Platform)」，以及 Deloitte 的「Applied AI SRE III」。但我没找到任何官方或大型招聘平台统计这个职位名的数量。
- **Andela 的「缝隙岗位」研究**（2026-09-10，分析 47,101 个财富 500 强技术岗）：53% 标题叫 AI/ML Engineer 的岗位，要的是至少两种传统岗位的技能组合。跟运维相关的新组合有 **MLOps Pipeline Engineer**、**FinOps Reliability Engineer**（36% DevOps + 27% SRE + 18% Cloud）、**DevSecOps Security Engineer**、**SecOps Observability Engineer**。注意 Andela 自己是做人才外包的，这是厂商研究。

**主要是产品品类、不是职位的：**
- **AI SRE**：搜下来几乎全是厂商页面（Rootly、Traversal、ClickHouse 等），讲的是「让 agent 做故障排查」的工具，不是招人的岗位名。
- **Agent Ops**：没找到任何招聘统计，列为未核实。

### 2. 招聘要求怎么变

- **AI 编码工具 / AI 技能**：
  - Dice：写了 AI 技能的科技岗占比从 15%（2024-01）涨到 73%（2026-05），样本是 700 万条美国岗位。2026 年 Q1，职位名带 AI 的岗位同比 +173%（CIO Dive，2026-07-09）。
  - Dice 2026 年 9 月：同比增长最快的技能是 **Claude AI +1033%**、Agentic AI +446%、Responsible AI +401%、AI agents +308%、AI infrastructure +283%（CIO，2026-10-16）。
  - Indeed AI Tracker：职位里提到 AI 的占比从 3.6%（2025-09）涨到 7.2%（2026-09），而且近三分之二带 AI 的职位名已经不在传统科技岗（2026-11-18）。
- **LLM 应用支撑 / AI 可观测**：以 Deloitte 的 Applied AI SRE III JD 为例，要求有：
  - 运维过生产环境的 AI/agent 负载，熟悉它们的失效模式（漂移、输出方差）；
  - MLOps/LLMOps 和 AI 控制面（模型/LLM gateway、guardrails）经验；
  - Bedrock、Azure OpenAI 或 Vertex AI，加上 LangFuse/LangSmith。

  Stack Overflow 2025 调查也看到，开发者在拿 Sentry（32%）、New Relic（13%）这些老工具监控 agent。
- **Terraform / K8s 等基本功**：同一份 JD 里，Kubernetes、Terraform、ArgoCD、OpenTelemetry/Prometheus/Grafana、SLO/错误预算、on-call、RBAC/最小权限仍然是硬要求，写在「5 年以上 / 3 年以上」那几行里。
- **安全与补丁速度**：
  - 英国 NCSC 的建议是「默认更新」：面向互联网的服务 5 天、操作系统和应用 7 天、内网 14 天。
  - 美国 CISA 2026-06-10 发了 BOD 26-04，改成按风险排补丁优先级。
  - Gartner 2026-11-18 预测：到 2030 年 70% 的企业会把 agent 开发流程放进沙箱（现在 10%）。

### 3. 工作内容重心怎么变

- **Toil 不降反升，只是换了形式。** Catchpoint SRE Report 2026：49% 的人说 AI 减少了 toil，16% 说增加了。而且管理层更多觉得变少，一线工程师更多觉得没变或变多。新的 toil 包括维护 AI 工具、审它的建议、核对 AI 做的操作。
- **AI 是放大器，平台是地基。** DORA 2025（近 5000 人）：AI 让交付吞吐变高，但不稳定性也在升，靠「快速失败、快速修复」抵消不了。高质量的内部平台是扩大 AI 收益的前提。
- **信任仍低。** Stack Overflow 2025：84% 在用或计划用 AI，但只有 29% 信任 AI 的准确度，66% 说花了更多时间修「差一点就对」的 AI 代码，72% 说 vibe coding 不是他们工作的一部分。
- **新活儿有了名字。** Gartner 2026-11-18 列出的「Harness engineering」，就是给 coding agent 配上下文、规范和护栏。Gartner 预测有这类基础设施的组织，部署后缺陷会少 60%（到 2028 年）。还有「Eval 驱动开发」，从不到 20% 涨到 70%（到 2028 年），和 agent 沙箱。这三样本质上都是平台/DevOps 团队的新活儿。
- **补丁窗口在缩短。** 被利用漏洞里零日占多数（GTIG：2026 年 1–8 月 62%）。VulnCheck 统计，2025 年已知被利用漏洞里有 **28.96%** 在 CVE 公布当天或之前就被利用。所以「发现 → 全量上线」的时间正在变成团队的核心指标。

### 4. 数据可信度分级

- **一手、方法透明**：LinkedIn Jobs on the Rise（有方法说明），LinkedIn Pulse / Economic Graph，Indeed Hiring Lab，DORA，Stack Overflow 调查，Jobs and Skills Australia（JSA）。
- **一手但商业机构**：Dice（招聘网站，样本大，口径是岗位广告），Lightcast（数据商，经 Fortune 转述），Gartner（预测不是测量），Catchpoint（厂商调查），platformengineering.org（社区调查，518 人）。
- **厂商研究**：Andela（做 AI 人才外包，结论对自己的生意有利）。
- **培训 / 营销网站**：「Gen AI 岗位涨 400%」「AI Engineer 需求涨 13 倍（引 LinkedIn）」这类在培训网站和社交帖里流传的数字，我没在 LinkedIn 原报告里找到出处，**不建议引用**。
- **口径冲突要注意**：同一时期，Dice 说软件开发岗位 2026 年 Q1 同比 **−22%**，Indeed 说比 2025-02 **+15%**。两家样本和基期都不同，不能直接对比。但两家都同意一点：增量集中在 AI 和资深岗。

## 我的判断（观点）

1. **DevOps 这个名字不会消失，但会像当年的「运维」一样被分层。** 底层的 YAML、流水线、IaC 越来越多交给 agent 写，人往上走到「平台产品」（给 agent 和开发者提供安全、可回滚的路）和「可靠性/风险」（AI 改动怎么审、权限怎么限、出事怎么回滚）。Gartner 说的 harness engineering 和 agent 沙箱，本质上就是平台团队的下一份工作。
2. **FDE 是给「懂系统又能见客户」的人开的新门，不是所有软件工程师的终点。** 它涨得快，但基数小：千倍级的增速说明起点很低，有一篇未注明来源的文章称 2025 年 4 月全球只有约 600 多个岗位。对 DevOps/SRE 来说，更现实的路径是做「接入层」：把 LLM 安全地接进企业现有的身份、数据、网络和合规体系，这正是 FDE 们最常卡住的地方。
3. **入门岗位最危险，资深岗位反而更值钱。** Indeed 的 71% 资深占比，加上澳洲 ICT 广告创新低，说明「资深 + AI 熟练」有溢价，新人和泛泛的中级岗最卷。
4. **Ruby2Shell 这类事件会把 DevSecOps 往上推。** AI 让找洞更快，「补丁速度」会像 DORA 指标一样被考核。这是运维人确定性最高的一块阵地。
5. **给从业者的建议**：
   - 基本功继续做扎实（K8s、Terraform、可观测、SLO），这是门槛；
   - 至少在生产环境运维过一次 LLM/agent 负载，包括 gateway、限流、成本、评测、trace，并写进简历；
   - 每天用 coding agent，并能讲清楚你怎么审它的改动；
   - 练一项「对外」能力，比如写方案、和业务方对需求，给 FDE 或平台 PM 方向留门；
   - 补丁和 SBOM 流程做成可度量的数字（从 CVE 公布到全量上线用了几小时）。

## 未核实 / 存疑

- 「AI Reliability」「Agent Ops」「AI SRE」作为职位名的岗位数量：没找到任何官方或大平台统计。
- LinkedIn 的「AI-augmented 岗位入门级招聘同比 −8.9%」（2026 年 4 月 AI Labor Market Update）：只在帖子评论里看到转述，没打开原 PDF。
- Mandiant「−7 天」、VulnCheck「28.96%」、GTIG「141 个」：来自一篇汇总文章（cyphrq，2026-11-04 更新）对原报告的引用，我没有逐份打开原报告。
- 一篇 LinkedIn 个人文章称 FDE 岗位从 2025 年 4 月的 643 个涨到 2026 年 4 月的 5,300 个（+729%），数据来源未说明。
- 有社交帖称 FDE 进了 LinkedIn Jobs on the Rise 2026 美国榜，我在榜单原文里没找到。

## 来源

- LinkedIn News — Jobs on the Rise 2026: The 25 fastest-growing roles in the U.S.（含方法说明），2026-01 — https://linkedin.com/pulse/linkedin-jobs-rise-2026-25-fastest-growing-roles-us-linkedin-news-dlb1c
- LinkedIn News — Jobs on the Rise 2026: The 15 fastest-growing jobs in Australia，2026-01 — https://linkedin.com/pulse/linkedin-jobs-rise-2026-15-fastest-growing-australia-w5xee
- LinkedIn Pressroom — LinkedIn data shows where opportunity is emerging as work changes，2026-10-27 — https://news.linkedin.com/2026/linkedin-data-shows-where-opportunity-is-emerging-as-work-changes
- LinkedIn Economic Graph — Labor Market Report 2026 — https://economicgraph.linkedin.com/research/labor-market-report-2026
- Indeed Hiring Lab — AI and Job Postings: From Destruction to Creation?，2026-07-08 — https://hiringlab.indeed.com/2026/07/08/ai-and-job-postings-from-destruction-to-creation
- Indeed Hiring Lab — 2026 Q3 US Tech Labor Market Update，2026-11-12 — https://hiringlab.indeed.com/2026/11/12/2026-q3-us-tech-labor-market-update
- Indeed Hiring Lab & Glassdoor — There is no AI Jobpocalypse, but plenty of AI-nxiety，2026-11-18 — https://hiringlab.indeed.com/2026/11/18/there-is-no-ai-jobpocalypse
- Fortune — Forward-deployed engineers, fast-growing six-figure job（Lightcast 数据），2026-09-03 — https://fortune.com/2026/09/03/forward-deployed-engineers-fast-growing-six-figure-silicon-valley-job-integrate-ai-with-customers-tech-careers-palantir
- IANS / hi INDiA — India's FDE job postings surge 19-fold（Indeed 报告），2026-10-14 — https://hiindia.com/indias-forward-deployed-engineer-job-postings-surge-19-fold-report
- Gartner — Top Strategic Technology Trends in Software Engineering for 2026 and Beyond，2026-11-18 — https://gartner.com/en/newsroom/press-releases/2026-11-18-gartner-identifies-the-top-strategic-technology-trends-in-software-engineering-for-2026-and-beyond
- Gartner — Hype Cycle（平台工程 80% 预测），2023-11-28 — https://gartner.com/en/newsroom/press-releases/2023-11-28-gartner-hype-cycle-shows-ai-practices-and-platform-engineering-will-reach-mainstream-adoption-in-software-engineering-in-two-to-five-years
- CIO Dive — AI skills now listed in 73% of tech job postings（Dice），2026-07-09 — https://www.ciodive.com/news/dice-report-tech-hiring-AI/824893/
- CIO — IT jobs report（Dice 2026 年 9 月数据），2026-10-16 — https://www.cio.com/article/4201347/information-technology-jobs-report.html
- Andela — 53% of AI job postings seek skills that don't match job title，2026-09-10 — https://andela.com/newsroom/andela-research-finds-that-53-of-ai-job-postings-seek-skills-that-dont-match-job-title-also-identifies-new-emerging-tech-roles
- The New Stack — 47,000 job listings reveal the engineering roles that AI is creating，2026-09-10 — https://thenewstack.io/ai-engineering-roles-emerging
- Catchpoint — SRE Report 2026: AI optimism and the economics of effort，2026-02-10 — https://catchpoint.com/blog/sre-report-2026-ai-optimism-and-the-economics-of-effort
- platformengineering.org — State of Platform Engineering in 2026: Salary, maturity, and shifting down，2026-01-20 — https://platformengineering.org/events/state-of-platform-engineering-in-2026-salary-maturity-and-shifting-down-2026-01-20
- InfoQ — DORA Report Finds AI Is an Amplifier，2025-09-29 — https://www.infoq.com/news/2025/09/dora-state-of-ai-in-dev-2025 ；原报告 https://dora.dev/research/2025/dora-report
- Stack Overflow — 2025 Developer Survey results，2025-12-29 — https://stackoverflow.blog/2025/12/29/developers-remain-willing-but-reluctant-to-use-ai-the-2025-developer-survey-results-are-here
- LinkedIn Jobs — Applied AI SRE III at Deloitte（真实 JD 示例） — https://linkedin.com/jobs/view/applied-ai-sre-iii-pxe-gps-at-deloitte-4470695142
- Flatiron Health — Senior AI Reliability Engineer (Platform)（真实 JD 示例） — https://flatiron.com/careers/open-positions/job?gh_jid=8041884
- Information Age (ACS) — Australian tech job ads hit new historic low（JSA 数据），2026-11-19 — https://ia.acs.org.au/article/2026/australian-tech-job-ads-hit-new-historic-low.html
- cyphrq — Mean Time to Exploit: 2026 Data（汇总 Mandiant / GTIG / VulnCheck / NCSC），2026-11-04 更新 — https://cyphrq.com/posts/time-to-exploit-emergency-patching
- programming.com — 「Gen AI 岗位自 2015 年涨 400%」原文（低可信度示例） — https://programming.com/Ai-index-report
