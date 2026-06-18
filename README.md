# mrblahhhh.github.io

Personal site and hardware build logs, powered by [Jekyll](https://jekyllrb.com/) and the [TeXt theme](https://kitian616.github.io/jekyll-TeXt-theme/).

## Local development

```bash
gem install bundler
bundle init
bundle add github-pages jekyll-remote-theme
bundle exec jekyll serve
```

Visit http://localhost:4000

## Publishing

Pushes to `main` deploy automatically via GitHub Actions.
