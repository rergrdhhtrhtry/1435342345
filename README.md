# GitHub Pages 部署说明（三项中国智造 · 像素交互版）

## 为什么之前 404

GitHub Pages **只认根目录（或你指定的文件夹）下的 `index.html`**。
直接丢一个 `三项中国智造_交互版.html` 进去，访问网站根目录时没有入口文件 → 直接 404。

次高概率原因：

1. **文件名含中文** → URL 百分号编码（`%E4%B8%89...`），手抄/转发链接极易断。
2. **只传了 HTML，没传 `assets/` 文件夹** → 页面能开但三张图全空白（区块本身就是空的）。
3. **Pages 来源没选对**：Settings → Pages → Source 必须选 `Deploy from a branch`，分支 `main`、文件夹 `/ (root)`。

本目录已经按"可直接当仓库根目录"的标准整理好了，**整个目录原样上传即可**。

## 目录结构（必须保持这个层级）

```
仓库根目录/
├── index.html          ← 入口，名字必须是这个
├── .nojekyll           ← 空文件，跳过 Jekyll 构建（省 1 分钟、避免静态资源被吞）
└── assets/
    ├── bci_implant.jpg
    ├── thermo_film.jpg
    └── laser_scene.jpg
```

> `assets/` 必须和 `index.html` **同级**。页面里写的是相对路径 `assets/xxx.jpg`，位置错了就全白。

## 上传步骤

1. 在 GitHub 新建仓库（Public，不要勾 README / .gitignore，保持空仓库）
2. 进仓库页 → **Add file → Upload files**
3. 把这个目录里的 **`index.html`、`assets/` 文件夹、`.nojekyll`** 全部拖进去
   （GitHub 网页上传不能拖空文件，`.nojekyll` 如果拖不上去：**先 Commit changes，再在仓库里 Add file → Create new file，文件名填 `.nojekyll` 直接 Commit**，内容为空即可）
4. 点 **Commit changes**
5. **Settings → Pages → Build and deployment**
   - Source：`Deploy from a branch`
   - Branch：`main` / 文件夹：`/ (root)`
   - Save
6. 等 1–2 分钟（首次最长 10 分钟），访问：
   `https://<你的用户名>.github.io/<仓库名>/`

## 排错清单

| 现象 | 原因 | 处理 |
|---|---|---|
| 打根路径就 404 | 没有 `index.html` | 必须叫 `index.html` |
| 页面开出来是 GitHub 404 页 | 仓库 private / Pages 没开 | Settings → Pages 确认已启用，且仓库为 Public |
| 页面开了但图片全无 | `assets/` 没传或层级错 | `assets/` 与 `index.html` 同级 |
| 改了代码刷不出来 | 浏览器缓存 | **Ctrl + F5** 强制刷新 |
| 本地双击能开、上传后不行 | 本地走的是 `file://`，相对路径侥幸生效 | 以上传后为准，照本目录结构传 |

## 本地自测（可选）

在本目录打开终端：

```bash
python -m http.server 8000
```

浏览器访问 `http://127.0.0.1:8000/` —— 能正常显示三关图片，说明上传结构没问题。
