# Changelog — @thehookclubdev/laptop

## 0.2.0
- Secrets now come from the Hook Club 1Password `developer` vault (`GitHub NPM Token`, `workspace` doc); no dependency on the wedops vault. SSH uses each developer's own key via the 1Password agent (no agent.toml).
- Install command is `curl -fsSL https://thehookclub.dev/install.sh | bash` — served by the thehookclubdev/site Dokku app, which proxies `/install.sh` here and hosts the instructions at `/laptop`.

## 0.1.0
- New repo: fresh-Mac bootstrap for Hook Club development, scoped to `~/thc` — `install.sh` → `mac` (Homebrew, Brewfile, shell, Node via mise, Claude Code, Redis service, 1Password SSH agent) plus `envrc` to seed each repo's `.envrc` from 1Password. Idempotent and safe to re-run.
