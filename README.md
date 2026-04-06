# runbooks

Shared runbook definitions for the [runbook](https://github.com/msjurset/runbook) CLI and [Runbook Mac](https://github.com/msjurset/runbook-mac) app.

## Quick Start

```sh
# Pull all runbooks
runbook pull github.com/msjurset/runbooks

# Or from the Runbook Mac app:
# Sidebar → Repositories → paste the URL → Pull
```

All runbooks use **variables** for environment-specific values (hostnames, users, paths). You'll be prompted for these on first run, or you can set defaults in the YAML.

## Runbooks

### System

| Runbook | Description |
|---------|-------------|
| [update-pihole](system/update-pihole.yaml) | Update OS packages, Pi-hole, and gravity database on a Pi-hole server |
| [update-homebrew](system/update-homebrew.yaml) | Update Homebrew packages and clean up old versions |
| [rotate-runbook-logs](system/rotate-runbook-logs.yaml) | Compress and archive a log file, updating the log index |

### Templates

Starter templates for building your own runbooks.

| Template | Description |
|----------|-------------|
| [shell-basic](templates/shell-basic.yaml) | Simple local shell command sequence |
| [ssh-remote](templates/ssh-remote.yaml) | Remote server maintenance via SSH |
| [http-healthcheck](templates/http-healthcheck.yaml) | HTTP endpoint health check with notifications |
| [docker-deploy](templates/docker-deploy.yaml) | Deploy a Docker container with image pull, swap, and health check |
| [database-backup](templates/database-backup.yaml) | Database dump, compress, and upload to remote storage |
| [ssl-cert-check](templates/ssl-cert-check.yaml) | Check SSL certificate expiry and warn if near |
| [log-rotation](templates/log-rotation.yaml) | Archive, compress, and clean old log files |
| [service-restart](templates/service-restart.yaml) | Check service health and restart if unhealthy |
| [git-deploy](templates/git-deploy.yaml) | Deploy from git with build, test, symlink swap, and cleanup |
| [multi-service-healthcheck](templates/multi-service-healthcheck.yaml) | Check multiple services in parallel, alert on failures |
| [api-deploy-with-rollback](templates/api-deploy-with-rollback.yaml) | Deploy with retry logic and automatic rollback on health failure |
| [secret-rotation](templates/secret-rotation.yaml) | Rotate API keys and tokens using 1Password references |
| [full-stack-healthcheck](templates/full-stack-healthcheck.yaml) | Comprehensive parallel health checks with Slack notifications |
| [backup-and-organize](templates/backup-and-organize.yaml) | Run goback backups then organize files with sortie |
| [backup-verify](templates/backup-verify.yaml) | Verify backup freshness across all goback jobs |
| [download-processing](templates/download-processing.yaml) | Process and organize downloaded files using sortie |
| [project-cleanup](templates/project-cleanup.yaml) | Clean build artifacts and archive old logs using sortie |

## How Variables Work

Runbooks use variables to stay portable. Instead of hardcoding hostnames or paths:

```yaml
variables:
  - name: host
    prompt: "Server hostname or IP"
    default: "my-server"
  - name: user
    default: "deploy"

steps:
  - name: Check uptime
    type: ssh
    ssh:
      host: "{{.host}}"
      user: "{{.user}}"
      command: "uptime"
```

Variable values are resolved in priority order:
1. `--var host=prod-01` (CLI flag)
2. `RUNBOOK_VAR_HOST=prod-01` (environment variable)
3. YAML `default` value
4. Interactive prompt

Secrets use 1Password references (`op://Vault/Item/field`) which are resolved and cached in the system keychain.

## Contributing

PRs welcome. Runbooks should:
- Use variables for all environment-specific values (no hardcoded IPs, hostnames, or credentials)
- Include a `description` field
- Use `on_error` policies and `timeout` on long-running steps
- Work with `runbook run --dry-run` for safe previewing

## License

[MIT](LICENSE)
