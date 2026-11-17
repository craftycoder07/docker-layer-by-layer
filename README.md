# Layer by Layer

An interactive Docker course: ten animated diagrams you step through, from "containers vs VMs" to replicas behind a load balancer. Each step pairs with the real command that does it.

Single self-contained `index.html` — no build step. Deployed to GitHub Pages by `.github/workflows/pages.yml` on every push to `main`.

## Run locally

Open `index.html` in a browser, or:

```sh
python3 -m http.server 8000
```

## Add a lesson

Append an object to the `LESSONS` array in `index.html` with `id`, `title`, `tag`, `lede`, a `scene()` that returns SVG, and a `steps` array (`t`, `d`, optional `cmd`). Elements in the scene use `in` / `out` / `hl` step numbers to appear, disappear, or highlight.
