# 导航站 — Cloudflare Pages 部署说明（A 方案）

零依赖纯静态导航页。**数据独立在 `sites.json`，改链接只动这一个文件，git push 后自动更新。**

## 文件说明

| 文件           | 作用                        |
| ------------ | ------------------------- |
| `index.html` | 页面结构 + 样式 + 搜索逻辑（**不用改**） |
| `sites.json` | 站点数据（**改链接只改这个**）         |
| `_headers`   | 缓存/安全响应头（不用改）             |

## 怎么加/改链接

打开 `sites.json`，按下面格式增删条目。每个分类是一个对象，站点是 `items` 里的数组：

```json
{
  "name": "分类名",
  "items": [
    { "name": "站点名", "desc": "一句话描述", "url": "https://...", "color": "#2f6fed", "icon": "字" }
  ]
}
```

字段说明：

- `name`：显示名字
- `desc`：卡片小字描述
- `url`：跳转地址（带 `https://`）
- `color`：图标底色（可省略，自动分配）
- `icon`：图标里的字（一个汉字或字母，如 "G"、"棋"）

## 部署（Git 方式，推荐，改数据最方便）

### 1. 本地建 git 仓库并推送到 GitHub

```bash
cd 导航站
git init
git add -A
git commit -m "导航站初版"
# 在 GitHub 新建一个仓库（如 nav），然后：
git remote add origin https://github.com/你的用户名/nav.git
git push -u origin main
```

### 2. Cloudflare Pages 连接仓库

1. 登录 <https://dash.cloudflare.com>
2. **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
3. 选 GitHub 授权 → 选 `nav` 仓库 → **Begin setup**
4. 构建命令留空、输出目录留空 → **Save and Deploy**
5. 得到地址 `https://nav.xxx.pages.dev`

### 3. 以后改链接

```bash
# 改完 sites.json 后
git add sites.json
git commit -m "更新导航"
git push
```

Cloudflare 自动检测到 push，几秒后重新部署完成，无需任何手动操作。

## 部署（拖拽方式，无 git）

1. Pages → **Create** → **Upload assets**
2. 把 `index.html`、`sites.json`、`_headers` 三个文件一起拖进去 → **Deploy**
3. 更新时重新拖拽即可（但每次都要手动传，不如 git 方便）

## 绑定自己的域名（可选）

Pages 项目 → **Custom domains** → 输入域名 → 按提示在 DNS 加 CNAME 记录。

---

> 提示：「我的项目 → 五子棋」的 `url` 目前是占位符 `https://你的域名/game`，确定域名后改 `sites.json` 里那行即可。
