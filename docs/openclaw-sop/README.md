# OpenClaw Windows 安装与运维 SOP

> **适用对象**：无技术背景的业务人员（投资部等非 IT 岗位）  
> **适用系统**：Windows 10 / Windows 11  
> **预计耗时**：15–25 分钟（含下载时间）  
> **文档版本**：v1.0 | 2026-04-10  
> **维护人**：[填写维护人姓名]

---

## 目录

1. [OpenClaw 是什么](#1-openclaw-是什么)
2. [安装前准备](#2-安装前准备)
3. [安装步骤（图文）](#3-安装步骤图文)
4. [首次配置向导](#4-首次配置向导)
5. [日常使用操作](#5-日常使用操作)
6. [安全与权限管控](#6-安全与权限管控)
7. [常见问题排查](#7-常见问题排查)
8. [卸载与重置](#8-卸载与重置)
9. [参考来源](#9-参考来源)

---

## 1. OpenClaw 是什么

OpenClaw 是一款**开源 AI 智能助手框架**，可以将 AI 大模型（如 DeepSeek、Kimi、Claude、GPT 等）接入到飞书、企业微信、Telegram 等平台，实现自动化任务执行。

**通俗理解**：它像一个 24 小时在线的私人助理，可以帮你处理邮件、整理文档、查询信息、执行自动化任务。所有数据保存在你自己的电脑上，不会上传到外部。

---

## 2. 安装前准备

### 2.1 硬件与系统要求

| 项目 | 最低要求 |
|------|----------|
| 操作系统 | Windows 10 或 Windows 11 |
| 内存 | ≥ 4GB |
| 硬盘空间 | ≥ 1GB 可用空间 |
| 网络 | 能访问互联网 |

### 2.2 注册阿里云百炼账号并获取免费 API Key

OpenClaw 需要接入一个 AI 大模型才能工作。推荐使用**阿里云百炼平台**——国内最稳定的大模型服务，新用户每个模型赠送 **100 万 Tokens 免费额度**（有效期 90 天），支持通义千问、DeepSeek、Kimi 等数十个模型。

**第一步：注册并开通百炼**

1. 浏览器打开 `https://bailian.console.aliyun.com`
2. 用手机号注册阿里云账号（需实名认证）
3. 登录后弹出服务协议，阅读并同意即可。开通不产生费用，系统自动发放免费额度

**第二步：创建 API Key**

1. 在百炼控制台，鼠标移到右上角头像 → 点击 **"API-KEY"**
2. 确认地域为 **华北2（北京）**
3. 点击 **"创建 API Key"** → 归属空间选"默认业务空间" → 权限选"全部" → 确定
4. 立刻点 **"复制"** 保存 Key（格式 `sk-xxxxxxxx`，只显示一次）

**第三步：开启"免费额度用完即停"（防扣费）**

1. 百炼控制台左侧 → **"模型用量"** → **"免费额度"** 页签
2. 点 **"一键开启所有模型"** 的即停开关

| 推荐模型 | 免费额度 | 有效期 | 说明 |
|----------|----------|--------|------|
| **qwen3.5-plus** | 100 万 Token | 90 天 | ⭐ 首选推荐，支持图片 |
| qwen3-coder-next | 100 万 Token | 90 天 | 代码能力强 |
| DeepSeek-R1 | 100 万 Token | 90 天 | 推理能力强 |
| 更多模型... | 各 100 万 | 90 天 | 在模型广场查看 |

> ⚠️ **安全提醒**：API Key 相当于密码，不要发给任何人、不要截图、不要写在公开文档中。

### 2.3 你不需要提前安装的东西

以下工具**不需要你手动安装**，安装脚本会自动处理：

- ❌ 不需要提前装 Node.js（脚本自动安装）
- ❌ 不需要装 Git
- ❌ 不需要装 Python
- ❌ 不需要会写代码

---

## 3. 安装步骤（图文）

### 步骤 1：以管理员身份打开 PowerShell

1. 按键盘 `Win + S`（Win 键就是键盘左下角有 Windows 图标的键）
2. 搜索框输入 `PowerShell`
3. 在搜索结果中，右键点击 **"Windows PowerShell"**
4. 选择 **"以管理员身份运行"**
5. 如果弹出提示"是否允许此应用对你的设备进行更改"，点击 **"是"**

> 💡 **提示**：PowerShell 窗口是一个蓝色/黑色背景的命令行窗口，这是正常的。

### 步骤 2：允许执行脚本

在 PowerShell 窗口中，**复制粘贴**以下命令，然后按回车：

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

如果提示确认，输入 `Y` 然后按回车。

> 💡 **这一步在做什么**：Windows 默认禁止运行外部脚本，这条命令允许运行经过签名的脚本，是安全的。

### 步骤 3：运行一键安装命令

在 PowerShell 窗口中，**复制粘贴**以下命令，然后按回车：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

等待安装完成。安装脚本会自动：
- 检测并安装 Node.js 22+
- 安装 OpenClaw CLI 工具
- 启动配置向导

> ⏱️ 安装时长取决于网络速度，通常 2–5 分钟。  
> 看到绿色文字 **"Installation complete!"** 表示安装成功。

#### 如果上面的命令失败了怎么办？

国内网络有时会导致脚本下载失败。请按以下替代方案操作：

**替代方案 A：手动安装 Node.js 后用 npm 安装**

1. 打开浏览器访问 https://nodejs.org
2. 下载 **LTS 版本**（页面上标注为"推荐大多数用户"的绿色按钮）
3. 双击下载的安装包，一路点"下一步"完成安装
4. **关闭 PowerShell，重新以管理员身份打开**
5. 依次运行以下命令：

```powershell
# 验证 Node.js 已安装（应显示 v22 或更高版本号）
node -v

# 设置国内镜像加速（解决下载慢的问题）
npm config set registry https://registry.npmmirror.com

# 安装 OpenClaw
npm install -g openclaw
```

### 步骤 4：验证安装成功

安装完成后，运行以下命令验证：

```powershell
openclaw --version
```

如果显示版本号（例如 `2026.3.28`），说明安装成功。

---

## 4. 首次配置向导

安装完成后，运行以下命令启动配置向导：

```powershell
openclaw onboard --install-daemon
```

> `--install-daemon` 参数的作用：让 OpenClaw 在后台持续运行，关闭终端窗口也不会停止。

向导会逐步引导你完成配置。以下是每一步的操作说明：

### 4.1 安全警告确认

屏幕显示安全提示，用**左右方向键**切换到 `Yes`，按**回车**确认。

### 4.2 选择模式

选择 **QuickStart**（推荐），按回车。QuickStart 会使用合理的默认值，适合首次使用。

### 4.3 选择 AI 模型

选择 **Skip for now**（先跳过）。我们后面手动配置百炼，效果更稳定。

### 4.4 其他选项

| 步骤 | 选择 |
|------|------|
| 聊天渠道 | Skip for now |
| Skills | No |
| Hooks | Skip for now |
| Gateway | Install（或 Restart） |

### 4.5 配置百炼模型（关键步骤）

向导完成后，通过 OpenClaw Web 控制面板手动配置百炼：

1. 浏览器打开 `http://127.0.0.1:18789`
2. 左侧菜单点 **配置 → RAW**（或 Config → RAW）
3. 把以下配置**完整复制**粘贴到 RAW JSON 输入框。**只需把 `sk-你的百炼APIKey` 替换为你的真实 Key**，其他内容不要改：

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "bailian": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1",
        "apiKey": "sk-你的百炼APIKey",
        "api": "openai-completions",
        "models": [
          {
            "id": "qwen3.5-plus",
            "name": "Qwen3.5-Plus",
            "reasoning": false,
            "input": ["text", "image"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 1000000,
            "maxTokens": 65536
          },
          {
            "id": "qwen3-coder-next",
            "name": "Qwen3-Coder-Next",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0 },
            "contextWindow": 262144,
            "maxTokens": 65536
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": "bailian/qwen3.5-plus",
      "models": {
        "bailian/qwen3.5-plus": {},
        "bailian/qwen3-coder-next": {}
      }
    }
  }
}
```

4. 点右上角 **Save** → 点 **Update**
5. 回到聊天界面，发送"你好"测试——收到回复说明配置成功

### 4.6 配置 Windows 环境变量（可选但推荐）

把 API Key 设为系统环境变量是安全最佳实践，防止 Key 写在文件中被误传播：

1. 按 `Win + Q`，搜索 **"编辑系统环境变量"**，打开
2. 点击 **"环境变量"** 按钮
3. 在 **"用户变量"** 区域，点 **"新建"**
4. 变量名：`DASHSCOPE_API_KEY`
5. 变量值：你的百炼 API Key（sk-开头的字符串）
6. 点"确定"保存

验证：**关闭所有 PowerShell 窗口**，重新打开一个，运行：

```powershell
echo $env:DASHSCOPE_API_KEY
```

显示你的 Key 就说明设置成功。

---

## 5. 日常使用操作

### 5.1 访问控制面板

打开浏览器，访问以下地址：

```
http://127.0.0.1:18789
```

或在 PowerShell 中运行：

```powershell
openclaw dashboard
```

### 5.2 常用命令速查表

| 操作 | 命令 | 说明 |
|------|------|------|
| 启动服务 | `openclaw gateway start` | 开机后手动启动 |
| 停止服务 | `openclaw gateway stop` | 暂停 OpenClaw |
| 查看状态 | `openclaw gateway status` | 检查是否正常运行 |
| 打开控制面板 | `openclaw dashboard` | 在浏览器中打开 |
| 检查问题 | `openclaw doctor` | 自动诊断常见问题 |
| 查看版本 | `openclaw --version` | 确认当前版本 |
| 升级到最新版 | `npm install -g openclaw@latest` | 更新 OpenClaw |
| 重新配置 | `openclaw configure` | 修改模型/渠道等设置 |

### 5.3 每天使用流程

1. **开机后**：如果已安装守护进程（`--install-daemon`），OpenClaw 会自动启动，无需手动操作
2. **使用**：打开浏览器访问 `http://127.0.0.1:18789`，在聊天界面输入任务
3. **关机前**：直接关机即可，下次开机自动恢复

### 5.4 判断服务是否正常

运行 `openclaw gateway status` 后，看到以下信息表示正常：

```
Runtime: running
RPC probe: ok
```

如果显示 `stopped` 或报错，运行 `openclaw gateway start` 重新启动。

---

## 6. 安全与权限管控

### 6.1 API Key 安全

| 规则 | 说明 |
|------|------|
| API Key 存储位置 | 仅保存在本机 `C:\Users\你的用户名\.openclaw\` 目录下的加密文件中 |
| 不要分享 | 不要把 API Key 发给任何人、不要截图分享 |
| 不要公开 | 不要把 API Key 写在公开文档、邮件或聊天中 |
| 定期轮换 | 建议每 90 天在模型提供商后台重新生成 Key |

### 6.2 网络访问控制

OpenClaw 默认只监听本机回环地址（`127.0.0.1`），**仅本机可访问**，局域网内其他电脑无法访问你的 OpenClaw。

如果需要开放给其他人访问，必须：
1. 经 IT 部门审批
2. 配置 Gateway 认证 Token
3. 配置防火墙规则

### 6.3 工具权限控制（重要）

OpenClaw 默认启用沙箱模式（`sandbox`），执行操作前会请求你的确认。

**⚠️ 严禁在未经 IT 审批的情况下关闭沙箱模式。**

以下配置仅供了解，**业务人员请勿自行修改**：

```
配置文件位置：C:\Users\你的用户名\.openclaw\openclaw.json
```

关键安全配置项：
- `sandbox.mode`：默认为开启，执行命令前需确认
- `tools.exec.security`：工具执行权限级别
- `gateway.auth`：网关认证方式

### 6.4 权限管控矩阵

| 操作 | 业务人员 | IT 管理员 |
|------|----------|-----------|
| 安装 OpenClaw | ✅ 可自行安装 | ✅ |
| 使用控制面板聊天 | ✅ | ✅ |
| 修改 AI 模型 | ⚠️ 需 IT 指导 | ✅ |
| 接入飞书/企微渠道 | ❌ 需 IT 配置 | ✅ |
| 修改 openclaw.json | ❌ 禁止 | ✅ |
| 关闭沙箱模式 | ❌ 严禁 | ⚠️ 需审批 |
| 开放外网访问 | ❌ 严禁 | ⚠️ 需安全评审 |

---

## 7. 常见问题排查

### 问题 1：安装脚本下载失败 / 超时

**现象**：运行一键安装命令后，PowerShell 显示红色错误信息或长时间无响应。

**解决**：使用替代方案，先手动安装 Node.js，再用 `npm install -g openclaw` 安装（详见第 3 节替代方案 A）。

### 问题 2：`openclaw` 命令找不到

**现象**：输入 `openclaw --version` 提示"无法将 openclaw 项识别为 cmdlet"。

**解决**：
1. 关闭当前 PowerShell 窗口
2. 重新以管理员身份打开 PowerShell
3. 再次尝试 `openclaw --version`
4. 如果仍然不行，运行 `npm config get prefix`，把输出的路径添加到系统环境变量 PATH 中

### 问题 3：控制面板打不开

**现象**：浏览器访问 `http://127.0.0.1:18789` 无法加载。

**解决**：
1. 运行 `openclaw gateway status` 检查服务状态
2. 如果显示 `stopped`，运行 `openclaw gateway start`
3. 临时关闭防火墙或杀毒软件测试
4. 确认使用的是 `http://` 而非 `https://`

### 问题 4：AI 模型无响应 / 报错 401

**现象**：在控制面板中发送消息后无回复，或提示认证失败。

**解决**：
1. 检查 API Key 是否正确（复制时不要有多余空格）
2. 确认模型服务商账号是否有余额
3. 运行 `openclaw configure` 重新配置 API Key

### 问题 5：Node.js 版本太低

**现象**：安装过程中提示 Node 版本不满足要求。

**解决**：
1. 运行 `node -v` 查看当前版本
2. 如果低于 v22，去 https://nodejs.org 下载最新 LTS 版本重新安装
3. 安装后**必须关闭 PowerShell 重新打开**

### 问题 6：端口被占用

**现象**：启动 Gateway 时提示 18789 端口已被占用。

**解决**：
```powershell
# 查找占用端口的进程
netstat -ano | findstr :18789

# 记下最后一列的 PID 数字，然后结束该进程
taskkill /PID <PID数字> /F
```

---

## 8. 卸载与重置

### 完整卸载

```powershell
# 第 1 步：使用 OpenClaw 自带卸载
openclaw uninstall

# 第 2 步：卸载 npm 全局包
npm uninstall -g openclaw

# 第 3 步（可选）：删除配置文件
# 以下命令会删除所有 OpenClaw 配置和数据，请确认后再执行
Remove-Item -Recurse -Force "$env:USERPROFILE\.openclaw"
```

### 重置配置（保留安装）

```powershell
openclaw onboard --reset
```

---

## 9. 参考来源

本 SOP 综合以下官方及权威来源编写，经交叉验证确保一致性：

| 来源 | 地址 | 说明 |
|------|------|------|
| **官方文档（中文）** | https://docs.openclaw.ai/zh-CN | 最权威来源 |
| 官方入门向导 | https://docs.openclaw.ai/zh-CN/start/wizard | 向导配置详解 |
| 官方 Windows 平台文档 | https://docs.openclaw.ai/platforms/windows | Windows 专用指引 |
| 官方 GitHub 仓库 | https://github.com/openclaw/openclaw | 源码与 README |
| 官方安装页面 | https://docs.openclaw.ai/zh-CN/install | 多种安装方式 |

**社区参考（仅作为补充验证）**：
- 知乎《OpenClaw 从安装到入门的完全指南》
- 知乎《Windows 10/11 OpenClaw 安装与配置说明》
- 腾讯云《安装 OpenClaw 全网最详细流程与步骤》
- 博客园《OpenClaw 国内用户 Windows 专用安装流程》

---

## 变更记录

| 日期 | 版本 | 变更说明 | 作者 |
|------|------|----------|------|
| 2026-04-10 | v1.0 | 初始版本 | [填写] |

---

> **免责声明**：OpenClaw 为开源软件，使用时请遵守所在组织的信息安全政策。AI 对话会消耗 API Token（按量计费），请关注用量以控制成本。本 SOP 基于 OpenClaw 2026.3.x 版本编写，后续版本可能有界面或命令变化，请以官方文档为准。
