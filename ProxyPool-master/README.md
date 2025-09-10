# ProxyPool

<p align="center">
  <img src="https://raw.githubusercontent.com/jhao104/proxy_pool/master/image/logo.png">
</p>

<p align="center">
    <a href="https://github.com/jhao104/proxy_pool/actions/workflows/python-app.yml"><img src="https://github.com/jhao104/proxy_pool/workflows/python-app/badge.svg" alt="Build Status"></a>
    <a href="https://app.codacy.com/gh/jhao104/proxy_pool/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade"><img src="https://app.codacy.com/project/badge/Grade/a6c1322d34a44c3bb734a79f30f308e9" alt="Codacy Badge"></a>
    <a href="https://github.com/jhao104/proxy_pool/blob/master/LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"></a>
    <a href="https://gitter.im/jhao104_proxy_pool/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge"><img src="https://badges.gitter.im/jhao104_proxy_pool/community.svg" alt="Gitter"></a>
</p>

**ProxyPool** 是一个开源的代理IP池项目，旨在提供高质量、稳定的代理IP资源，支持多种代理类型（HTTP、HTTPS、SOCKS5等），并能够自动检测代理的可用性。

## 主要特性

- **自动抓取**：定时从多个免费代理网站抓取代理IP
- **自动检测**：自动检测代理的可用性、延迟、匿名度等信息
- **自动过滤**：过滤掉不可用或质量较差的代理IP
- **提供API**：提供简洁的API接口，方便获取代理IP
- **支持多种代理类型**：支持HTTP、HTTPS、SOCKS5等代理类型
- **支持Docker部署**：提供Docker配置文件，方便快速部署

## 系统架构

![架构图](https://raw.githubusercontent.com/jhao104/proxy_pool/master/image/架构图.png)

## 快速开始

### Docker部署

```bash
# 克隆项目
git clone https://github.com/jhao104/proxy_pool.git
cd proxy_pool

# 启动Docker容器
docker-compose up -d
```

### 手动部署

1. **安装依赖**

```bash
pip install -r requirements.txt
```

2. **配置文件**

修改 `config.json` 文件，根据自己的需求进行配置。

3. **启动服务**

```bash
python run.py
```

## API接口

### 获取代理IP

```
GET /get
```

参数：
- `type`：可选，指定代理类型，如 `http`、`https`、`socks5` 等
- `country`：可选，指定代理所在国家
- `anonymity`：可选，指定代理匿名度，如 `high`、`middle`、`low` 等

返回示例：

```json
{
  "proxy": "127.0.0.1:8080",
  "type": "http",
  "anonymity": "high",
  "country": "CN",
  "response_time": 0.5
}
```

### 获取代理IP列表

```
GET /get_all
```

参数同 `/get` 接口。

返回示例：

```json
{
  "proxies": [
    {
      "proxy": "127.0.0.1:8080",
      "type": "http",
      "anonymity": "high",
      "country": "CN",
      "response_time": 0.5
    },
    {
      "proxy": "127.0.0.1:8081",
      "type": "https",
      "anonymity": "middle",
      "country": "CN",
      "response_time": 0.8
    }
  ]
}
```

### 检查代理IP可用性

```
GET /check?proxy=127.0.0.1:8080
```

返回示例：

```json
{
  "proxy": "127.0.0.1:8080",
  "available": true,
  "response_time": 0.5
}
```

## 项目结构

```
proxy_pool/
├── crawlers/        # 爬虫模块，负责从各代理网站抓取代理IP
├── processors/      # 处理器模块，负责处理抓取到的代理IP
├── testers/         # 测试模块，负责测试代理IP的可用性
├── storages/        # 存储模块，负责存储代理IP
├── utils/           # 工具模块，提供通用功能
├── api.py           # API接口模块
├── scheduler.py     # 调度器模块，负责调度各模块的运行
├── setting.py       # 配置模块，提供配置信息
└── run.py           # 入口文件
```

## 配置说明

配置文件 `config.json` 的主要配置项如下：

```json
{
  "HOST": "0.0.0.0",         // API服务监听地址
  "PORT": 5555,              // API服务监听端口
  "DB_CONN": "redis://localhost:6379/0",  // Redis连接地址
  "PROXY_FETCHER": [         // 代理获取器列表
    "freeProxy01",
    "freeProxy02",
    // ... 其他代理获取器
  ],
  "TEST_URL": "https://www.baidu.com",  // 测试代理可用性的URL
  "TEST_TIMEOUT": 10,        // 测试超时时间（秒）
  "TEST_INTERVAL": 300,      // 测试间隔时间（秒）
  "MAX_PROXY_COUNT": 500     // 最大代理数量
}
```

## 自定义代理获取器

如果你想添加自定义的代理获取器，可以在 `crawlers` 目录下创建一个新的Python文件，实现 `BaseCrawler` 类，并在 `config.json` 的 `PROXY_FETCHER` 配置项中添加该获取器的名称。

## 贡献指南

如果你想为项目做贡献，请遵循以下步骤：

1. Fork 本项目
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 开启一个 Pull Request

## License

本项目采用 MIT 许可证 - 详见 [LICENSE](https://github.com/jhao104/proxy_pool/blob/master/LICENSE) 文件
