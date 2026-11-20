# Status

Last reviewed: 2026-11-20 (github-audit pass 02, docs only; nothing was run).

## What works

- Unknown. No playbook was run for this review. The simple ones (`common`, `nginx`, `nodejs`, `dogecoind`) look self-contained.

## What's broken or unknown

- Ansible 1.x syntax: `sudo: yes`, `result|failed` and other forms are removed in modern Ansible.
- The `rippled` role copies files that are not in the repo: `90-nfile.conf`, `ripple-statsd.conf`, `afrank.crontab`, `rippled.crontab`, and the user public keys (commented out). It also copies `localConfig.js` while the repo has `localconfig.js` (case mismatch).
- The `rippled` role installs a statsd checkout under another person's home directory (`afrank`), and `rippled/files/localconfig.js` holds a fixed Graphite host IP. The role header credits "NBR", so part of this was written by someone else **(inference: a colleague at Ripple)**.
- `roles/logstash/tasks/main.yml` is empty. No playbook uses the ELK roles.
- `hosts` lists two public IPs under `gatewayd` only. The groups used by the other playbooks are not defined. `ansible.cfg` names a private key file path; the key is not in the repo.
- `roles/rippled/files/sshd_config` and `rules.v4` replace the server's SSH and firewall config wholesale. Review before any use.
- Packages and PPAs from 2014 (`ppa:chris-lea/node.js`, Oracle Java 8, Elasticsearch and Kibana download URLs, `libdb5.1`) are very likely gone or unsafe today.
- No secrets were seen in a scan for passwords, seeds and keys.

## What's next

Nothing is planned. Archiving is Steven's call.

## Not yet from the standard

- LICENSE: none declared, none added. Steven decides.
- CONTRIBUTING and SECURITY
- `.editorconfig`
- `.github` issue and PR templates
- Dependabot
- `ci.yml` with gitleaks
- Rename default branch `master` to `main`
- CloudEvents catalog
- Product ONE-PAGER
