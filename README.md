# ansible-play

Ansible playbooks and roles from 2014 to 2015 for provisioning servers: common packages, nginx, Node.js, rippled, dogecoind, Codius sandbox and host, and an Elasticsearch, Logstash and Kibana (ELK) logging stack.

> **Status: dormant since May 2015.** Twelve commits, 2014-11-08 to 2015-05-29, from Steven's Ripple and Codius era. The playbooks use Ansible 1.x syntax and were written for Ubuntu on AWS. None of them were run for this review, and several will not run as they are today (see [docs/STATUS.md](docs/STATUS.md)).

## What is in it

| Playbook | Hosts group | Roles |
|---|---|---|
| `codius_host.yml` | `codius_host` | common, nodejs, codius_host |
| `codius_sandbox.yml` | `codius_sandbox` | common, nodejs, codius-sandbox |
| `dogecoind.yml` | `dogecoind` | dogecoind |
| `gatewayd.yml` | `gatewayd` | common, nginx (nodejs, postgres and gatewayd are commented out) |
| `nodejs.yml` | `nodejs` | common, nodejs, nginx |
| `rippled.yml` | `rippled` | rippled |

Other roles in `roles/`: `java`, `elasticsearch`, `kibana` and `logstash` (the ELK pieces; `logstash` is an empty file, and no playbook uses these roles yet).

## Install and usage

Install Ansible (the 1.x series these files were written for), put your hosts under the matching groups in `hosts`, then:

```sh
ansible-playbook -i hosts rippled.yml
```

Each play connects as `remote_user: ubuntu` and uses `sudo: yes`. These commands are untested here.

## Configuration

No environment variables. `ansible.cfg` points at an SSH private key file by path (the key itself is not in the repo). `hosts` is the inventory. Only `gatewayd` has addresses in it, `rippled` is an empty group, and the `codius_host`, `codius_sandbox`, `dogecoind` and `nodejs` groups are not defined at all. Do not publish real host addresses in these docs.

## How it works

Each top-level playbook applies a list of roles to an inventory group. Roles are plain task lists (`roles/<name>/tasks/main.yml`) built from `apt`, `git`, `shell`, `copy` and `lineinfile` modules. The `rippled` role is the largest: user accounts with passwordless sudo, an apt repo, firewall rules, RAID disk setup, an SSH config, and a StatsD setup. It does not install the `rippled` package itself (the task named "install rippled" installs `build-essential`). The `rippled.yml` playbook header credits another author, "NBR", dated 2014-12-06.

## Status

See [docs/STATUS.md](docs/STATUS.md). Agents should read [AGENTS.md](AGENTS.md).

## License

No license is declared. See STATUS for the open decision.
