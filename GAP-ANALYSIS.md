# GAP-ANALYSIS：原 README vs 扩写语料

## 原 README 已覆盖（浓缩在一页）

- 论点：装机量竞赛 / QWERTY / KYAML 套缰绳
- 后缀 `.yml` vs `.yaml`
- 2001 出身与命名
- 设计血统表
- 版本线 1.0–1.2.2 与 MIME（点到为止）
- Rails → Ansible → K8s → Actions 扩散链
- 云原生爱用的原因
- 隐式类型与挪威 / `on:` / 版本号等坑（列表级）
- mindmap.html / report.html

## 原 README 的缺口（本次补齐）

| 缺口 | 对应文档 |
| --- | --- |
| 仓库内 README 正文在 §7 中部截断，后续造反者/KYAML/结论不完整 | 重写为索引 + 分篇 |
| 挪威问题无规范正则与 1.1/1.2 分界深挖 | `08` |
| Actions `on:` 无独立互操作分析 | `09` |
| 解析器默认版本矩阵缺失 | `10` |
| anchors / 多行 / tags / 多文档各自成篇缺失 | `11`–`14` |
| 安全（RCE / bomb）几乎未写 | `15` |
| schema/lint 工具链未展开 | `16` |
| 与 TOML/HCL/CUE/Jsonnet 对照表缺失 | `17` |
| KYAML 仅点名，无 KEP 状态与语法要点 | `18` |
| Helm 模板缩进/注入未写 | `19` |
| 可执行最佳实践与 CI 清单缺失 | `20` |
| 可引用时间线与分级书目缺失 | `21` `22` |
| 性能与生态地图可选篇 | `23` `24` |

## 仍可继续加深（未做或不强做）

- DoorDash 等具体生产事故：需原始 postmortem，本稿不编造。
- 各解析器符合性测试集跑分：需专项 harness。
- KYAML 与 helm 混用的大规模案例访谈。
