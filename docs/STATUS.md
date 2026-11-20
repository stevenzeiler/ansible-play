# Status

Last reviewed: 2026-11-20 (github-audit pass 02, docs only; nothing was run).

## What works

- Unknown. No playbook was run for this review. The simple ones (`common`, `nginx`, `nodejs`, `dogecoind`) look self-contained.

## What's broken or unknown

- Ansible 1.x syntax: `sudo: yes`, `result|failed` and other forms are removed in modern Ansible.
- The `rippled` role copies files that are not in the repo: `90-nfile.conf`, `ripple-statsd.conf`, `afrank.crontab`, `rippled.crontab`, and the user public keys (commented out). It also copies `localConfig.js` while the repo has `localconfig.js` (case mismatch).
- The `rippled` role installs a statsd checkout under another person's home directory (`afrank`), and `rippled/files/localconfig.js` holds a fixed Graphite host IP. The `rippled.yml` header credits "NBR" (dated 2014-12-06, before Steven's 2015-01-14 commit of the role), so part of this was written by someone else **(inference: a colleague at Ripple)**. Any license decision must account for that.
- The `rippled` role never installs rippled: the task named "install rippled" installs `build-essential`. The apt repo task uses the `file` module with `src=ripple.list`, which does not copy the file **(inference: it should be `copy`)**.
- `roles/logstash/tasks/main.yml` is empty. No playbook uses the ELK roles. The `elasticsearch` and `kibana` roles use the `command` module with pipes, `cd` and `&&`, which `command` does not run through a shell, and `service` states `restart` and `start`, which are not valid **(inference: these roles fail as written)**.
- `hosts` lists two public IPs under `gatewayd` (not repeated here). `rippled` is an empty group, and `codius_host`, `codius_sandbox`, `dogecoind` and `nodejs` are not defined. `ansible.cfg` names a private key file path; the key is not in the repo.
- `roles/rippled/files/sshd_config` and `rules.v4` replace the server's SSH and firewall config wholesale. Review before any use.
- Packages and PPAs from 2014 (`ppa:chris-lea/node.js`, Oracle Java 8, Elasticsearch and Kibana download URLs, `libdb5.1`) are very likely gone or unsafe today.
- No secrets were seen in a scan for passwords, seeds and keys.

## What's next

Nothing is planned. Archiving is Steven's call.

## Not yet from the standard

- LICENSE: none declared, none added. Steven decides, keeping in mind that part of the `rippled` role may be a colleague's work.
- CONTRIBUTING and SECURITY
- `.editorconfig`
- `.github` issue and PR templates
- Dependabot
- `ci.yml` with gitleaks
- Rename default branch `master` to `main`
- CloudEvents catalog
- Product ONE-PAGER
