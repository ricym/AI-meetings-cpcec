# AI 会议记录助手（meetings 技能）

将会议转录文本（含时间戳 + 说话人）自动转换为标准会议纪要 Word 文档。核心流程：**知识库纠错 → 信息提取 + 口语书面化 → 模板填充**。

支持在两个工具上运行，按需选择：

| 工具 | 特点 |
|------|------|
| **Claude Code**（命令行） | 功能全、可自定义模型、处理较快 |
| **Workbuddy**（腾讯客户端） | 对话框操作、微信扫码即用、更简单 |

## 仓库内容

```
AI-meetings-cpcec/
├── README.md                      本说明
├── SKILL.md                       技能定义
├── config.json                    项目根路径配置（首次需改 project_root）
├── CHANGELOG.md                   更新记录
├── guidebook/                     站点目录（GitHub Actions 从此发布到 Pages）
│   ├── index.html                 首页
│   ├── manual-claude.html         Claude Code 用户手册
│   └── manual-workbuddy.html      Workbuddy 用户手册
└── .github/workflows/deploy.yml   Pages 部署工作流
```

> 在线访问：<https://ricym.github.io/AI-meetings-cpcec/>（由 `.github/workflows/deploy.yml` 把 `guidebook/` 发布到 GitHub Pages；需在仓库 Settings → Pages → Source 选「GitHub Actions」）。

## 快速开始

### 1. 获取技能

下载本仓库（GitHub 页面 Code → Download ZIP），解压后得到根目录的 `SKILL.md`（技能定义）与 `config.json`（配置）。

### 2. 配置（首次）

编辑 `config.json`，把 `project_root` 改成你自己的 ai-meeting 目录：

```json
{
  "project_root": "C:\\Users\\你的用户名\\Documents\\ai-meeting"
}
```

> 其余配置（registry.json / profile.json / profiles 目录）关系到模板与脚本匹配，通常由部署人员搭好，日常无需改动。

### 3. 按工具使用

- **Claude Code**：把 `SKILL.md` 与 `config.json` 放进 `~/.claude/skills/meetings/`（两者需在同一技能目录），命令行运行 `/meetings 转录文本.txt 这是例会，按例会出纪要`
- **Workbuddy**：对话框「技能 → 导入技能」粘贴 `SKILL.md`，导入文件后选技能、发需求

详见对应手册 HTML 文件（浏览器打开即可）。

## 使用示例

**起点 1 · 文本文件**（已转写，不用 iTingnao）：

```
/meetings C:\会议\4月例会.txt 这是例会，按例会出纪要
```

**起点 2 · 音频文件**（一条指令调两次技能）：

```
/itingnao transcribe C:\会议\4月例会.mp3
/meetings <转写文本> 这是例会，按例会出纪要
```

> 音频文件需额外安装 iTingnao 技能；只有文本文件则不需要。

## 知识库三件套

技能依赖三个 Markdown 文件（放在 `project_root/profiles/` 下），是用户唯一需要维护的内容：

| 文件 | 用途 |
|------|------|
| `关键字纠错.md` | ASR 纠错词条、地名/系统名别称、业务术语 |
| `公司概况.md` | 公司信息、人员档案、部门职责、地名/系统详情 |
| `用户需求.md` | 输出格式、会议模式、模板映射、书面化规则 |

## 截图功能说明

两份手册内置「节点悬停弹出截图」功能：每个步骤节点右上角有 📷 角标，悬停显示截图。当前为占位图，后续把节点上的 `data-screenshot="placeholder"` 改成图片路径即可显示真实截图，无需改其他代码。

## 许可与作者

作者：liuhongjian
