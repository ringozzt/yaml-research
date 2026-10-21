# YAML 调研报告

> 一个 2001 年诞生于"反 XML"邮件列表的数据格式，如何靠 Rails → Ansible → Kubernetes 这条载体链，坐上云原生的配置王座。
> 调研日期 2026-10-21 · 来源按一手（yaml.org / Wikipedia 条目与规范史 / Ruby 官方文档）与二手（技术博客与媒体复盘）分级标注

一句话结论：**YAML 赢的不是设计竞赛，是装机量竞赛**——2001 年它站在"人人恨 XML"的风口上，2004 年被 Rails 带进一代 Web 程序员的日常，2014 年被 Kubernetes 加冕为声明式配置的官方语言。它是配置格式里的 QWERTY 键盘：坑人人会背（挪威问题、`on:` 变 `true:`），但整个云原生生态的手指已经长成了它的形状，连 Kubernetes 官方 2025 年的最新动作都是给它套缰绳（KYAML），而不是换掉它。

## 思维导图

```mermaid
mindmap
root((YAML<br/>配置格式))
名字与后缀
官方推荐 .yaml（2006 年 FAQ 起）
.yml 是 DOS/Windows 三字符扩展名残留
同 .htm 与 .html 的关系
名字先是玩笑 Yet Another Markup Language
2001 年底改递归缩写 YAML Ain't Markup Language
出身（2001）
Clark Evans 提议，Ingy döt Net、Oren Ben-Kiki 共创
诞生于 sml-dev 邮件列表（简化 XML 运动）
初衷：给人读的数据序列化，不是文档标记
设计血统
缩进分层借 Python
序列化思路借 Perl 的 Data::Denter（Ingy）
--- 分隔符借 MIME（RFC 2046）
键值风格借 RFC 822 邮件头
转义借 C；流式处理受 SAX 启发
版本线
1.0（2004）
1.1（2005）布尔爆炸的源头
1.2（2009）把 JSON 变成子集
1.2.2（2021）现行版
application/yaml MIME 2024 年定稿
扩散链
2003 Syck（why the lucky stiff）进 Ruby 标准库
2004 Rails 的 database.yml：一代人的第一次 YAML
2012 Ansible playbook：运维圈跟上
2014 Kubernetes manifest：加冕
Docker Compose / Helm / GitLab CI 全面跟进
GitHub Actions 2018 用 HCL，2019 改版投奔 YAML
为什么云原生爱它
声明式：map/list/scalar 1:1 映射 API 对象
有注释（JSON 的硬伤）
diff 干净、PR 可逐行 review：喂饱 GitOps
--- 一文件多资源
对手的缺陷衬托：XML 啰嗦、TOML 深嵌套弱
原罪
隐式类型推断：猜错不报错，静默地错
挪威问题：country NO 被读成 false（1.1）
on 键被 1.1 解析器读成 true（GitHub Actions 经典坑）
version 1.10 变 1.1；22:30 变六十进制 1350
规范复杂度比 JSON 大一个数量级，解析器行为不一致
造反者与缰绳
TOML（2013）：冲着 YAML 的含糊来
Jsonnet / CUE / HCL：给配置加类型与逻辑
KYAML（2025，KEP 5295）：K8s 官方的严格子集
判断
赢的是时间点 + 载体链，不是设计最优
资深 = 肌肉记忆 + 知道坑埋在哪
```

交互版：[mindmap.html](./mindmap.html) · 图文版：[report.html](./report.html)

## 1. 先分清：.yml 和 .yaml 是同一个东西

- **没有功能区别**，解析器一视同仁。
- **官方推荐 `.yaml`**：yaml.org 的 FAQ 自 2006 年起就明确推荐四字符后缀。
- **`.yml` 的由来**：DOS/Windows 时代扩展名只能三个字符（如同 `.htm` 之于 `.html`），加上 Rails 时代 `database.yml` 这类祖传文件名把习惯钉死，于是两派并存至今：Kubernetes 社区偏 `.yaml`，老牌 Ruby/CI 生态偏 `.yml`。
- 2024 年 MIME 类型 `application/yaml` 定稿，后缀之争算是有了官方句号——但存量习惯不会改。

## 2. 出身：2001 年，"反 XML"运动里的一封帖子

- **人物**：Clark Evans 提出，Ingy döt Net（Brian Ingerson）与 Oren Ben-Kiki 共同设计。
- **现场**：2001 年 5 月 11 日，Evans 在 **sml-dev** 邮件列表发布 YAML Draft 0.1——这个列表本身就是从 xml-dev 分裂出来、专门研究"如何简化 XML"的。按 Grokipedia 的复盘，更早的线索是 1999 年底 Evans 已在讨论代号 "YML" 的想法，yaml.org 域名注册于 2001 年 1 月 5 日。（二手来源）
- **名字的两次人生**：最初叫 *Yet Another Markup Language*——"又一个标记语言"，是 XML 家族疯狂膨胀年代的自嘲；2001 年 12 月至 2002 年 4 月间改成递归缩写 ***YAML Ain't Markup Language***，刻意声明：我处理的是数据，不是文档标记。
- **它要解决的问题**：XML 写配置太啰嗦，标签噪音淹没数据本身。YAML 的赌注是"让数据文件长得像人随手会写的样子"。

## 3. 设计血统：没有原创，全是借来的好零件

| 零件 | 借自 |
| --- | --- |
| 缩进表示层级 | Python |
| 纯文本序列化思路 | Perl 的 Data::Denter（Ingy 本人作品） |
| `---` 文档分隔符 | MIME（RFC 2046） |
| `key: value` 风格 | RFC 822 邮件头 |
| 转义序列 | C |
| 流式读写模型 | SAX |

这个拼装血统解释了它的性格：每个零件都"眼熟、好上手"，但拼在一起的整体语义（尤其是类型推断）没人能一眼看全——日后所有坑的种子都埋在这里。

## 4. 版本线：1.1 埋雷，1.2 收编 JSON

| 版本 | 日期 | 关键变化 |
| --- | --- | --- |
| 1.0 | 2004-01-29 | 首个正式规范 |
| 1.1 | 2005-01-18 | 功能膨胀期；布尔识别规则在这里失控（见 §7） |
| 1.2 | 2009-07-21 | 与 JSON 对齐：合法 JSON 即合法 YAML，JSON 成为其子集 |
| 1.2.2 | 2021-10-01 | 现行版本，主要是勘误澄清 |

注意一个长尾事实：规范 2009 年就进了 1.2，但**最流行的解析器 PyYAML 至今默认仍按 1.1 的类型规则工作**——"规范说 A、解析器做 B"是 YAML 生态的常态。

## 5. 扩散链：三级跳登上王座

1. **2003，Syck**：传奇黑客 why the lucky stiff 写的 C 语言 YAML 解析器被收进 **Ruby 标准库**——YAML 第一次有了主流语言的"户口"。Ruby 2.0 后由 Psych（libyaml 封装）接棒，Syck 退场。（Ruby 官方文档）
2. **2004，Rails**：`database.yml` 成为无数 Web 教程的第一课。一代程序员对"配置文件"的肌肉记忆，是 Rails 给的。
3. **2012，Ansible**：playbook 用 YAML 写，运维/DevOps 圈集体入坑；同期 Puppet/Chef/Salt 的配置生态也在往声明式文本靠。
4. **2014，Kubernetes**：加冕时刻。K8s 的声明式 API 需要一种"写给人看的期望状态文件"，YAML 的 map/list/scalar 与 API 对象 1:1 对应，`---` 支持一文件多资源，注释可写运维备注——manifest 从此默认 YAML。
5. **生态引力期**：Docker Compose、Helm、Kustomize、Argo CD、GitLab CI、OpenAPI 描述全面 YAML 化；**GitHub Actions 2018 年第一版用的还是 HCL，2019 年改版直接换成 YAML**——这是最有说服力的一票：连有自研语言（HCL）的阵营之外、坐拥 VS Code 生态的微软，都选择了顺应存量习惯。

## 6. 为什么云原生资深程序员爱用它

**真顺手的部分（对手衬托的）：**
- **有注释**。JSON 为数据交换而生、不支持注释，这对"需要写明为什么这么配"的配置文件是硬伤；XML 有注释但标签噪音太大。
- **diff 友好**。缩进即结构、无闭合标签，改一行就是一行——PR review 和 GitOps（配置即代码、Git 为唯一事实源）的工作流靠这个成立。
- **声明式契合**。K8s manifest 是"期望状态文件"，YAML 的数据导向语法与之天然同构。

**没得选的部分：**
- 工具链全员说 YAML：不用它，就是和 Helm、Argo、CI、文档里的一切示例过不去。
- 所谓"资深偏好"，相当比例是**幸存者经验**：他们亲历过 XML 配置地狱和 JSON 无注释的痛，YAML 是那一代人筛剩下的最优解；同时他们知道每个坑埋在哪，并有 linter + schema 校验（kubeconform 等）的缰绳可用。

## 7. 原罪：隐式类型推断——配置文件会静默地撒谎

YAML 最大的设计债：**它猜你写的值是什么类型，猜错不报错**。著名事故模式：

- **挪威问题**：YAML 1.1 认 22 种布尔写法（`y/n/yes/no/on/off` 及大小写变体）。于是 `country: NO`（挪威国家代码）被读成 `false`——DoorDash 等公司的生产配置中过招，K8s manifest、CI 配置里都有案底。
- **`on:` 变 `true:`**：GitHub Actions 工作流的触发键 `on`，被 1.1 规则的解析器读成布尔键 `true`，是 YAML 工具链 interop 的经典坑。
- **版本号蒸发**：`version: 1.10` → 浮点数 `1.1`。
- **六十进制**：YAML 1.1 里 `22:30` 可被读成整数 1350（22×60+30）。
- **前导零**：`0644` 在 1.1 是八进制 420、在 1.2 是十进制 644，同一个文件两个数。

这类 bug 的共同点：**文件合法、无报错、行为悄悄错**——比语法错误贵得多。缓解手段只有纪律：可疑字符串一律加引号，配
...[truncated 579 chars]