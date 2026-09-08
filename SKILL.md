---
name: douyin-multi-account-workbench
description: 在多个用户明确指定的 EasyBR 浏览器环境中隔离执行抖音公开内容分析，汇总账号诊断、评论需求洞察和竞品研究结果。用户需要多账号并行检查、统一报告或按账号分组内容建议时使用；不保存登录信息，不自动评论、私信或发布。
---

# 抖音多账号内容协同

这是一个编排 Skill，不重复实现账号诊断、评论洞察、热点搜索和竞品研究。它为每个账号绑定一个明确的 `browserId` 和独立 session，分别调用已声明的原子 Skill，最后汇总公开证据、差异化建议和可执行脚本草稿。

依赖必须已安装并可运行：`douyin-live-hot-free`、`douyin-account-diagnosis`、`douyin-comment-insights-free`、`douyin-competitor-research-free`。运行前由 `scripts/workbench.mjs` 校验映射；缺少依赖时立即失败，不把空结果当成功。

## 输入

```json
{
  "accounts": [
    {"id":"account-a","browserId":"easybr-a","accountUrl":"https://www.douyin.com/user/a"},
    {"id":"account-b","browserId":"easybr-b","accountUrl":"https://www.douyin.com/user/b"}
  ],
    "tasks": ["hot-topic", "diagnosis", "comment-insights", "competitor-research", "script"],
  "mode": "real"
}
```

每个账号必须有唯一 `id`、公开页 URL 和用户指定的 `browserId`。同一 `browserId` 不得同时运行两个任务；不同浏览器可以并行，但并发数默认不超过 2。

## 执行步骤

1. 校验账号清单，拒绝重复 `id`、重复 `browserId` 和缺少 URL 的条目。
2. 为每个账号创建独立 session 和证据目录，不共享页面状态或输出文件。
3. 按 `tasks` 调用固定原子 Skill：`hot-topic` → `douyin-live-hot-free`，`diagnosis` → `douyin-account-diagnosis`，`comment-insights` → `douyin-comment-insights-free`，`competitor-research` → `douyin-competitor-research-free`。每个子结果必须保留采集时间、模式、样本范围和证据链接；`script` 使用本 Skill 内置的脚本草稿生成器。
4. 使用 `scripts/workbench.mjs` 生成执行计划和汇总，写入 `multi-account-workbench.json` 与 `multi-account-workbench.md`，按账号分节，标记成功、部分完成或需要用户处理，并为每个账号写入 `scriptDraft`（标题、钩子、正文、行动引导）。
5. 仅在全部账号结果写入后关闭 session；一项失败不能污染其他账号结果。

可执行入口：

```bash
node scripts/workbench.mjs list
node scripts/workbench.mjs plan --input request.json
node scripts/workbench.mjs run --input request.json --output ./output/multi-account
```

先用 `list` 查看在线 EasyBR 环境，再由用户把选中的 `browserId` 写入账号清单。`run` 会先检查四个原子 Skill 的 `SKILL.md` 和脚本是否存在，按账号并行、同一账号内串行调用原子 Skill，读取各自 JSON 结果后再生成汇总；缺少依赖立即失败，不生成伪造成功报告。

## 输出合同

顶层 JSON 必须包含 `schemaVersion`、`mode`、`startedAt`、`finishedAt`、`accounts`、`summary`、`limitations`。每个账号包含 `id`、`browserId`、`status`、`results`、`evidence`、`scriptDraft` 和 `error`。`status` 只允许 `completed`、`partial`、`needs_user_action`、`failed`。

Markdown 必须显示账号隔离情况、三个任务的结果链接、证据数量、失败原因和下一步建议。不得把部分账号结果写成全部账号结论。

## 页面异常和边界

- 登录失效、验证码、风控、实名或权限不足只标记当前账号为 `needs_user_action`，不得绕过。
- 不保存 Cookie、密码、令牌或完整私密资料；只保留公开字段和必要的证据 URL。
- `mode=mock` 结果必须显式标记 mock，不得伪装成真实平台数据。
- 本 Skill 不执行点赞、评论、私信、投稿或批量发布。需要上传时必须调用独立的、用户明确触发的上传 Skill。

## 自测

```bash
node scripts/self-test.mjs
```
