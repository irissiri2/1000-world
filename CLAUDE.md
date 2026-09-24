# CLAUDE.md

Guidance for Claude Code working in this repo.

## What this is

1000.world is Iris Alonzo's personal site — a single hand-written HTML page
plus three images. No build step, no framework, no server, no database.

| File | Role |
|---|---|
| `index.html` | The entire site. Inline `<style>`, no external CSS. |
| `IRIS_VENN.png` | The venn diagram, the page's main content |
| `Iris2025.jpeg` | Social-preview image (`og:image`) |
| `favicon-32x32.png` | Favicon — 162KB, ~200x larger than it needs to be. Fix when touching it. |
| `CNAME` | Contains `1000.world`. **Deleting this breaks the custom domain.** |

Imported from the old host 2026-09-17, byte-for-byte, as the starting point for
a rebuild.

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
