# Brewfile — system dependencies for a Hook Club (~/thc) dev machine.
#
# Consumed by `./mac` via `brew bundle`. Idempotent: running it again just
# verifies everything is present. Every entry has a reason noted next to it —
# keep this list honest, it's the source of truth for what a machine needs.

# --- CLIs / daemons -------------------------------------------------------

brew "mise"        # runtime version manager — provisions Node per ~/.config/mise/config.toml
brew "direnv"      # loads each repo's .envrc (NPM_GITHUB_TOKEN for GitHub Packages)
brew "gh"          # GitHub CLI — auth, `gh api`, PR/release workflows in the thehookclubdev org
brew "starship"    # shell prompt (wired into ~/.zshrc)
brew "redis"       # local Redis — admin's @wedops/zeus-redis cache defaults to localhost:6379
brew "libpq"       # Postgres client CLIs (psql, pg_dump) for the THC database; keg-only
brew "netlify-cli" # deploys planning + production (Netlify sites)
brew "jq"          # JSON on the CLI (op / gh api / curl pipelines)
brew "ripgrep"     # fast recursive code search across the repos
brew "ffmpeg"      # audio transcoding — yt-dlp's mux/encode backend
brew "yt-dlp"      # song reference downloads (~/thc/get_songs.sh)

# --- GUI apps / casks -----------------------------------------------------

cask "orbstack"      # Docker engine — reproduce the admin / setlist-generator Dokku image builds locally
cask "1password"     # desktop app; CLI integration + SSH agent unlock `op` and git-over-SSH
cask "1password-cli" # `op` — repos' .envrc files read the GitHub Packages token from 1Password
cask "zed"           # editor
cask "linear"        # Linear desktop app (THC issues live in Linear)
cask "tableplus"     # Postgres GUI for the THC database (psql has no GUI)
cask "ghostty"       # terminal emulator
cask "rectangle"     # window tiling / management
