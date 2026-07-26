# Free AI Tokens, Credits and API Trials

这是 AI 免费额度清单的公开网站仓库。仓库维护结构化 JSON、HTML 模板、样式和构建脚本；最终 HTML 仅在 CI/CD 构建阶段生成，不提交到 Git。

## 本地构建

```bash
python3 -m unittest discover -s tests -v
python3 scripts/build.py --output dist
```

生成结果位于 `dist/`。中文页面展示全部条目，英文页面只展示 `international` 条目。

## 数据更新

`data/ai-free-quotas.json` 由 `yoyoworks/free_token_scripts` 中的维护 Skill 核验并通过 Pull Request 同步。页面标题、描述、SEO 主域名及其他配置位于 `src/seo.json`。

## SEO 与 GEO

构建脚本生成预渲染正文、canonical、hreflang、Open Graph、JSON-LD、`robots.txt`、`sitemap.xml`、`llms.txt` 和公开 JSON。页面不依赖浏览器 JavaScript 获取正文。

## 主域名与镜像域名

`src/seo.json` 中的 `canonical_site_url` 是唯一 SEO 主域名。主站和镜像站应部署同一份构建结果，站内导航使用相对路径，因此会留在用户当前访问的域名；canonical、hreflang、JSON-LD、sitemap 和 llms.txt 则始终指向主域名，避免重复内容分散权重。

普通 `SITE_URL` 环境变量不会改变 canonical。只有明确设置 `CANONICAL_SITE_URL` 才会临时覆盖主域名，例如验证尚未写入配置的新正式域名：

```bash
CANONICAL_SITE_URL=https://example.com/ python3 scripts/build.py --output dist
```

镜像构建不要设置 `CANONICAL_SITE_URL`。
