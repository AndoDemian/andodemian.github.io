# andodemian.com

Hugo + PaperMod, deployed to GitHub Pages by GitHub Actions. Posts are edited in the browser with Sveltia CMS at `/admin/`, or as plain Markdown in `content/posts/`.

## Writing

**In the browser:** go to <https://andodemian.com/admin/>, sign in, and create a post. New posts start as drafts. Untick *Draft* and save to publish. Every save is a commit to `main`, and the site rebuilds in about a minute.

**Locally:**

```sh
git clone --recurse-submodules git@github.com:AndoDemian/andodemian.github.io.git
hugo new content posts/my-post/index.md   # creates a draft page bundle
hugo server -D                            # preview at http://localhost:1313 (drafts included)
```

Put images next to `index.md` in the post folder and reference them by file name.

## One-time setup

1. **Pages source.** Repo *Settings → Pages → Build and deployment → Source:* **GitHub Actions**.
2. **Custom domain.** `static/CNAME` already contains `andodemian.com`. Keep *Enforce HTTPS* ticked in Settings → Pages.
3. **Editor login.** Until step 4 is done, use *Sign In with Token* on the `/admin/` login screen. Create a fine-grained token scoped to this repo only, with **Contents: Read and write**.
4. **"Sign in with GitHub" button (optional).**
   1. Deploy <https://github.com/sveltia/sveltia-cms-auth> to Cloudflare Workers (free tier) and note the Worker URL.
   2. Create a GitHub OAuth app (Settings → Developer settings → OAuth Apps). Set the callback URL to `<WORKER_URL>/callback`.
   3. In the Worker's settings, add `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET` (encrypted), and `ALLOWED_DOMAINS=andodemian.com`.
   4. Uncomment `base_url` in `static/admin/config.yml` and set it to the Worker URL.
5. **Analytics (optional).** Sign up at <https://www.goatcounter.com>, then set `goatcounter = "<your-code>"` in `hugo.toml`. Leave it empty to disable tracking.

## Maintenance

1. **Hugo version** is pinned in `.github/workflows/hugo.yml` (`HUGO_VERSION`).
2. **Theme** is a git submodule. To update it, run `git submodule update --remote themes/PaperMod`, then commit.
