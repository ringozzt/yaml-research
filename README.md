# YAML 调研报告

> 一个 2001 年诞生于"反 XML"邮件列表的数据格式，如何靠 Rails → Ansible → Kubernetes 这条载体链，坐上云原生的配置王座。
> 调研日期 2026-10-21 · 扩写为多文档语料 · 来源按一手（yaml.org / 规范 / RFC / KEP / 官方文档）与二手（技术博客与媒体复盘）分级标注

一句话结论：**YAML 赢的不是设计竞赛，是装机量竞赛**——2001 年它站在"人人恨 XML"的风口上，2004 年被 Rails 带进一代 Web 程序员的日常，2014 年被 Kubernetes 加冕为声明式配置的官方语言。它是配置格式里的 QWERTY 键盘：坑人人会背（挪威问题、`on:` 变 `true:`），但整个云原生生态的手指已经长成了它的形状，连 Kubernetes 官方 2025 年的最新动作都是给它套缰绳（KYAML），而不是换掉它。

## 可视化入口

- 交互思维导图：[mindmap.html](./mindmap.html)
- 图文版：[report.html](./report.html)
- 缺口对照：[GAP-ANALYSIS.md](./GAP-ANALYSIS.md)

## 文档目录（`docs/`）

| # | 文档 | 一句话 |
| --- | --- | --- |
| 01 | [命名：.yml vs .yaml](docs/01-naming-yml-vs-yaml.md) | 无功能区别；官方推 `.yaml`；RFC 9512 收口 |
| 02 | [出身 2001 反 XML](docs/02-origin-2001-anti-xml.md) | SML-DEV → Evans / Ben-Kiki / Ingy |
| 03 | [设计血统](docs/03-design-heritage.md) | Python / MIME / RFC822 / Data::Denter 拼装 |
| 04 | [规范版本](docs/04-spec-versions.md) | 1.0→1.2.2 + `application/yaml` |
| 05 | [扩散链](docs/05-diffusion-rails-ansible-k8s.md) | Rails → Ansible → K8s → Actions |
| 06 | [云原生为何爱它](docs/06-why-cloud-native-loves-it.md) | 注释、diff、声明式；以及没得选 |
| 07 | [隐式类型坑](docs/07-implicit-typing-pitfalls.md) | 猜错不报错的设计债总览 |
| 08 | [挪威问题深挖](docs/08-norway-problem.md) | 1.1 bool 正则；1.2 修了生态没修完 |
| 09 | [GitHub Actions `on:`](docs/09-github-actions-on-key.md) | 键被 1.1 工具链改成 `true` |
| 10 | [1.1 vs 1.2 解析器](docs/10-yaml11-vs-12-parsers.md) | PyYAML/libyaml/Psych/go-yaml/serde… |
| 11 | [Anchors / merge](docs/11-anchors-aliases-merge-keys.md) | 复用、round-trip 丢失、炸弹温床 |
| 12 | [多行标量](docs/12-scalars-multiline.md) | `\|` `>` 与 chomping |
| 13 | [Tags](docs/13-tags-and-custom-types.md) | `!!str` 与语言对象标签 |
| 14 | [多文档流](docs/14-multi-document-streams.md) | `---` / `load_all` 陷阱 |
| 15 | [安全反序列化](docs/15-security-deserialization.md) | unsafe load、YAML bomb |
| 16 | [Schema 工具](docs/16-schema-validation-tooling.md) | yamllint / kubeconform / Spectral |
| 17 | [对照其它格式](docs/17-vs-json-toml-hcl-cue-jsonnet.md) | 造反者与局部胜利 |
| 18 | [KYAML](docs/18-kubernetes-kyaml.md) | KEP-5295；1.34 Alpha / 1.35 Beta |
| 19 | [Helm 坑](docs/19-helm-templating-gotchas.md) | nindent、toYaml、YAML 注入 |
| 20 | [最佳实践](docs/20-best-practices-and-lint.md) | 纪律 + CI 清单 |
| 21 | [时间线](docs/21-timeline.md) | 有据节点年表 |
| 22 | [书目](docs/22-bibliography.md) | 一手/二手链接分级 |
| 23 | [性能与大文件](docs/23-performance-large-files.md) | 可选：别把 YAML 当吞吐格式 |
| 24 | [生态地图](docs/24-ecosystem-map.md) | 可选：产消与缰绳鸟瞰 |

## 怎么读

1. 先读本页论点，再打开 `mindmap.html`。
2. 要故事：`02` → `05` → `06`。
3. 要避坑：`07` → `08` → `09` → `19`。
4. 要落地：`10` → `16` → `20` → `18`。

## 方法论

- **不编造**未核到的数字、日期、引言。
- 每篇文末 **来源** 区分一手 / 二手 / 工程 issue。
- 原单文件 README 曾在仓库中截断；本索引保留原论点，细节迁入 `docs/*`。
