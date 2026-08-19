# Hiwonder Docs 部署说明

适用于仓库 `Hiwonder-docs-vite`，线上入口为：

```text
https://wiki.hiwonder.com/
```

本仓库作为 Hiwonder 文档门户首页，构建产物放在：

```text
projects/Hiwonder-docs/en/latest/
```

根目录 `index.html` 会自动跳转到：

```text
/projects/Hiwonder-docs/en/latest/docs/index.html
```

## 本地构建

```bash
npm ci
npm run docs:build
npm run docs:stage-main
```

构建完成后确认：

- `projects/Hiwonder-docs/en/latest/index.html` 存在
- `projects/Hiwonder-docs/en/latest/docs/index.html` 存在
- HTML 中资源路径为 `/projects/Hiwonder-docs/en/latest/assets/...`

## GitHub Pages 配置

仓库地址：

```text
https://github.com/Hiwonder-docs/Hiwonder-docs-vite
```

GitHub Pages 设置：

```text
Source: Deploy from a branch
Branch: main
Folder: / (root)
Custom domain: 留空
```

直连测试地址：

```text
https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/
```

## 宝塔 Nginx 配置

在 `wiki.hiwonder.com` 站点的 `server {}` 中添加：

```nginx
# Hiwonder Docs portal root
location = / {
    return 302 /projects/Hiwonder-docs/en/latest/docs/index.html;
}

# Hiwonder Docs portal static site
location ^~ /projects/Hiwonder-docs/ {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_ssl_server_name on;
}
```

保存后重载 Nginx。

## 验证

访问：

```text
https://wiki.hiwonder.com/
```

应跳转到：

```text
https://wiki.hiwonder.com/projects/Hiwonder-docs/en/latest/docs/index.html
```

页面样式和产品图片正常显示即部署成功。
