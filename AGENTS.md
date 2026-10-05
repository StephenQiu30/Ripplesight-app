# HotKey App 工程规范

适用于整个仓库。`CLAUDE.md` 通过导入加载本文件，规则只在这里修改。技术约定见 [PROJECT](PROJECT.md)。仓库当前已冻结，没有立项之前不初始化 Flutter 工程。

## 1. 开发

- 只在实际需要时创建目录和模块；不建通用包装层，不维护第二套 DTO。
- 命名规则：文件用 snake_case，类用 UpperCamelCase，成员用 lowerCamelCase；遵守 Dart 的格式化与静态分析规则。
- 每个界面都要处理五种状态：正常、空、加载、错误、无权限。同时检查触控、键盘、语义标签、字体缩放和对比度。
- 页面重建时不能无限重复调用接口；断网重连、取消和重试按接口语义处理。

## 2. 完成前的检查（初始化后）

`dart format --set-exit-if-changed .`、`flutter analyze`、`flutter test`、生成客户端无漂移、目标平台构建通过、`integration_test` 通过。检查失败要照实报告。

## 3. 安全

不提交 `.env`、签名私钥、keystore、账号会话、真实用户数据或本地产物。日志、截图和崩溃信息都要脱敏。

## 4. 提交

- 未经本人明确授权，不提交、不推送。
- 提交信息格式：`type(scope):中文动宾描述`（冒号后不加空格，≤72 字符）。type 只能是 feat/fix/test/refactor/docs/chore/perf/build/ci/revert；正文用中文写清改了什么、为什么改、怎么验证的。
