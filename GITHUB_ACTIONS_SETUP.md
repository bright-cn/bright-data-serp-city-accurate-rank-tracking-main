# 🚀 GitHub Actions 设置指南

本指南介绍如何使用 GitHub Actions 设置自动化的每日 SERP 排名跟踪。

## 📋 前提条件

- 包含 SERP 排名跟踪器代码的 GitHub 仓库
- Bright Data API 凭据
- GitHub 仓库管理员权限

## 🔐 设置密钥

### 1. 前往仓库设置
进入你的仓库 → **Settings** → **Secrets and variables** → **Actions**

### 2. 添加必需的密钥

#### `BRIGHT_DATA_API_KEY`
- **名称**：`BRIGHT_DATA_API_KEY`
- **值**：你的 Bright Data API 密钥
- **说明**：用于访问 Bright Data 搜索引擎 API 的 API 密钥

#### `BRIGHT_DATA_ZONE`（可选）
- **名称**：`BRIGHT_DATA_ZONE`
- **值**：`serp_api1`（或你的自定义区域）
- **说明**：Bright Data 区域标识符

## ⏰ 计划配置

工作流已配置为**每天欧洲时间上午 6:00**运行：

- **冬季（CET）**：中欧时间上午 6:00
- **夏季（CEST）**：中欧夏令时间上午 6:00
- **对应 UTC 时间**：冬季 UTC 上午 5:00 / 夏季 UTC 上午 4:00

### 自定义计划

编辑 `.github/workflows/daily-rank-tracking.yml` 并修改 cron 表达式：

```yaml
schedule:
  - cron: '0 5 * * *'  # Current: 5:00 AM UTC daily
```

**常用 Cron 模式：**
- `0 5 * * *` - 每天 UTC 上午 5:00
- `0 */6 * * *` - 每 6 小时
- `0 5 * * 1-5` - 仅工作日 UTC 上午 5:00
- `0 5 1 * *` - 每月第一天 UTC 上午 5:00

## 🎯 手动触发

你可以随时手动运行工作流：

1. 前往仓库中的 **Actions** 选项卡
2. 选择 **🚀 每日 SERP 排名跟踪**
3. 点击 **Run workflow**
4. 选择选项：
   - **测试模式**：使用有限数据运行（更快、成本更低）
   - **并发数**：同时发出的 API 请求数量

## 📊 监控与结果

### 工作流状态
- **绿色勾号**：成功 ✅
- **红色 X**：失败 ❌
- **黄色圆点**：进行中 🔄

### 访问结果
1. **构件**：从 Actions 选项卡下载结果
2. **摘要**：在工作流运行页面查看结果摘要
3. **日志**：查看详细执行日志

### 构件保留
- 结果将保留 **30 天**
- 每次运行都会创建带时间戳的新构件
- 之前的运行记录仍可在 Actions 历史记录中访问

## 🔧 故障排除

### 常见问题

#### API 速率限制
```
Error: Rate limit exceeded
```
**解决方案**：降低并发数或在请求之间添加延迟

#### 身份验证错误
```
Error: 401 Unauthorized
```
**解决方案**：检查 `BRIGHT_DATA_API_KEY` 密钥是否正确

#### 缺少依赖项
```
Error: Cannot find module
```
**解决方案**：确保已提交 `package-lock.json`

### 调试模式
通过在工作流中添加以下内容启用调试日志：

```yaml
- name: 🧪 Run rank tracking
  env:
    DEBUG: "*"
  run: npm run dev -- --concurrency 3
```

## 📈 性能优化

### 推荐设置
- **生产环境**：并发数 5-10
- **测试环境**：并发数 2-3
- **大型数据集**：考虑拆分为多个工作流

### 成本管理
- 在 Bright Data 控制面板中监控 API 使用量
- 开发时使用测试模式
- 设置使用量提醒

## 🔄 更新工作流

### 自动更新
Dependabot 将自动为以下内容提供更新建议：
- Node.js 版本
- GitHub Actions 版本
- npm 依赖项

### 手动更新
1. 编辑 `.github/workflows/daily-rank-tracking.yml`
2. 提交并推送更改
3. GitHub Actions 将在下次运行时使用新配置

## 📞 支持

如果遇到问题：
1. 检查工作流日志
2. 确认密钥设置正确
3. 使用 `npm run dev` 在本地进行测试
4. 检查 Bright Data API 状态

## 🎉 成功指标

满足以下条件即表示设置运行正常：
- ✅ 工作流在计划时间自动运行
- ✅ 结果已生成并作为构件上传
- ✅ 摘要显示正确的结果数量
- ✅ 没有身份验证或速率限制错误 