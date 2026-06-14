# Logic Encoder Sitemap Manager

Operator-controlled **XML sitemap generation** for logicencoder.com — content selection, URL blacklist, scheduled regeneration, Search Console verification helpers, and settings import/export.

Private plugin: [logicencoder/logicencoder-sitemap-manager-plugin](https://github.com/logicencoder/logicencoder-sitemap-manager-plugin).

## Public output

**[logicencoder.com/sitemap-generated.xml](https://logicencoder.com/sitemap-generated.xml)**

Referenced from robots.txt and search tools. No visitor-facing UI — the product is reliable, operator-controlled sitemap XML.

## Admin workspace

**Sitemap Manager** top-level menu:

| Screen | Purpose |
|--------|---------|
| Dashboard | Last run, URL counts, quick actions |
| Manage Content | Include/exclude post types and taxonomies |
| URL List | Flat view of emitted URLs |
| Blacklist | Block paths from ever appearing |
| File Editor | Syntax-highlighted XML preview |
| Settings | Change frequency, priority defaults |
| Statistics | Historical generation stats |
| Test & Verify | Ping Google/Bing, validate XML |
| Activity Log | Audit trail of operator actions |

Dashboard widget for one-click regenerate.

## Automation

- Optional regenerate on **`save_post`**
- WP-Cron scheduled rebuild (`lsm_cron_regenerate`)
- **REST** `sitemap-manager/v1/status` and `/generate`
- **WP-CLI:** `wp sitemap generate`, `wp sitemap stats`

## AJAX actions

Generate sitemap, test individual URLs, save editor theme preference, search posts for inclusion, blacklist management, trigger cron manually, import/export settings JSON, bulk content actions, ping search engines.

Complements [le-settings-plugin](https://github.com/logicencoder/le-settings-plugin-overview) SEO meta — this plugin owns the XML artifact; settings plugin owns robots and meta descriptions.

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
