# 个人技能工坊 · 1.0.1 公开测试版

通过一问一答，把你的工作经验、材料和重复任务整理成可复用的 AI 技能。

**版本：1.0.1 · 作者：FueTsui · 许可：[MIT](LICENSE)**
**发行类型：公开测试版。** 文档整理日期：2026-10-13。[下载 v1.0.1](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)。

1.0.1 以受测 RC7 为来源，本次仅更新发行版本和发布文档，没有新增行为修复或模型测试。RC7 的完整稳定性验收未通过，输出限制遗漏和验证记录不准确的问题仍保留在本版。详细情况见[实测结果](docs/TEST-RESULTS.md)和[已知问题](docs/KNOWN-ISSUES.md)。

## 它能帮你做什么

描述一件想让 AI 反复完成的工作，提供已有材料或好坏样例。工坊会逐次补问关键缺口，吸收你主动补充的要求，整理输入、步骤、输出和完成标准，生成 `SKILL.md` 及必要资源。以后可以使用生成的业务技能完成重复工作，也可以继续通过工坊调整规则。

无需自己编写复杂提示词或填写整套问卷。实际的文件生成、脚本执行和模型能力由所用 AI 工具提供。详见[功能介绍](docs/FEATURES.md)。

## 三步开始

1. 从[发布页](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)下载 `personal-skill-builder-1.0.1.zip`，解压后进入 `personal-skill-builder` 文件夹，保留内部目录结构。
2. 让支持文件读取的 AI 显式读取该文件夹的 `SKILL.md`，并在需要时读取其引用资源；Windows 用户也可以使用[安装说明中的一键命令](docs/INSTALL.md)安装到当前用户的 `.agents\skills`，自动备份已有同名技能。
3. 发送下面这句话，并随时补充你的材料和要求：

> 使用刚加载的 personal-skill-builder 1.0.1，帮我把每周整理工作周报的流程制作成可复用技能。每次只问一个关键问题，信息足够后生成，并用样例验证。

生成完成后，日常工作调用生成的业务技能；需要改变工作规则时再用工坊升级它。

## 从公开仓库安装

在线指令从公开仓库安装；需要固定本次版本时，可使用 [v1.0.1 发布包](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)。

向支持技能安装的 AI 工具发送：

```text
Install this skill for me: https://github.com/FueTsui/personal-skill-builder
```

也可以在已安装 Node.js 和 npm 的终端运行，默认安装到当前项目：

```text
npx skills add https://github.com/FueTsui/personal-skill-builder
```

需要为 Codex 全局安装时，见[在线安装说明](docs/INSTALL.md#从公开仓库在线安装)。安装后按宿主方式刷新技能列表，在下一轮对话或新会话中使用，并读取实际 `VERSION` 确认版本；宿主的发现与触发能力需单独验证。

## 实测摘要

| 范围 | 已有实测结论 |
| --- | --- |
| 一问一答、主动补充、多轮生成 | 两个制作环境的本次样例观察到相应能力；不等于制作链整体通过 |
| 豆包制作阶段 | 未通过：可见金额泄漏及与正文不符的通过自评 |
| 豆包下游任务 | 6 次中 5 次通过；排序批次的备注未隐藏受限金额 |
| code-generator 制作阶段 | 未通过：把未触发的当天到期分支写成已验证 |
| code-generator 下游任务 | 6 次最终业务交付通过，过程中有自检或初稿修正 |
| 代码回归 | 73 项通过；这是工具检查结果，不是模型可靠率 |

测试材料为合成数据。上述结论来自 2026-10-13 的既有 RC7 测试，不是 1.0.1 的新增模型实测。换模型、换材料或换宿主后，仍需验证自己的任务。

## 包内导航

- [功能介绍](docs/FEATURES.md)：功能、适用场景与交付内容。
- [安装使用说明](docs/INSTALL.md)：在线安装、本地安装、创建、升级与重复执行。
- [实测结果](docs/TEST-RESULTS.md)：测试范围、结果、重复执行与证据边界。
- [已知问题](docs/KNOWN-ISSUES.md)：未修复问题、影响、临时核对方法和反馈格式。
- [发布说明](docs/RELEASE-NOTES.md)：本版介绍与版本说明。
- [机器可读结果](evidence/RESULTS.json)：供人工复核或工具读取的结果摘要。
- [版本来源](RELEASE-METADATA.json)与[文件校验清单](MANIFEST.sha256)：核对受测版本和交付文件。
- [技能入口](SKILL.md)：位于仓库及解压后技能目录的根目录。

## 文件与版本说明

发行版本由 `1.0.1-rc.7` 调整为 `1.0.1`，版本信息和公开文档同步更新；技能行为规则、脚本、模板及调用策略沿用 RC7。历史测试名称和问题编号保持原样，以便追溯，版本号变化不表示已知问题得到修复。

只提供一个 `personal-skill-builder-1.0.1.zip`：解压后得到 `personal-skill-builder/`，其中包含技能入口、资源、文档和脱敏实测摘要。仓库中的 `SKILL.md` 直接位于根目录。安装方法见[安装说明](docs/INSTALL.md)。

原始对话、完整实验日志、私人材料、环境专用审计代码和凭据不随本公开测试包分发。本技能不要求 TypeSafe/Jev API 密钥。自动发现、任意模型兼容性和长期稳定性不在已验证结论内。
