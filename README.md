# taintwire-docs

Source for the [taintwire](https://github.com/js-recon/taintwire) docs site, served at https://taintwire.js-recon.io. Built with [Docusaurus](https://docusaurus.io).

## Develop

```sh
npm install
npm start     # dev server with live reload
npm run build # production build; fails on broken links
```

## Layout

- `docs/`: **Next** docs (served at `/next`), matching taintwire's `dev` branch.
- `versioned_docs/`, `versioned_sidebars/`, `versions.json`: snapshots of stable releases. `/` serves `lastVersion` from `docusaurus.config.ts`. Fix an old version in `versioned_docs/version-X.Y.Z/`, not in `docs/`.
- `static/img/`: branding assets (banner, logo, favicon).

## Branches

`dev` tracks taintwire `dev`. `main` tracks taintwire releases: merge `dev` into `main` with each taintwire release. For a stable release, snapshot first:

```sh
npx docusaurus docs:version X.Y.Z
```

Then set `lastVersion: "X.Y.Z"` in `docusaurus.config.ts`.

## License

[MIT](LICENSE)
