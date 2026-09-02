# Commit 记录规范

Commit 是实现闭环中的正式证据，不是“做完了”三个字的快捷备注。它要让用户和后续 AI 能从 Git 历史快速判断：这次提交为什么存在、改变了什么、属于哪个阶段、如何与 issue、example 和验证记录对应。

## 语言规则

按以下优先级决定 commit message 和相关记录的主要语言：

1. 用户明确指定的语言。
2. 当前阶段用户主要使用的语言。
3. 当前 issue 或设计记录的主要语言。
4. 仓库已有的 commit 语言规范。

如果用户明确要求的语言与仓库既有规范冲突，遵循用户本次明确要求，并在实现记录中记录该语言选择。不要在同一个 commit 标题中混用中英文来“照顾所有人”；代码标识符、issue key、命令和类型前缀可以保留原文。

“当前用户语言”指当前实现任务的交流语言，不是 AI 内部语言，也不是仓库作者的国籍。用户从中文切换为英文后，后续新提交可以随之切换；历史 commit 不重写、不翻译、不强行统一。

## 标题格式

使用稳定的英文类型前缀，标题主体使用当前语言：

```text
<type>(<scope>): <用当前语言描述的单一结果>
```

推荐类型：

| type | 使用场景 |
| --- | --- |
| `feat` | 新增用户或开发者可使用的能力 |
| `fix` | 修复已有行为错误 |
| `refactor` | 不改变外部行为的结构调整 |
| `docs` | 文档、issue 记录、阶段完成卡或实现记录 |
| `test` | 独立的测试行为或测试覆盖变更 |
| `chore` | 构建、依赖、工具和仓库维护 |

`scope` 使用最小且稳定的范围，例如 `example`、`api`、`parser`、`docs`、`loop`、`tests`。如果仓库已有 Conventional Commits 或其他提交格式，优先兼容既有格式；本规范只补齐语言和闭环字段，不要擅自引入第二套格式。

标题要求：

- 一个 commit 只表达一个主要目的。
- 使用能描述结果的主动动词，不使用“修改一些内容”“更新代码”“完成任务”这类空话。
- 标题尽量控制在 72 个字符以内；中文按可读性优先，不为了硬凑字符截断关键信息。
- 不在标题中写完整验证日志、长 issue 描述或多个互不相关的目的。

中文任务示例：

```text
feat(example): 增加最短公开组装路径
test(example): 验证 add 买牛奶的完整调用结果
docs(loop): 记录阶段完成卡与关键断点
fix(parser): 正确拒绝缺少标题的新增命令
```

英文任务示例：

```text
feat(example): add the shortest public assembly path
test(example): verify the complete add-milk execution path
docs(loop): record the stage card and key breakpoints
fix(parser): reject add commands without a title
```

## Commit body

需要解释取舍、风险或验证时，使用短正文；正文标签也使用当前语言。不要复制整个 issue：

```text
为什么：<这次变更解决的具体问题>
做了什么：<主要变更和边界>
验证：<实际运行的命令及结果>
关联：<issue key、example 路径或设计记录>
```

英文版本：

```text
Why: <specific problem this change solves>
What: <main change and boundary>
Verify: <actual command and result>
Links: <issue key, example path, or design record>
```

正文只写已经发生的事实。计划、未验证的推测和下一阶段愿望放到 issue 或实现记录中。

## 实现 commit 与记录 commit

默认把阶段闭环拆成两个 commit：

```text
C1  feat(example): 增加最短公开组装路径
    代码、公开 API、example、测试

C2  docs(loop): 记录阶段完成卡与关键断点
    阶段完成卡、实现记录、issue/example/commit 回链
```

C1 的标题描述代码行为，C2 的标题描述记录行为；不要让 C2 只写“更新文档”，也不要让 C1 混入尚未验证的完成宣言。实现记录中分别保存 C1/C2 的完整 SHA 和原始标题。

若仓库必须使用单一 commit，在记录中明确说明代码、example、测试和闭环记录如何在同一个最终 SHA 中完成；不要记录一个尚不存在的 SHA，也不要为了填字段伪造 commit message。

## 提交前检查

1. 确认标题语言与当前用户语言一致。
2. 确认 type/scope 与仓库既有约定兼容。
3. 确认一个 commit 只有一个主要目的。
4. 确认标题是具体结果，不是过程口号。
5. 确认正文中的验证命令已经实际运行。
6. 确认实现记录会保存完整 SHA、原始标题、语言和关联对象。
7. 确认没有把历史 commit 重写成新语言或新格式。
