# 42 Chain blog

The stories and articles published at [42chain.org/blog](https://42chain.org/blog).
Plain Markdown with no code; the site (the `site` repository) turns each file into a
post page and lists it in the blog.

## Layout

| Path | What it holds |
|---|---|
| `<lang>/<slug>.md` | One post. The file name is its address: `es/restaurant-sign.md` → `/es/blog/restaurant-sign`. |
| `images/` | The posts' images, referenced from a post as `../images/<file>`. Credits in `images/CREDITS.md`. |

Posts are written in Spanish first (`es/`). A post missing in English shows the
Spanish one with a notice; to translate it, add `en/<same-slug>.md`.

## Writing a post

Front matter, checked by the site's schema (`apps/web/src/kit/schemas.ts`):

| Field | |
|---|---|
| `title`, `summary` | Required. |
| `category` | Required: one of the ids in the site's `content/site/<lang>/site.md` → `blog.categories` (for example `historia`). |
| `date` | Optional; without it, the date of the file's last commit. |
| `image`, `alt`, `credit` | Optional cover: the path (`../images/…`), its description and its credit. |
| `featured`, `weight` | Optional: highlight it and order it in the blog index. |

No HTML, MDX or scripts: Markdown and the site's `:::` blocks only.

## Seeing a post locally

Nothing runs here on its own. Use the site, where this repository is the
`apps/web/content/blog` submodule:

```bash
git clone --recurse-submodules https://github.com/<org>/site.git
cd site && npm ci && npm run dev   # http://localhost:4321/blog
```

## How a post is published

1. A pull request runs **Check** (`.github/workflows/check.yml`): it builds the
   whole site with the post and checks every internal link.
2. Merging into `main` runs **Publish** (`.github/workflows/publish.yml`): it
   calls the Cloudflare Pages deploy hook of 42chain.org, whose build takes the
   `main` of every content repository.

Setup, once: in the site's Pages project, **Settings → Builds → Add deploy
hook** (branch `main`), and save its URL here as the Actions secret
`CF_DEPLOY_HOOK`. Anyone with the URL can start a build: keep it secret.

## License

Texts under [CC BY 4.0](LICENSE). © The 42 Chain contributors. Images: see
`images/CREDITS.md`.
