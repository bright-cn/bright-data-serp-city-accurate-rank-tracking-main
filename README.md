# 🚀 Bright Data SERP 排名追踪器 - Node.js 项目

[![Bright Data Promo](https://github.com/bright-cn/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/)

[![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Daily%20Tracking-blue?style=for-the-badge&logo=github)](https://github.com/your-username/bright-data-serp-rank-tracker-nodejs-project/actions)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green?style=for-the-badge&logo=node.js)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

一个功能强大的 Node.js 工具，基于 Bright Data SERP API 跨多关键词、多城市、多设备进行搜索引擎排名追踪。非常适合需要全面排名数据的 SEO 专业人士、市场人员与企业。

## ✨ 功能

- 🔍 多查询支持：可同时处理数百个关键词
- 🌍 地理定向：使用 Google 的 `uule` 参数实现精准城市级结果
- 📱 设备定向：同时支持桌面端与移动端搜索结果
- ⚡ 并发处理：可配置并发度以获得最佳性能
- 📊 双重输出：带时间戳的 JSON 与 CSV 导出
- 🔄 智能去重：按域名去重结果
- 📈 实时进度：实时追踪处理状态
- 🌐 国际覆盖：支持多国家与多语言
- 💾 缓存地理数据：本地缓存提升地理定向效率

## 🏗️ 架构

脚本采用精巧的架构设计：
- 解析 Bright Data API 响应（数据在 `body` 字段中返回）
- 提取自然搜索结果并进行字段映射
- 通过受控并发避免触发 API 速率限制
- 提供完善的错误处理与日志记录
- 生成干净、结构化的可分析输出

## 📋 前置条件

- Node.js 16+
- npm 或 yarn
- Bright Data SERP API 账号
- 有效的 API key 与 zone

## 🚀 快速开始

### 1. 克隆与安装

```bash
git clone <repository-url>
cd bright-data-serp-city-accurate-rank-tracking
npm install
```

### 2. 配置环境

```bash
cp env.example .env
```

编辑 `.env` 并填入你的凭据：
```env
BRIGHT_DATA_API_KEY=your_api_key_here
BRIGHT_DATA_ZONE=serp_api1
```

### 3. 运行脚本

```bash
# 使用默认设置的基础运行
npm run dev

# 自定义并发
npm run dev -- --concurrency 10

# 使用更小的数据集进行测试
npm run dev -- --queries data/test-queries.csv --locations data/test-locations.csv
```

## 🤖 每日自动追踪

仓库内置 GitHub Actions，用于每日自动化排名追踪：

- ⏰ 调度：每天欧洲时间早上 6:00 运行
- 🎯 手动触发：可随时手动运行
- 📊 结果：自动作为工件上传
- 🔐 安全：使用 GitHub Secrets 存储 API 凭据

设置方法：详见 [GITHUB_ACTIONS_SETUP.md](GITHUB_ACTIONS_SETUP.md)。

## 📁 输入文件

### 查询 CSV（`data/queries.csv`）
```csv
keyword,brand
"best pizza near me",
"coffee shops",
"dentist office",
"plumber services",
"restaurant delivery"
```

### 地点 CSV（`data/locations.csv`）
```csv
city,country,language,device
"New York","US","en","desktop"
"Los Angeles","US","en","desktop"
"London","GB","en","desktop"
"Paris","FR","fr","mobile"
"Tokyo","JP","ja","desktop"
```

## 🎯 使用示例

### 基础用法
```bash
npm run dev
```
以默认并发（5）处理所有查询与地点。

### 自定义并发
```bash
npm run dev -- --concurrency 10
```
同时处理 10 个请求以更快完成。

### 测试运行
```bash
npm run dev -- --queries data/test-queries.csv --locations data/test-locations.csv --concurrency 3
```
适合小数据集测试（2 个关键词 × 2 个城市 = 4 个任务）。

### 包含地图结果
```bash
npm run dev -- --surface maps --maps
```
同时采集自然结果与本地/地图结果。

### 演示模式
```bash
npm run demo
```
在不调用 API 的情况下展示界面与能力（非常适合演示）。

## 📜 可用脚本

| 脚本 | 描述 |
|------|------|
| `npm run dev` | 运行主 SERP 排名追踪脚本 |
| `npm run start` | `npm run dev` 的别名 |
| `npm run demo` | 运行演示模式（不发起 API 调用） |

## 📊 输出格式

### JSON 结构
```json
{
  "keyword": "coffee shops",
  "engine": "google",
  "surface": "search",
  "city": "New York",
  "country": "US",
  "device": "desktop",
  "position": 1,
  "title": "Coffeehouse",
  "url": "https://en.wikipedia.org/wiki/Coffeehouse",
  "domain": "en.wikipedia.org",
  "snippet": "A coffeehouse, coffee shop, or café..."
}
```

### CSV 列
- `keyword` - 搜索关键词
- `engine` - 搜索引擎（google/bing）
- `surface` - 搜索界面（search/maps）
- `city` - 目标城市
- `country` - 目标国家
- `device` - 设备类型（desktop/mobile）
- `position` - 排名位置
- `title` - 结果标题
- `url` - 结果 URL
- `domain` - 提取出的域名
- `snippet` - 结果描述

## ⚙️ 配置选项

| 选项 | 描述 | 默认值 |
|------|------|--------|
| `--engine` | 搜索引擎（google/bing） | `google` |
| `--surface` | 搜索界面（search/maps） | `search` |
| `--queries` | 查询 CSV 路径 | `data/queries.csv` |
| `--locations` | 地点 CSV 路径 | `data/locations.csv` |
| `--maps` | 是否包含地图界面 | `false` |
| `--concurrency` | 并发请求数量 | `5` |

## 🌍 支持的地区

脚本支持带精确地理定向的国际地区：

- 北美：美国、加拿大
- 欧洲：英国、法国、德国
- 亚洲：日本
- 大洋洲：澳大利亚
- 拉美：墨西哥

每个地点均使用 Google 的 `uule` 参数实现精准地理定向。

## 📈 性能

### 测试结果
- 小数据集（2×2）：约 30 秒返回 25 个结果
- 全量（10×10）：约 2 分钟返回 314 个结果
- 并发：可配置为 1–20+ 个请求

### 优化建议
- 使用 5–10 的并发以获得最佳性能
- 先用小数据集测试
- 关注 API 速率限制
- 本地缓存地理定向数据

## 🔧 故障排除

### 常见问题

API 超时错误
```bash
# 降低并发
npm run dev -- --concurrency 3
```

无结果返回
- 检查 API key 与 zone 配置
- 校验查询格式与地点数据
- 确认 Bright Data 账号余额充足

地理定向问题
- 核对城市/国家的组合是否有效
- 检查地理目标缓存
- 留意 uule 参数生成逻辑

### 调试模式
脚本包含全面日志：
- API 响应结构分析
- 地理定向参数生成
- 各任务进度追踪
- 失败请求的错误详情

## 🏗️ 项目结构

```
├── index.js              # 主应用脚本
├── geo-targets.js        # 地理定向工具
├── data/                 # 输入 CSV 文件
│   ├── queries.csv      # 待追踪关键词
│   └── locations.csv    # 目标城市/地区
├── output/              # 生成的结果
├── package.json         # 依赖与脚本
├── .env                 # 环境配置
└── README.md           # 本文件
```

## 🤝 贡献

1. Fork 本仓库
2. 新建功能分支
3. 完成更改
4. 充分测试
5. 提交 Pull Request

## 📄 许可证

MIT License - 详见 LICENSE 文件。

## 🆘 支持

如需帮助：
- 脚本功能：查看本 README 与故障排除章节
- Bright Data API：联系 Bright Data 支持
- 地理定向：查看 geo-targets.js 与地点数据

## 🎯 演示就绪

本仓库已为演示优化：
- ✅ 清晰有序的代码结构
- ✅ 全面的文档
- ✅ 可运行示例与样例数据
- ✅ 性能基准
- ✅ 故障排除指南
- ✅ 专业的 README

非常适合向客户、干系人或技术团队展示 SERP 排名追踪能力！
