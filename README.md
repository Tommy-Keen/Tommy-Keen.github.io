# Blog Workflow

This repository stores the Hexo source for `https://tommy-keen.github.io/`.

## Local writing

Create a new post:

```powershell
npm run new:post -- "Post Title"
```

The post file will be created in `source/_posts/`.

Write in Markdown. If you need LaTeX, use `$...$` for inline math and `$$...$$` for display math.

## Local preview

```powershell
npm run dev
```

Open `http://localhost:4000/`.

## Publish

1. Commit your source changes.
2. Push the `master` branch to GitHub.
3. GitHub Actions builds Hexo and deploys the generated `public/` site to GitHub Pages.

Do not commit `public/` or `node_modules/`.
Do not use `hexo d` in this repository.
