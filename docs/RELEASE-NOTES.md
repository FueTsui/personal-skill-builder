# 个人技能工坊 1.0.1 · 公开测试版

作者：FueTsui。许可：MIT。日期：2026-10-13。发行标签：`v1.0.1`。

## 功能介绍

个人技能工坊通过一问一答，把你的工作经验、已有材料和重复任务整理成可复用的 AI 技能。

你只需描述想反复完成的工作，随时补充要求或提供样例。AI 会逐步整理输入、处理步骤、输出和完成标准，生成技能文件，并支持后续修改和验证。无需自己编写复杂提示词，也不用填写整套问卷。

**1.0.1 是公开测试版，以受测 RC7 为来源。本次仅调整发行版本和发布文档，没有新增行为修复或模型测试；RC7 的已知失败仍保留，完整稳定性验收未通过。**

## 本版内容

- 一问一答创建技能，使用已有材料并吸收主动补充。
- 支持多轮生成、规则调整和已有工作的继续。
- 生成便携的 `SKILL.md` 及必要引用、模板或脚本。
- 提供可选的离线结构审计、实验计划与记录评估工具。
- 附功能介绍、安装使用说明、实测结果和已知问题。
- 安装说明附 Windows PowerShell 一键命令，安装到当前用户的 `.agents\skills`，自动备份同名旧技能。
- 提供 AI 安装指令和 `npx skills add` 在线安装方法，另附指定 Codex 全局安装的命令。

## 安装

向支持技能安装的 AI 工具发送：

```text
Install this skill for me: https://github.com/FueTsui/personal-skill-builder
```

或在已安装 Node.js 和 npm 的终端运行，默认安装到当前项目：

```text
npx skills add https://github.com/FueTsui/personal-skill-builder
```

为 Codex 安装到当前用户的全局目录：

```text
npx skills add https://github.com/FueTsui/personal-skill-builder --skill personal-skill-builder --agent codex --global
```

也可在[本版发布页](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)下载 `personal-skill-builder-1.0.1.zip`，解压后得到 `personal-skill-builder/`。Windows 通用 agents 技能位置为 `C:\Users\用户名\.agents\skills\personal-skill-builder`；[安装说明](https://github.com/FueTsui/personal-skill-builder/blob/v1.0.1/docs/INSTALL.md)提供自动备份同名旧版的 PowerShell 命令。

安装后按宿主方式刷新技能列表，在下一轮对话或新会话中使用，读取实际 `VERSION` 和 `SKILL.md` 确认版本。宿主是否支持该目录及自动触发，仍需实际验证。

## 实测与已知限制

既有 RC7 测试中，code-generator 六次最终业务交付通过；豆包六次下游任务中五次通过，一次在备注中显示了要求隐藏的金额。两个制作环境均有未通过项：豆包制作输出的金额限制未执行完整，code-generator 的验证记录把未触发的分支写成通过。

另有 73 项代码检查通过。上述计数适用于当次合成材料、加载方式和环境，不是 1.0.1 新增模型实测，也不代表长期可靠率、任意模型兼容性或自动触发成功率。最终业务交付通过保留了初稿和自检修正过程，不表示首次生成即完整。生成结果及验证声明仍需核对。

详细记录见[实测结果](https://github.com/FueTsui/personal-skill-builder/blob/v1.0.1/docs/TEST-RESULTS.md)和[已知问题](https://github.com/FueTsui/personal-skill-builder/blob/v1.0.1/docs/KNOWN-ISSUES.md)。原始对话、私人材料、账号信息及完整实验日志不随公开包分发。

## 文件与版本

发行版本由 `1.0.1-rc.7` 调整为 `1.0.1`，技能行为规则、脚本、模板及调用策略沿用 RC7。历史测试名称和问题编号保留，版本号变化不意味着已知问题已解决。

只提供一个 `personal-skill-builder-1.0.1.zip`，包含全部技能资源、文档和脱敏实测摘要。仓库 `SKILL.md` 位于根目录；ZIP 额外包装一层 `personal-skill-builder/`，便于直接安装整个目录。请结合随附校验文件检查下载完整性。

[查看 v1.0.1 发布页](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)。
