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

## Build

```sh
hugo --minify
```

The generated site goes to `public/`.

## Cloudflare Pages

- Build command: `hugo --minify`
- Build output directory: `public`
- Hugo version: `0.157.0` or newer

