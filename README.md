# BFF API 契约

前端消费的 BFF REST 契约,按 **consumer / module** 两级分目录(如 `merchant/auth/`)。

- 浏览:根 `index.html`(gh-pages 根)或各模块 `<consumer>/<module>/index.html`(ReDoc)。
- 调用时序:`<consumer>/flows/<NN-name>.html`,按页面组织(每个页面先调谁、后调谁),一份会用到几个模块的接口;先读。
- codegen / Postman:各模块 `<consumer>/<module>/openapi.yaml`(自包含 OpenAPI 3.1)。

## ⚠️ 阶段

**design / review 阶段**契约 —— 实现前可能调整,勿当冻结契约。变更经本仓 commit 追踪,版本见各 `openapi.yaml` 的 `info.version`。

## 鉴权

浏览器持 httpOnly opaque 会话 cookie;非安全方法带 `X-CSRF-Token`(详见「API 基础约定」与契约 description)。

## 反馈

接口 / 字段疑问 → 提 issue。
