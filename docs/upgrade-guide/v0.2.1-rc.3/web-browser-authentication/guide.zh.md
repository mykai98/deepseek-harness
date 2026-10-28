---
kind: upgrade-guide
description: "Web bundle 默认允许浏览器在没有启动 token 或 cookie 的情况下访问。"
---

# Web 浏览器认证改为可选

[English](guide.md) | 中文

## 变更

随附 Web bundle 将 Connection 行的 `requireBrowserAuthentication` 设为 `false`。此前，打开 index、调用 RPC 或建立 WebSocket 流需要通过打印的启动 URL 获取签名浏览器 cookie。现在，可信浏览器请求无需凭据即可使用共享 owner Peer。Host/Origin 检查仍然生效；API `/connect` 仍要求进程 token。

## 迁移

1. 重启 `dsh web`，打开不含查询参数的根 URL。页面及其 API 调用应在没有浏览器 cookie 时成功。
2. 需要浏览器认证的部署，必须在 `$DSH_HOME/profiles/web/cordis.patch.yml` 的 `connection` 行设置 `config.requireBrowserAuthentication: true`。patch 会替换完整 config，因此应保留该行的 `trustedHosts` 及其他设置。确认未认证请求返回 HTTP 401，并且打印的启动 URL 仍可打开页面。
