# 统一输入输出协议

在系统调用、JSON 输入或机器可读输出场景使用。普通对话保持自然 Markdown。

## 输入

输入必须符合 [input.schema.json](input.schema.json)。关键字段：

- `mode`: `topic_direction`, `audit`, `rewrite`, `compare`, `data_review` 或 `auto`
- `platform`: 目标平台
- `content`: 单稿正文；对比模式可改用 `variants`
- `context`: 受众、作者定位、目标、证据和约束
- `rewrite_options`: 改动幅度与表达形态
- `publish_data`: 数据复盘所需的已观察指标

`mode=auto` 时，根据实际载荷选择一个主模式。缺少执行所必需的内容时，返回 `status=needs_input`，不要虚构。

## 输出

输出必须符合 [output.schema.json](output.schema.json)。

- `protected_elements` 只记录输入中真实存在或用户明确提供的内容。
- `scores.topic_80` 与 `scores.audit_60` 可以分别为空，但不得相加。
- `observations` 放输入中可直接看到的事实。
- `inferences` 放从文本或数据推导出的判断，并说明依据。
- `rewrite.before_after` 只列实质修改；`final_draft` 给出完整稿件。
- `caveats` 记录缺失证据、未知平台因素或不能验证的绩效结果。

## 状态

- `ok`: 已按当前证据完成。
- `needs_input`: 缺少完成请求所必需的正文、版本或数据。
- `unsupported`: 请求超出内容导演边界，例如自动发布或伪造数据。

## 兼容原则

协议新增可选字段时保持向后兼容。改变字段含义、枚举或必填项时才提升主版本。

