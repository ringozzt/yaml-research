# 文献与链接书目

> 按一手 / 二手 / 工程 issue 分级。写报告时优先一手。

## 一手：规范与注册

- YAML 1.2.2：https://yaml.org/spec/1.2.2/
- YAML 1.1：https://yaml.org/spec/1.1/
- 历史草案索引：https://yaml.org/spec/history/
- Bool type 1.1：https://yaml.org/type/bool.html
- Libraries 列表：https://yaml.org/libraries/
- RFC 9512 YAML Media Type：https://www.rfc-editor.org/rfc/rfc9512.html
- IANA `application/yaml`：https://www.iana.org/assignments/media-types/application/yaml

## 一手：Kubernetes / Helm / Actions

- KEP-5295 KYAML：https://github.com/kubernetes/enhancements/blob/master/keps/sig-cli/5295-kyaml/README.md
- KYAML Reference：https://kubernetes.io/docs/reference/encodings/kyaml/（以当前版本 docs 为准）
- K8s 1.34 发布说明：https://kubernetes.io/blog/2025/08/27/kubernetes-v1-34-release/
- Helm YAML Techniques：https://helm.sh/docs/chart_template_guide/yaml_techniques/
- GitHub Actions workflow syntax：https://docs.github.com/en/actions

## 一手：语言库

- PyYAML 文档：https://pyyaml.org/wiki/PyYAMLDocumentation
- Psych：https://docs.ruby-lang.org/en/master/Psych.html
- sml-dev-archive：https://github.com/yaml/sml-dev-archive

## 工程 issue（复现证据）

- openapi-generator#18510（NO→false）
- nacos#12969
- carvel-ytt#275（on→true）
- PyYAML load deprecation wiki

## 二手（复盘，引用需谨慎）

- allan.reyes.sh《YAML-Norway Law》
- StrictYAML 站点对隐式类型的批评（注意 1.1/1.2 表述争议：issue #186）
- 各类技术博客对 Actions HCL→YAML、DoorDash 等传闻——**使用前核原始材料**

## FAQ

- YAML Company FAQ：https://yaml.com/resources/faq/
- Wayback 2006 FAQ：https://web.archive.org/web/20060924190202/https:/yaml.org/faq.html
