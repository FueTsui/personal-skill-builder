# 安装与使用

适用版本：**1.0.1 公开测试版**。本版以 RC7 为来源，仅更新发行版本和发布文档，没有新增行为修复或模型测试，已知失败保留；先用样例核对自己的工作流程。安装不会自动提供模型、账号或文件权限。

## 下载发布包

| 文件 | 包含内容 | 解压方式 |
| --- | --- | --- |
| `personal-skill-builder-1.0.1.zip` | 技能及资源、功能介绍、使用说明、实测结果、已知问题和校验信息 | 解压后进入 `personal-skill-builder/`，其中直接包含 `SKILL.md`、`VERSION` 和 `docs/` |

从 [v1.0.1 发布页](https://github.com/FueTsui/personal-skill-builder/releases/tag/v1.0.1)下载此 ZIP。安装时复制整个 `personal-skill-builder` 文件夹，保留内部结构；仓库中的 `SKILL.md` 也直接位于根目录。

## 从公开仓库在线安装

在线指令使用公开仓库当前内容；需要固定 `1.0.1` 时，使用上面的发布包和后续本地安装方法。

向支持技能安装的 AI 工具发送以下指令，让它按自身支持的方式读取仓库并安装：

```text
Install this skill for me: https://github.com/FueTsui/personal-skill-builder
```

也可以使用 [Skills CLI](https://github.com/vercel-labs/skills#install-a-skill)。安装 Node.js 和 npm 后，在目标项目目录运行：

```text
npx skills add https://github.com/FueTsui/personal-skill-builder
```

此命令默认安装到当前项目，按 CLI 提示选择目标工具。若要指定 Codex 并安装到当前用户的全局技能目录，可运行：

```text
npx skills add https://github.com/FueTsui/personal-skill-builder --skill personal-skill-builder --agent codex --global
```

该 CLI 的 Codex 全局目标为 `~/.agents/skills`，其中 `~` 为当前用户目录。Windows 对应的技能位置通常为 `C:\Users\用户名\.agents\skills\personal-skill-builder`；其他工具的支持目录与加载方式以其实际配置为准。安装后读取实际路径下的 `VERSION` 和 `SKILL.md` 确认版本。有同名旧版时，先备份并检查安装目标。在线 CLI 说明不代表已验证所用宿主的自动发现与触发能力。

## 方法一：先体验，不替换已有安装

把完整包解压到自己选择的测试目录。让具有文件读取能力的 AI 显式读取该位置的入口，例如：

> 请读取我提供的 personal-skill-builder 文件夹根目录中的 SKILL.md，按任务需要读取它引用的资源。先核对 VERSION 和入口版本均为 1.0.1，再使用这一份技能帮我创建周报整理技能。

将“我提供的 personal-skill-builder 文件夹”替换为实际路径，或在宿主中直接附加/引用该文件。已有同名技能时，明确要求读取这次解压的 `1.0.1`，避免使用旧安装。

纯聊天工具需通过其支持的附件或粘贴方式提供入口和所需引用资料；能否读取 ZIP、相对路径、文件格式取决于宿主。不要只提供入口却省略它需要的资源。无法创建文件或运行脚本时，让 AI 交付可复制内容并标记未执行项。

## 方法二：安装到技能目录

### Windows：一键安装到通用 agents 技能目录

解压发布 ZIP `personal-skill-builder-1.0.1.zip`，进入 `personal-skill-builder` 文件夹的根目录，确认其中直接有 `SKILL.md`、`VERSION`、`README.md` 和 `docs`，在这里打开 PowerShell。此命令用于发布 ZIP 的解压目录，不用于含 `.git` 的克隆仓库。复制下面整段命令执行一次，即可安装到 `C:\Users\用户名\.agents\skills\personal-skill-builder`。`$env:USERPROFILE` 自动取得当前用户目录，无需手动替换“用户名”。

```powershell
& {
    param([string]$UserDirectory = $env:USERPROFILE)
    $ErrorActionPreference = 'Stop'
    $source = (Resolve-Path -LiteralPath '.').Path
    if (-not (Test-Path -LiteralPath (Join-Path $source 'SKILL.md') -PathType Leaf) -or (Get-Content -LiteralPath (Join-Path $source 'VERSION') -Raw).Trim() -ne '1.0.1') { throw 'Please open PowerShell in the extracted personal-skill-builder 1.0.1 directory.' }
    $agentsRoot = [IO.Path]::GetFullPath((Join-Path $UserDirectory '.agents'))
    $skillsRoot = Join-Path $agentsRoot 'skills'
    $target = [IO.Path]::GetFullPath((Join-Path $skillsRoot 'personal-skill-builder'))
    if ([IO.Path]::GetDirectoryName($target) -ne $skillsRoot -or $source.TrimEnd('\') -eq $target.TrimEnd('\')) { throw 'Invalid installation location.' }
    New-Item -ItemType Directory -Path $skillsRoot -Force | Out-Null
    if (Test-Path -LiteralPath $target) {
        if (-not (Test-Path -LiteralPath $target -PathType Container)) { throw 'The installation target is not a directory.' }
        $backupRoot = Join-Path $agentsRoot 'skill-backups'
        New-Item -ItemType Directory -Path $backupRoot -Force | Out-Null
        $backup = Join-Path $backupRoot ('personal-skill-builder-' + (Get-Date -Format 'yyyyMMdd-HHmmss') + '-' + [guid]::NewGuid().ToString('N').Substring(0,8))
        Move-Item -LiteralPath $target -Destination $backup
        Write-Output "Previous version backed up to: $backup"
    }
    Copy-Item -LiteralPath $source -Destination $target -Recurse
    Write-Output "Installed 1.0.1 to: $target"
}
```

已有同名技能时，命令会先将整个旧目录移到当前用户的 `.agents\skill-backups`，再复制 `1.0.1`，避免旧文件混入新版；其他技能不受影响。若复制失败，可按命令输出的位置找回旧版。务必从解压后的 `personal-skill-builder` 根目录运行，不要从其上一级目录运行。

命令只安装本地文件，不下载模型、不安装依赖。所用 AI 工具仍需支持读取 `.agents\skills`；安装完成不等于已经验证该工具的自动发现和触发能力。若已有其他技能目录中的同名版本，应在首次使用时确认实际加载路径与 `VERSION`。

### 其他已配置的技能目录

也可以手动安装到当前工具实际使用的目录。以支持 `~/.codex/skills` 的 Codex 环境为例，放置为：

```text
~/.codex/skills/personal-skill-builder/
  SKILL.md
  VERSION
  README.md
  CHANGELOG.md
  LICENSE
  agents/
  references/
  assets/
  scripts/
  docs/
  evidence/
```

Windows 中 `~` 对应当前用户目录。若已有同名技能，先比较版本并把原文件夹备份到技能搜索目录之外，再替换整个文件夹；不要把两版文件混合覆盖。需要恢复时，用备份的完整旧目录替回。

安装后按宿主方式刷新技能列表，在下一轮对话或新会话中使用；加载时机和发现能力以实际宿主为准。若宿主支持 `$技能名` 调用，可输入：

> 使用 $personal-skill-builder，先确认加载的是 1.0.1，再帮我制作个人技能。

确认依据应是实际读取的 `VERSION` 和 `SKILL.md`，不能仅依赖模型口头说“已加载”。若无法自动找到技能，使用上面的显式路径方式。文件安装与旧版备份的检查不等于宿主加载验收；自动发现和触发需在所用 AI 工具中单独验证。其他宿主的目录和调用语法需按其实际支持方式设置。

## 创建第一个技能

给出一件重复工作即可开始：

> 使用 personal-skill-builder，帮我把每周整理工作周报的流程制作成可复用技能。每次只问一个关键问题，吸收我主动补充的要求，信息足够后生成，并用样例验证。

然后逐轮回答，也可以一次主动补充多项要求：

> 我会提供本周工作记录。周报保留“进展、问题、下周计划”三部分；没有来源的信息不要补写。这里有两份以往周报供你参考。

不知道怎样选时可以说“你先给建议”。若建议改变了业务口径，补充自己的实际要求。需要固定输出位置时，直接说明保存路径。

## 更新与继续

修改已有技能：

> 根据这份新模板升级已有技能，保留仍有效的要求，保存旧版，并重新验证受影响的场景。

换会话继续：

> 读取这份技能和需求记录，从上次进度继续。先采用最新明确要求，不重复询问已经有答案的问题。

恢复依赖你提供的文件或宿主可访问的历史；技能本身不会跨工具自动同步私有会话。

## 日常重复执行

假设工坊生成了名为 `weekly-report` 的业务技能。之后每周直接加载它：

> 使用 weekly-report，根据附件中的本周记录生成周报，沿用已确认的格式，标出缺少依据的内容。

工坊负责制作和维护规则；重复工作使用生成的业务技能。每批提供本次材料和必要参数，不让上一批记录替代新输入。

## 可选本地检查

先确认 `python --version` 为 3.10 或以上。进入安装包内的 `personal-skill-builder` 目录，在技能目录之外创建结果目录，然后运行：

```text
python -c "from pathlib import Path; Path('../personal-skill-builder-checks').mkdir(exist_ok=True)"
python scripts/toolkit.py audit . --output ../personal-skill-builder-checks/audit.json
```

结构检查通过不等于业务结果正确。没有 Python 时仍可做文本问答，但不能宣称已运行这些检查。

如果报告写入失败，先检查输出目录和宿主文件权限，不把写入失败当作检查通过。工具支持在原命令后加 `--stdout`，将完整 JSON 输出到标准输出，再由宿主文件工具保存；此时仍需提供 `--output` 参数。Windows 出现中文编码问题时，可使用 `python -X utf8` 运行。不同保存通道的实际结果应单独记录。

需要设计批量实验时可运行：

```text
python scripts/toolkit.py plan assets/experiment-suite.json --skill . --output ../personal-skill-builder-checks/run.json --rounds 2 --variants candidate
```

这一步只生成计划。随后在实际环境执行请求，保存每轮原始回复、产物和判定证据，按[评估方法](../references/evaluation.md)完善记录，最后再运行：

```text
python scripts/toolkit.py evaluate ../personal-skill-builder-checks/run.json --output ../personal-skill-builder-checks/evaluation.json
```

未执行或缺少证据的项目不得标为通过。需要检查任务产物的明确文本约束或数值范围时，另见[产物约束契约](../references/artifact-contracts.md)。这些工具不会调用模型，也不代替业务语义审阅。

`assets/fixtures/U13-damaged.xlsx` 是刻意损坏的失败测试材料，不是正常业务表格；不要为了“修好安装包”替换它。

## 交付前核对

核对结果是否保留本次规则、普通备注和例外情况；限制某类内容显示时，要检查正文、示例、代码块、备注与总结。若 AI 声称测试通过，检查对应输入是否真正触发该分支，并查看实际输出。具体失败案例与反馈方式见[已知问题](KNOWN-ISSUES.md)。
