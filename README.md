# y2.nz — Chhoto URL deployment

Self-hosted URL shortener for `y2.nz`. Runs
[Chhoto URL](https://github.com/SinTan1729/chhoto-url) in Docker behind the
existing Apache reverse proxy.

## Layout

| File | Purpose |
|---|---|
| `docker-compose.yml` | Chhoto container, bound to `127.0.0.1:4567`, data in `./data` |
| `.env.example` | Template for `.env` (password, API key, site URL, DB path) |

`.env` and `data/` are git-ignored: they hold the secrets and the live database.

## First deployment

```bash
sudo mkdir -p /opt/chhoto-url && cd /opt/chhoto-url
git clone <this repo> .
cp .env.example .env && chmod 600 .env     # set CHHOTO_PASSWORD and CHHOTO_API_KEY
docker compose up -d
curl http://127.0.0.1:4567/api/version     # sanity check
```

## Day-to-day

```bash
docker compose pull && docker compose up -d        # update (pinned to a minor tag)
docker compose logs -f                             # logs
```

The database is `data/urls.sqlite`. Chhoto keeps its own rolling backups in
`data/backups/`; include `data/` in the server's normal backup job as well.
