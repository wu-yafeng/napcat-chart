# NapCat Helm Chart

这是 NapCat 的 Helm Chart，用于在 Kubernetes 集群中部署 NapCat。

## 安装

```bash
# 添加 Helm 仓库
helm repo add napcat https://wu-yafeng.github.io/napcat-chart

# 更新仓库
helm repo update

# 安装 chart
helm install my-napcat napcat/napcat
```

## 配置

主要配置选项：

```yaml
# values.yaml
replicaCount: 3

image:
  repository: mlikiowa/napcat-docker
  tag: "latest"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  
ingress:
  enabled: true
  className: "traefik"
  webui:
    hosts:
      - host: napcat.example.com

httpServers:
  - name: httpServer
    enabled: true
    port: 3000
    token: "your-token-here"
```

## 升级

```bash
# 升级到最新版本
helm upgrade my-napcat napcat/napcat

# 或指定版本
helm upgrade my-napcat napcat/napcat --version 1.0.0
```

## 卸载

```bash
helm uninstall my-napcat
```

## 开发

### 本地测试

```bash
# 验证 chart
helm lint chart/

# 模板渲染测试
helm template my-napcat chart/

# 试运行安装
helm install my-napcat chart/ --dry-run --debug
```

### 发布新版本

1. 更新 `chart/Chart.yaml` 中的版本号
2. 提交更改到 main 分支
3. GitHub Actions 将自动构建和发布新版本

### 手动发布

```bash
# 打包 chart
helm package chart/

# 更新仓库索引
helm repo index . --url https://wu-yafeng.github.io/napcat-chart

# 提交到 gh-pages 分支
git add .
git commit -m "Release new version"
git push origin gh-pages
```

## Chart 结构

```
chart/
├── Chart.yaml          # Chart 元数据
├── values.yaml         # 默认配置值
├── templates/          # Kubernetes 模板文件
│   ├── _helpers.tpl    # 模板助手
│   ├── configmap.yaml  # ConfigMap
│   ├── ingress.yaml    # Ingress
│   ├── service.yaml    # Service
│   ├── stateful.yaml   # StatefulSet
│   └── tests/          # 测试文件
└── charts/             # 依赖的子 charts
```

## 贡献

欢迎提交 Issues 和 Pull Requests！

## 许可证

请查看 [LICENSE](LICENSE) 文件。
