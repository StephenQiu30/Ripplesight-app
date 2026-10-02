# HotKey App 项目与架构

HotKey独立客户端固定Flutter+Dart，Web由同级hotkey-server/frontend维护。本仓库尚未初始化Flutter，当前只有工程规范和协作配置，无pubspec.yaml、lib、平台工程或安装包；App交付暂停，统一状态见 [Server BACKLOG](https://github.com/StephenQiu30/hotkey-server/blob/main/BACKLOG.md)。

## 技术与目录

Flutter/Dart版本在初始化时锁定，依赖使用pub并提交应用pubspec.lock。目标平台、应用标识、状态管理、导航、HTTP库和Dart OpenAPI生成器尚未冻结，首个实际切片确定，不预装无使用方的框架或所有平台。

```text
lib/main.dart           # 启动
lib/app/                # 装配、主题和导航
lib/features/           # 业务功能
lib/api/                # OpenAPI生成客户端
test/                   # 单元/Widget
integration_test/       # 设备集成
pubspec.yaml
pubspec.lock
```

以上为初始化后的约定，平台目录由Flutter官方工具生成；按真实使用创建业务模块。

## 契约与安全

API类型/端点由同版本服务端运行时/openapi.json生成，离线输入仅使用同提交CI自动导出产物，不手写第二契约或复制Web DTO。单一传输入口消费生成ErrorView，区分HTTP/网络/超时/取消/非JSON，读取details与请求ID头/body回退；204/文件不包JSON，失败任务正常查询保持业务状态。按code/status分支，禁止message匹配、自动重试写请求及通用万能包装。

公共响应/错误与身份合同以 [Server PROJECT](https://github.com/StephenQiu30/hotkey-server/blob/main/PROJECT.md) 和 [Design001](https://github.com/StephenQiu30/hotkey-server/blob/main/docs/design/001-热点舆情监控平台总体设计.md) 为准。App实现前核验同版本服务端HTTP/OpenAPI合同，APP-02独立完成生成客户端和设备验证；Web通过不代表App通过，服务端门禁不等待App初始化。

移动端鉴权/刷新/过期/退出按服务端实际合同确定，不直接照搬Web同源Cookie/CSRF假设。客户端不直连PostgreSQL/Redis/Kafka/MinIO管理；生产API使用HTTPS，会话用平台安全存储，退出清理必要缓存。API地址是公开客户端配置，不在资源/源码/dart-define中放服务端秘密、签名私钥或账号资料。

## 验证与维护

UI覆盖正常/空/加载/失败/权限，核对触控/键盘/语义标签/字体缩放/对比度和平台交互；生命周期/重连/取消/重试遵守接口语义，页面重建不得无界重复调用。优先核心主题、内容/评论、事件和证据，需求来自服务端统一PRD，设备验收独立记录。

初始化后执行flutter pub get、Dart格式、flutter analyze、flutter test、生成漂移、目标平台构建和integration_test；当前无应用，不宣称这些检查通过。工程规范见 [AGENTS](AGENTS.md)，贡献/安全见 [CONTRIBUTING](CONTRIBUTING.md)/[SECURITY](SECURITY.md)。技术边界在本文维护，状态只在统一BACKLOG维护，不追加交接流水。
