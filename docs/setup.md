# 宣传仓 + 专有源码仓 — 维护说明

## 目标与许可口径（强制）

**RELang OS 产品不是开源软件。** 宣传仓公开 ≠ 开源协议。

| 仓库 | 可见性 | 内容 | 许可 |
|------|--------|------|------|
| **RELangOS**（本仓） | Public | README、`docs/` 静态页 | 版权所有 · 见根目录 `LICENSE` |
| **ReLang**（或其它名） | Private | RVM、OS、framework、SDK | **专有 · 非开源** · 另行授权 |

勿在 README / Pages / Issues 模板中写「MIT / Apache / 欢迎 PR 源码」等易误解为开源的措辞。合作话术用「评估授权 / NDA / 私有只读」，不用「开源下载」。

GitHub **不能**只公开 README 而隐藏同仓代码；因此采用 **两仓分离**。

## 一次性：创建公开宣传仓

1. 在 GitHub 新建仓库 **`RELangOS`**（Public，不要勾选「Add README」若本地已有）。
2. 在本目录（宣传仓根）执行：

```sh
git init -b main
git add -A
git commit -m "docs: initial RELang OS public site"
git remote add origin git@github.com:ShujianLv/RELangOS.git
git push -u origin main
```

3. **启用 GitHub Pages**  
   Settings → Pages → Build and deployment → Source: **Deploy from a branch** → Branch: **main** → Folder: **/docs** → Save。  
   数分钟后访问：`https://shujianlv.github.io/RELangOS/`

4. 若使用 **Organization**：把 `docs/index.html` 与 README 里的 `ShujianLv` 换成组织名；Pages URL 为 `https://<org>.github.io/RELangOS/`。

## 一次性：源码仓保持私有

1. 产品源码仓（例如 `ShujianLv/ReLang`）→ Settings → **Private**。
2. 确认 CI、协作者、Fork 策略；未授权用户无法 clone。
3. 源码仓根目录 `LICENSE` 应写明 **专有 / All rights reserved**，不要误用 Apache-2.0 / MIT 冒充开源（若历史文件仍写 Apache，应改为与产品口径一致）。

## 可选：Organization 主页 README

在组织下创建 **`.github`** 仓库（Public），添加 `profile/README.md`，内容可简短指向本宣传仓与 Pages，并写明 **RELang OS 为专有软件**。  
个人账号则用 **`用户名/用户名`** 仓库的 `README.md` 作为 Profile README。

## 日常更新

- 产品叙事、路线图摘要、垂直场景：只改 **RELangOS** 的 README / `docs/index.html`。
- 实现与测试：只在 **私有源码仓** 提交；勿把源码 copy 进宣传仓。

## 链接一致性

修改 GitHub 用户名、组织名或仓库名后，请同步更新：

- `README.md` 中的 Pages URL
- `docs/index.html` 中的 Issues / 仓库链接
- `LICENSE` 中的仓库说明（如有）
