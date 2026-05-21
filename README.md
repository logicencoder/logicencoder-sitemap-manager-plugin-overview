# LogicEncoder Sitemap Manager — public overview

WordPress plugin that **generates, schedules, and operates** the XML sitemap for [logicencoder.com](https://logicencoder.com) — with blacklist controls, cron, search-engine ping, and an operator dashboard (not a generic SEO suite).

**Implementation (private):** [logicencoder-sitemap-manager-plugin](https://github.com/logicencoder/logicencoder-sitemap-manager-plugin)

---

## The problem

Search engines discover new LogicEncoder articles, tool pages, and category archives through sitemaps. Core WordPress sitemaps or third-party SEO bundles may:

- Include URLs the operator does not want indexed (staging paths, thin tags, duplicate archives).  
- Lack a **visible activity log** when auto-generation fails silently on cron.  
- Mix unrelated SEO features (schema, redirects) with sitemap maintenance, making incidents harder to debug.

This plugin does **one job well**: build `sitemap-generated.xml` at the site root from chosen post types and taxonomies, with explicit operator controls.

---

## Who benefits

| Audience | Benefit |
|----------|---------|
| **SEO visitor (indirect)** | Fresher `lastmod` on legitimate URLs |
| **Site operator** | Dashboard, blacklist, manual regenerate, ping Google/Bing |
| **Editor** | Automatic regen on publish when enabled |
| **Developer** | REST status/generate + WP-CLI for deploy scripts |

---

## How this fits the stack

| Piece | Role |
|-------|------|
| **logicencoder-sitemap-manager-plugin** | XML file generation + admin UI |
| **le-settings-plugin** | Meta titles/descriptions, security — **not** sitemap file replacement |
| **logicencoder-login-system-plugin** | Login gates — unrelated to URL discovery |

Submit `https://logicencoder.com/sitemap-generated.xml` in Search Console (operator task outside plugin).

---

## Capabilities (detailed)

### XML file at site root

**What:** Writes `sitemap-generated.xml` under WordPress `ABSPATH` following sitemaps.org urlset schema: `loc`, `lastmod`, `changefreq`, `priority`.

**Why:** Some hosts and CDNs expect a predictable filename; operator can open the file directly or via admin File Editor.

**Who benefits:** Operator verifying output; crawlers fetching a single stable URL.

### Homepage entry always included

**What:** Home URL gets priority `1.0` and `changefreq` daily.

**Why:** Root is the highest-value landing page for brand queries.

**Who benefits:** SEO for brand terms; correct relative weighting vs inner pages.

### Selectable post types and taxonomies

**What:** Settings choose which CPTs (default post + page) and taxonomies (default category) enter the sitemap.

**Why:** Custom types like `application` (shop) or internal types may need inclusion/exclusion per launch phase.

**Who benefits:** Operator launching new CPT without code change; avoids indexing junk types.

### Blacklist by post ID and URL

**What:** Blacklist admin adds post IDs or resolves a pasted URL to an ID; blacklisted posts skipped during generation. AJAX quick-add from content screens.

**Why:** Retired landing pages, test posts, or duplicate thin content should not reappear every nightly cron.

**Who benefits:** SEO hygiene; operator undoing mistaken publishes without deleting post.

### Optional author archives

**What:** Toggle `lsm_include_authors` to add author archive URLs with lower priority.

**Why:** Multi-author blogs benefit; single-brand sites may prefer omitting author thin pages.

**Who benefits:** SEO strategist; reduces clutter when disabled.

### Scheduled regeneration (WP-Cron)

**What:** Hook `lsm_cron_regenerate` on schedule stored in `lsm_cron_schedule` (default daily). `lsm_setup_cron` ensures event exists.

**Why:** Sites that publish frequently need sitemap `lastmod` updates without manual clicks.

**Who benefits:** Editor workflow — publish post, cron updates sitemap overnight (or on interval).

### Regenerate on publish

**What:** `save_post` hook `lsm_maybe_regenerate_on_publish` when auto-generate option enabled.

**Why:** Time-sensitive articles should ping crawlers with fresh `lastmod` soon after go-live.

**Who benefits:** News-style posts; operator not clicking “Generate” after every publish.

### Manual generate from dashboard

**What:** One-click generate on dashboard + AJAX `lsm_generate_sitemap` with admin nonce.

**Why:** Immediate validation after bulk import or blacklist change.

**Who benefits:** Operator during migration; developer after staging → prod sync.

### URL list and statistics pages

**What:** Admin views enumerate URLs from last build and show file size, count, last modified timestamp.

**Why:** “Search Console says URL not in sitemap” requires proving inclusion in last artifact.

**Who benefits:** SEO debugging; operator screenshots for contractors.

### Test & Verify screen

**What:** AJAX `lsm_test_url` fetches candidate URLs to confirm HTTP success before relying on sitemap entry.

**Why:** 404s in sitemap hurt crawl budget and trust.

**Who benefits:** Operator before large ping to search engines.

### Search engine ping

**What:** `lsm_ping_engines` notifies Google and Bing sitemap endpoints (standard ping URLs) after generation.

**Why:** Crawlers may not notice file mtime change instantly on high-traffic sites.

**Who benefits:** Operator after major site restructure; faster rediscovery.

### Activity log (last 100 events)

**What:** Option `lsm_activity_log` stores who regenerated, cron runs, blacklist edits, REST calls — with mysql timestamp.

**Why:** Silent cron failures are common on low-traffic sites (WP-Cron only runs on visits).

**Who benefits:** Operator answering “who regenerated sitemap Sunday?” without SSH.

### File editor with theme preference

**What:** Admin can view/edit `sitemap-generated.xml` inline with light/dark editor theme saved per user via AJAX.

**Why:** Emergency manual tweak (priority fix, one-off URL) without SFTP — use sparingly.

**Who benefits:** Operator incident response; developer quick patch.

### Settings export / import

**What:** AJAX JSON export/import of plugin options for staging → production parity.

**Why:** Blacklist and CPT selections should match across environments.

**Who benefits:** Deployer; reduces “prod sitemap missing application URLs” drift.

### Manage Content bulk tools

**What:** Content manager submenu with bulk actions (via `lsm_bulk_action` AJAX) aligned to inclusion workflow.

**Why:** Large sites need batch operations beyond single blacklist form.

**Who benefits:** Operator curating hundreds of posts after migration.

### REST API (authenticated admin)

**What:** `GET sitemap-manager/v1/status` returns stats, last generated time, next cron. `POST .../generate` rebuilds file.

**Why:** External monitoring or deploy script can verify sitemap freshness without browser login automation.

**Who benefits:** Developer wiring uptime checks; CI after deploy.

### WP-CLI commands

**What:** When WP-CLI available: `wp sitemap generate`, `wp sitemap stats`.

**Why:** SSH operators on Hostinger sometimes prefer CLI over clicking admin.

**Who benefits:** Developer; cron on real system cron calling WP-CLI.

### Dashboard widget

**What:** wp-admin home widget with last run summary and link to regenerate.

**Why:** Passive reminder if sitemap is weeks old.

**Who benefits:** Operator routine login.

---

## Output file contract

| Field | Typical value |
|-------|----------------|
| Path | `/sitemap-generated.xml` (site root) |
| Format | XML urlset 0.9 |
| Default inner priority | 0.8 (configurable) |
| Default changefreq | weekly (configurable) |

Operator should reference this exact URL in `robots.txt` and Search Console — not assume `/wp-sitemap.xml` from core.

---

## What this overview does not include

- Plugin PHP or server filesystem permissions  
- Google Search Console account steps (operator documentation elsewhere)  
- Structured data / Open Graph from other plugins

---

## Operational tips

1. After first install, open dashboard → Generate → confirm file size &gt; 0.  
2. Add test posts to blacklist before enabling author archives if author pages are thin.  
3. If cron stale, trigger **Run cron now** or rely on publish hook; check Activity Log.  
4. Ping engines only after confirming Test & Verify passes for sample URLs.

---

## Comparison to “just use Yoast / RankMath”

LogicEncoder runs a **custom, auditable** sitemap path tied to LogicEncoder post-type strategy and blacklist rules — without dragging full SEO plugin upgrades into security-sensitive Hostinger deploys. Marketing documentation here describes behaviour operators actually see in the Sitemap Manager menu.

---

## Related repositories

See [REPOS.md](REPOS.md).

---

**Made by [logicencoder](https://github.com/logicencoder)**
