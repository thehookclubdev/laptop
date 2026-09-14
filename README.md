# laptop

Set up a fresh Mac for [The Hook Club](https://github.com/thehookclubdev) development in one command — the `~/thc`-scoped cut of [wedops/laptop](https://github.com/wedops/laptop), itself inspired by [thoughtbot's `laptop`](https://github.com/thoughtbot/laptop).

## Install

Paste this into Terminal:

```bash
curl -fsSL https://dev.thehookclub.nyc/install.sh | bash
```

(`dev.thehookclub.nyc` is this repo served by GitHub Pages. The raw URL works too: `https://raw.githubusercontent.com/thehookclubdev/laptop/main/install.sh`.)

You'll be asked for your macOS password once. It's safe to run more than once — every step checks before it acts, so re-running on a configured machine is close to a no-op. Open a new terminal afterward (or `source ~/.zshrc`).

The bootstrap clones this repo to `~/thc/laptop`, so it sits next to the app repos it provisions for.

## What it does

- Installs **Homebrew** and the Xcode Command Line Tools
- Installs the system dependencies in [`Brewfile`](Brewfile):
  - `mise` — runtime version manager (provisions Node)
  - `direnv` — loads each repo's `.envrc` (which pulls the GitHub Packages token from 1Password)
  - `gh` — GitHub CLI
  - `starship` — shell prompt
  - `redis` — local Redis; the admin app's cache defaults to `localhost:6379`
  - `libpq` — Postgres client tools (`psql`, `pg_dump`)
  - `netlify-cli` — deploys the Netlify-hosted apps (planning, production)
  - `jq`, `ripgrep` — CLI utilities
  - `ffmpeg`, `yt-dlp` — song reference downloads
  - `orbstack` — Docker engine, to reproduce the Dokku image builds locally
  - `1password` + `1password-cli` — secrets & SSH via `op` (see below)
  - `zed`, `linear`, `tableplus`, `ghostty`, `rectangle`
- Wires `mise`, `starship`, `direnv`, and keg-only `libpq` (so `psql`/`pg_dump` are on `PATH`) into `~/.zshrc` (only if not already there)
- Provisions **Node** via mise (latest LTS — Node 24, which satisfies every repo's `engines`)
- Installs **Claude Code** via Anthropic's native installer (into `~/.local/bin`)
- Starts **Redis** as a login service
- Points `~/.ssh/config` at the **1Password SSH agent** (so `git` uses a vault-stored key)

It does **not** clone or modify any app repo. Cloning and per-repo auth are the two follow-up commands the script prints when it finishes (see below).

## Secrets & SSH

Secrets come from the **Hook Club 1Password** account rather than living on disk. The shared **`developer`** vault holds the GitHub Packages token (`GitHub NPM Token`), the `workspace` clone script, and the app secrets; an admin adds each developer to it.

- Every THC repo has a committed `.npmrc` that reads `${NPM_GITHUB_TOKEN}` (GitHub Packages hosts `@wedops/*` and `@thehookclubdev/*`) and a gitignored `.envrc` that provides it. [`envrc`](envrc) writes that `.envrc` for you, holding an `op://developer/GitHub NPM Token/password` *reference* (not the value); direnv resolves it through the 1Password CLI.
- `mac` points `~/.ssh/config` at the 1Password SSH agent. Keep your own GitHub SSH key in your Private/Personal vault (1Password can generate one: **New Item → SSH Key**) and add its public key to your GitHub account. The agent offers keys from your personal vault by default, with a biometric tap — no private key on disk.

After the install finishes:

1. 1Password app → **Settings → Developer** → enable **"Integrate with 1Password CLI"**, **"Use the SSH agent"**, and Touch ID unlock.
2. Confirm you can see the **`developer`** vault, and that `ssh -T git@github.com` greets you.
3. Sign in and pull the Hook Club repos into `~/thc`:

   ```bash
   op signin
   op document get workspace --vault developer | bash
   ```

   (Set `WORKSPACE_ROOT=/path` to clone elsewhere.)

4. Give each repo its `.envrc` (idempotent — skips repos that already have one, so a hand-written PAT `.envrc` is left alone):

   ```bash
   ~/thc/laptop/envrc            # or: ~/thc/laptop/envrc --dry-run
   ```

Then `cd ~/thc/admin && npm install` just works.

> If you're signed in to more than one 1Password account, add `export OP_ACCOUNT=<shorthand>` to `~/.zshrc` so `op` resolves against the Hook Club account.

## dev.thehookclub.nyc

The apex domain is on Squarespace, which can't proxy a path to GitHub. So this repo is published with GitHub Pages under the custom domain `dev.thehookclub.nyc` (see [`CNAME`](CNAME) and [`index.html`](index.html)); `install.sh` is served straight from the checkout. DNS: a `CNAME` record `dev → thehookclubdev.github.io` at the registrar.

## Customizing

The `Brewfile` is the source of truth for what a machine needs. Add a line, re-run the install command, done. Keep each entry's trailing comment honest about why it's there.

`OP_VAULT=<name>` overrides the vault `mac` and `envrc` read from.
