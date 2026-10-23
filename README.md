# 电影音效设计 Skill

`film-sound-design` 是一个面向电影、短片、广告与短视频的 Codex Skill，用“声音承担什么叙事工作”作为核心决策依据，帮助完成音效选择、节奏设计、转场处理、分层混音与交付检查。

## 能力范围

- 按剧情、悬念、动作、情绪、转场和产品广告等场景建立声音策略。
- 设计对白、环境声、Foley、动作音效、设计音效与音乐的分层。
- 规划同步声、J-cut、L-cut、声音桥、转场、冲击和静默。
- 为 whoosh、impact、riser、UI、车辆、材质声等提供使用边界。
- 输出可执行的 sound cue sheet。
- 检查对白掩蔽、低频浑浊、重复素材、尾音、响度、真峰值和手机外放表现。

## 仓库结构

```text
film-sound-design/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── sound-strategy.md
│   ├── edit-and-mix.md
│   └── video-study-notes.md
├── TECHNICAL.md
├── AI_USAGE.md
└── dist/
    ├── film-sound-design-v1.0.2.zip
    └── film-sound-design-v1.0.2.zip.sha256.txt
```

## 快速安装

### Windows

1. 下载并解压 [`dist/film-sound-design-v1.0.2.zip`](dist/film-sound-design-v1.0.2.zip)。
2. 将解压后的 `film-sound-design` 文件夹放入：

```text
C:\Users\<用户名>\.codex\skills\
```

3. 确认路径为：

```text
C:\Users\<用户名>\.codex\skills\film-sound-design\SKILL.md
```

4. 重启 Codex，新建对话，然后使用：

```text
使用 $film-sound-design 给这段画面设计音效层级、关键入点和混音方案。
```

### macOS / Linux

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R film-sound-design "${CODEX_HOME:-$HOME/.codex}/skills/"
```

完整安装、升级、验证与故障排查见 [技术文档](TECHNICAL.md)。

人工智能工具参与范围、信息来源、限制和人工责任见 [AI 工具使用声明](AI_USAGE.md)。

## 直接使用仓库文件

如果不需要压缩包，可以把以下内容复制到 Codex Skill 目录：

- `SKILL.md`
- `agents/`
- `references/`

不要只复制 `SKILL.md`；参考文件用于提供完整的场景策略、混音和 QC 细节。

## 设计原则

1. 先判断画面的叙事任务，再从声音素材开始选择。
2. 用环境声、Foley、设计音效和音乐分工，避免所有声音同时争夺注意力。
3. 节奏来自“重音 + 留白 + 频率变化”，而不是持续堆叠 sound pack。
4. 每个设计音效都应有明确工作：揭示、定位、强调、连接、误导或表达材质。
5. 对白始终是信息锚点，音乐与音效必须为可懂度让位。

## 版本与校验

- 当前版本：`1.0.2`
- 发布日期：`2026-10-23`
- 压缩包 SHA-256：

```text
d56d6d56022b04455341db09cc767fa967491524bb8f5b584a5c8611a3bfae6e
```

Windows 校验：

```powershell
Get-FileHash ".\film-sound-design-v1.0.2.zip" -Algorithm SHA256
```

## 许可证

本项目采用 [MIT License](LICENSE) 开源。允许使用、复制、修改、合并、发布、分发、再许可和销售本项目内容，但须保留版权声明与许可声明。

MIT License 仅适用于本项目原创内容。第三方视频、影片、音频、商标或其他素材仍归各自权利人所有，不在本项目的 MIT 授权范围内。

技能中的视频学习记录来自用户提供文档中的 Bilibili 参考视频，并核对页面章节、镜头和时间线。
