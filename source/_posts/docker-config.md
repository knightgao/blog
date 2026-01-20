---
title: 2026国内使用docker的正确姿势
date: 2026-01-20
updated: 2026-01-20
---

## 安装

```bash
curl -fsSL https://get.docker.com | bash -s docker
```

## 配置镜像

### 一键方案

一键配置，简单快捷，告别拉取超时：

```bash
sudo bash -c "$(curl -sSL https://n3.ink/helper)"
```

### 传统方案

手动配置 Docker 镜像加速器：

1. 创建或编辑 `/etc/docker/daemon.json` 文件：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://dockerproxy.com",
    "https://docker.nju.edu.cn",
    "https://docker.mirrors.sjtug.sjtu.edu.cn"
  ]
}
EOF
```

2. 重启 Docker 服务使配置生效：

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

3. 验证配置是否生效：

```bash
docker info | grep -A 10 "Registry Mirrors"
```

---

注册【毫秒镜像】你即可获得200M流量包，立刻体验极速拉取容器的快感吧！快来行动吧！

https://1ms.run/?aff=24420