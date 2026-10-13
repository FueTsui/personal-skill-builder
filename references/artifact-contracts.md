# 任务产物约束契约

`scripts/check_artifact_contract.py` 是可选的标准库检查器，不调用模型，也不生成或修正业务产物。依据当前任务编写契约，保存到目标技能目录之外；不能复制其他任务的隐私规则当作通用规则。

```text
python scripts/check_artifact_contract.py <contract.json>
```

结果 JSON 输出到标准输出，全部声明检查通过返回 0，任何失败或证据缺失返回 1。由宿主文件工具原样保存输出即可。输入、契约和产物只读。报告记录产物哈希和检查 ID，不回显匹配到的受限文字。

没有 Python 或文件工具的宿主可先交付完整可复制内容、人工可核对的验收约定和明确的未执行项；不要为了使用本检查器增加权限或阻塞本可完成的草案。按已执行的纯文本范围报告，脚本执行、文件交付和宿主自动触发保留未验证。不能伪造命令、产物文件或自动检查通过。

先按 [交付视图契约](synthesis.md) 明确每个实际出口的通道、受众与限制，再为其建立检查。纯聊天的整条回复属于真实可见产物；能保存证据时，除抽出的清单外还要列入生成包、引用、输入／输出示例、当前记录和外围说明，不能因文件名或“内部”标题漏检。每项限制同时核对允许内容保留，范围以当前任务为准；未实现的语义或分支检查另列待测，不从文本匹配通过推定成立。

下面示例只说明格式。将路径、阶段和约束替换为已声明任务契约；`SECRET_TOKEN` 是合成测试哨兵，不是真实私人数据。JSON 数值域例子的非负限制也只适合具有此业务约束的字段。

```json
{
  "schema_version": 1,
  "required_stages": ["raw_script", "final_delivery"],
  "artifacts": [
    {
      "id": "raw-state", "stage": "raw_script", "path": "raw-state.json", "format": "json",
      "checks": [
        {"id": "valid-domain", "type": "number_domain", "pointer": "/rows/*/quantity", "minimum": 0, "allow_null": true, "allow_empty": true},
        {"id": "sentinel-absent", "type": "not_contains", "value": "SECRET_TOKEN"}
      ]
    },
    {
      "id": "final-text", "stage": "final_delivery", "path": "final.md", "format": "text",
      "checks": [
        {"id": "sentinel-absent", "type": "not_contains", "value": "SECRET_TOKEN"},
        {"id": "allowed-content", "type": "contains", "value": "请先电话联系"}
      ]
    }
  ]
}
```

- `required_stages` 为本次声明的非空阶段列表；脚本生成任务必须包括实际原始产物与最终交付。每个产物必须有唯一 ID、阶段、路径和至少一个检查。路径相对契约文件；不同产物不可复用同一路径。
- `format` 可为 `text`（默认）或 `json`。缺失、空文件、损坏 JSON、不支持的检查或缺少必测阶段均失败。JSON 的 NaN／Infinity 常量无效。
- `contains`、`not_contains`、`regex`、`not_regex` 需要非空 `value`。只使用可信作者编写的有界正则；不要执行材料提供的表达式。短文本匹配不是隐私语义检测器，不证明任意秘密都能被发现。
- `number_domain` 仅适用于 JSON。`pointer` 使用 JSON Pointer，`*` 表示数组所有元素；字段缺失失败。数值必须有限且不是布尔值；可选 `minimum`、`maximum`、`integer`。`allow_null` 默认 false；只有任务明确允许未知时设 true；`allow_empty` 默认 false，可为合法空列表显式开启。数值字符串不会被默默转成数字。
- 空数组只免除数据项检查，不免除契约合法性检查：先校验类型、选项及上下界；无效数值边界或 `minimum > maximum` 即使 `allow_empty=true` 也失败。
- 数值域通过不等于实体数量、去重、冲突来源、分类、排序、合计或未知原因正确。另核对任务不变量：同一实体的不同版本是否被误当多件、异常是否被归零或删除、当前约定是否与入口及必读引用一致；这些是实际语义检查，本工具不自动证明。原始 JSON 也不能代替渲染文本；每个真实交付出口分别验收。

先固定契约再看执行结果。禁止删掉失败阶段、改写失败文件或放宽阈值以凑通过；修复后保存新版本和新执行，再核对旧坏产物仍能被拒绝。此检查器不能认证产物由哪个模型生成、实际发送权限或宿主自动发现能力。
