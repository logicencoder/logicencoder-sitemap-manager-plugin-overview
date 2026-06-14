# Logic Encoder Sitemap Manager

Operator-controlled **XML sitemap generation** for logicencoder.com — blacklist unwanted URLs, schedule regeneration, verify in Search Console, and export/import settings.

Private plugin: [logicencoder/logicencoder-sitemap-manager-plugin](https://github.com/logicencoder/logicencoder-sitemap-manager-plugin).

## Public output

**[logicencoder.com/sitemap-generated.xml](https://logicencoder.com/sitemap-generated.xml)** — referenced from robots.txt and search tools.

No visitor UI — the product is reliable sitemap XML under operator control.

## Admin workspace

**Sitemap Manager** menu: Dashboard, Manage Content, URL List, Blacklist, File Editor, Settings, Statistics, Test & Verify, Activity Log. Dashboard widget for quick status.

## Automation

- Regenerate on `save_post` when configured
- WP-Cron scheduled rebuild
- REST `sitemap-manager/v1/status` and `/generate`
- WP-CLI: `wp sitemap generate`, `wp sitemap stats`
- AJAX: test URLs, bulk actions, ping search engines, import/export settings

Complements [le-settings-plugin](https://github.com/logicencoder/le-settings-plugin-overview) SEO meta — does not replace it.

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
