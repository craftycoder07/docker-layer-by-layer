# Layer by Layer

An interactive Docker course: ten animated diagrams you step through, from "containers vs VMs" to replicas behind a load balancer. Each step pairs with the real command that does it.

Examples switch between **Node.js, .NET, Python, Java, Go and Rust** (picker in the header; choice is remembered per browser).

Single self-contained `index.html` — no build step. Deployed to GitHub Pages by `.github/workflows/pages.yml` on every push to `main`.

## Practice area

`#practice` on the site is a browser-only Docker simulator (terminal + live diagram), a Dockerfile playground that simulates layer caching and image size, and 11 auto-checked challenges. For real Docker, the repo ships a dev container: **Code → Codespaces → Create codespace**, then follow `practice/README.md`.

## Accounts

Optional sign-in (GitHub, Google, Microsoft, email magic link) syncs lesson and challenge progress through Firebase. It stays hidden until `FIREBASE_CONFIG` in `index.html` is set. See [docs/firebase-setup.md](docs/firebase-setup.md).

## Run locally

Open `index.html` in a browser, or:

```sh
python3 -m http.server 8000
```

## Add a stack

Add an entry to `STACKS` in `index.html` (copy an existing one) and its key to `STACK_ORDER`.

## Add a lesson

Append an object to the `LESSONS` array in `index.html` with `id`, `title`, `tag`, `lede`, a `scene(S)` that returns SVG, and a `steps(S)` function returning an array (`t`, `d`, optional `cmd`). Elements in the scene use `in` / `out` / `hl` step numbers to appear, disappear, or highlight.
