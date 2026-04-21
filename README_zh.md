# GenericAgent（中文说明）

> 英文版说明见 [README.md](README.md) · 技术报告 [assets/GenericAgent_Technical_Report.pdf](assets/GenericAgent_Technical_Report.pdf)

<div align="center">
<img src="assets/images/bar.jpg" width="880"/>

<a href="https://trendshift.io/repositories/25944" target="_blank"><img src="https://trendshift.io/api/badge/repositories/25944" alt="lsdefine/GenericAgent | Trendshift" style="width: 250px; height: 55px;" width="250" height="55"/></a>

</div>

---

## 项目简介

**GenericAgent** 是一个极简、可自我进化的自主 Agent 框架。核心仅 **约 3K 行代码**，通过 **9 个原子工具 + 约百行 Agent Loop**，让任意大模型获得对本地计算机的系统级控制能力：浏览器、终端、文件系统、键鼠输入、屏幕视觉以及移动设备（ADB）等。

设计哲学：**不预设技能，靠进化获得能力。**

每完成一类新任务，GenericAgent 会把执行路径固化为可复用的 Skill，下次同类任务可直接调用。使用越久，技能树越丰富——全部从这份「种子代码」生长出来。

> **自举实证**：本仓库中从安装 Git、`git init` 到提交说明等流程，曾由 GenericAgent 自主完成（详见项目故事与报道）。

---

## 核心特性

- **自我进化**：任务执行路径自动沉淀为 Skill，能力持续增长  
- **极简架构**：核心约 3K 行，循环约百行，依赖轻、部署简单  
- **强执行**：注入真实浏览器（保留登录态），原子工具直接操作系统  
- **高兼容**：支持 Claude / Gemini / Kimi / MiniMax 等常见模型，跨平台  
- **省 Token**：上下文约 30K 量级，配合分层记忆，降低噪声与成本  

---

## 自我进化机制

```
[遇到新任务] → [自主摸索]（装依赖、写脚本、调试验证）
→ [固化为 Skill] → [写入记忆层] → [下次同类任务直接调用]
```

| 你说的一句话 | Agent 第一次 | 之后每次 |
|---|---|---|
| *「监控股票并提醒我」* | 安装分析库 → 搭流程 → 配定时任务 → 存 Skill | **一句话启动** |
| *「用 Gmail 发这个文件」* | 配 OAuth → 写发送脚本 → 存 Skill | **开箱即用** |

#### 示例展示

| 🧋 外卖下单 | 📈 量化选股 |
|:---:|:---:|
| <img src="assets/demo/order_tea.gif" width="100%" alt="外卖下单"> | <img src="assets/demo/selectstock.gif" width="100%" alt="量化选股"> |
| 自动导航外卖应用、选品并完成结账 | 按量化条件筛选标的 |
| 🌐 自主网页探索 | 💰 支出追踪 | 💬 批量消息 |
| <img src="assets/demo/autonomous_explore.png" width="100%" alt="网页探索"> | <img src="assets/demo/alipay_expense.png" width="100%" alt="支付宝支出"> | <img src="assets/demo/wechat_batch.png" width="100%" alt="微信批量"> |
| 定时浏览与汇总网页 | 通过 ADB 等驱动应用查询支出等 | 批量消息等场景 |

---

## 最新动态

- **2026-04-11**：**L4 会话归档记忆**；接入 scheduler cron  
- **2026-03-23**：支持个人微信作为 Bot 前端  
- **2026-03-10**：[百万级 Skill 库](https://mp.weixin.qq.com/s/q2gQ7YvWoiAcwxzaiwpuiQ?scene=1&click_id=7)  
- **2026-03-08**：[「政务龙虾」Dintal Claw](https://mp.weixin.qq.com/s/eiEhwo-j6S-WpLxgBnNxBg)  
- **2026-03-01**：[机器之心报道](https://mp.weixin.qq.com/s/uVWpTTF5I1yzAENV_qm7yg)  
- **2026-01-16**：GenericAgent V1.0 公开发布  

---

## 快速开始

### 1. 获取代码

官方仓库：<https://github.com/lsdefine/GenericAgent>

```bash
git clone https://github.com/lsdefine/GenericAgent.git
cd GenericAgent
```

社区常见 Fork 示例：<https://github.com/YeLuo45/GenericAgent>（将上述 URL 替换为对应地址即可）。

### 2. 安装依赖

最小依赖（默认桌面壳 + Streamlit）：

```bash
pip install streamlit pywebview
```

**Windows PowerShell 提示**：若 `pip` 不可用，可尝试 `python -m pip` 或 `py -3 -m pip`。  
若 **`pywebview` 安装失败**（例如 Python 过新导致 `pythonnet` 无法编译），可只装 **`streamlit`**，改用下方「仅 Web 界面」方式启动。

### 3. 配置密钥

```bash
cp mykey_template.py mykey.py   # Windows: Copy-Item mykey_template.py mykey.py
```

编辑 **`mykey.py`**，按注释填写 LLM API 等配置（详见文件内「推荐最优配置」区块）。

### 4. 启动

**默认（Streamlit + pywebview 窗口壳）：**

```bash
python launch.pyw
```

**仅 Web 界面（不依赖 pywebview）：**

```bash
streamlit run frontends/stapp.py --server.address 127.0.0.1
```

更完整的图文与进阶说明见 **[GETTING_STARTED.md](GETTING_STARTED.md)**。  
新手图文指南（飞书）：<https://my.feishu.cn/wiki/CGrDw0T76iNFuskmwxdcWrpinPb>

---

## Bot / 多前端（可选）

### Telegram

在 `mykey.py` 中配置：

```python
tg_bot_token = 'YOUR_BOT_TOKEN'
tg_allowed_users = [YOUR_USER_ID]
```

```bash
python frontends/tgapp.py
```

### 微信（个人微信）

```bash
pip install pycryptodome qrcode requests
python frontends/wechatapp.py
```

首次启动扫码绑定，之后在微信内与 Agent 对话。

### QQ

使用 `qq-botpy` 长连接，**无需公网 webhook**：

```bash
pip install qq-botpy
```

在 `mykey.py` 中：

```python
qq_app_id = "YOUR_APP_ID"
qq_app_secret = "YOUR_APP_SECRET"
qq_allowed_users = ["YOUR_USER_OPENID"]  # 或 ['*'] 表示不校验用户
```

```bash
python frontends/qqapp.py
```

在 [QQ 开放平台](https://q.qq.com) 创建机器人获取凭据；用户 openid 可记录在 `temp/qqapp.log`。

### 飞书（Lark）

```bash
pip install lark-oapi
python frontends/fsapp.py
```

```python
fs_app_id = "cli_xxx"
fs_app_secret = "xxx"
fs_allowed_users = ["ou_xxx"]  # 或 ['*']
```

详细说明：[assets/SETUP_FEISHU.md](assets/SETUP_FEISHU.md)

### 企业微信（WeCom）

```bash
pip install wecom_aibot_sdk
python frontends/wecomapp.py
```

```python
wecom_bot_id = "your_bot_id"
wecom_secret = "your_bot_secret"
wecom_allowed_users = ["your_user_id"]
wecom_welcome_message = "你好，我在线上。"
```

### 钉钉（DingTalk）

```bash
pip install dingtalk-stream
python frontends/dingtalkapp.py
```

```python
dingtalk_client_id = "your_app_key"
dingtalk_client_secret = "your_app_secret"
dingtalk_allowed_users = ["your_staff_id"]  # 或 ['*']
```

### 其他桌面 / Streamlit 前端

```bash
python frontends/qtapp.py                 # Qt 桌面端
streamlit run frontends/stapp2.py         # 另一套 Streamlit UI
```

---

## 与同类产品对比

| 特性 | GenericAgent | OpenClaw | Claude Code |
|------|:---:|:---:|:---:|
| **代码体量** | ~3K 行 | ~53 万行量级 | 开源但体量大 |
| **部署** | pip + API Key | 多服务编排 | CLI + 订阅 |
| **浏览器** | 真实浏览器、保留会话 | 沙箱 / 无头等 | 多依赖 MCP 等 |
| **系统控制** | 键鼠、视觉、ADB 等 | 多 Agent 分工 | 偏文件与终端 |
| **自我进化** | 自主沉淀 Skill | 插件生态 | 会话间偏无状态 |
| **上手** | 少量核心文件 + 初始 Skills | 模块众多 | CLI 工具丰富 |

---

## 工作机制

通过 **分层记忆 × 最小工具集 × 自主执行循环** 完成复杂任务，并在运行中持续积累经验。

### 分层记忆

- **L0 — 元规则**：行为边界与系统约束  
- **L1 — 洞察索引**：轻量索引，便于路由与召回  
- **L2 — 全局事实**：长期稳定知识  
- **L3 — 任务 Skill / SOP**：可复用任务流程  
- **L4 — 会话归档**：已完成会话的提炼归档，支持长程回忆  

### 自主循环

感知状态 → 推理与规划 → 调用工具执行 → 写入记忆 → 循环。核心实现约 **`agent_loop.py` 百行量级**。

### 原子工具（示例）

| 工具 | 作用 |
|------|------|
| `code_run` | 执行代码 |
| `file_read` / `file_write` / `file_patch` | 读、写、改文件 |
| `web_scan` / `web_execute_js` | 感知网页、驱动浏览器 |
| `ask_user` | 人机确认 |

另有 **`update_working_checkpoint`**、**`start_long_term_update`** 等记忆类工具，用于跨会话持久化与长期更新。

通过 **`code_run`** 可在运行时安装包、写脚本、调 API、控硬件，并把临时能力固化为长期能力。

<div align="center">
  <img src="assets/images/workflow.jpg" alt="GenericAgent 工作流程" width="400"/>
  <br><em>工作流程示意</em>
</div>

---

## 支持与社区

若本项目对你有帮助，欢迎 **Star**。

欢迎加入交流群讨论与反馈（微信群二维码见仓库 `assets/images/wechat_group*.jpg`，与 [README.md](README.md) 中一致）。

## 友情链接

感谢 [LinuxDo](https://linux.do/) 社区支持。

[![LinuxDo](https://img.shields.io/badge/社区-LinuxDo-blue?style=for-the-badge)](https://linux.do/)

## 许可

MIT License — 详见 [LICENSE](LICENSE)。

## Star 历史

<a href="https://star-history.com/#lsdefine/GenericAgent&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=lsdefine/GenericAgent&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=lsdefine/GenericAgent&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=lsdefine/GenericAgent&type=Date" />
 </picture>
</a>
