# 抖音多账号内容协同

为多个已授权 EasyBR 环境分别运行公开内容分析，再把结果按账号汇总。适合热点搜索、账号诊断、评论需求洞察和竞品研究的批量整理，并为每个账号生成可继续核验的脚本草稿。

每个账号使用独立 `browserId` 和 session；不同账号可以并行，同一个浏览器不会并发操作。结果会区分完整、部分完成和需要用户处理的账号。

该 Skill 只读公开页面，不保存 Cookie 或密码，不自动评论、私信、投稿或发布。

运行前请安装并验证 `douyin-live-hot-free`、`douyin-account-diagnosis`、`douyin-comment-insights-free`、`douyin-competitor-research-free` 四个原子 Skill。缺少任一依赖时，工作台会明确失败。
