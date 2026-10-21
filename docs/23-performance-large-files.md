# 性能与大文件（简记）

> YAML 为人读优化，不为超大日志吞吐优化。大文件场景要降期望。

## 观察

- 缩进块解析、隐式类型正则、别名图组合，都会让解析成本高于等价 JSON。
- 多 MB 的清单合并、上千文档的 stream，常见瓶颈在：**一次载入 AST**、重复 round-trip、锚点展开。
- libyaml/C 绑定通常快于纯 Python；但仍可能被别名炸弹或超深嵌套拖死。

```mermaid
flowchart LR
  Big[大 YAML] --> Parse[解析为树/事件]
  Parse --> Mem[内存峰值]
  Mem --> App[应用变换]
  App --> Dump[再序列化]
```

## 建议

1. 机器对机器优先 JSON / protobuf；YAML 留给人改的配置。
2. 流式场景用 event/token API（若库支持），避免整文件 `load`。
3. 限别名、限深度、限尺寸——既是安全也是性能。
4. Helm 渲染产物过大时，拆 chart / 用 Kustomize 分层，而不是单文件巨无。

## 来源

- 规范对 one-pass / 流的设计目标（1.2.2 Goals）
- RFC 9512 对资源耗尽类风险的提示
- 工程经验：K8s 清单体量与 CI 耗时（个案测量，不编造基准数字）
