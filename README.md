# Logic Encoder Sitemap Manager — WordPress plugin

**Logic Encoder Sitemap Manager** is operator-controlled XML sitemap generation for [logicencoder.com](https://logicencoder.com): pick which post types and taxonomies ship in the index, blacklist sensitive URLs, schedule automatic rebuilds, audit every emitted link, and edit `robots.txt` beside the live sitemap file.

The public artifact is **[logicencoder.com/sitemap-generated.xml](https://logicencoder.com/sitemap-generated.xml)** — submit that URL in Google Search Console and Bing Webmaster Tools. No visitor-facing UI; the product is reliable sitemap XML under your control.

- **Dashboard** — URL totals, last run, next scheduled rebuild, one-click **Generate Now**.
- **Manage Content** — checkbox grids for post types, taxonomies, and author archives.
- **URL List** — searchable flat table of every URL in the current file.
- **Blacklist** — exclude posts by ID, URL, or live search.
- **File Editor** — edit `robots.txt`, `.htaccess`, and the generated XML with automatic backup before save.
- **Settings** — auto-generate on/off, hourly-to-weekly schedule, default priority and change frequency.
- **Statistics** — breakdown by URL type and recent activity snapshot.
- **Test & Verify** — HTTP check and XML validation on the live sitemap URL.
- **Activity Log** — who generated, blacklisted, or edited files.

## Tech stack

| Layer | Technologies |
|-------|--------------|
| WordPress plugin | PHP single-file (`logicencoder-sitemap-manager.php`, v2.0.x) |
| Persistence | WordPress options |
| Output | `sitemap-generated.xml` at site root |
| Admin UI | Nine wp-admin screens + dashboard widget |
| Scheduling | Optional automatic regeneration on a chosen interval |
| Hosting | WordPress on shared hosting; administrator-only menus |

## Dashboard

**Sitemap Manager → Dashboard** is the home screen after activation.

Stat cards show **total URLs**, how many **content types** are enabled, **blacklisted** item count, and **time since last generation**. A cron table lists whether auto-generate is on, the schedule label, and the **next run** datetime. When the file exists, a info block shows the public URL, file size, and last modified time.

| Button | Result |
|--------|--------|
| **Generate Now** | Rebuilds the XML immediately (confirm dialog) |
| **View Sitemap** | Opens the live file in a new tab |
| **Test** | Jumps to Test & Verify |
| **View URLs** | Opens URL List |
| **Configure Cron** | Opens Settings |

On first activation the plugin builds an initial sitemap and links you back here.

On first publish to Search Console: activate → open Dashboard → **Generate Now** → **View Sitemap** → copy the URL into Google Search Console → add the same line to `robots.txt` in File Editor.

## Manage Content

**Manage Content** is where you choose what enters the next build.

Two checkbox grids list every **public post type** and **public taxonomy** (categories, tags, custom). Toggle **Include author archive pages** for `/author/` URLs. **Save Settings** stores choices — run **Generate Now** on the Dashboard (or wait for auto-generate) to refresh the file.

Defaults on a fresh install: **Posts** and **Pages** on; **Categories** on; author archives off.

To add a custom post type, tick the CPT label → **Save Settings** → Dashboard → **Generate Now** → URL List → filter by type to confirm new archive URLs appeared.

## URL List

**URL List** is the audit view of the current sitemap.

When no file exists yet, an empty state links to Dashboard. After generation, a filter bar and table list every URL with **type badge**, **priority**, **last modified**, and a **View** link that opens the live page. Header shows total count.

| Control | Use |
|---------|-----|
| **Filter by type** | Homepage, Posts, Pages, Categories, Tags, Authors, or All |
| **Search URLs** | Live text filter on the URL column |
| **Print** | Browser print of the filtered table |

To confirm a landing page is indexed, search the slug → open **View** → if missing, check Manage Content and Blacklist, then regenerate.

## Blacklist

**Blacklist** keeps specific posts out of the XML even when their post type is enabled.

Add by **numeric post ID**, by **pasting a full post URL**, or via **Search & Add** (type two or more characters, pick **Blacklist** on a result). The table shows ID, title, type, link, and **Remove**. Blacklisted items stay out until removed and the sitemap is regenerated.

To hide a staging page, open Blacklist → search the page title → **Blacklist** → Dashboard → **Generate Now** → URL List search proves the URL is gone.

## File Editor

**File Editor** edits site-root files in one place:

| Tab | File |
|-----|------|
| **robots.txt** | Crawler rules — add `Sitemap: https://logicencoder.com/sitemap-generated.xml` |
| **.htaccess** | Server rules (edit with care) |
| **sitemap-generated.xml** | Raw XML preview/edit |

Switch tabs, edit the textarea, toggle **Dark Mode** (preference remembered), **Save File** writes to disk — a timestamped backup copy is created beside the original before overwrite.

To wire `robots.txt` to the sitemap, open File Editor → `robots.txt` → add the `Sitemap:` line pointing at `sitemap-generated.xml` → **Save File** → Test & Verify → fetch `robots.txt`.

## Settings

**Settings** controls automatic rebuilds and default entry metadata.

| Field | Options |
|-------|---------|
| **Enable automatic sitemap generation** | On / off |
| **Regeneration schedule** | Hourly, Twice Daily, Daily, Weekly |
| **Default priority** | 0.0–1.0 for posts and pages (homepage always highest in output) |
| **Default change frequency** | Always, Hourly, Daily, Weekly, Monthly, Yearly, Never |

**Save Settings** applies the schedule and shows the **next scheduled run** when auto-generate is on. With auto-generate enabled, publishing a post or page of an enabled type also triggers a rebuild.

For daily freshness, enable auto-generate → **Daily** → **Save** → publish a new blog post → Dashboard shows an updated “last generated” time.

## Statistics

**Statistics** summarizes the current file: total URLs, file size, time since update, and a **URL Breakdown** table (homepage, post, page, category, tag, author, other) with counts, percentages, and progress bars. **Recent Activity** lists the last ten log entries. Empty state links to Dashboard to generate first.

## Test and verify

**Test & Verify** checks what crawlers will fetch.

Enter any URL (defaults to the main sitemap) and **Test URL** — result panel shows HTTP status, content-type, and robots header. **Quick Tests** rows link the main sitemap, `robots.txt`, and legacy `sitemap.xml` for side-by-side checks. **Sitemap Validation** reports valid/invalid XML, URL count, or parse errors. **System Information** lists WordPress version, PHP version, site URL, and whether the site root is writable.

To fix a 404 reported in Search Console, paste the failing URL → **Test URL** → if 404, fix the post or blacklist → regenerate → retest.

## Activity log

**Activity Log** keeps the last **100** operator events: time, user, action badge, and details. Generations, blacklist edits, settings saves, file edits, and log clears appear here. **Clear Log** wipes the table (the clear action itself is logged).

## What goes into the sitemap

Each generation writes a single XML urlset at the site root:

| Content | Typical treatment |
|---------|-------------------|
| **Homepage** | Highest priority, marked updated daily |
| **Published posts/pages** of enabled types | Your default priority and change frequency from Settings |
| **Taxonomy archives** you enabled | Lower priority, weekly change frequency |
| **Author archives** (when enabled) | Lowest priority, monthly change frequency |
| **Blacklisted posts** | Omitted entirely |

Manual **Generate Now** always available. Automatic runs follow the schedule you pick. Deactivating the plugin stops scheduled rebuilds.

## WordPress dashboard widget

The home **Dashboard** widget **Sitemap Manager Status** shows URL count, last generated time, next scheduled run, and links **View Dashboard** / **Generate now** — regenerate without opening the full menu.

Private code: [logicencoder/logicencoder-sitemap-manager-plugin](https://github.com/logicencoder/logicencoder-sitemap-manager-plugin) (v2.0.x)

Complements [le-settings-plugin-overview](https://github.com/logicencoder/le-settings-plugin-overview) for site meta and robots allow rules.

See [REPOS.md](REPOS.md).

---

**Made by [Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
