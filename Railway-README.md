# ConvertX 部署到 Railway 指南

## 前提条件

- GitHub 账号
- Cloudflare 管理的域名（可选）

---

## 第一步：Fork 项目

访问 [https://github.com/C4illin/ConvertX](https://github.com/C4illin/ConvertX)，点右上角 **Fork**，fork 到自己的 GitHub 账号下。

---

## 第二步：注册 Railway

访问 [https://railway.app](https://railway.app)，用 GitHub 账号登录注册。

---

## 第三步：创建项目并部署

1. 点击 **New Project**
2. 选择 **Deploy from GitHub repo**
3. 选择你 fork 的 `ConvertX` 仓库
4. Railway 会自动识别 Dockerfile 并开始构建（首次构建需要 5~15 分钟）

---

## 第四步：配置环境变量

部署过程中或部署完成后，点击服务卡片 → 顶部 Tab 选 **Variables** → 添加以下变量：

| 变量名 | 值 | 说明 |
|--------|-----|------|
| `JWT_SECRET` | `Kx9#mP2$vL7nQ4wR8jT6yF3hD5bN1cG0` | 换成你自己的随机字符串 |
| `NODE_ENV` | `production` | 生产环境 |
| `HTTP_ALLOWED` | `false` | 强制 HTTPS |
| `ACCOUNT_REGISTRATION` | `false` | 禁止注册新账号 |
| `AUTO_DELETE_EVERY_N_HOURS` | `24` | 每24小时自动删除文件 |
| `ALLOW_UNAUTHENTICATED` | `true` | 免登录使用（可选） |

添加完成后会自动触发重新部署。

---

## 第五步：生成访问域名

1. 点击服务卡片 → **Settings** → **Networking**
2. 点击 **Generate Domain**
3. 端口填 **8080**
4. 点击 **Generate Domain** 生成访问地址

生成后即可通过 `xxx.up.railway.app` 访问服务。

---

## 第六步：首次使用

访问生成的域名，会自动跳转到 `/setup` 页面，填写邮箱和密码创建管理员账号。

> 如果设置了 `ALLOW_UNAUTHENTICATED=true`，之后访问无需登录。

---

## 第七步：绑定自定义域名（可选）

### Railway 这边

**Settings** → **Networking** → **Custom Domain** → 输入你的子域名（如 `convert.你的域名.com`）→ Railway 会给出一个 CNAME 地址。

### Cloudflare 这边

登录 Cloudflare → 选择你的域名 → **DNS** → 添加记录：

| 类型 | 名称 | 内容 |
|------|------|------|
| CNAME | `convert` | Railway 给出的 CNAME 地址 |

> 云朵代理状态开启或关闭均可正常访问。

等待几分钟 DNS 生效后，即可通过自定义域名访问。

---

## 费用说明

Railway 提供 **$5 一次性免费额度**（Trial 计划）。

ConvertX 实测消耗极低，预计每月约 **$0.02**，$5 额度可使用数年。

---

## 常见问题

**Q：为什么每次改环境变量部署很慢？**

A：Railway 每次改动都会重新构建 Docker 镜像，ConvertX 需要安装大量工具（ffmpeg、ImageMagick 等），正常需要 5~15 分钟。

**Q：数据会丢失吗？**

A：Railway 免费计划不支持持久化存储卷，重启后数据库会重置。如需持久化，需升级到付费计划后添加 Volume（挂载路径 `/app/data`）。

**Q：访问很慢怎么办？**

A：Railway 免费服务器在美国，国内访问有延迟。可以在 Cloudflare 开启橙色云朵代理来改善访问速度。
