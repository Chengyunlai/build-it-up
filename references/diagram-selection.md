# 图表选择、命名与质量参考

本参考把 draw.io 官方文档中的图型和最佳实践转成可执行的选图规则。它不是要求每次都画完整架构图，而是帮助 AI 根据当前问题选择最能解释事实的最小图。

## 先判断问题，再选图

| 用户要理解什么 | 首选图型 | 图中必须出现 | 不要画成 |
| --- | --- | --- | --- |
| 用户做什么、系统接下来做什么 | 流程图 / swimlane | 真实输入、处理步骤、成功结果、至少一个重要失败分支 | “目标 → 方案 → 好处” |
| 前端、服务、数据库如何先后调用 | UML 时序图 | 参与者、调用方向、请求 / 返回值、关键异常 | 没有消息内容的横向方框 |
| 一个对象如何从创建走到完成或失败 | 状态图 | 状态、触发事件、守卫条件、状态输出 | 把状态名当作流程步骤 |
| 服务或模块如何分层协作 | C4 / 组件图 | 使用者、系统边界、容器 / 组件、依赖方向 | 把所有类和文件都塞进去 |
| 数据在哪里产生、处理、保存和输出 | 数据流图 | 外部实体、处理、数据存储、数据流名称 | 只有模块名没有数据名 |
| 数据表如何关联 | ER 图 | 实体、关键字段、主外键、基数 | 只有几个数据库图标 |
| 业务流程由谁负责、如何交接 | BPMN / 跨职能流程图 | 泳道、任务、事件、网关、交接关系 | 用普通箭头替代所有业务语义 |
| 类、接口和依赖如何组织 | UML 类图 | 类 / 接口、公开方法、关系类型、必要属性 | 把运行时调用顺序画成类图 |
| 方案的本阶段边界是什么 | 范围图 / 分层图 | 本阶段、后续、明确不包含项、验证证据 | 抽象的“大 / 快 / 好”比较 |

### 选图优先级

1. 先问“读者需要回答哪个具体问题”，不要从图形库倒推图型。
2. 如果问题同时包含“用户如何操作”和“系统如何协作”，拆成两张互补图：一张用户流程图，一张时序图或架构图。
3. 如果只是为了证明一条调用路径，优先画 3–8 个节点的具体流程，不要提前画完整系统架构。
4. 只有当边界、依赖或数据关系本身是当前决策时，才增加架构、数据流或 ER 图。
5. 每张图都要保留一个可复现的输入和一个可观察的结果；不能用解释文字补救空泛图。

## 高质量参考例子

以下优先使用 draw.io 官方文档和官方 `jgraph/drawio-diagrams` 仓库中的可打开源文件。它们是学习布局、语义和图型边界的参考，不是要求复制视觉样式。

### 官方图型说明

- [Example technical diagrams](https://www.drawio.com/docs/diagram-types/)：按软件开发、IT 基础设施、安全、工程、流程等场景组织示例。
- [Process map / flowchart](https://www.drawio.com/docs/best-practice/process-map-flowchart/)：判断什么时候使用流程图、过程模型或泳道图。
- [UML sequence diagrams](https://www.drawio.com/docs/diagram-types/uml/sequence-diagrams)：参考参与者、生命线和消息顺序。
- [UML state machine diagrams](https://www.drawio.com/docs/diagram-types/uml/state-diagrams)：参考状态、事件和转换条件。
- [C4 modelling](https://www.drawio.com/docs/diagram-types/c4-modelling)：参考从系统上下文到容器、组件的分层架构表达。
- [Data flow diagrams](https://www.drawio.com/docs/diagram-types/data-flow-diagrams)：参考外部实体、处理、数据存储和数据流的表达。
- [Entity relationship models](https://www.drawio.com/docs/diagram-types/entity-relationship-tables)：参考实体、字段、主外键和关联基数。
- [BPMN 2.0](https://www.drawio.com/docs/diagram-types/bpmn-2-0)：参考标准化业务流程、事件、任务和网关。

### 可直接在 draw.io 中打开的官方源文件

这些是官方文档引用的 raw `.drawio` 文件；生成网页编辑器链接时，可以使用对应的 raw URL 作为模板或导入源。

- [Gitflow examples](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/blog/gitflow-examples.drawio)：适合参考分支、合并和发布路径如何分层。
- [C4 example](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/blog/C4.drawio)：适合参考系统上下文、容器和组件的层级关系。
- [Sequence diagram examples](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/examples/sequence-diagram-examples.drawio)：适合参考参与者、消息和返回路径。
- [Class diagram example](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/examples/class-diagram-example.drawio)：适合参考类、属性、方法和关系线。
- [UML activity example](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/examples/uml-activity-example.drawio)：适合参考活动、分支和合并。
- [UML state diagram example](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/blog/uml-state-diagram-smart-lock.drawio)：适合参考对象状态和触发条件。
- [Data flow example](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/blog/data-flow.drawio)：适合参考数据在实体、处理和存储之间的流动。
- [Kanban examples](https://raw.githubusercontent.com/jgraph/drawio-diagrams/dev/examples/kanban-examples.drawio)：适合参考状态列、卡片和工作流可视化。

官方模板库入口：[More templates on GitHub](https://www.drawio.com/docs/diagram-types/template-diagrams-on-github/)。模板可作为结构参考，但必须替换成当前任务的真实输入、处理和结果；不能拿模板标题代替需求分析。

## 图表命名规范

### 两层命名

每张图使用两层名称：

1. **文件 / 页面名**：用于检索和稳定互链。issue 已发布时使用 `<issue-key>__<category>__<short-kebab-title>`；未发布时使用 `stage-<nn>__<category>__<short-kebab-title>`。名称保持短、稳定，不随聊天措辞反复变化。
2. **画布底部可见名称**：在主图下方单独放置一条文本，作为读者看到的任务名称。它不能挡住节点和连线，也不能放在主流程中间。

### 底部名称格式

根据用户语言生成，中文任务默认使用：

```text
图名：<具体任务结果>｜图型：<图型>｜阶段：<阶段或 issue key>
```

例如：

```text
图名：输入命令后创建任务｜图型：用户流程｜阶段：stage-01
```

英文任务使用同样的结构和英文文本：

```text
Diagram: Create a task from a command | Type: User flow | Stage: stage-01
```

命名要求：

- 优先描述“用户做了什么，以及最后得到什么”，例如“输入命令后创建任务”，不要只写“系统架构”或“流程图”。
- 通常控制在 8–24 个汉字或 4–10 个英文词；必要时可加 issue key，但不要把完整 issue 标题复制到底部。
- 多图时让名称区分视角，例如“用户提交订单｜用户流程”“订单创建调用链｜时序图”“订单状态变化｜状态图”。
- 名称必须与图内具体输入和结果一致；如果图改了场景，名称也要同步更新。
- 底部名称是图内可见说明，不替代文件名、页面名或 issue 互链。

### 底部放置与样式

- 放在所有主要节点和连线下方，水平居中；为画布底部预留可见空间。
- 使用普通文本或无边框文本，不连接到任何节点，不参与布局算法。
- 使用比主节点更弱的视觉层级，例如较小字号、灰色文字；名称仍必须可读。
- 多页图每页都有自己的底部名称；如果一个文件中的页面属于同一 issue，页面名和底部名称都要区分视角。
- 导入 Mermaid 或 raw XML 后，检查底部名称是否仍在画布内、是否被裁切、是否与缩放 / 页面边界重叠。

## 生成前后的质量检查

生成前：

- [ ] 已从需求中提取具体输入、处理动作和可观察结果。
- [ ] 图型是由问题决定，而不是默认使用流程图。
- [ ] 已确定图名、图型、阶段 / issue 身份和语言。
- [ ] 如果使用模板，已确认模板结构适合当前问题，并计划替换所有示例内容。

生成后：

- [ ] 读者只看图也能回答“谁做了什么、系统如何处理、最后发生什么”。
- [ ] 节点、箭头、泳道或关系线的语义一致，成功和失败路径没有混淆。
- [ ] 图中没有与当前决策无关的装饰节点和未来架构。
- [ ] 底部名称存在、可读、没有挡住主图，并与文件 / 页面命名一致。
- [ ] 在 draw.io 中导入或修改后已截图 / 读取图形源回读验证。

## 资料来源与使用边界

本参考使用 draw.io 官方文档和官方 `jgraph/drawio-diagrams` 仓库作为资料来源，访问日期为 2026-09-03。外部模板、图标库和第三方 MCP 不因被列为参考就获得本项目的安全或可用性保证；实际绑定仍必须遵守 [drawio-binding.md](drawio-binding.md) 的 D0–D4 探测和回读规则。
