# system-architecture-design

将架构设计方法写成可复用的 Agent Skill，通过「结构化访谈 → 架构推理 → 文档产出 → 自检」，生成一份中文 Markdown 架构设计方案草稿。

适用于系统架构设计、技术选型、架构方案编写和评审准备。它提供工作流程、判断要求与文档模板，最终方案仍需架构师结合实际需求复查。

## 包含什么

- `SKILL.md`：整体流程与执行规则。
- `references/interview-protocol.md`：按主题分组的需求访谈问题。
- `references/quality-attributes.md`：质量属性目录与典型权衡。
- `references/adr-format.md`：架构决策记录格式。
- `references/document-template.md`：十章及附录的文档模板。
- `evals/evals.json`：三个示例评测场景和预期检查项，不是已通过的评测报告。

## 在 Claude Code 中使用

Claude Code 支持从个人目录 `~/.claude/skills/<skill-name>/SKILL.md` 或项目目录 `.claude/skills/<skill-name>/SKILL.md` 加载 skill，并支持使用 `/skill-name` 调用。参见 [Claude Code 官方说明](https://code.claude.com/docs/en/skills)。

### 安装

下载本仓库，将完整目录命名为 `system-architecture-design`，放到以下任一位置。

```text
# 所有本机项目使用
~/.claude/skills/system-architecture-design/

# 仅当前项目使用
<项目目录>/.claude/skills/system-architecture-design/
```

目录内应直接包含 `SKILL.md` 和 `references/`，不要只复制主文件。若已有同名 skill，先比较文件内容，再决定是否更新。

也可以使用 Git 克隆到个人目录，目标目录应不存在。

```sh
mkdir -p ~/.claude/skills
git clone https://github.com/wind7rui/system-architecture-design.git ~/.claude/skills/system-architecture-design
```

### 提供业务需求并调用

可以先提供需求文档，再输入下面的指令。

```text
根据业务功能需求，使用 system-architecture-design skill 做系统架构设计。
```

或直接使用以下调用形式，并附上你的业务需求。

```text
/system-architecture-design
```

一个低代码平台的输入示例，可按实际项目修改。

```text
根据以下业务需求，使用 system-architecture-design skill 做系统架构设计。

我们要设计一个低代码平台。
用户通过可配置页面定义数据库表结构，平台自动生成表数据的 CRUD 代码，
最终交付可直接集成的 SDK jar 包。
系统用户人数在 200 人以内，这不是并发量或业务数据量。
方案需要说明多数据库支持，并包含 PostgreSQL。
其他未提供的信息请先向我确认，不能确认的内容明确标记为假设或待确认项。
```

模型应先针对缺口提问，复述关键事实与约束，再设计方案。非交互环境允许基于已有事实与显式假设产出草稿，并汇总待确认项。

### 复查产物

预期交付为 `架构设计文档-<系统名>.md`，包含十章及附录，架构视图使用内嵌 Mermaid 表达。

检查需求是否完整覆盖，关键决策是否记录真实备选和代价，图文是否一致，以及所有指标是否有来源。假设必须保留为假设，不能因为进入正式文档就视为已确认。

修改数据库支持范围等设计内容后，继续核对相关决策与视图，避免章节之间出现矛盾。生成文档不代表已经实现系统、完成压测或通过架构评审。

## 按自己的方法修改

需要修改访谈重点时，调整 `interview-protocol.md`。修改交付格式时，调整 `document-template.md`。关键决策的记录方式放在 `adr-format.md` 中；质量属性与取舍规则放在 `quality-attributes.md` 中。

主文件负责串联各阶段。修改工作流程时，一并检查对应参考文件中的要求是否一致。

## 运行环境与可选能力

本仓库提供文本指令和参考文件，无需运行项目代码。运行它需要支持加载 skill、读取文件与生成 Markdown 的 AI 工具，以及你自己的模型访问权限。

结构化提问工具属于可选能力，缺少工具时可以直接在对话中提问。独立 HTML/SVG 绘图工具也属于可选能力，本仓库不依赖另一个绘图 skill，默认使用 Mermaid。

本仓库以 Claude Code 为安装示例。其他支持 Agent Skills 的工具可按各自的加载规则使用；未对所有工具与模型做兼容性验证。

## 评测说明

`evals/evals.json` 包含电商订单中心、在线教育直播与工业 IoT 三个输入场景，供手工或评测工具验证需求处理、决策记录和文档完整性。文件中的数值是示例输入，不应套用到你的项目中。

仓库暂不提供自动评测执行器，也不将这些检查项视为已通过的结果。使用时，应结合真实项目和人工审查验证效果。

## 反馈

欢迎通过仓库 Issues 提供使用问题与修改建议。描述触发问题的输入、预期行为与实际结果；分享案例前，请移除项目中的非公开信息。

## 许可证

采用 [MIT License](LICENSE)。
