---
kind: upgrade-guide
description: "The Web bundle allows browser access without a launch token or cookie by default."
---

# Web browser authentication is optional

English | [中文](guide.zh.md)

## Change

The shipped Web bundle sets the Connection row's `requireBrowserAuthentication` to `false`. Previously, opening the index, calling RPC, or opening WebSocket streams required a signed browser cookie obtained from the printed launch URL. Trusted browser requests now use the shared owner Peer without credentials. Host/Origin checks remain active; API `/connect` still requires the process token.

## Migration

1. Restart `dsh web` and open its root URL without query parameters. The page and its API calls should succeed without a browser cookie.
2. Deployments that require browser authentication must set `config.requireBrowserAuthentication: true` on the `connection` row in `$DSH_HOME/profiles/web/cordis.patch.yml`. A patch replaces the complete config, so preserve the row's `trustedHosts` and any other settings. Confirm that an unauthenticated request returns HTTP 401 and that the printed launch URL still opens the page.
