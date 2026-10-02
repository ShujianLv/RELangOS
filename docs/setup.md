# 宣传仓 + 私有源码仓 — 维护说明

## 目标

| 仓库 | 可见性 | 内容 |
|------|--------|------|
| **relang-os-public**（本仓） | Public | README、`docs/` 静态页、合作说明 |
| **ReLang**（或其它名） | Private | RVM、OS、framework、SDK 全量源码 |

GitHub **不能**只公开 README 而隐藏同仓代码；因此采用 **两仓分离**。

## 一次性：创建公开宣传仓

1. 在 GitHub 新建仓库 **`relang-os-public`**（Public，不要勾选「Add README」若本地已有）。
2. 在本目录（宣传仓根）执行：

```sh
git init -b main
git add -A
git commit -m "docs: initial RELang OS public site"
git remote add origin git@github.com:ShujianLv/relang-os-public.git
git push -u origin main
```

3. **启用 GitHub Pages**  
   Settings → Pages → Build and deployment → Source: **Deploy from a branch** → Branch: **main** → Folder: **/docs** → Save。  
   数分钟后访问：`https://shujianlv.github.io/relang-os-public/`

4. 若使用 **Organization**：把 `docs/index.html` 与 README 里的 `ShujianLv` 换成组织名；Pages URL 为 `https://<org>.github.io/relang-os-public/`。

## 一次性：源码仓改为私有

1. 打开私有源码仓（例如 `ShujianLv/ReLang`）→ Settings → General → Danger Zone → **Change repository visibility** → **Private**。
2. 确认 CI、协作者、Fork 策略；私有后未授权用户无法 clone。

## 可选：Organization 主页 README

在组织下创建 **`.github`** 仓库（Public），添加 `profile/README.md`，内容可简短指向本宣传仓与 Pages 链接。  
个人账号则用 **`用户名/用户名`** 仓库的 `README.md` 作为 Profile README。

## 日常更新

- 产品叙事、路线图摘要、垂直场景：只改 **relang-os-public** 的 README / `docs/index.html`。
- 实现与测试：只在 **私有源码仓** 提交；勿把大段源码 copy 进宣传仓。

## 链接一致性

修改 GitHub 用户名、组织名或仓库名后，请同步更新：

- `README.md` 中的 Pages URL
- `docs/index.html` 中的 Issues / 仓库链接
