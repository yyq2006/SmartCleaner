<div align="center">

# 🧹 SmartCleaner（智净）

**AI 辅助的 Windows 硬盘清理与数据迁移工具**

让小白也能放心清理硬盘 —— AI 看懂每一个文件夹，决定权永远在你手里

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyQt6](https://img.shields.io/badge/UI-PyQt6-41CD52)](https://riverbankcomputing.com/software/pyqt/)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows)](https://www.microsoft.com/windows)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

</div>

---

## ✨ 它解决什么问题？

打开 Windows 资源管理器，你只能看到一堆文件夹的名字。
**SmartCleaner 借助大语言模型，让你能看懂每个文件夹是干什么的、能不能删、删了会怎么样**，然后你勾选要清理或迁移的项，程序帮你执行。

- 🤖 **AI 决策卡片**：每个候选都附带"是什么 / 怎么来的 / 删了会怎样 / 怎么恢复"
- 🛟 **后悔药机制**：所有删除走隔离区，30 天可还原
- 🚚 **无感迁移**：C 盘减负 + Junction 符号联接，软件感知不到搬家
- 💬 **AI 助手「小净」**：聊天式交互，能调用工具自行调查并给方案
- 🌳 **树状扫描结果**：层级展开，每层显示大小（>1G 用 GB / <1G 用 MB）
- 🔐 **本地运行**：API Key DPAPI 加密，调用 AI 只发文件**元信息**

---

## 📸 截图

> 把截图放到 `docs/` 目录后在这里 reference

```
docs/
├── screenshot-dashboard.png   首页/各盘空间
├── screenshot-scan.png        树状扫描结果
├── screenshot-chat.png        AI 助手对话
├── screenshot-migration.png   迁移中心
└── screenshot-restore.png     还原中心
```

---

## 🚀 快速开始

### 方式一：下载打包好的 exe（推荐）

到 [Releases](../../releases) 下载最新的 `SmartCleaner.exe`，双击运行（Windows 会弹 UAC 授权管理员，点【是】）。

### 方式二：从源码运行

需要 Python 3.10+：

```bash
git clone https://github.com/yourname/SmartCleaner.git
cd SmartCleaner
pip install -r requirements.txt

# 普通模式
python -m src.main

# 管理员模式（能扫描系统目录）
run_admin.bat
```

### 方式三：自己打包 exe

```bash
build.bat
```

产物在 `dist\SmartCleaner.exe`，大小约 60–90 MB，零依赖，发给朋友也能直接跑。

---

## 🔑 配置 API Key（必做）

没 Key 只能用本地规则引擎，AI 功能用不了。

**推荐 DeepSeek**（国内可直连 + 便宜 + 新用户送免费额度）：

1. 打开 [https://platform.deepseek.com/](https://platform.deepseek.com/) 用手机号注册
2. 左侧菜单【API Keys】→【创建 API Key】→ 复制弹出的 `sk-xxxx`
3. 打开 SmartCleaner →【⚙️ 设置】→ 粘贴 Key → 点【🔌 测试】→ 点【💾 保存】

也支持 OpenAI / Claude / 任何 OpenAI 兼容协议（智谱 / 月之暗面 / 通义等）。

> 一次扫描通常 < 0.1 元，10 元能用大半年。

---

## 🧩 功能板块

| 板块 | 干啥用 |
|---|---|
| 🏠 **首页** | 各盘空间环形图（带卷标）、统计卡、快捷操作 |
| 🤖 **AI 助手** | 跟「小净」聊天，它会调用工具调查并给方案，确认后执行 |
| 🔍 **扫描** | 多盘联合 + 自定义文件夹扫描，AI 给每项打标签 |
| 📋 **扫描结果** | 树状展开，全选/取消全选，问小净，详情抽屉可最小化 |
| 🚚 **迁移中心** | 真实扫描 C 盘可外迁目录，按风险三档展示，Junction 透明迁移 |
| 🛟 **还原中心** | 删除还原点 + 迁移历史，展开看具体文件 |
| 📖 **说明文档** | 11 章完整使用说明，包括 FAQ |
| ⚙️ **设置** | AI 配置 / 主题 / 权限 / GitHub 入口 |

---

## 🛡️ 安全护栏

下列规则在代码层面**强制执行**，即使 AI 也无法绕过：

| 护栏 | 说明 |
|---|---|
| 🔒 **保护清单** | `C:\Windows` / `Program Files` / `System32` 等代码层锁死 |
| 🛟 **后悔药** | 所有删除都进隔离区 `<盘符>:\.SmartCleaner_Trash\`，30 天可还原 |
| ⚠️ **大文件二次确认** | >1GB 即使 AI 说安全也会再问 |
| 🔐 **API Key 本地加密** | DPAPI 绑定当前 Windows 账户 |
| 🚫 **危险操作必须确认** | AI 工具调用走 ask_confirm 流程，等用户点按钮 |
| 📝 **隐私优先** | 调用 AI 只发文件元信息（路径/大小/扩展名分布），不发内容 |
| 🔗 **跳过系统兼容联接** | `My Music`/`My Pictures`/`My Videos` 等 XP 遗留联接自动跳过 |

---

## 🛠️ 技术栈

| 模块 | 选型 |
|---|---|
| 语言 | Python 3.10+ |
| UI 框架 | PyQt6 |
| 数据库 | SQLite（用户数据存 `%APPDATA%\SmartCleaner\`） |
| LLM 适配 | DeepSeek / OpenAI / Claude / 自定义（OpenAI 协议） |
| 加密 | Windows DPAPI（pywin32） |
| 跨盘搬运 | `robocopy /MOVE /XJ`（处理权限更稳） |
| 符号联接 | `mklink /J` （NTFS Junction） |
| 打包 | PyInstaller（`--onefile --windowed --uac-admin`） |
| 图标 | Pillow 动态生成多尺寸 ICO |

---

## 📂 项目结构

```
SmartCleaner/
├── src/
│   ├── core/                  扫描 / AI / 迁移 / 清理 / 还原
│   │   ├── scanner.py
│   │   ├── ai_engine.py
│   │   ├── migration.py
│   │   ├── migration_scanner.py
│   │   ├── cleaner.py
│   │   ├── agent.py           AI 助手 + 工具调用
│   │   ├── orchestrator.py
│   │   ├── rules_engine.py
│   │   └── logger.py
│   ├── infra/
│   │   ├── db.py              SQLite
│   │   ├── llm.py             多供应商适配
│   │   ├── key_vault.py       DPAPI 加密 Key
│   │   ├── fs.py              文件系统辅助
│   │   ├── admin.py           UAC 提权
│   │   ├── paths.py
│   │   ├── config.py
│   │   └── links.py           外部链接常量
│   ├── ui/
│   │   ├── main_window.py
│   │   ├── dashboard_page.py
│   │   ├── chat_page.py
│   │   ├── scan_page.py
│   │   ├── result_page.py     树状扫描结果
│   │   ├── migration_page.py
│   │   ├── restore_page.py
│   │   ├── help_page.py
│   │   ├── settings_page.py
│   │   ├── setup_wizard.py    首启配置向导
│   │   ├── intro_dialog.py    强制说明弹窗（10s）
│   │   ├── card_widget.py
│   │   ├── detail_drawer.py
│   │   ├── icons.py
│   │   └── theme.py           浅蓝 / 暗色主题
│   ├── rules/
│   │   ├── known_patterns.yaml
│   │   └── protected.yaml
│   └── main.py
├── tools/
│   └── generate_logo.py       动态生成 logo
├── assets/                    Logo / 图标
├── launch.py                  PyInstaller 入口
├── build.bat                  一键打包
├── run.bat / run_admin.bat
├── requirements.txt
└── README.md
```

---

## 🤝 贡献

欢迎 PR！开发流程：

1. Fork 仓库
2. `git checkout -b feature/my-feature`
3. 改代码 + 测试
4. `git commit -m "feat: ..."`
5. 提 Pull Request

如果发现 bug：[New Issue](../../issues/new)，附上：
- 操作步骤
- 期望结果 vs 实际结果
- 截图
- 日志：`%APPDATA%\SmartCleaner\logs\tech.log`

---

## 📜 协议

[MIT License](LICENSE) — 自由使用、修改、商用、分发，附带原协议即可。

---

## 🙏 致谢

- AI 决策的核心思路来自和 Claude 的反复打磨
- Icons：使用 Emoji 作为内联图标，主 logo 用 Pillow 程序化生成
- 灵感：WinDirStat / TreeSize 等老牌磁盘分析工具

---

<div align="center">

**如果这个工具帮到你了，欢迎点 ⭐ Star 支持。**

</div>
