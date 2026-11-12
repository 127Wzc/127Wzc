# 部署清单 · 127Wzc 的 GitHub 主页

> 配色：**小埋橙** `#FF8F72` / `#F5896E` / 深棕 `#885455` / 奶白 `#FFF6F3` / 描边 `#FFD9CB`
> 头像：已移除，README 里不再放装饰图（GitHub 左侧头像是账号头像，去 Settings 换）
> 语言：README 全英文；技术栈用 for-the-badge 品牌色，按分组排列

---

## 0. 文件结构

```
127Wzc/
├── README.md                       # 主页本体
├── SETUP.md                        # 本文
├── assets/                         # 目前为空（metrics.svg 生成后会落在这里）
└── .github/workflows/
    ├── snake.yml                   # 贪吃蛇（推上去自动跑）
    ├── waka.yml                    # WakaTime 统计（每天更新）
    └── metrics.yml                 # 年度热力图 / 成就（需要 PAT）
```

---

## 1. 先改 3 个占位符

| 位置 | 改什么 |
| :-- | :-- |
| `README.md` → About | `your@email.com` → 你的邮箱 |
| `README.md` → Tech Arsenal | 每个 `<h4>` 分组下 4–6 个核心 badge，其余塞进 `<details><summary><sub>more</sub></summary>…</details>`。格式：`https://img.shields.io/badge/名称-色号?style=for-the-badge&logo=图标名&logoColor=white` |
| `README.md` → Featured Projects | 四个项目卡片：标题链接、英文描述、`<code>` 技术栈 |

> 展示名是 **cc**（`<h1>`、typing SVG、About、卡片 `custom_title`），
> 但所有 URL 里的 `127Wzc` 是 GitHub 登录名，**不能改**，否则接口全挂。

**徽章两个坑：**
1. 标签里的连字符要写成 `--`，否则会被 shields 当成「标签-消息」分隔符
   （`MyBatis-Plus` → `MyBatis--Plus`，`shadcn-vue` → `shadcn--vue`）。
2. `logo=` 填 simpleicons 的 slug；填错不会报错，只是图标静默消失。
   已确认失效的：`openai`、`tencentqq`、`css3`（改用 `qq`、`css`，OpenAI 直接不挂图标）。

---

## 2. 加 Secrets（Settings → Secrets and variables → Actions）

| Secret 名 | 用途 | 怎么拿 |
| :-- | :-- | :-- |
| `GH_TOKEN` | waka 脚本要往 README 写数据 | 头像 → Settings → Developer settings → **Personal access tokens (classic)** → 勾 `repo` + `user` |
| `WAKATIME_API_KEY` | 拉 WakaTime 数据 | [wakatime.com/settings/api-key](https://wakatime.com/settings/api-key) |
| `METRICS_TOKEN` | metrics.yml 用（可选） | 同上，再生成一个 classic PAT，勾 `repo` + `read:user` |

`GITHUB_TOKEN` 是内置的，snake.yml 不用额外配。

---

## 3. 装 WakaTime（不装这个，waka 那块是空的）

1. 注册 [wakatime.com](https://wakatime.com)，复制 API Key。
2. VS Code / JetBrains / Neovim 装 WakaTime 插件，粘 Key。
3. 写代码就会自动计时，第二天 README 里出现「我是夜猫」「本周消耗时间」那些中文统计。

`waka.yml` 里已经设好：`LOCALE: zh`、`SHOW_LOC_CHART`、`SHOW_DAYS_OF_WEEK`、进度条配色 `#FF8F72`。

> `SHOW_AI_CODING` / `SHOW_AI_CODE_TIME` 已移除：它们依赖 WakaTime 的 AI 追踪（要在 Claude Code / Cursor 里另装 WakaTime 集成），没数据时会在 README 上留一行「No AI Coding Activity Tracked This Week」。等真配好了再加回来。

---

## 4. 让数据卡片永久可用（推荐，3 分钟）

公共实例 `github-readme-stats.vercel.app` 经常 503，README 里暂时用了镜像 `gh-readme-stats.vercel.app`。想要永久稳定就自己部署一份：

1. Fork [anuraghazra/github-readme-stats](https://github.com/anuraghazra/github-readme-stats)。
2. Vercel → New Project → 选这个 fork → 部署。
3. 项目 Settings → Environment Variables 加 `PAT_1` = 你的 classic PAT（勾 `repo`）。
4. 拿到域名后，把 README 里所有 `gh-readme-stats.vercel.app` 换成 `<你的项目>.vercel.app`。

> 备用镜像（挂了就换）：`github-readme-stats-nu-six.vercel.app`、`github-readme-stats-sigma-five.vercel.app`（后者要配 `PAT_1`）

---

## 5. 推上去之后

```bash
git add .
git commit -m "feat: umaru-orange profile 🐻"
git push -u origin main
```

推送即触发 `snake.yml`（push 到 main 会自动跑），约 1 分钟后贪吃蛇出现。
**第一次推送后到 Actions 页面确认三个 workflow 都是绿色勾**，再手动点一次 `Waka Readme` 和 `Metrics`。

| 现象 | 原因 / 处理 |
| :-- | :-- |
| 贪吃蛇位置裂图 | `snake.yml` 还没跑完，或 `output` 分支还没生成。手动 Run workflow 一次 |
| stats 卡片显示 "Something went wrong" | 镜像挂了 → 换域名，或按第 4 步自建 |
| waka 区块空白 | 没配 `GH_TOKEN` / `WAKATIME_API_KEY`，或 WakaTime 还没数据 |
| 私有仓库贡献不显示 | PAT 没勾 `repo`，或没开 `count_private=true`（已开） |
| metrics.svg 404 | `METRICS_TOKEN` 没配；配好后去掉 README 里那段注释 |
| 中文显示成方框 | 换 `LOCALE: zh` 的版本（已是）；少数卡片本身不支持中文，属正常 |

---

## 6. 想再加东西（都按橙色系）

| 想要 | 项目 | 关键参数 |
| :-- | :-- | :-- |
| 技术栈图标 | `skillicons.dev` | `?i=ts,js,py&theme=light&perline=8` |
| 访客计数 | `komarev/ghpvc` | `&color=FF8F72&style=flat-square` |
| 打字机 | `readme-typing-svg` | `&color=F5896E&width=820` |
| 连续贡献 | `streak-stats.demolab.com` | `&ring=FF8F72&fire=F5896E&background=FFF6F3` |
| 贡献总览 | `github-profile-summary-cards` | `&chart_color=FF8F72`（这家的图表色可以改） |
| 年度热力图 | `lowlighter/metrics` | `plugin_isocalendar: yes` |
| 贪吃蛇 | `Platane/snk` | `color_snake=#F5896E` |
| AI Token | `tokens.ci` | 登录后直接给一段 markdown |
| 博客自动更新 | `gautamkrishnar/blog-post-workflow` | 有 RSS 就能接 |

---

## 7. 主题速查

```
主橙    #FF8F72   深橙    #F5896E   浅橙    #FFA184
描边    #FFD9CB   奶白    #FFF6F3   深棕    #885455
```

---

## 8. 本地预览 README

```bash
# 方式 A：仓库自带的预览页（GitHub 同款排版，已配好深浅色）
pip3 install markdown            # 只需一次
python3 .preview/build.py        # 生成 .preview/index.html
python3 -m http.server 8765      # 起服务
open http://127.0.0.1:8765/.preview/

# 想直接看深色：    http://127.0.0.1:8765/.preview/?theme=dark
# 想直接看浅色：    http://127.0.0.1:8765/.preview/?theme=light
```

`.preview/` 已写进 `.gitignore`，不会推到 GitHub。

```bash
# 方式 B：grip —— 走 GitHub 官方 Markdown API 渲染，最接近线上效果
pipx install grip
grip README.md -b 8765           # 打开 http://localhost:8765
```

```bash
# 方式 C：VS Code 里直接 Cmd+Shift+V（快，但表格/HTML 的还原度不如前两种）
```

> 预览里出现这两种「异常」是正常的：贪吃蛇裂图（`snake.yml` 还没跑）、
> tokens.ci 显示 `User @127Wzc was not found`（还没在 tokens.ci 登录绑定）。
