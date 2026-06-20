# ☁ Serverless Cloud Notepad 云笔记 (Pages 部署版)

## 📌 项目声明与来源

自用项目，对原仓库 [s0urcelab/serverless-cloud-notepad](https://github.com/s0urcelab/serverless-cloud-notepad) 修改而来的CloudFlare Pages版本，请前往 [原仓库主页](https://github.com/s0urcelab/serverless-cloud-notepad#-donate) 支持原作者。


## ✨ 核心特性

* ✏️ 无需登录或注册，即刻开始书写。
* 💾 自动保存机制，防止数据意外丢失。
* ❌ 彻底的 **Serverless（无服务器）** 架构，无需自建后端程序或传统关系型数据库。
* ⚡ 依托 Cloudflare 全球边缘网络，提供高可用性与极速响应。
* 🌍 全面支持中文 URL 路径解析。
* 🔒 支持为特定笔记设置密码保护与只读分享模式。
* 📦 **专属优化**：原生适配 Cloudflare Pages 部署流程，剥离了与 Pages 不兼容的顶层变量声明。

## 🚀 Cloudflare Pages 部署指南

以下是为 Cloudflare Pages 量身定制的简易部署步骤。

### 1. 创建基础数据源

* 登录 Cloudflare 控制台，进入侧边栏的 **Workers & Pages** -> **KV**。
* 创建两个独立的 KV 命名空间数据库：
* 创建一个用于存储笔记正文内容，例如命名为 `cloud-notepad-notes`。
* 创建一个用于存储分享链接映射，例如命名为 `cloud-notepad-share`。



### 2. 关联仓库并执行初始部署

* 将本仓库 Fork 到你自己的 GitHub 账号下。
* 在 Cloudflare 控制台中新建一个 **Pages** 项目，选择连接到你刚 Fork 的 GitHub 仓库。
* 在构建设置中：
* **框架预设**：选择 `None`。
* **构建命令**：留空。
* **构建输出目录**：留空。


* 点击 **保存并部署**。
* *提示：由于此时尚未注入环境变量，首次部署后的网页访问会报错，这是正常现象，请继续进行环境配置。*

### 3. 配置运行时环境变量与资源（关键）

* 进入该 Pages 项目的 **设置 (Settings)** -> **函数 (Functions)** 页面。
* 找到 **变量和机密 (Environment variables)** 区域，添加以下两个文本变量（请确保在**生产环境**和**预览环境**中都填写一致）：
* 变量名称：**`SCN_SALT`**，值：随意输入一段复杂的随机字符串（用作数据加密盐值）。
* 变量名称：**`SCN_SECRET`**，值：随意输入另一段复杂的随机字符串（用作鉴权密钥）。


* 向下滚动至 **KV 命名空间绑定 (KV namespace bindings)** 区域，添加以下两个绑定（同样需要确保**生产环境**和**预览环境**均已配置）：
* 变量名称：**`NOTES`**，右侧下拉选择你在第一步创建的笔记数据库（如 `cloud-notepad-notes`）。
* 变量名称：**`SHARE`**，右侧下拉选择你在第一步创建的分享数据库（如 `cloud-notepad-share`）。



### 4. 重试部署以应用配置

* 切换回该项目的 **部署 (Deployments)** 页面。
* 点击列表最上方那条最新记录进入详情页，点击右上角的 **重试部署 (Retry deployment)** 按钮。
* 等待此次构建完成，所有的 KV 资源和环境变量即可正确注入模块。此时访问分配的 `.pages.dev` 域名或你绑定的自定义域名，即可正常使用云笔记。

## 🔨 使用说明

* 直接访问站点根目录 `/`，系统会自动重定向并为你创建一篇随机生成的专属笔记。
* 在浏览器地址栏手动输入 `/<任何你想要的名称>`（例如 `https://你的域名/测试笔记`），即可直接新建、查看或编辑指定名称的专属云笔记。