# 项目级导航与 Example 归档规范

这份规范解决两个问题：阶段 example 太多以后找不到，以及其他 IDE / AI 不知道应该先读什么、如何运行和如何继续迭代。

## 默认目录

先检查仓库已有约定；没有约定时，使用下面这组稳定入口：

```text
README.md                         项目给人和 AI 的总入口
AGENTS.md                         AI / 贡献者的工作规则
docs/context.md                   项目事实、术语、架构和入口
examples/README.md                所有阶段 example 的索引
examples/<stage-id>-<slug>/       每个阶段唯一的 example 目录
docs/implementation/<stage-id>.md 阶段完成卡和实现记录
```

`AGENTS.md` 是项目级 AI 指令文件，不是本 skill 自己的 `agents/openai.yaml`。如果仓库已经使用 `CLAUDE.md`、`.cursor/rules/`、`.github/copilot-instructions.md` 或其他等价文件，沿用已有入口，不要为了套用默认结构重复创建一套指令。

如果仓库已有 `docs/context/`，可以将 `docs/context.md` 替换为 `docs/context/README.md`；如果已有 `docs/examples/`，可以将 `examples/` 替换为它。关键不是目录名称本身，而是整个项目只能有一个 canonical example 根目录，并且所有入口都链接到它。

## Example 命名和内容

每个阶段创建一个目录，并按 [example-contract.md](example-contract.md) 分成 `user_code/` 主线和 `core/` 核心代码；不要把多个阶段的代码堆在一个 `example.ts` 或临时目录里：

```text
examples/
├── README.md
└── ISSUE-123-command-routing/
    ├── README.md
    ├── user_code/
    │   ├── README.md         使用者视角和最短公开 API 路径
    │   └── main.*            使用者真正运行的代码
    ├── core/
    │   ├── README.md         本次核心代码和真实源码映射
    │   └── ...                必要时放最小核心源码 / fixture
    └── breakpoints.md         断点较多时拆出；少时写进两个 README
```

`<stage-id>` 优先使用已经确认的 issue key；没有 issue key 时使用稳定的 `stage-01`、`stage-02` 等阶段编号。`<slug>` 用当前记录语言的短描述。目录一旦被其他记录引用，后续迭代不要随意改名；如果 issue key 后来才生成，保留原目录并在索引中补充 issue key，避免历史链接失效。

每个 example 的 README 至少写清楚：

- 它验证哪个 issue / 阶段，以及明确不验证什么。
- 一条最短启动命令、一个具体输入和一个具体输出。
- `user_code/` 从公开导入到真实结果的连续调用路径；`core/` 如何承接这条路径。
- `user_code/` 为什么足够清爽；如果不够清爽，记录公开 API / 组装边界问题及处理结果。
- 入口、组装、核心行为、状态变化、输出等适用断点；每个断点写稳定符号、观察变量和预期值。
- 使用的实现 commit（C1）和记录 commit（C2），或说明当前仍未提交。
- 指向阶段实现记录的链接。

`examples/README.md` 是总索引，不复制每个 example 的教程。每新增或修改一个阶段，都更新一行：

| 阶段 / Example | Issue | 运行命令 | 预期结果 | 实现记录 | Commit |
| --- | --- | --- | --- | --- | --- |
| `ISSUE-123-command-routing` | `#123` | `...` | `...` | [`docs/implementation/...`](../docs/implementation/...) | `C1 / C2` |

## 三个项目级入口分别写什么

### README.md：让人先找到路

只保留项目级导航和快速开始，不把每个阶段的讨论复制进来。至少包含：

1. 项目现在解决什么问题。
2. 最短启动 / 测试命令。
3. “给人和 AI 的导航”小节，链接 `AGENTS.md`、context、`examples/README.md` 和实现记录目录。
4. 当前阶段 example 的入口；不要只写源代码目录。

### context：让 AI 先获得稳定事实

只记录会影响后续实现判断的长期事实：项目目标、领域术语、模块边界、公开入口、数据 / 状态流、约束、验证命令和 example 总入口。每次阶段完成后，仅在这些事实发生变化时更新；不要把临时讨论、未确认方案或完整 issue 复制进去。

### AGENTS.md 或等价 agents 文件：让 AI 知道如何工作

至少说明以下规则：

```text
开始工作前先读：README.md → AGENTS.md → context → examples/README.md → 相关实现记录
新增阶段 example 放入 canonical example 根目录
先运行 `user_code/`，再按需阅读 `core/` 和正式内部实现
修改公开 API、目录或调用路径时同步更新索引和记录
不要把旧 commit 的行号当作当前断点
提交前按 issue、example、验证和 commit 规则回填记录
```

这里写稳定的工作规则，不写某一个阶段的详细答案。若仓库已有 agents 指令，追加最少的导航约定，并保留原有项目规则。

## 变更时的同步关系

完成一个阶段后，至少检查以下链接是否能从任意入口走通：

```text
项目 README
  → AGENTS / 等价 AI 指令
  → context
  → examples/README.md
  → examples/<stage-id>/README.md
  → docs/implementation/<stage-id>.md
  → issue
  → C1 / C2 commit
```

代码路径变化时，更新 example README、断点、context 中受影响的事实和实现记录；新增阶段时，更新 examples 索引。不要为了“看起来完整”在每个文件中重复全部内容。

建议将项目级导航、example 索引、context 和阶段完成记录放入记录 commit（C2），这样 C1 先固化可运行代码，C2 再固化完整可发现性。若仓库使用单一 commit，仍要在最终记录中说明这些入口和代码如何在同一 SHA 闭合。

## 交付前检查

- [ ] 所有阶段 example 都位于同一个 canonical 根目录。
- [ ] `examples/README.md` 能索引每个阶段，并给出运行命令和预期结果。
- [ ] 项目 README 能把新贡献者导航到 AI 指令、context、example 和实现记录。
- [ ] AGENTS 或等价文件说明了读取顺序、example 位置和同步规则。
- [ ] context 记录的是当前稳定事实，而不是未经确认的计划。
- [ ] example、实现记录、issue 和 C1/C2 commit 之间的链接已实际检查。
