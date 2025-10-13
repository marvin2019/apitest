# Apifox 自动化API测试 CICD 配置指南

本项目提供了一个基于 GitHub Actions 的自动化 API 测试 CICD 流程，使用 Apifox CLI 执行测试并将结果推送到飞书通知。

## 环境变量配置

在使用此 CICD 流程前，需要在 GitHub 仓库的 Settings > Secrets and variables > Actions 中配置以下环境变量：

### 1. 必需的环境变量

| 变量名称 | 描述 | 如何获取 |
|---------|------|----------|
| `APIFOX_ACCESS_TOKEN` | Apifox 访问令牌，用于身份认证 | 在 Apifox 平台的「个人设置」>「访问令牌」中创建 |
| `FEISHU_WEBHOOK_URL` | 飞书机器人 Webhook URL，用于发送测试报告通知 | 在飞书群中添加自定义机器人，获取 Webhook 地址 |

### 2. 测试参数配置

在 `.github/workflows/main.yml` 文件中，需要修改以下参数以匹配您的 Apifox 项目配置：

| 参数 | 描述 | 如何获取 |
|------|------|----------|
| `APIFOX_PROJECT_ID` | Apifox 项目 ID | 在 Apifox 项目 URL 中可以找到 |
| `APIFOX_ENV_ID` | 环境 ID | 在 Apifox 环境设置中可以查看环境的 ID |
| `APIFOX_TEST_CASE_IDS` | 测试用例 ID（支持多个） | 在测试用例详情页 URL 中可以找到，多个 ID 用逗号分隔，如 '1,2,3' |

## Apifox CLI 参考

Apifox CLI 提供了丰富的命令选项，详情请参考 [Apifox CLI 命令选项文档](https://docs.apifox.com/cli-command-options)。

## 如何使用

### 1. Fork 或复制此仓库

将本仓库的文件结构（特别是 `.github/workflows/main.yml`）复制到您的项目中。

### 2. 配置环境变量

按照上述说明在 GitHub 仓库中配置必需的环境变量。

### 3. 修改测试参数

编辑 `.github/workflows/main.yml` 文件，更新测试参数以匹配您的 Apifox 项目。

### 4. 配置多个测试用例

如果需要运行多个测试用例，可以在 `APIFOX_TEST_CASE_IDS` 中用逗号分隔多个测试用例 ID，例如：

```yaml
APIFOX_TEST_CASE_IDS: '1,2,3,4'
```

### 5. 触发测试

测试将在以下情况下自动触发：
- 推送到 `main` 或 `master` 分支
- 创建/更新针对 `main` 或 `master` 分支的 Pull Request

## 测试结果

### 1. GitHub Actions 页面

在 GitHub Actions 页面中可以查看测试的详细日志输出。

### 2. 测试报告

测试完成后，HTML 格式的测试报告会作为 Artifact 上传，可以在 Actions 运行页面下载。

### 3. 飞书通知

测试结果将通过配置的飞书 Webhook 发送通知，包含以下信息：
- 仓库信息
- 分支信息
- 测试状态
- 总耗时
- 执行的测试用例 ID
- HTTP 接口总数和失败数
- 断言总数和失败数
- 报告下载链接

## 自定义配置

您可以根据需要修改 `.github/workflows/main.yml` 文件中的其他配置，例如：

- 修改触发条件（如添加特定分支或标签）
- 更改测试报告格式
- 调整飞书通知的内容和格式

## 故障排除

### 常见问题

1. **测试失败**：检查 Apifox 访问令牌是否有效，以及项目 ID 和环境 ID 是否正确
2. **飞书通知未收到**：确认 Webhook URL 是否正确，以及网络连接是否正常
3. **报告下载失败**：检查 Artifact 上传步骤是否成功执行
4. **多测试用例执行问题**：确保测试用例 ID 之间使用英文逗号分隔，不要有空格

### 查看详细日志

在 GitHub Actions 运行页面中，点击失败的步骤可以查看详细的日志输出，帮助定位问题。

## 联系与支持

如有任何问题或建议，请随时提交 Issue 或 Pull Request。