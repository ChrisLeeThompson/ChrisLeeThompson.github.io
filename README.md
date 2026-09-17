# Chris's GitHub Page

Source for https://chrisleethompson.github.io/, a [Hugo](https://gohugo.io/) site using the [Hextra](https://github.com/imfing/hextra) theme. It documents the Python scripts published under [github.com/ChrisLeeThompson](https://github.com/ChrisLeeThompson).

## Previewing locally

Requires the Hugo extended edition (0.157 or newer) and Go (Hextra is pulled in as a Hugo module). From the repository root:

```
hugo server
```

## Deploying

Every push to `main` runs `.github/workflows/hugo.yml`, which builds the site with Hugo extended and publishes it to GitHub Pages. Nothing is deployed by hand.

## License

MIT, see [LICENSE](LICENSE). The Catbug artwork in `assets/catbug/` and `assets/gif/` is not covered by the MIT license; see [LICENSE](LICENSE).
