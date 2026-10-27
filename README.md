# AppDeck 站点（支持页 + 隐私政策）

App Store Connect 要填的两个 URL 就来自这个目录。

| 文件 | 对应 ASC 字段 | 上线后的地址 |
|---|---|---|
| `index.html` | 支持 URL | `https://petterxf.github.io/appdeck-site/` |
| `privacy.html` | 隐私政策 URL | `https://petterxf.github.io/appdeck-site/privacy.html` |

**这个目录是唯一源文件。** `dist/store-assets/privacy-policy.html` 只是同一份内容的旧副本（供 UPLOAD-CHECKLIST 引用），改内容请改这里。

## 内容约定

- 不引用任何外部资源（字体、CDN、图片、统计脚本），全部内联 CSS —— 境内访问时少一层被墙的风险。
- 中英双语写在同一页，不依赖 JS 切换（审核员多数看英文，用户多数看中文）。
- 联系邮箱 `brandy11112@qq.com` 在两个页面都必须出现，且带 `mailto:`。

## 发布

```bash
bash Scripts/deploy_site.sh
```

脚本会：读钥匙串里的 GitHub 凭据 → 没有 `petterxf/appdeck-site` 就创建公开仓库 → 推送 → 开启 Pages → 轮询到上线并打印两个 URL。

### 手工发布（脚本跑不通时）

1. 在 GitHub 网页建一个 **public** 仓库 `appdeck-site`（不要初始化 README）。
2. `git clone` 它，把本目录的 `index.html`、`privacy.html` 拷进去，commit，push 到 `main`。
3. 仓库 Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`，目录 `/`。
4. 等 1–2 分钟，访问 `https://petterxf.github.io/appdeck-site/`。

## 镜像（国内可达）

GitHub Pages 本身合规，但 `*.github.io` 在中国大陆经常打不开（DNS 污染 / 限速）。中国区审核员若在境内打开失败，会走 2.1 Information Needed。

镜像要求：**国内能开 + 免备案 + 地址长期稳定**。目前满足这三条的是腾讯云 EdgeOne Pages（免费，默认域名走它自己的已备案域名，国内可访问；自定义域名才需要备案）。把这两个 html 拖上去即可，两个 URL 都换成镜像地址。

**注意**：备案域名 + 境内主机是唯一 100% 可控的方案；EdgeOne 的默认域名属于"可用但依赖第三方"。真要在国内长期做，还是走备案。

## 改动后必须复查

```bash
grep -n "example.com\|请替换" *.html   # 必须为空
grep -c "brandy11112@qq.com" *.html    # 两页都要有
```
