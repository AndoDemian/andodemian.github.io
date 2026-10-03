# andodemian.com

Personal site for technical writing. Built with [Hugo](https://gohugo.io) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme (git submodule), deployed to GitHub Pages with GitHub Actions.

## Writing

Posts are page bundles in `content/posts/<slug>/index.md`, with images next to the post. They can be edited in the browser at `/admin/` ([Sveltia CMS](https://sveltiacms.app)) or as plain Markdown. New posts start as drafts.

```sh
git clone --recurse-submodules git@github.com:AndoDemian/andodemian.github.io.git
hugo new content posts/my-post/index.md
hugo server -D
```

## Notes

- Analytics: optional [GoatCounter](https://www.goatcounter.com), enabled by setting `params.goatcounter` in `hugo.toml`. Empty means off.
- Update the theme with `git submodule update --remote themes/PaperMod`.
