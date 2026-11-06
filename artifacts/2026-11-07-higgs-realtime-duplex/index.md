---
title: 扒一扒李沐的全双工 AI：Higgs Realtime Duplex 到底强在哪
---

# 扒一扒李沐的全双工 AI：Higgs Realtime Duplex 到底强在哪

> 2026-11-07 · 起因：群里转了李沐的小红书预告“明天发个45分钟的技术视频”，demo 里 AI 坐进视频会议跟人自然交流，大家觉得“好真”。这页是查底的结果：只用一手资料，数字都带链接，观点单独标出。

**一句话：** AI 自然地进视频会议，是真的，但不是独家。2026 年 Thinking Machines、OpenAI、Google 都做到了全双工。Boson 真正的卖点是**中等尺寸的前台模型自己会想、带一张实时生成的脸、而且便宜**。“智能提高了”要打折听：在它自己公布的跑分里，OpenAI 最好的配置 7 项赢 5 项。

## 1. 这是什么

- **谁做的：** [Boson AI](https://www.boson.ai/blog/higgs-realtime-duplex)，李沐和 Alex Smola 创办，主业是给企业做语音 agent（李沐在硅谷101大会讲过给 500 强保险公司做 AI 销售）。所以这是**企业产品发布**，不是学术成果。
- **正片：** [李沐 YouTube 技术讲解](https://www.youtube.com/watch?v=pU556FTWRkE)（2026-11-05，44 分钟）。章节：三个 demo → 级联式 ASR+LLM+TTS → 端到端 → 全双工 → 训练与评测。
- **能不能用：** 不开源权重，Duplex 的 API 还在 waitlist。现在能用的是上一代、按回合说话的 Higgs Realtime。这是 [Boson 员工在 Reddit 上亲口说的](https://www.reddit.com/r/MachineLearning/comments/1xpn5pv/higgs_realtime_duplex_a_45b_moe_fullduplex_model/)。**所以会议 demo 外人暂时没法复现，只能当剪辑过的宣传片看。**

## 2. “自然”是怎么做出来的

| 设计 | 做法 | 带来什么 |
|---|---|---|
| 按时间切，不按回合切 | 所有输入输出切成 **160ms** 一块：你的声音和画面、它的声音和脸、后台的思考和工具调用，都在同一条流里 | 沉默也算一块，所以它能自己决定什么时候插一句“嗯”、什么时候停、哪句话不是对它说的 |
| token 预算很紧 | 音频输入 1 token/块、输出 2 token/块，每 6 块一帧画面（约 12 token） | 音频加视频约 **2.6K token/分钟**，带思考和工具 2.6–5K；最近 5 分钟只占 13–25K，更早的压成文字 → 开一小时会也便宜 |
| 零样本声音和脸 | 声音靠一小段参考音频，脸靠一张照片加一个因果扩散解码器（causal DiT），不用微调 | 换形象只要换参考素材 |
| 边说边干活 | 思考和工具调用跨多个块进行，结果分批回来，没回完也能先开口 | demo：工程师全程看书，AI 帮他比对图纸、叫 Claude Code 写工具、发邮件 |

这条路线是 **Thinking Machines 先开的**：2026-05 的 [interaction models](https://thinkingmachines.ai/blog/interaction-models/) 切 200ms，模型 276B（每次只激活 12B）。OpenAI 的 [GPT-Live-1](https://developers.openai.com/api/docs/models/gpt-live-1) 也已经是全双工加后台委派，$0.05/分钟，但不支持视频。

## 3. “智能提高了”，底子是这样的

**架构上的分歧是真的。** TML 的前台模型把推理和工具调用都交给后台大模型。Boson 认为那样一次回答可能要等 10 秒以上，成本由大模型决定。Boson 的做法是让 45B 的前台模型自己边听边想、直接调工具，**只有难题才交给更大的模型**。见 [Smola 的文章](https://alex.smola.org/posts/50-talking-while-thinking/)：“the voice should not be doing the calculus”。

**跑分（Boson 自己公布的，取各家最好配置）：**

| 基准 | Duplex 45B | TML 276B | OpenAI 最好 | Google 最好 |
|---|---|---|---|---|
| Big Bench Audio | **98.9** | 96.5 | 97 | 98 |
| Audio MultiChallenge | 44.9 | 43.4 | **48.5** | 36.1 |
| Full-Duplex-Bench v1.5（交互节奏） | 75.5 | 77.8 | **80.1** | 54.3 |
| QIVD 视频理解 | 56.0 | 54.0 | **58.2** | 56.1 |
| ProactiveVideoQA（看视频主动说话） | 31.9 | **33.5** | 25.0 | – |
| τ-Voice 任务完成 | 69.8 | – | **74.5** | 68.6 |
| IFEval 指令遵循 | 94.1 | 89.7 | **95.2** | 90.0 |

**看跑分要注意的地方：**
- 只有 1 项第一。**交互节奏（Full-Duplex-Bench）恰恰是差得最多的一项**，Reddit 帖子里他们自己承认了。
- “45B 打平 276B”口径不对等：TML 每次只激活 12B，Boson **没公开**激活参数量。
- TML 的 96.5 是开了后台 agent 的成绩，关掉只有 75.7。Boson 自己跑分时开没开委派**没写清楚**。
- 对手的数字一半以上是 Boson 自己复现的，其余引用公开报告。

## 4. 我的判断

1. **突破在交互层，不在智商。** 全双工、听出哪句不是对自己说的、边说边干活，这是 2026 年行业一起跨过的门槛。Boson 的贡献是做小、做便宜、加上脸。
2. **“前台自己思考”这条路线对企业场景更对路。** 客服和销售更在乎延迟和每小时成本，不在乎多几分智商。但 45B 的智商有上限，复杂任务还得靠背后的前沿模型，demo 里写代码就是叫 Claude Code。
3. **会议 demo 是精挑细选的。** 真实环境里会不会抢话、听错、网络一抖就出戏，API 开放、有人实测之前，没有独立数据。
4. **它让“AI 同事坐在会议里”的成本门槛明显变低了。** Boson 的目标是 AI 一小时的成本不超过人一小时的十分之一。真正值得讨论的是这个，不是“像不像真人”。

## 5. 没核实的

- 支不支持中文（demo 是英文的）
- Duplex 的价格（[现有 Higgs Realtime 数字人](https://www.boson.ai/pricing) $0.07/分钟起）
- 45B 激活参数量；跑分时有没有开委派

## 来源
- Boson 官方技术博客 · [Higgs Realtime Duplex](https://www.boson.ai/blog/higgs-realtime-duplex)
- Alex Smola · [Talking while thinking](https://alex.smola.org/posts/50-talking-while-thinking/)
- Boson 员工 Reddit 帖 · [r/MachineLearning](https://www.reddit.com/r/MachineLearning/comments/1xpn5pv/higgs_realtime_duplex_a_45b_moe_fullduplex_model/)
- Thinking Machines · [Interaction Models](https://thinkingmachines.ai/blog/interaction-models/)
- OpenAI · [GPT-Live-1 模型页](https://developers.openai.com/api/docs/models/gpt-live-1)
- 李沐 · [YouTube 讲解视频](https://www.youtube.com/watch?v=pU556FTWRkE)
