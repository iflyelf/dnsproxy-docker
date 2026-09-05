# dnsproxy-docker

基于 Ubuntu 的 dnsproxy Docker 镜像，支持多种加密 DNS 协议。

## 项目说明

本项目使用 Ubuntu 作为基础镜像，集成 [AdguardTeam/dnsproxy](https://github.com/AdguardTeam/dnsproxy) 最新版本，提供完整的 DNS 代理服务。

## 特性

- ✅ 基于 Ubuntu resolute 构建
- ✅ 完整的开发工具链（gcc、g++、golang、python3、nodejs）
- ✅ 支持多架构（amd64、arm64）
- ✅ 自动从 GitHub 获取最新 dnsproxy 版本
- ✅ 支持 DNS-over-TLS (DoT) - 使用 TLS 加密的 DNS 查询，安全性高
- ✅ 支持 DNS-over-HTTPS (DoH) - 使用 HTTPS 加密的 DNS 查询，兼容性强
- ✅ 支持 DNS-over-QUIC (DoQ) - 使用 QUIC 协议的 DNS 查询，低延迟
- ✅ 支持 DNS-over-HTTP/3 (DoH3) - 基于 HTTP/3 协议的 DNS 查询，兼具低延迟与加密
- ✅ 中文环境优化（时区、字体、输入法）
- ✅ 配置文件热加载

## 架构支持

- linux/amd64
- linux/arm64

## 快速开始

### 使用 Docker 运行

```bash
docker run -d \
  --name dnsproxy \
  --network host \
  -v ./conf/dnsproxy:/etc/dnsproxy:rw \
  iflyelf/dnsproxy:latest
```

### 使用 Docker Compose

```bash
# 下载配置文件
mkdir -p conf/dnsproxy
wget -O conf/dnsproxy/config.yaml https://raw.githubusercontent.com/iflyelf/dnsproxy-docker/main/conf/dnsproxy/config.yaml

# 启动服务
docker-compose up -d
```

## 配置说明

主配置文件位于 `conf/dnsproxy/config.yaml`，主要配置项：

```yaml
# 监听地址
listen-addrs:
  - "0.0.0.0"

# UDP 监听端口 (标准 DNS 端口)
listen-ports:
  - 53

# DNS-over-TLS (DoT) 监听端口 (标准端口 853)
tls-port:
  - 853

# DNS-over-HTTPS (DoH) 监听端口 (标准端口 443)
https-port:
  - 443

# DNS-over-QUIC (DoQ) 监听端口 (标准端口 UDP 853)
quic-port:
  - 853

# 启用 HTTP/3 支持 (DoH3 使用 UDP 443，与 DoH 共用端口号)
http3: true

# 上游 DNS 服务器
upstream:
  - "tcp://127.0.0.1:53"
  - "127.0.0.1:53"
```

## 端口说明

所有端口均使用 IETF 标准端口：

- `53/udp` + `53/tcp` - **DNS** - 标准 DNS 服务端口
- `853/tcp` - **DNS-over-TLS (DoT)** - 使用 TLS 加密的 DNS 查询，安全性高
- `853/udp` - **DNS-over-QUIC (DoQ)** - 使用 QUIC 协议的 DNS 查询，低延迟（与 DoT 共用端口号但协议不同）
- `443/tcp` - **DNS-over-HTTPS (DoH)** - 使用 HTTPS 加密的 DNS 查询，兼容性强
- `443/udp` - **DNS-over-HTTP/3 (DoH3)** - 基于 HTTP/3 协议的 DNS 查询（与 DoH 共用端口号，通过 `http3: true` 启用），兼具低延迟与加密

## 构建信息

### 环境变量

- `TZ=Asia/Shanghai` - 时区设置
- `LANG=zh_CN.UTF-8` - 语言设置
- `GO_VERSION=1.27.1` - Golang 版本
- `GOPROXY=https://goproxy.cn,direct` - Go 模块代理

### 包含工具

- 开发工具：gcc、g++、make、cmake、autoconf
- 编程语言：golang、python3、nodejs
- 网络工具：tcpdump、telnet、curl、wget、nmap
- 系统工具：vim、git、htop、iftop、lsof

## GitHub Actions

本项目已配置自动化构建流程：

- 推送 Dockerfile 时自动构建
- 每天北京时间 5:00 定时构建
- 支持手动触发构建
- 自动推送到 Docker Hub

## 许可证

MIT License

## 作者

iflyelf

## 相关链接

- [AdguardTeam/dnsproxy](https://github.com/AdguardTeam/dnsproxy)
- [Docker Hub](https://hub.docker.com/r/iflyelf/dnsproxy)
- [GitHub Repository](https://github.com/iflyelf/dnsproxy-docker)
