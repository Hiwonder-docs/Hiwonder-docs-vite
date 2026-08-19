# Hiwonder Docs 部署说明

适用于仓库 `Hiwonder-docs-vite`，线上入口固定为：

```text
https://wiki.hiwonder.com/
```

本仓库按根域名构建，VitePress `base` 为 `/`。构建产物仍暂存在：

```text
projects/Hiwonder-docs/en/latest/
```

Nginx 负责把 `wiki.hiwonder.com` 的根路径内部反代到这份产物，浏览器地址栏保持短链接。

## 本地构建

```bash
npm ci
npm run docs:build
npm run docs:stage-main
```

构建完成后确认：

- `projects/Hiwonder-docs/en/latest/index.html` 存在
- HTML 中资源路径为 `/assets/...`
- 侧边栏链接为 `/#raspberry-pi` 这类根路径锚点

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

## 宝塔 Nginx 配置

在 `wiki.hiwonder.com` 站点配置文件的 `server {}` 内添加或替换为下面这组规则。

```nginx
# Hiwonder Docs portal homepage
location = / {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/index.html;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_ssl_server_name on;
    add_header Cache-Control "no-cache";
}

location = /index.html {
    return 301 /;
}

location = /docs/index.html {
    return 301 /;
}

location = /docs/ {
    return 301 /;
}

# Hiwonder Docs portal assets
location ^~ /assets/ {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/assets/;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_ssl_server_name on;
}

location = /vp-icons.css {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/vp-icons.css;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_ssl_server_name on;
}

location = /favicon.ico {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/favicon.ico;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_ssl_server_name on;
}

location = /e-logo.png {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/e-logo.png;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_ssl_server_name on;
}

location = /hashmap.json {
    proxy_pass https://hiwonder-docs.github.io/Hiwonder-docs-vite/projects/Hiwonder-docs/en/latest/hashmap.json;
    proxy_set_header Host hiwonder-docs.github.io;
    proxy_ssl_server_name on;
}

# Old long URL compatibility
location = /projects/Hiwonder-docs/en/latest/docs/index.html {
    return 301 /;
}

location = /projects/Hiwonder-docs/en/latest/ {
    return 301 /;
}
```

保存后执行：

```bash
nginx -t
/etc/init.d/nginx reload
```

## 验证

访问：

```text
https://wiki.hiwonder.com/
```

应直接显示文档门户首页。点击侧边栏后，地址应变成类似：

```text
https://wiki.hiwonder.com/#raspberry-pi
```
