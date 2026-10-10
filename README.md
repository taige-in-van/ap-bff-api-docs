# BFF API 契约

前端、收银台与终端（POS / 商户 App）消费的 REST 契约,按 **consumer / module** 两级分目录(如 `merchant/auth/`)。

- 浏览:根 `index.html`(gh-pages 根)或各模块 `<consumer>/<module>/index.html`(ReDoc)。
- 调用时序:`<consumer>/flows/<name>.html`(前端面带两位序号),按页面组织(每个页面先调谁、后调谁),一份会用到几个模块的接口;先读。
- codegen / Postman:各模块 `<consumer>/<module>/openapi.yaml`(自包含 OpenAPI 3.1)。

## ⚠️ 阶段

**design / review 阶段**契约 —— 实现前可能调整,勿当冻结契约。变更经本仓 commit 追踪,版本见各 `openapi.yaml` 的 `info.version`。

## 鉴权

- 商户后台(`merchant/`):浏览器持 httpOnly opaque 会话 cookie;非安全方法带 `X-CSRF-Token`(详见「API 基础约定」与契约 description)。
- 收银台(`checkout/`):先调入口接口拿会话令牌,之后每个请求在 `X-Checkout-Session` 头里带上(详见该契约顶部「会话」一节)。
- 终端(`terminal/`):沿用现网端的凭据——POS 每个请求带 `Merchant-Id` / `Device-Id` / `Device-Type` / `Global-Sign` 四个头,商户 App 在 `Authorization` 头里带登录令牌、另带 `devCid` 头(详见各契约顶部「凭据」一节)。失败响应是现网外壳加整数 `code`,码义见 `terminal/error-codes.json`。

## 反馈

接口 / 字段疑问 → 提 issue。
