# Claude 与 ChatGPT 的 MCP / 连接器生态：官方资料对比（核验日期 2026-11-25）

> 不构成投资建议。本文是对公开官方文档的整理与个人判断，不是产品评测。

## 一句话结论

两家都已支持远程 MCP 接入，但差别在"谁来管、怎么授权、能不能写"：Claude 偏向"组织统一添加、成员用自己的账号连接"的目录模式；ChatGPT 偏向"管理员在工作区里创建并发布、可以写入、并有确认步骤"的插件模式。个人用户两边都能用，但团队与企业的管控细节差别大。

## 先分清几个概念

- **MCP 服务器（MCP server）**：按 Model Context Protocol 暴露工具和数据的服务，是接入的底层协议。
- **连接器（connector / app）**：产品里给用户看的入口，背后通常是一个 MCP 服务器加上授权界面。Claude 叫 connector，ChatGPT 的界面里叫 app 或 plugin。
- **插件（plugin）**：ChatGPT 当前把 MCP 连接打包成 plugin 来分发、安装和管理；Claude 的说法是 connector。
- **Skills**：用说明文件教模型怎么做事，不等于接入外部系统。
- **代理间协议**：代理与代理之间通信的协议，与"把应用接进聊天"是两回事。本文只讨论前三类，不把"接入应用"等同于"代理互联"。

## 1. 个人聊天端

**Claude**
- 自定义远程 MCP 连接器在 Free、Pro、Max、Ultra、Team、Enterprise 上都能用；Free 用户只能加 1 个自定义连接器。（来源：Claude 帮助中心 "Get started with custom connectors using remote MCP"）
- 连接器可在 claude.ai、Claude Desktop、Cowork、移动端使用；Claude Code 也共用同一个目录。（来源：Connectors Directory 文档）
- 远程连接器由 Anthropic 云端发起连接，不是从你的电脑发出。这意味着内网或 VPN 后的服务器必须对 Anthropic 的 IP 放行，且只能走 443 端口。（同上）

**ChatGPT**
- 官方帮助文档写明，自定义 MCP 服务器的完整支持面向 ChatGPT Business 与 Enterprise/Edu 的网页端。我读到的页面没有写明个人套餐是否可以自行添加，这一点我未核实。
- 当前界面可直接在 Plugins 里添加自定义 MCP 服务器，不必先开开发者模式，但要确认一次风险提示。（来源：OpenAI 帮助中心 "Connecting custom MCP servers in ChatGPT"）
- 个人用户可用的 Lockdown Mode 会限制实时连接器、Agent Mode 等与外部服务交互的功能，从 2026-06-04 起也向个人账户开放。（来源：OpenAI "Introducing Lockdown Mode and Elevated Risk labels in ChatGPT"）

## 2. 团队与企业端

**Claude（Team / Enterprise）**
- 由组织的 Owner 在 Organization settings > Connectors 里为整个组织添加连接器，成员各自用自己的账号连接。（来源：Connectors Directory 与 Get started with connectors）
- 没有权限的 Team 成员看到的是 Request 按钮，请求会发给 Owner，由 Owner 决定是否启用。（来源：Connectors Directory）
- Enterprise 上，拥有"管理资料库"权限的自定义角色也能添加连接器。（同上）
- 连接器能看到的内容 = 该用户在源系统里的权限。共享凭据（如 API Key）的连接器则能访问该凭据的全部范围。（来源：Use connectors to extend Claude's capabilities）
- Owner 可以关闭某些工具调用。（来源：Get started with custom connectors using remote MCP）
- **Enterprise Managed Auth（EMA）**：用企业身份提供方签发的 JWT 静默换取访问令牌，成员不再看到逐个连接器的授权页。前提是 Team 或 Enterprise 套餐，且 MCP 服务器的授权服务器要支持 JWT bearer 授权，并需要邮件申请目录标记。（来源：Enterprise Managed Auth for connectors）

**ChatGPT（Business / Enterprise / Edu）**
- Business：只有管理员和 Owner 可以创建并发布自定义 MCP 插件。（来源：Connecting custom MCP servers in ChatGPT）
- Enterprise / Edu：管理员可以通过 RBAC 把"创建 MCP 插件"的权限授予特定成员；只有管理员和 Owner 能把应用发布给整个工作区。（同上）
- 管理员可以在 Admin console 里一次管理最多 25 个插件的可用性与安装。（来源：Business release notes，2026-10-15）
- 2026-09-29 的更新：团队 Owner 可以为团队启用工作区级的连接，团队成员不必各自绑定同一个服务账号。（来源：Business release notes）这一点与 Claude 的"成员各自连接"模式不同，也带来不同的风险面：一个共享连接的权限会被整个团队共享。
- Business 上连接器默认开启，管理员可以逐个关闭。（来源：Business release notes）

## 3. 官方与第三方目录

- **Claude**：Connectors Directory 是一个统一目录，同时包含"已验证"和"社区"两类。官方说明"验证不等于安全审计"。目录按使用量排名，类似应用商店；不要求域名所有权证明。（来源：Connectors Directory）
- **ChatGPT**：在 Plugins 中可以发现和安装插件，管理员控制哪些插件对工作区可用。我未核实 ChatGPT 的公开目录的规模与审核标准，因此这一项不做比较。

## 4. 读写能力与确认

- **Claude**：连接器可以"读取并执行动作"，例如 Linear 建 issue、Slack 发消息、Google Drive 搜索文件。官方帮助中心也给出"只允许读、不允许创建或编辑文档"这类收窄的例子（该示例来自搜索摘要，未逐条打开核对）。
- **ChatGPT**：官方文档明确写到 MCP 工具可以包括写入、修改类动作，例如更新 CRM、创建项目任务。执行前会按应用权限和动作上下文请求确认。（来源：Connecting custom MCP servers in ChatGPT）
- **ChatGPT agent** 的页面写明：该功能已不再提供，改用 ChatGPT Work 处理较长的多步骤任务。因此我不引用旧的 agent 用量数字。（来源：ChatGPT agent 帮助页）

## 5. 身份授权与权限继承

- **Claude**：官方明确"Claude 继承每个人在源系统里的权限"。成员通过 Connect 完成自己的 OAuth。MCP 服务器必须实现 OAuth 2.0 Protected Resource Metadata（RFC 9728）。（来源：Use connectors；MCP authorization 规范）
- **ChatGPT**：OAuth 连接若不包含 offline_access，令牌可能过期，用户需要重新授权。官方给出了检查 .well-known 元数据的步骤。（来源：Connecting custom MCP servers in ChatGPT）
- **MCP 规范**：2025-11-25 版本规定 HTTP 传输下 MCP 服务器必须实现 RFC 9728 的资源元数据，客户端必须使用它来发现授权服务器。规范页同时标注"你正在查看较旧的 2025-11-25 版本，最新版本为 2026-07-28"。本文按 2025-11-25 版本核对。（来源：modelcontextprotocol.io 规范页）

## 6. 管理员审批、审计与数据治理

- **Claude**：Team 与 Enterprise 的连接器启用、成员请求、停用都由 Owner 控制；EMA 进一步把身份交给企业的 IdP。（来源：Connectors Directory；EMA 文档）
- **ChatGPT**：RBAC、审计日志、只允许发布后的应用对成员可见，这些在 Enterprise 页面和帮助文档里都有提到。（来源：OpenAI "Introducing Lockdown Mode..."；Connecting custom MCP servers in ChatGPT）
- 本文没有逐项核对两家的审计日志导出格式与保留期限，这部分未核实。

## 7. 提示注入与跨系统委托风险

- 这是两家都承认的核心风险。OpenAI 官方写道：提示注入是把恶意指令藏在第三方内容里，诱导模型把敏感信息送到外部。（来源：Lockdown Mode 公告）
- 历史案例：2025 年 8 月，安全研究人员展示过通过 Google Drive 中的"毒文档"经 ChatGPT Connectors 外泄 API Key 的攻击，OpenAI 称已缓解。（来源：WIRED 报道，2025-08-08）这是旧案例，不代表今天仍存在同样的漏洞，但它说明：连接器里读到的任何外部文档都应视为不可信输入。
- ChatGPT agent 的旧页面写明，在连接应用后，代理可以访问邮件、文件并代表用户执行操作，且存在提示注入风险；官方设有高影响动作的确认与"监督模式"。（来源：ChatGPT agent 帮助页）
- Claude 的官方文档没有像 OpenAI 那样用独立的 Lockdown 开关描述同类机制。我没有找到 Claude 对应的"限制连接器能力"的官方说明，这一点我未核实，不下结论。
- **跨系统委托**：当一个连接器能读邮件或文档、另一个能发消息或改记录时，攻击者只需把指令放进第一个系统的内容里，就可能借第二个系统执行动作。这是最需要设计约束的地方。

## 8. 两个真实办公协作集成（核验能力，不止看品牌）

**Slack**
- Claude：Slack 连接器可以在 claude.ai 中搜索你所在工作区的频道、私信和文件，前提是先在 Slack 里安装 Claude 应用。（来源：Claude Slack integration 文档）
- ChatGPT：Business 版的 @ChatGPT 可以在 Slack 与 Microsoft Teams 中私信或在频道中被提及，回答问题、总结讨论、起草内容。管理员要先安装并配置它，连接的账号决定它能访问哪些资源和动作。（来源：Business release notes，2026-09 一条）
- 对比：Claude 的 Slack 连接器偏"搜索和读取"；ChatGPT 的 @ChatGPT 是"在团队频道里当成员"，影响面更大，要有管理员把关。

**Google Drive**
- 两家都有 Google Drive 连接器，都会继承用户在 Drive 里的权限。
- 上面提到的 2025 年毒文档攻击就是通过 Drive 发生的，说明读取型连接器也会成为注入入口。
- 我没有核实两家在 2026 年当前版本的 Drive 连接器的具体读写范围，只能确认官方帮助页的概要。

## 9. "SaaS 全面转向 MCP，转不了就被淘汰"：证据与反例

**支持这个说法的证据（均为厂商自述或官方文档）**
- Claude 与 ChatGPT 都把连接器做成一级功能，并有目录、管理员后台与团队分发。
- MCP 规范已经迭代到 2026 年的版本，并有授权、资源元数据等正式机制。

**反例与限制**
- 官方目录只收录满足安全与兼容标准的服务器，服务商要主动提交、通过审核，并维护 endpoint。Claude 文档提到服务商改 endpoint 后，老连接会变成"自定义"连接器，需要用户删除再重新添加。这说明连接并不是一次配置、永久可用。
- 企业级的 EMA 要求服务商的授权服务器支持 JWT bearer 授权，并且 DCR 不支持。这意味着"接入 MCP"不等于"一键接入企业身份"。
- 两家的连接器都依赖服务商的授权与权限模型；没有 RBAC 或细粒度权限的服务商，接入后只能做粗粒度授权。
- 我没有找到可验证的数据，说明主流 SaaS 厂商中有多少比例已上线 MCP、实际使用率如何。因此"全面转向"这一点未核实，只能说"很多主流服务已有接入"，不能说"全部"。
- 也没有找到"不做 MCP 就被淘汰"的可核实案例。用 API、Webhook 或本地工具的产品同样可以工作。

## 10. 我的判断（观点，不是事实）

1. **普通能力的代理，也能稳健地协作**，前提是：先只开读，后开写；每个写动作要人工确认；读到的外部内容一律当作不可信输入。
2. **个人使用时**：先选一两个你真正需要的连接器（如日历、Drive 的只读），不要一次全开。
3. **团队使用时**：Claude 的"成员各自连接"模式更容易做到"每人只看到自己有权限的数据"；ChatGPT 的"团队共享连接"模式更省事，但要把共享连接的权限当作团队权限来审。
4. **治理的最小可行方案**：审批白名单连接器 → 只读默认 → 写动作需要人工确认 → 记录每次工具调用 → 定期复查权限。这些在两家都能做到，但做法不同，要按各自后台逐项核对。
5. **不建议**因为"平台都在转 MCP"就急着替换已经稳定运行的 API 或脚本。先问清楚你要的是"连接"还是"自动化"，两者需要的治理不一样。

## 11. 未核实与存疑

- ChatGPT 个人套餐能否自行添加自定义 MCP 服务器：我读到的官方页面未写明，未核实。
- 两家各地区的可用性与限制：未逐地区核对。
- Claude 是否有与 ChatGPT Lockdown Mode 对应的官方限制开关：未找到，未核实。
- 两家的审计日志导出格式与保留期限：未核实。
- 主流 SaaS 厂商 MCP 上线比例与真实使用率：未找到可靠数据。
- Google Drive 连接器 2026 年当前的读写范围：未逐条核对。
- WIRED 的毒文档案例是 2025 年的旧报道，OpenAI 称已缓解，当前是否仍可复现未核实。

## 来源（均为官方或一手页面，按访问顺序）

- Claude：Connectors Directory，https://claude.com/docs/connectors/directory
- Claude：Enterprise Managed Auth for connectors，https://claude.com/docs/connectors/building/enterprise-managed-auth
- Claude 帮助中心：Get started with custom connectors using remote MCP，https://support.claude.com/en/articles/11175166
- Claude 帮助中心：Use connectors to extend Claude's capabilities，https://support.claude.com/en/articles/11176164-use-connectors-to-extend-claude-s-capabilities
- Claude：Slack integration，https://claude.com/docs/connectors/slack/index
- OpenAI 帮助中心：Connecting custom MCP servers in ChatGPT，https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt
- OpenAI 帮助中心：Developer mode and MCP connectors in ChatGPT，https://help.openai.com/en/articles/12584461-developer-mode-and-mcp-connectors-in-chatgpt
- OpenAI 帮助中心：ChatGPT Business release notes，https://help.openai.com/en/articles/11391654-chatgpt-business-release-notes
- OpenAI 帮助中心：ChatGPT agent，https://help.openai.com/en/articles/11752874-chatgpt-agent
- OpenAI：Introducing Lockdown Mode and Elevated Risk labels in ChatGPT（2026-02-13，2026-06-04 更新），https://openai.com/index/introducing-lockdown-mode-and-elevated-risk-labels-in-chatgpt
- MCP 规范：Authorization（2025-11-25），https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
- WIRED：Poisoned document could leak secret data via ChatGPT（2025-08-08），https://wired.com/story/poisoned-document-could-leak-secret-data-chatgpt

个人判断与未核实部分已分别标注。欢迎指正。
