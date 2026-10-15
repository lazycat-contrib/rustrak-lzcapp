# Rustrak - LazyCat App

[Rustrak](https://github.com/rustrak/rustrak)（超轻量自托管错误追踪，兼容 Sentry SDK）的懒猫微服（LazyCat）打包。

## 模式

- **双商店发布**：官方商店（含截图）+ 喵喵商店。
- **镜像模式（mirror）**：`docker.io/rustrak/rustrak-server` 镜像经 `docker.1ms.run` 加速器拉取，`require_digest_match: true` 校验镜像内容与上游一致。
- **自动跟随上游**：每日检查上游 semver tag，发现新版本自动更新版本号与镜像、构建 LPK、发布商店。
- **懒猫 OIDC 单点登录**：`application.oidc_redirect_path: /auth/sso/callback` + `LAZYCAT_AUTH_OIDC_*` 注入变量，登录页直接用懒猫账号。
- **不支持移动端**（`unsupported_platforms: ios/android/tvos`）。

## 配置

- SQLite 单容器，数据卷 `/lzcapp/var/rustrak:/data`
- `PUBLIC_URL` 自动指向子域名（DSN 生成与 Cookie 依赖）
- `SESSION_SECRET_KEY` 用 `stable_secret` 自动生成
- 健康检查 `/health`

## 使用

在应用里把 Sentry SDK 的 DSN 指向本应用生成的 DSN 即可开始收集错误。

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `LZC_API_TOKEN` | 懒猫开放平台 PAT（官方商店） |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `APP_ID` / `PRIVATE_STORE_GROUP_CODES` | 可选 |

## 许可

GPL-3.0（上游）
