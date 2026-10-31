# SeeEnglish 静态官网页

这是 SeeEnglish 的独立静态官网版本，位于项目的 `site/` 目录。

它和本地应用完全隔离：

- 没有后端
- 没有 API
- 没有构建步骤
- 没有外部字体或脚本请求
- 不需要数据库
- 可以直接上传到任何静态服务器

## 页面内容

页面是一条连续的产品叙事，底部 Dock 分为四个章节：

1. **首页**：一本不会催你的英语笔记。没有每日任务、没有标准答案、只有可见的变化。
2. **体验**：两个可操作的演示。
   - 记词：词形匹配、补充 / 新建、例句、关联词和联想。
   - 复习：先回想、再翻开、自己判断；每 5 次结算一次，展示全局总分和档位。
3. **功能**：四个核心动作的详细说明。
   - 记词：词形自动匹配，笔记没有固定格式。
   - 复习：加权随机，5 次一档，一个总分。
   - 词库：筛选、修改、归档、删除、JSON 导出。
   - 个人：头像、名字、签名、30 天趋势和外观偏好。
   - 最后是本地优先说明。
4. **源码**：GitHub 直达地址和产品收束。

## 本地预览

最简单的方式：直接双击 `index.html`。

也可以在本目录运行：

```powershell
python -m http.server 8899
```

然后访问 `http://127.0.0.1:8899`。

演示数据只保存在当前页面内存里，刷新即重置。

## 部署

把 `site/` **里面的所有文件**上传到 Web 根目录即可，例如：

```text
/var/www/seeenglish/
├─ index.html
├─ app.js
├─ review-demo.js
├─ base.css
├─ styles.css
├─ site-v2.css
├─ robots.txt
├─ .nojekyll
└─ assets/
```

Nginx 示例：

```nginx
server {
    listen 80;
    server_name YOUR_PUBLIC_IP;

    root /var/www/seeenglish;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /assets/ {
        add_header Cache-Control "public, max-age=31536000, immutable";
    }
}
```

这是一个静态单页站点，不需要 Node、Python 或反向代理。

## 文件说明

```text
site/
├─ index.html          # 页面结构、产品文案和所有章节
├─ styles.css          # 基础官网样式
├─ site-v2.css         # 2026 版新增章节、复习演示、个人中心和功能详情
├─ base.css            # 从应用 design system 复制的基础样式
├─ app.js              # 导航、滚动、记词演示、演示词库、复制地址
├─ review-demo.js      # 复习演示：回想、翻开、自评、结算
├─ robots.txt
├─ .nojekyll
└─ assets/
   ├─ favicon.svg
   └─ fonts/           # 本地打包字体，不依赖 Google Fonts
```

## 修改入口

- **首页与章节文案**：`index.html`
- **复习演示状态和分数**：`review-demo.js`
- **记词演示匹配规则**：`app.js` 里的 `lemmaCandidates`、`findMatches`
- **演示词库**：`app.js` 里的 `SAMPLE_ENTRIES`
- **颜色 / 字体 / 圆角**：`base.css` 的 `:root`
- **官网页专属布局**：`styles.css` 与 `site-v2.css`
- **GitHub 地址**：
  - `index.html` 里的 `https://github.com/Yu-Sea/SeeEnglish`
  - `app.js` 里 `const url = 'https://github.com/Yu-Sea/SeeEnglish'`

> 如果应用的 `web/src/styles.css` 后续改版，可以重新复制到 `base.css`，再检查 `styles.css` 里对应的覆盖样式。

## 设计说明

- UI 组件不使用渐变，渐变只出现在固定环境背景
- 液态玻璃由透明底色、细边框、模糊和阴影组成
- 桌面端与移动端共用同一套视觉语言
- 支持 `prefers-reduced-motion`
- 所有交互都在浏览器内完成，不上传任何输入
- 品牌 S 标记、字标和颜色也保留在项目根目录的 `assets/brand/`

## 源码地址

GitHub 仓库目前还没有正式建立，地址先按项目名保留：

https://github.com/Yu-Sea/SeeEnglish

正式仓库建立后，直接使用这个地址即可，不需要再改页面。
