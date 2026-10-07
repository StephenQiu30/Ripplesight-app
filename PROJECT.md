# Ripplesight App 技术约定

## 1. 状态

已冻结。恢复开发需要本人在 Ripplesight 主仓库的 BACKLOG 中重新立项，并先确定目标平台、应用标识、状态管理、导航方案和 HTTP 库。

## 2. 技术

- Flutter + Dart，依赖用 pub 管理，提交 `pubspec.lock`；Flutter/Dart 版本在初始化时锁定。
- 只消费 Ripplesight 主仓库的 HTTP API。接口类型和端点从服务端运行时的 `/openapi.json` 生成到 `lib/api/`，不手写，也不复制 Web 端的类型。
- App 不直接连接 PostgreSQL、Redis、Kafka 或 MinIO。

## 3. 目录（初始化后）

```text
lib/main.dart          # 启动
lib/app/               # 装配、主题、导航
lib/features/          # 按业务功能划分
lib/api/               # 生成的 API 客户端
test/                  # 单元与 Widget 测试
integration_test/      # 设备集成测试
```

平台目录由 Flutter 官方工具生成，只创建实际需要的平台。

## 4. 接口与安全

- 只有一个 HTTP 传输入口：按错误 `code` 和 HTTP 状态分支处理，读取 `details` 和请求 ID；分别处理网络错误、超时、取消、非 JSON 响应、204 和文件下载；不自动重试写请求。
- 移动端的登录、刷新、过期和退出，以服务端的实际接口为准，不照搬 Web 的 Cookie/CSRF 方式。
- 生产环境使用 HTTPS。会话保存在平台的安全存储中，退出时清理。源码、资源和 `--dart-define` 中不放任何服务端密钥。
