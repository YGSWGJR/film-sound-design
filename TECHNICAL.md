# 技术文档：电影音效设计 Skill

- Skill 名称：`film-sound-design`
- 显示名称：电影音效设计
- 包版本：`1.0.2`
- 文档日期：`2026-10-23`
- 适用环境：支持 Codex Skills 的 Codex 桌面端、CLI 或 IDE 环境
- 依赖：无第三方软件、无 MCP 服务、无可执行脚本

本文档面向需要部署、迁移、审核或维护本 Skill 的使用者，覆盖文件架构、跨平台安装、调用方式、验证、升级、安全边界与故障排查。项目概览和快速开始请先阅读 [README.md](README.md)，人工智能工具参与情况见 [AI_USAGE.md](AI_USAGE.md)。

## 1. 压缩包内容

解压后应看到：

```text
电影音效设计Skill-v1.0.2.zip
└── film-sound-design/
    ├── SKILL.md
    ├── agents/
    │   └── openai.yaml
    └── references/
        ├── sound-strategy.md
        ├── edit-and-mix.md
        └── video-study-notes.md
└── 技术文档-电影音效设计Skill跨电脑安装与使用.md
```

各文件作用：

| 路径 | 是否必需 | 作用 |
| --- | --- | --- |
| `film-sound-design/SKILL.md` | 必需 | Skill 入口，包含触发描述和核心工作流 |
| `film-sound-design/agents/openai.yaml` | 建议保留 | Codex UI 显示名称、简述和默认提示词 |
| `film-sound-design/references/sound-strategy.md` | 建议保留 | 场景、声音类型和分层配方 |
| `film-sound-design/references/edit-and-mix.md` | 建议保留 | 入点、转场、混音和 QC 清单 |
| `film-sound-design/references/video-study-notes.md` | 建议保留 | 视频学习核验记录和来源 |
| 技术文档 | 可选 | 安装、迁移和维护说明，不参与 Skill 执行 |

不要把整个压缩包直接放进 `skills` 目录，也不要只复制 `SKILL.md`。应解压后复制完整的 `film-sound-design` 文件夹，才能保留所有参考文档和相对链接。

## 2. 安装位置

Codex 从以下目录发现个人 Skill：

```text
$CODEX_HOME/skills
```

如果没有设置 `CODEX_HOME`，默认位置是：

```text
用户主目录/.codex/skills
```

常见操作系统默认路径：

| 系统 | 安装后的完整路径 |
| --- | --- |
| Windows | `C:\Users\<用户名>\.codex\skills\film-sound-design` |
| macOS | `/Users/<用户名>/.codex/skills/film-sound-design` |
| Linux | `/home/<用户名>/.codex/skills/film-sound-design` |

正确结构必须类似：

```text
%USERPROFILE%\.codex\skills\film-sound-design\SKILL.md
```

错误结构示例：

```text
skills\film-sound-design\film-sound-design\SKILL.md
skills\电影音效设计Skill-v1.0.2\film-sound-design\SKILL.md
skills\SKILL.md
```

后三种情况下，Codex 通常无法正确发现该 Skill。

## 3. Windows 安装步骤

### 3.1 解压

1. 将 `电影音效设计Skill-v1.0.2.zip` 解压到一个临时目录。
2. 确认临时目录中存在 `film-sound-design\SKILL.md`。

### 3.2 确定 Codex 技能目录

在 PowerShell 中执行：

```powershell
if ($env:CODEX_HOME) {
    $codexHome = $env:CODEX_HOME
} else {
    $codexHome = Join-Path $env:USERPROFILE ".codex"
}

$skillsDir = Join-Path $codexHome "skills"
$skillsDir
```

输出结果就是应使用的 Skill 根目录。如果目录不存在：

```powershell
New-Item -ItemType Directory -Force -Path $skillsDir
```

### 3.3 复制技能文件夹

把命令中的 `<解压后的目录>` 替换为实际路径：

```powershell
Copy-Item `
  -LiteralPath "<解压后的目录>\film-sound-design" `
  -Destination $skillsDir `
  -Recurse
```

示例目标：

```text
C:\Users\<用户名>\.codex\skills\film-sound-design
```

如果该目录已经存在，先退出 Codex，备份旧目录，再整体替换。不要只覆盖部分文件，以免新旧说明和引用文件版本不一致。

### 3.4 验证文件结构

```powershell
$skillDir = Join-Path $skillsDir "film-sound-design"

Test-Path "$skillDir\SKILL.md"
Test-Path "$skillDir\agents\openai.yaml"
Test-Path "$skillDir\references\sound-strategy.md"
Test-Path "$skillDir\references\edit-and-mix.md"
Test-Path "$skillDir\references\video-study-notes.md"
```

五项都应返回 `True`。

## 4. macOS / Linux 安装步骤

解压后，在终端执行：

```bash
if [ -n "${CODEX_HOME:-}" ]; then
  codex_home="$CODEX_HOME"
else
  codex_home="$HOME/.codex"
fi

skills_dir="$codex_home/skills"
mkdir -p "$skills_dir"
cp -R "<解压后的目录>/film-sound-design" "$skills_dir/"
```

验证：

```bash
test -f "$skills_dir/film-sound-design/SKILL.md" && echo "Skill 安装正确"
find "$skills_dir/film-sound-design" -maxdepth 2 -type f
```

如果目标目录已经存在，请先备份，再替换整个目录。

## 5. 启用与调用

安装完成后：

1. 完全退出并重新打开 Codex。
2. 新建对话，确保新会话重新加载 Skill 清单。
3. 在提示词中显式写 `$film-sound-design`。

推荐调用方式：

```text
使用 $film-sound-design 给这段 60 秒汽车广告设计音效层级和关键入点。
```

```text
使用 $film-sound-design 诊断这条剪辑：为什么转场很生硬，声音应该怎么调整？
```

```text
使用 $film-sound-design 根据下面的画面对照表生成 sound cue sheet，
重点处理对白、悬念、两次转场和最终品牌落点。
```

显式调用比“帮我加点电影感”更可靠。若运行环境支持 Skill 选择器，也可以在可用技能列表中选择“电影音效设计”。

## 6. 如何使用这个 Skill 得到好结果

输入尽量包含以下信息：

| 信息 | 示例 |
| --- | --- |
| 成片类型 | 剧情短片、汽车广告、旅拍、科技产品片 |
| 目标时长 | 30 秒 / 60 秒 / 3 分钟 |
| 画面对照表 | 时间码 + 镜头内容 + 动作或卖点 |
| 情绪曲线 | 平静 → 悬疑 → 冲击 → 品牌落点 |
| 对声音的约束 | 无旁白、只用音效、音乐克制、需要真实感 |
| 交付平台 | 影院、电视、视频网站、手机竖屏 |
| 已有工程信息 | 对白轨、音乐轨、关键音效和音量问题 |

如果提供了视频或工程文件，Skill 会优先按真实剪辑点分析；如果只有文字，则会输出可执行的 cue sheet 和声音设计建议。

## 7. 常见问题

| 现象 | 原因 | 处理 |
| --- | --- | --- |
| 新电脑中看不到 Skill | 目录层级多了一层 | 确保路径以 `skills/film-sound-design/SKILL.md` 结尾 |
| 能看到文件夹但无法调用 | Codex 启动时尚未重新加载 | 完全退出并重启，再新建对话 |
| 只复制了 `SKILL.md` 后部分方法缺失 | 参考文档没有一起复制 | 复制完整目录，保留 `references/` |
| `SKILL.md` 被识别为普通文本 | 文件被改名或扩展名变化 | 文件名必须是 `SKILL.md` |
| UI 名称没有显示 | `agents/openai.yaml` 缺失或格式被改坏 | 从压缩包恢复原文件，保持 YAML 缩进 |
| 引用页打不开 | 移动时拆散了目录 | 恢复原始相对结构，不要单独移动 `references` 内文件 |
| 想升级版本 | 新包覆盖了旧目录的一部分 | 先备份旧目录，再整体替换 `film-sound-design` |
| 目标环境不支持 Skills | 使用了没有 Skill 功能的客户端 | 可把 `SKILL.md` 作为提示词附件或手动粘贴使用，但自动调用能力取决于客户端 |

## 8. 版本升级与备份

Windows 建议先备份：

```powershell
$old = Join-Path $skillsDir "film-sound-design"
$backup = Join-Path $skillsDir "film-sound-design-backup-2026-10-23"
Copy-Item -LiteralPath $old -Destination $backup -Recurse
```

确认备份完成后，再替换完整技能目录。不要把备份目录改名成另一个包含 `SKILL.md` 的有效 `skills/film-sound-design` 同级目录，否则可能产生重复技能。

## 9. 安全说明

这个压缩包只包含 Markdown 指令和 YAML 配置：

- 不包含可执行程序、脚本或安装器。
- 不要求管理员权限。
- 不调用网络服务、MCP、API 或云端账号。
- 不包含用户媒体、工程文件、账号密码或个人隐私数据。
- 从不可信来源获得的同名压缩包应先检查内容，不要直接运行其中的任何可执行文件。

安装 Skill 不等于授予 Codex 对视频、素材库或第三方平台的额外操作权限。实际剪辑、渲染和上传仍按具体任务授权执行。

## 10. 最终验收清单

- [ ] 已解压压缩包，看到完整的 `film-sound-design` 文件夹。
- [ ] 目标路径以 `.codex\skills\film-sound-design\SKILL.md` 结尾。
- [ ] `references/` 下三个文档都存在。
- [ ] 已重启 Codex 并新建对话。
- [ ] 能通过 `$film-sound-design` 显式调用。
- [ ] 用一段实际画面或时间码表测试，输出包含声音层级、入点、尾音和混音目的。

如需校验压缩包完整性，可在 Windows PowerShell 中执行：

```powershell
Get-FileHash ".\电影音效设计Skill-v1.0.2.zip" -Algorithm SHA256
```

核对随包提供的 SHA-256 校验值即可确认传输过程中文件未被修改。
