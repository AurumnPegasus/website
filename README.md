# aurumnpegasus.com

Minimal Hugo source for the blog.

## Local

```sh
hugo server
```

Open `http://localhost:1313`.

## New post

```sh
hugo new content/posts/my-post.md
```

Then edit the file in `content/posts/` and set `draft = false` when you want it published.

## Publish

```sh
git add .
git commit -m "Add new post"
git push origin master
```

GitHub Actions builds the site and GitHub Pages publishes it to `https://aurumnpegasus.com/`.

## Build

```sh
hugo --minify
```

The generated site goes to `public/`.

## GitHub Pages

- Deploy workflow: `.github/workflows/hugo.yaml`
- Source branch: `master`
- Published URL: `https://aurumnpegasus.com/`
