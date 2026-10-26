# BFF API 契约

前端、收银台、终端（POS / 商户 App）与微信确认页消费的 REST 契约,按 **consumer / module** 两级分目录(如 `merchant/auth/`)。

- 浏览:根 `index.html`(gh-pages 根)或各模块 `<consumer>/<module>/index.html`(ReDoc)。
- 调用时序:`<consumer>/flows/<name>.html`(前端面带两位序号),按页面组织(每个页面先调谁、后调谁),一份会用到几个模块的接口;先读。
- codegen / Postman:各模块 `<consumer>/<module>/openapi.yaml`(自包含 OpenAPI 3.1)。

## ⚠️ 阶段

**design / review 阶段**契约 —— 实现前可能调整,勿当冻结契约。变更经本仓 commit 追踪,版本见各 `openapi.yaml` 的 `info.version`。整批契约的版本见根 `publish.json` 的 `contractStage`(首页也显示),与 `ready.json` 的 `prdStage`(联调环境部署所依据的版本)相同即两边一致。根 `index.html` 的接口清单里,标 🆕 的模块是本次发布相对上一次版本有变化的(括号里写上次 → 本次的版本;新加的模块写「新增」)。

## 联调就绪标识

调用时序页的接口表里,接口后面的标识表示它在联调环境上的状态:✅ 已部署、可对接调试;⚠️ 已部署,但契约在实现之后有改动、待后端按新契约重验;无标识 = 尚未部署。标识来自根目录 `ready.json`(后端在联调环境部署后更新),页头写明环境与生成时间。根 `index.html` 的调用时序列表里,页面名后的 ✅ 表示该页接口表里的接口全部 ✅。

联调环境的地址、本地代理、CSRF、测试账号与数据重置说明见根目录 `dev-env.html`(后端随环境变化更新;还没有联调环境时没有这份文件)。

## 鉴权

- 商户后台(`merchant/`):浏览器持 httpOnly opaque 会话 cookie;非安全方法带 `X-CSRF-Token`(详见「API 基础约定」与契约 description)。
- 收银台(`checkout/`):先调入口接口拿会话令牌,之后每个请求在 `X-Checkout-Session` 头里带上(详见该契约顶部「会话」一节)。
- 终端(`terminal/`):沿用现网端的凭据——POS 每个请求带 `Merchant-Id` / `Device-Id` / `Device-Type` / `Global-Sign` 四个头,商户 App 在 `Authorization` 头里带登录令牌、另带 `devCid` 头(详见各契约顶部「凭据」一节)。失败响应是现网外壳加整数 `code`,码义见 `terminal/error-codes.json`。
- 微信确认页(`wechat/`):不带凭据,凭页面地址里带来的确认材料调用(详见该契约顶部说明);码义见根 `error-codes.json`。

## 反馈

接口 / 字段疑问 → 提 issue。
