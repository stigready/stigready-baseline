# stigready_baseline — Ansible role (stigready-baseline)

**Ansible hardening role** for **stigready** (OpenSCAP). Suitable for playbooks, Packer/Ansible provisioners, and golden-image pipelines. Search keywords: `ansible`, `ansible-role`, `compliance`, `devsecops`, `hardening`, `infrastructure`, `openscap`, `security`, `stigforge`.

StigForge-exported Ansible role **`stigready_baseline`** · release **`0.3.49`**.
Matrix cell status: **`green`**.

## Install

This repository root **is** the Ansible role (Galaxy-style layout). OpenSCAP evidence
lives under `compliance/` and is not loaded when the role runs.

From **GitHub** (public):

```yaml
# requirements.yml
roles:
  - src: https://github.com/stigready/stigready-baseline
    scm: git
    version: v0.3.49   # or an immutable commit SHA
    name: stigready_baseline
```

```bash
ansible-galaxy role install -r requirements.yml -p ./roles
ansible-playbook -i inventory site.yml   # role: stigready_baseline
```

## Verification status (this release)

This release carries no per-role OpenSCAP score. A container cannot evaluate most STIG rules, so the
role is scored where it runs: the StigReady AMI build scores each image against the unmodified SSG
profile. `compliance/releases/0.3.49/manifest.json` records this.

## Reports & review

- **[reports/index.html](reports/index.html)** — HTML report index
- **[CHANGELOG.md](CHANGELOG.md)** — release notes and verify summary

## Verify the score (customer)

Re-run OpenSCAP in Docker and compare to this release's evidence:

```bash
make prove RELEASE=0.3.49
```

Or score your own `results.xml`: see **[compliance/README.md](compliance/README.md)**.

## License

- **[LICENSE](LICENSE)** (MIT) — StigForge export packaging
- **[NOTICE](NOTICE)** — ComplianceAsCode / BSD-3-Clause task body attribution

## Catalog

- [https://stigready.com/#stigforge](https://stigready.com/#stigforge)

