# WebNav Hub

一个纯静态的单页网址导航站：深色底、橙色高亮，卡片式布局，打开即用。

- **分类**：AI 搜索、托管社交、实用工具、科技资讯、云存储、电子邮箱，共 6 个分类
- **链接**：194 个站点，全部写在 `index.html` 里
- **交互**：顶部导航贴顶锚点定位，卡片网格响应式排列
- **图标**：通过 jsDelivr 引入 Font Awesome，配置了 SRI 与 `no-referrer`
- **无障碍**：提供跳转到主内容链接、语义化标签、`aria-hidden` 与焦点样式
- **技术形态**：单个 `index.html` + `favicon.svg`，无框架、无构建步骤、无后端与数据库

仓库地址：https://github.com/bbylw/infrlop

## 文件结构

```text
index.html        站点主体，所有分类与链接都在这里
favicon.svg       站点图标
requirements.txt  仅用于让 Infrlo 识别 Python 运行时，无任何依赖
```

## 部署到 Infrlo

Infrlo 是一个从 GitHub 仓库直接构建并托管应用的平台。本站按下面的步骤部署。

### 1. 准备 GitHub 仓库

本仓库已经是公开仓库，分支为 `main`，包含上面三个文件即可。

### 2. 连接 GitHub

1. 打开 https://dash.infrlo.com ，用 Google 账号登录。
2. 按提示安装 Infrlo 的 GitHub App，并授权 `bbylw/infrlop` 这个仓库。
3. 新建应用（Deploy），选择仓库 `bbylw/infrlop`，分支 `main`。

### 3. 填写构建与运行命令

在配置页面把两项改成：

**Build command**

```bash
true
```

**Run command**

```bash
python -m http.server 5000 --bind 0.0.0.0
```

### 4. 部署并访问

点击 Deploy，等待构建日志出现 `Deployment Successful`。之后用 Infrlo 分配给你的 `https://<应用名>.infrlo.com` 地址访问即可。

部署过程中旧版本会继续对外服务，新版本构建成功后才替换。

## 为什么这样配置

- `requirements.txt` 里只有注释，没有 Python 依赖。如果用 Infrlo 默认的 `pip install -r requirements.txt`，pip 会因为 `site-packages` 不可写而退化到用户目录，接着报 `Failed to save build`。本站不需要任何依赖，用 `true` 直接跳过最干净。
- Infrlo 的网关固定转发到容器内的 **5000** 端口，并且不注入 `PORT` 环境变量。实测 8000、8080 和 `$PORT` 都会返回 `Bad Gateway`：日志里虽然显示 `Application started` / `Deployment Successful`（只表示进程启动了），但网关连不上业务端口。因此端口必须写死为 5000。

## 更新站点

修改 `index.html` 后推送到 `main`，Infrlo 会自动重新构建并发布。新增或修改链接不需要碰其他文件。
