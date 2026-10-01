# CLAUDE.md

Guidance for Claude Code working in this repo.

## What this is

1000.world is Iris Alonzo's personal site — hand-written HTML, no build step,
no framework, no server, no database.

| Path | Role |
|---|---|
| `index.html` | The site. One page: intro, current projects, timeline, folded Past Work and Civics, contact. Inline `<style>` and `<script>`, no external files. |
| `img/` | Every picture and video the page uses, resized to 900px on the long side, JPEG quality 82. |
| `Iris2025.jpeg` | Social-preview image (`og:image`) and the 2025 timeline photo |
| `favicon.svg`, `favicon-32x32.png`, `apple-touch-icon.png` | The "i" from the iris.market wordmark, black on the site's yellow. The SVG is the source; the PNGs are rendered from it. |
| `1/` | The previous site: the venn diagram page and its PNG. Kept at 1000.world/1. |
| `2/index.html` | A redirect to the root. The new page lived at 1000.world/2 while it was built and links to it were shared. |
| `CNAME` | Contains `1000.world`. **Deleting this breaks the custom domain.** |

The venn page was imported from the old host 2026-09-17, byte-for-byte. The
rebuild went live at the root 2026-10-01.

### How the page works

- Sections are plain `<section>`s under `<main>`. The nav highlights whichever
  one is most in view.
- Past Work and Civics are `<section class="fold">` wrapping a `<details>`.
  The `<summary>` is the heading. A link into a fold (`#american-apparel`) or
  to it (`#past-work`) opens it first.
- Picture grids show two rows, then a "+N more" button. The script measures
  rows after layout, and again when a fold opens.
- The timeline (`.record`) opens each row's picture on hover, fixed in the
  empty half of the screen. On phones a "Show images" button shows them all.
- The iris.market video uses a `poster` taken from its first frame plus a
  Play button that hides while it plays. Regenerate the poster with
  `ffmpeg -i img/iris-market-sound.mp4 -frames:v 1 img/iris-market-poster.jpg`.

Hidden captions, credits and alt text carry names and dates. Never invent them;
ask Iris.

## Deploying

Push to `main`. GitHub Pages rebuilds and the change is live in about a minute.
There is nothing else — no deploy command, no CI, no preview environment.

Repo: `github.com/irissiri2/1000-world` (public, which free Pages requires).

**Verify the live URL after every push.** A merge is not a deploy.

## DNS — read before touching anything

The domain is managed at **Register.com**, NOT Namecheap. Hosting moved off
Namecheap shared hosting on 2026-09-17.

| Record | Value | Note |
|---|---|---|
| A `@` | 185.199.108–111.153 | GitHub Pages, four records |
| CNAME `www` | `irissiri2.github.io` | redirects to the apex |
| MX | `smtp.google.com` | **Never touch.** Runs iris@1000.world. |
| TXT `@` | `v=spf1 include:_spf.google.com ~all` | |
| TXT `_dmarc` | `v=DMARC1; p=quarantine` | **No `rua` on purpose.** Reporting was removed 2026-09-24 — Iris does not want the daily reports. Don't re-add it as if it were missing. |
| TXT `google._domainkey` | DKIM, 2048-bit, selector `google` | Authenticated in Google Admin 2026-09-19. The published key matches the one in Admin — **never click GENERATE NEW RECORD**, it mints a new key and invalidates this record. |
| CNAME `mail`, `autodiscover` | Register.com mail infra | leave alone |

There is no wildcard A record, deliberately. A new subdomain needs its own record.

Mail from Google passes SPF and DKIM. Big-company mail gateways (UMG's, for one)
rewrite messages in transit and break the DKIM signature — those DMARC failures are
normal and not worth chasing. `p=quarantine`, not `p=reject`: a mangled message
should land in spam, not vanish.

**Known quirk:** Register.com's two nameservers (dns101/dns102) served different
answers for hours after the 2026-09-17 edits while reporting the same zone
serial. If a record looks missing, query both servers before concluding anything
was saved wrong — and never re-save a record that the UI already shows correctly.

## Who you're working with

Iris is not a programmer. She reads, judges and approves; she does not write
code, and she works through Claude Code in Ghostty. She is sharp about
inconsistency and will catch a thing that looks built but isn't, so surface
those first.

This changes how to write, not what to build:

- **Give one path.** The simplest, cleanest solution with literal commands to
  paste. No tradeoff lists, no "you could also". If there's a real fork that
  changes her decision, ask one question.
- **Explain in plain terms.** Name what a thing does, not what it's called.
- **She owns design judgment.** Her taste is the standard, not a framework's
  defaults.

## Working rules

1. **Ask before committing or pushing.** Every time.
2. **Every sentence carries a fact.** Delete anything that justifies, announces,
   hedges, softens, or restates. Applies to documents, UI copy, and replies to
   her in chat.
3. **Design minimal and intuitive.** Cut explanatory copy, helper text, captions.
   Let layout and affordances carry meaning. If a screen needs a paragraph to
   explain it, the design is wrong, not the copy.
4. **Surface dead ends immediately** — a link to nowhere, a setting nothing
   reads, a deferred promise. Flatly, when you find it, not when she trips on it.
5. **Build on demand.** If nobody asked for it, ask before building it.
6. **Comms drafts inline in chat**, never as a file in this repo. Reusable
   artifacts can be files.
7. **Anything written about a person** — pay, performance, negotiation — goes in
   `~/Documents/`, never in a repo.
8. **Shareable deliverables go somewhere findable**: the repo or a Google Doc,
   not a hidden plan file.
9. **`iris` lowercase is the marketplace brand** (iris.market, a separate
   project). **`Iris` capitalized is the person** — this site carries her name,
   so capitalize it here.
10. **Before rescuing a "modified" file, check the diff direction.**
    `git show origin/main:<file> | diff - <file>` — a file can be older than the
    remote, and committing it would revert newer work.

## Not from here

iris.market is a separate product in a separate repo under a different GitHub
owner (`irismarket/iris-app`). Its conventions — listing parity, deal flow, its
component language, its deploy gotchas — do not apply to this site.
