# 个人 GitHub Actions 部署

- 仓库：https://github.com/kang340/TrendRadar
- 网页报告：https://kang340.github.io/TrendRadar/
- 运行记录：https://github.com/kang340/TrendRadar/actions/workflows/crawler.yml
- 签到入口：https://github.com/kang340/TrendRadar/actions/workflows/clean-crawler.yml

## 运行与报告

`Get Hot News` 默认每小时第 33 分钟运行一次，GitHub 调度可能延迟。
时区为 `Asia/Shanghai`。每次抓取完成后，工作流将本次生成的报告发布到 GitHub Pages。
热榜接口使用 OmniTrends 作者的公共服务 `https://trends.zzzkkkccc.site/api/s`，
保留原有 11 个热榜平台并增加小红书，共 12 个平台。该服务由作者维护。
报告使用上游默认关键词，报告模式为当前榜单；只显示匹配关键词的内容。
如需恢复 NewsNow 默认接口，将 `platforms.api_url` 改为空字符串并移除小红书源。
默认雅虎财经 RSS 地址在首次部署验证中返回 404，已暂时禁用；Hacker News RSS 正常启用。

尚未配置消息推送和 AI 密钥，对应功能暂时关闭。
尚未配置 S3 存储，每次运行使用临时本地数据库；网页保留最新一次报告，
不提供跨次运行的历史汇总或增量去重。需要这些功能时可按官方文档配置 S3 兼容存储。

## 每周签到

保留上游的 7 天签到机制。每 7 天内在签到入口手动运行一次 `Check In`。
签到会清理 Actions 运行历史；需要排错日志时请先保存。
如果 `Get Hot News` 已因到期禁用，签到后还需手动启用它。

## 接入消息通知

1. 在仓库 Settings → Secrets and variables → Actions 添加渠道凭据。
2. 在 `config/config.yaml` 将 `notification.enabled` 改为 `true`。
3. 手动运行 `Get Hot News`，检查日志并确认目标渠道收到消息。

例如飞书使用 `FEISHU_WEBHOOK_URL`，企业微信使用 `WEWORK_WEBHOOK_URL`，
Telegram 使用 `TELEGRAM_BOT_TOKEN` 和 `TELEGRAM_CHAT_ID`。
密钥和 Webhook 地址只存入 Secrets。

## 官方文档

- https://trendradar.sandev.cc/zh/docs/github-actions/
- https://trendradar.sandev.cc/zh/docs/push-channels/
- https://trendradar.sandev.cc/zh/docs/storage/
