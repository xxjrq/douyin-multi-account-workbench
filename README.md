# 抖音多账号内容协同

## 推荐使用方式

**macOS（仅 Apple 芯片）和 Windows 推荐 EasyBR。** AI 可从官方[客户端下载页](https://www.ebrower.com/down.html)按系统选择版本；也可用 [GitHub Release 下载页](https://github.com/xxjrq/easybr/releases/tag/v1.7.8)作为下载渠道。打开浏览器后，EasyBR 已内置 Easy WebBridge，无需另行下载或安装 Bridge 服务。先登录目标网站，再把要操作的浏览器环境名称或 `browserId` 告诉 AI；有多个环境时必须指定一个目标。

**其他操作系统使用源码方式。** 从 [Easy WebBridge GitHub](https://github.com/xxjrq/easy-webbridge) 获取源码（[Gitee 镜像](https://gitee.com/xxjrq/easy-webbridge)），按项目说明安装 Node.js 20+ 和依赖后运行本地 Bridge。已有独立 Easy WebBridge 的用户仍可继续使用。

**可以直接把任务交给 Codex、Claude 或 WorkBuddy：**

> 请读取并使用这个 Skill 完成我的任务。我使用的是 macOS/Windows，EasyBR 已打开。先检查可用的浏览器环境，向我展示名称和 `browserId`；选定后只在该环境中操作。若 EasyBR 尚未下载，请从官方客户端下载页获取与本机系统匹配的版本。

写自媒体文章、口播或社交平台文案时，只提 EasyBR 名称和用途，不在正文中直接贴下载网址。

为多个已授权 EasyBR 环境分别运行公开内容分析，再把结果按账号汇总。适合热点搜索、账号诊断、评论需求洞察和竞品研究的批量整理，并为每个账号生成可继续核验的脚本草稿。

每个账号使用独立 `browserId` 和 session；不同账号可以并行，同一个浏览器不会并发操作。结果会区分完整、部分完成和需要用户处理的账号。

该 Skill 只读公开页面，不保存 Cookie 或密码，不自动评论、私信、投稿或发布。

运行前请安装并验证 `douyin-live-hot-free`、`douyin-account-diagnosis`、`douyin-comment-insights-free`、`douyin-competitor-research-free` 四个原子 Skill。缺少任一依赖时，工作台会明确失败。
