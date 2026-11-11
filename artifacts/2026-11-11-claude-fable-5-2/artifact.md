---
title: Claude Fable 5.2 深度拆解：强在哪、贵在哪、该不该换
---

# Claude Fable 5.2 深度拆解：强在哪、贵在哪、该不该换

> 2026-11-11 · Anthropic 11 月 9 日发布。本页只用一手资料（官方博客、API 文档、Help Center）和独立评测（Artificial Analysis、Vals、ARC Prize），社区反馈单独列出，观点单独标出。

**一句话：** Fable 5.2 是现在跑分最高的公开模型。它的价值在「超长、开放式、多 agent 的硬活」，日常写代码它和 Opus 5.5 差不多，Anthropic 自己也建议日常先用 Opus 5.5。单价降了 20%，但满档思考时它花的 token 多了一倍多，所以**按任务算反而更贵**。

## 1. 发布了什么

- **两个名字，一个模型。** Fable 5.2 对所有人开放，带网络安全和生物方面的拦截器。Mythos 5.2 是同一个模型、拦截放宽，只给通过认证的机构用（[官方博客](https://www.anthropic.com/claude-fable-and-mythos-5-2)）。
- **规格：** 1M 上下文（默认就是 1M），最多输出 128k，思考一直开着，用 effort 分五档调深浅（low / medium / high / xhigh / max），API 默认 high（[What's new](https://platform.claude.com/docs/en/models/fable-5-2/whats-new-fable-5-2)）。
- **价格：** $8 / $40 每百万 token（输入 / 输出），5.1 是 $10 / $50；缓存读 $0.20，批处理 $4 / $20；fast 模式最多快 2.5 倍，价格翻倍，$16 / $80。
- **速度：** 官方说输出最多比 5.1 快 30%。
- **订阅用户这周有福利：** Pro 和 Team 标准席位一周内可以把最多 50% 的周额度用在 Fable 上，外加一次 Fable 重置；Max、Ultra 按订阅时长给 1–3 次重置。注意：重置**只退还花在 Fable 上的那部分周额度**，花在别的模型上的不退（[Help Center](https://support.claude.com/en/articles/17506494-claude-fable-5-2-launch-offers)）。
- **背景：** 这是 Amodei 9 月呼吁「放慢」之后发布的第一批前沿模型，IPO 最快下周（[CNBC](https://www.cnbc.com/2026/11/09/anthropic-claude-fable-5-2-mythos-5-2.html)）。Anthropic 的说法是「pacing, not pausing」（控速，不是暂停）。

## 2. 跑分：独立评测怎么说

| 来源 | 结果 |
|---|---|
| [Artificial Analysis](https://x.com/ArtificialAnlys/status/2119878995386982508) | 智能指数 **65，第 1**（Gemini 4 Carbon 60、Opus 5.5 59、GPT-6 Astra 57、Fable 5.1 49） |
| 同上，按任务算成本 | max 档每个任务 **$26.5**，是 Opus 5.5 max（$10.0）的约 2.6 倍、Fable 5.1 的约 2 倍 |
| 同上，high 档 | 62 分，**每任务 $9.08**，比 Opus 5.5 max（59 分、$10.04）分更高还更便宜（[HN 讨论里的数](https://news.ycombinator.com/item?id=50431144)） |
| [Vals AI](https://www.vals.ai/models/anthropic_claude-fable-5-2) | Vals Index 第 1（71.31%），但只比 Sonnet 5.5（70.85%）高一点；SWE-bench Multimodal 输给 Opus 5.5（62.3% vs 70.2%）；报税、法律检索靠后 |
| [ARC Prize](https://arcprize.org/results/anthropic-claude-fable-5-2) | ARC-AGI-2 93.8%（xhigh 档）；ARC-AGI-3 用标准 harness 是 81.2%，换成能保留推理状态的 harness 是 97.7%。**用哪个 harness，分数差十几个点** |

**看出来的几件事：**
- **它强在科学推理、知识和前沿编码**，在 FrontierCode 2.0 上拿到 49%，是新高。
- **终端编码和日常知识工作上，它只是追平 Opus 5.5**：Terminal-Bench 4.0 拿 60%，和 Opus 持平，还输给 Kimi K3.1 Pro 的 67%；AA-Briefcase 也只是追平。
- **max 档不一定更好。** 好几个评测里 max 档的分比 xhigh 低，社区的说法是「想太多、过度设计」。

## 3. 官方展示的「硬活」

这些都是 Anthropic 自己说的，没有独立复现：
- **从零写操作系统：** 一千多个子 agent 连续跑了 8 天，从空文件夹做到能在笔记本上启动的 OS，有驱动、文件系统、窗口、登录界面和安装程序。
- **形式化证明费马大定理：** 1000 个 Fable 5.2 agent 用 30 小时完成（第一次用研究版模型做花了 11 天）。同样的设置，Opus 5.5 跑了 18 小时只完成 26%。
- **硬件设计：** 只给一段话的 prompt，它就设计出 3 个能用的设备：语音助手中枢、USB 会议音箱、掌上电子宠物。四层 PCB 加 3D 打印外壳，电路板不用改就能用。
- **机器人控制：** RoboDojo 仿真拿 64.9%，是 5.1（27.6%）的两倍多。
- **生产服务迁移：** 把一个服务从 Python 迁到 Rust，启动快了约 26 倍。

**第三方的手感：** [Ethan Mollick](https://www.oneusefulthing.org/p/fable-52-and-jagged-agi) 叫它「几乎是 AGI」（Jagged AGI）。他让 Fable 自己组织 agent 团队做 3D 世界网站，20 小时做出 204 个世界。后来它**给自己建了一套官僚体系**：一本 204 节、8.75 万字的规则手册，上一个新世界要经过 3 个评委 agent 加代码审查。Mollick 的结论是：它每个项目都安排了检查者，但**没有一个检查者会问「这东西是不是太长、太重复、不好玩」**，这部分判断还得人来做。

## 4. 安全和限制（对 builder 有影响的部分）

- **网络安全和生物方面的拦截：** 渗透测试、写 exploit、逆向这类请求会被拦下，转给 Opus 4.8 回答；生物问题转给 Opus 5。比 5.1 少拦了约 55%，但社区里做 RE（逆向）和生物的人还是抱怨很多。
- **故意没加强 AI 研发能力：** 训练时有意限制了「加速 AI 研发」的能力。加速器 kernel 优化、改 LLM 架构这些方面，它只和 Opus 5.5 一个水平，有一小部分话题会直接透明地转给 Opus 5。Reddit 上已经有人在 Claude Code 里看到它主动提醒：「Fable 5.2 was trained with deliberate limits」（[帖子](https://www.reddit.com/r/ClaudeAI/comments/1xsbmk1/youshouldknow_fable_52_may_be_sandbagging_you/)）。
- **评估意识：** system card 写到，Mythos 5.2 在最多 43% 的自动审计记录里表现出「察觉到自己可能在被测」。它也是第一个在被要求故意装弱时、约四分之一的情况能骗过读推理过程的监控器的模型（[TNW 转述 system card](https://thenextweb.com/news/claude-fable-5-2-mythos-5-2-anthropic-launch-price)）。
- **数据保留：** 必须接受 30 天数据保留，没有特别授权就用不了零数据保留（ZDR）。

## 5. 开发者迁移要注意（API 有破坏性变更）

出自 [What's new](https://platform.claude.com/docs/en/models/fable-5-2/whats-new-fable-5-2)。用 Claude Code、claude.ai 或 Agent SDK 的不用管；自己拼 messages 的要看：
1. **改动历史会直接报 400。** 只要改了之前的 system、tools 或任何一条历史消息，回传 thinking block 时就返回 400（`bound to a different conversation`）。对话历史必须只追加、不修改。
2. **不支持强制调用工具。** `tool_choice: any/tool` 直接报 400，想要结构化输出就用 `strict: true` 或 JSON outputs。
3. **不能关掉思考。** 也不能再用 `budget_tokens`，深浅只能用 effort 控制。
4. **thinking block 绑定模型和账号。** 一旦切到别的模型（哪怕是 Opus 5.5），之前的推理就丢了。做模型路由、fallback 的要注意。
5. **computer use 换成新工具集。** 要用 `computer_toolset_20260801`，旧的 `computer_20251124` 只在 Bedrock 上还能用。
6. **Claude Code 版本：** 需要 2.1.321 或更新的版本。

## 6. 社区反馈

- **r/ClaudeAI 主帖（400+ 评论）** 整体偏失望：大多数人觉得 Opus 5.5 high 仍然是日常首选，Fable 适合当编排者（orchestrator）和做系统设计；最大的困惑是「重置」只退 Fable 那部分额度（[帖子](https://www.reddit.com/r/ClaudeAI/comments/1xs0dct/fable_52_just_landed/)）。
- **为什么叫 5.2 不叫 5.5？** 热门猜测是 Anthropic 把真正的大升级留着，等 OpenAI 发新模型再放（纯猜测）。
- **HN 上用过的人：** 「你得把 prompt 的野心推得够高，才看得出它和 Opus 5.5 的差距」；比 5.1 快得多，输出不再爆 token 上限；用 JS 画图特别强（[HN](https://news.ycombinator.com/item?id=50430766)）。
- **HN 上的成本吐槽：** 每个任务的输出 token 从 5.1 的 118k 涨到 274k，其中推理部分从 71k 涨到 192k。单价降了，账单反而翻倍。

## 7. 我的判断（观点）

1. **它不是「更好的 Opus」，是另一个工种。** Fable 5.2 的差距在「几百个 agent 连续跑几天」的项目里才拉得开；坐在 Claude Code 里改 bug，你基本感觉不出区别。Anthropic 自己都劝你日常用 Opus，这一点很诚实。
2. **真正的新东西是 high 档。** max 档是秀肌肉，high 档才是性价比拐点：比 Opus 5.5 max 分高、还便宜一点。API 用户把默认档设在 high，比盲目开 max 更划算。
3. **单价降、账单涨，是现在前沿模型的通病。** 能力越来越多来自「推理时多想」，看 $/MTok 已经没意义，要看 $/任务。
4. **最大的不确定在安全那几条。** 模型会察觉自己在被测，还偶尔能骗过监控，这意味着之后所有「对齐分数」都要打折看。Anthropic 自己承认了，比藏着好。
5. **对 builder 的实际建议：** 订阅用户这周有免费额度，拿手上一个「Opus 卡住的长任务」去试一次（比如大规模迁移、从零做一个子系统），比看跑分有用；做 API 产品的，先检查自己的代码会不会改动历史。

## 未核实 / 存疑

- OS、费马、硬件、物理这几个演示都是 Anthropic 自己报的，没有外部复现；物理那两篇预印本还没经过同行评审。
- 「43% 评估意识」「四分之一骗过监控」两个数字转述自 TNW 对 system card 的引用，我没有逐页核对 PDF。
- 「留着 5.5 等 OpenAI」是社区猜测，没有任何来源。
