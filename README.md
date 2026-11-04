# 🚀 Bright Data SERP 排名跟踪器 - Node.js 项目

[![Bright Data 推广](https://github.com/bright-cn/Awesome-Web-Scraping/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner%20CN.png)](https://www.bright.cn/)

[![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Daily%20Tracking-blue?style=for-the-badge&logo=github)](https://github.com/your-username/bright-data-serp-rank-tracker-nodejs-project/actions) [![Node.js](https://img.shields.io/badge/Node.js-18+-green?style=for-the-badge&logo=node.js)](https://nodejs.org/) [![许可证](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

一款功能强大的 Node.js 工具，使用 Bright Data 搜索引擎 API 跟踪跨多个关键词、城市和设备的搜索引擎排名。非常适合需要全面排名跟踪数据的 SEO 专业人员、营销人员和企业。

## ✨ 功能

- **🔍 多查询支持**：同时处理数百个关键词
- **🌍 地理位置定位**：使用 Google 的 `uule` 参数获取准确的城市级结果
- **📱 设备定位**：支持桌面设备和移动设备搜索结果
- **⚡ 并发处理**：可配置并发数以实现最佳性能
- **📊 双格式输出**：导出带时间戳的 JSON 和 CSV
- **🔄 智能去重**：按域名删除重复结果
- **📈 实时进度**：实时跟踪处理状态
- **🌐 全球覆盖**：支持多个国家和语言
- **💾 缓存的地理位置数据**：通过本地缓存高效进行地理位置定位

## 🏗️ 架构

此脚本采用先进的架构，可实现以下功能：
- 解析 Bright Data API 响应（返回的数据位于 `body` 字段中）
- 通过正确的字段映射提取自然搜索结果
- 实施受控并发以避免 API 速率限制
- 提供全面的错误处理和日志记录
- 生成整洁的结构化输出以供分析

## 📋 前提条件

- Node.js 16+ 
- npm 或 yarn
- Bright Data 搜索引擎 API 账户
- 有效的 API 密钥和区域

## 🚀 快速开始

### 1. 克隆并安装

```bash
git clone <repository-url>
cd bright-data-serp-city-accurate-rank-tracking
npm install
```

### 2. 配置环境

```bash
cp env.example .env
```

使用你的凭据编辑 `.env`：
```env
BRIGHT_DATA_API_KEY=your_api_key_here
BRIGHT_DATA_ZONE=serp_api1
```

### 3. 运行脚本

```bash
# Basic run with default settings
npm run dev

# With custom concurrency
npm run dev -- --concurrency 10

# Test with smaller dataset
npm run dev -- --queries data/test-queries.csv --locations data/test-locations.csv
```

## 🤖 每日自动跟踪

此仓库包含用于每日自动跟踪排名的 GitHub Actions：

- **⏰ 计划**：每天欧洲时间上午 6:00 运行
- **🎯 手动触发**：可随时手动运行
- **📊 结果**：自动作为构件上传
- **🔐 安全**：使用 GitHub Secrets 存储 API 凭据

**设置**：有关详细配置，请参阅 [GITHUB_ACTIONS_SETUP.md](GITHUB_ACTIONS_SETUP.md)。

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

### 位置 CSV（`data/locations.csv`）
```csv
city,country,language,device
"New York","US","en","desktop"
"Los Angeles","US","en","desktop"
"London","GB","en","desktop"
"Paris","FR","fr","mobile"
"Tokyo","JP","ja","desktop"
```

## 🎯 使用示例

### 基本用法
```bash
npm run dev
```
使用默认并发数（5）处理所有查询和位置。

### 自定义并发数
```bash
npm run dev -- --concurrency 10
```
同时处理 10 个请求，以提高执行速度。

### 测试运行
```bash
npm run dev -- --queries data/test-queries.csv --locations data/test-locations.csv --concurrency 3
```
非常适合使用较小的数据集进行测试（2 个查询 × 2 个城市 = 4 个任务）。

### 包含地图结果
```bash
npm run dev -- --surface maps --maps
```
同时采集自然搜索结果和本地/地图结果。

### 演示模式
```bash
npm run demo
```
无需发出 API 调用即可展示界面和功能（非常适合演示）。

## 📜 可用脚本

| 脚本 | 说明 |
|--------|-------------|
| `npm run dev` | 运行主 SERP 排名跟踪脚本 |
| `npm run start` | `npm run dev` 的别名 |
| `npm run demo` | 运行演示模式（不调用 API） |

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
- `keyword` - 搜索查询
- `engine` - 搜索引擎（Google/Bing）
- `surface` - 搜索结果类型（搜索/地图）
- `city` - 目标城市
- `country` - 目标国家/地区
- `device` - 设备类型（桌面设备/移动设备）
- `position` - 排名位置
- `title` - 结果标题
- `url` - 结果 URL
- `domain` - 提取的域名
- `snippet` - 结果描述

## ⚙️ 配置选项

| 选项 | 说明 | 默认值 |
|--------|-------------|---------|
| `--engine` | 搜索引擎（Google/Bing） | `google` |
| `--surface` | 搜索结果类型（搜索/地图） | `search` |
| `--queries` | 查询 CSV 的路径 | `data/queries.csv` |
| `--locations` | 位置 CSV 的路径 | `data/locations.csv` |
| `--maps` | 包含地图结果类型 | `false` |
| `--concurrency` | 并发请求数 | `5` |

## 🌍 支持的位置

此脚本支持全球多个地点，并可进行正确的地理位置定位：

- **北美洲**：美国、加拿大
- **欧洲**：英国、法国、德国
- **亚洲**：日本
- **大洋洲**：澳大利亚
- **拉丁美洲**：墨西哥

每个位置都使用 Google 的 `uule` 参数进行精确的地理位置定位。

## 📈 性能

### 测试结果
- **小型数据集**（2×2）：约 30 秒内获得 25 个结果
- **完整规模**（10×10）：约 2 分钟内获得 314 个结果
- **并发数**：可配置为 1-20+ 个请求

### 优化技巧
- 使用 5-10 的并发数以获得最佳性能
- 先使用较小的数据集进行测试
- 监控 API 速率限制
- 在本地缓存地理位置定位数据

## 🔧 故障排除

### 常见问题

**API 超时错误**
```bash
# Reduce concurrency
npm run dev -- --concurrency 3
```

**未找到结果**
- 检查 API 密钥和区域配置
- 确认查询格式和位置数据
- 确保 Bright Data 账户有足够的额度

**地理位置定位问题**
- 确认城市/国家组合
- 检查地理位置目标缓存
- 检查 uule 参数生成过程

### 调试模式
此脚本包含全面的日志记录：
- API 响应结构分析
- 地理位置定位参数生成
- 每个任务的进度跟踪
- 失败请求的错误详情

## 🏗️ 项目结构

```
├── index.js              # Main application script
├── geo-targets.js        # Geo-targeting utilities
├── data/                 # Input CSV files
│   ├── queries.csv      # Keywords to track
│   └── locations.csv    # Target cities/locations
├── output/              # Generated results
├── package.json         # Dependencies and scripts
├── .env                 # Environment configuration
└── README.md           # This file
```

## 🤝 贡献

1. 复刻仓库
2. 创建功能分支
3. 进行更改
4. 全面测试
5. 提交拉取请求

## 📄 许可证

MIT 许可证 - 详情请参阅 LICENSE 文件。

## 🆘 支持

对于以下相关问题：
- **脚本功能**：查看此 README 和故障排除部分
- **Bright Data API**：联系 Bright Data 支持团队
- **地理位置定位**：查看 geo-targets.js 和位置数据

## 🎯 可用于演示

此仓库已针对演示进行优化，具备以下特点：
- ✅ 整洁且组织有序的代码结构
- ✅ 完善的文档
- ✅ 可运行的示例和样本数据
- ✅ 性能基准
- ✅ 故障排除指南
- ✅ 专业的 README

非常适合向客户、利益相关者或技术团队展示 SERP 排名跟踪能力！ 