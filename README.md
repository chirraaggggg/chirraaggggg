# Setup — live stats card

This profile uses the same architecture as Andrew Grant's `Andrew6rant/Andrew6rant`
repo: a Python script + GitHub Action bake real stats directly into an SVG,
instead of relying purely on third-party badge services.

## 1. Create the repo
Create a repository named exactly `chirraaggggg` (must match your username) and
push these files to it. GitHub automatically renders its `README.md` on your
profile page.

## 2. Add a Personal Access Token
`GITHUB_TOKEN` (the default Action token) can't read your full commit history
across all repos reliably, so:

1. Go to **Settings → Developer settings → Personal access tokens → Fine-grained tokens**.
2. Create a token with `repo` (read) and `read:user` scopes.
3. In your `chirraaggggg` repo, go to **Settings → Secrets and variables → Actions**.
4. Add a new secret named `ACCESS_TOKEN` with that token's value.

## 3. Enable Actions
Go to the **Actions** tab of the repo and enable workflows if prompted. The
`profile-card.yml` workflow will then run every 12 hours, recompute your stats,
and commit the refreshed SVGs automatically.

## 4. (Optional) Contribution snake
The README references a snake animation at
`chirraaggggg/chirraaggggg` on an `output` branch. To enable it, add this
workflow as `.github/workflows/snake.yml`:

```yaml
name: generate animation
on:
  schedule:
    - cron: "0 */6 * * *"
  workflow_dispatch:

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: Platane/snk@v3
        with:
          github_user_name: chirraaggggg
          outputs: dist/github-contribution-grid-snake-dark.svg
      - uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## 5. First run
Trigger the "Update Profile Card" workflow manually once (**Actions → Update
Profile Card → Run workflow**) so the SVGs get populated before your first
visitor arrives.
