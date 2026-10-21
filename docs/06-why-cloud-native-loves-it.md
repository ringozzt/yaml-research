# 为什么云原生爱 YAML

> 一半是真顺手，一半是没得选。资深 ≠ 觉得 YAML 完美；资深 = 肌肉记忆 + 知道坑在哪 + 有缰绳。

## 真顺手（对手衬托）

| 能力 | YAML | JSON | XML |
| --- | --- | --- | --- |
| 注释 | 有 | 无（硬伤） | 有但标签噪音大 |
| 人读密度 | 高 | 中 | 低 |
| diff / PR | 缩进即结构，行级清晰 | 括号噪音 | 开闭标签噪音 |
| 与 K8s 对象同构 | map/list/scalar 直接对应 | 也可，但无注释 | 需映射 |

GitOps 工作流（Git 为唯一事实源、PR 审配置）特别吃 **diff 友好 + 注释**——这两条 JSON 都吃瘪。

## 声明式契合

K8s manifest 是"期望状态文件"，不是脚本。YAML 的数据导向语法与之同构；`---` 一流多资源；标签/注解/备注都能自然写进文件。

```mermaid
flowchart LR
  Desire[期望状态 YAML] --> Git[Git PR]
  Git --> CD[Argo/Flux 等]
  CD --> API[Kubernetes API]
  API --> Actual[集群实际状态]
  Actual -->|reconcile| Desire
```

## 没得选的部分

- 工具链示例、Helm chart、CI 模板、培训材料默认 YAML。
- 换格式 = 与文档、同事、生成器、政策引擎同时开战。
- "资深爱用"里，相当比例是**幸存者偏差**：活过 XML 配置地狱和 JSON 无注释之后，YAML 是那一代筛剩下的实用解；同时他们会强制引号、开 yamllint / kubeconform。


## GitOps 放大了 diff 友好

当配置成为唯一事实源，审查单元从"机器上的一次变更"变成"PR 里的一行行 diff"。XML 的开闭标签与属性重排会产生嘈杂 diff；JSON 无注释导致"为什么这样配"只能写到 PR 描述或外挂文档。YAML 允许把意图写在字段旁，又让大多数编辑表现为行级变化——这与 GitHub/GitLab 的审查 UI 同构。

## 声明式并不等于 YAML，但 YAML 刚好够用

声明式的本质是"期望状态"，可以用 JSON、PB、CRD 生成物表达。YAML 获胜是因为它在**人写的期望状态**赛道上提供了注释与密度，而不是因为它有独特的声明式原语。一旦期望状态改由 UI/GitOps 生成器主导，KYAML 或 JSON 的比重可能上升；但人手修补与示例文档仍会长期留在 YAML 附近。

## 来源

- [一手] Kubernetes 文档与对象模型（声明式 API）
- [一手] YAML 1.2.2 Goals（human readable / native data structures）
- 本仓库总论：装机量 > 设计最优
