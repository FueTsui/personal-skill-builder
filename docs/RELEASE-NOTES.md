# 个人技能工坊 1.0.1 · 公开版

作者：FueTsui。许可：MIT。日期：2026-10-13。发行标签：`v1.0.1`。

## 功能介绍

个人技能工坊通过一问一答，把你的工作经验、已有材料和重复任务整理成可复用的 AI 技能。

你只需描述想反复完成的工作，随时补充要求或提供样例。AI 会逐步整理输入、处理步骤、输出和完成标准，生成技能文件，并支持后续修改和验证。无需自己编写复杂提示词，也不用填写整套问卷。

## 本版内容

- 一问一答创建技能，使用已有材料并吸收主动补充。
- 支持多轮生成、规则调整和已有工作的继续。
- 生成便携的 `SKILL.md` 及必要引用、模板或脚本。
- 提供可选的离线结构审计、实验计划与记录评估工具。
- 附功能介绍、安装使用说明、实测结果和已知问题。
- 安装说明统一使用当前用户的通用 `.agents\skills` 目录。

## 安装

安装 Node.js 和 npm 后，在终端运行：

```text
npx skills add https://github.com/FueTsui/personal-skill-builder --skill personal-skill-builder --agent codex --global
```

安装目录为 `C:\Users\用户名\.agents\skills\personal-skill-builder`，命令自动使用当前登录用户目录。安装后刷新所用 AI 工具的技能列表，在下一轮对话或新会话中使用，并读取实际 `VERSION` 和 `SKILL.md` 确认版本。详见[安装说明](https://github.com/FueTsui/personal-skill-builder/blob/v1.0.1/docs/INSTALL.md)。

[发布页](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)同时提供包含全部技能资源和文档的 `personal-skill-builder-1.0.1.zip`。

## 实测与已知限制

保留的历史实测中，code-generator 六次最终业务交付通过，制作阶段仍有未通过项：验证记录把未触发的分支写成通过。

另有 73 项代码检查通过。上述计数适用于当次合成材料、加载方式和环境，不代表长期可靠率、任意模型兼容性或自动触发成功率。最终业务交付通过保留了初稿和自检修正过程，不表示首次生成即完整。生成结果及验证声明仍需核对。

详细记录见[实测结果](https://github.com/FueTsui/personal-skill-builder/blob/v1.0.1/docs/TEST-RESULTS.md)和[已知问题](https://github.com/FueTsui/personal-skill-builder/blob/v1.0.1/docs/KNOWN-ISSUES.md)。原始对话、私人材料、账号信息及完整实验日志不随公开包分发。
