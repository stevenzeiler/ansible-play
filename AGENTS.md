# AGENTS.md

Guidance for AI agents working in this repo. Humans: see [README.md](README.md).

## What this is

Dormant 2014 to 2015 Ansible playbooks for Ripple, Codius, dogecoin and ELK servers on Ubuntu. Baseline-tier docs only. Profile: `legacy`.

## Stack

- Ansible 1.x syntax (`sudo: yes`, `with_items`, `result|failed`, `key=value` module style). Modern Ansible removed or changed several of these. Not verified on any version.
- Target hosts: Ubuntu (the playbooks use `ubuntu` as the remote user, Upstart-era paths such as `/etc/init/`, and 2014 package names like `libdb5.1-dev`).
- Python 2 era tooling on the control machine.

## Layout

- `*.yml` top-level playbooks, one per server type
- `roles/<name>/tasks/main.yml` (plus `handlers/` and `files/` where present)
- `hosts` inventory and `ansible.cfg`
- `docs/STATUS.md` current state

## Commands

- Syntax check: `ansible-playbook -i hosts <playbook>.yml --syntax-check` (likely fails on modern Ansible)
- Run: `ansible-playbook -i hosts <playbook>.yml`. Do not run against real hosts without Steven.
- There are no tests. Say plainly when something is known not to work today.

## Rules

- Add or update tests (for example a syntax-check CI job) with every change.
- Never commit secrets, private keys, `.pem` files or real inventory addresses.
- Use Conventional Commits.
- Keep `docs/STATUS.md` current.

## What agents may and may not do

- May: improve docs, add checks, open PRs against `master`.
- May not, without Steven: archive the repo, publish it, rewrite history, run playbooks against any real server, or change the license.
