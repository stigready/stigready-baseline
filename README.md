# stigready_baseline — Ansible role (stigready-baseline)

**StigReady baseline role** (hand-written; not a CIS or STIG profile). Suitable for playbooks, Packer/Ansible provisioners, and golden-image pipelines. Search keywords: `ansible`, `ansible-role`, `compliance`, `devsecops`, `hardening`, `infrastructure`, `openscap`, `security`, `stigforge`.

StigForge-exported Ansible role **`stigready_baseline`** · release **`0.3.51`**.
Matrix cell status: **`green`**.

## Before you run this role

Each default below changes how a running host behaves. Read them before you apply the role to a host you depend on.

### SSH password login is turned off

_Requirement: Key-only SSH is a StigReady product requirement for a published AMI (AWS Marketplace), not a STIG or CIS rule. PermitRootLogin no and PermitEmptyPasswords no are also STIG and CIS rules in SSG's profiles (for example RHEL-09-255045, RHEL-09-255040)._

**What the role does.** Sets `PasswordAuthentication no`, `PermitRootLogin no`, `PermitEmptyPasswords no`, `ChallengeResponseAuthentication no` and `UsePAM yes` in /etc/ssh/sshd_config. Where the distribution supports `Include`, it also writes them, with `KbdInteractiveAuthentication no`, to /etc/ssh/sshd_config.d/10-stigready.conf, which sshd reads before cloud-init's 50-cloud-init.conf, so cloud-init cannot turn password login back on.

**Who is affected.** Anyone who logs in over SSH with a password, and any tool that uses password or keyboard-interactive login. Before you run the role, check that every account you need has a key in its ~/.ssh/authorized_keys, or you can lock yourself out. root cannot log in over SSH at all.

**How to change it.** Both lists are role defaults. To allow password login, set `stigready_baseline_sshd_directives` and `stigready_baseline_sshd_dropin` with `PasswordAuthentication yes` (and `KbdInteractiveAuthentication yes` in the drop-in if you need it), and copy the other entries unchanged. There is no separate on/off variable for this one setting.

**Effect on compliance.** PasswordAuthentication is not a STIG or CIS rule, so changing it does not change a benchmark score. A StigReady AMI always ships with password login off. Keep PermitRootLogin and PermitEmptyPasswords at `no`: they are STIG and CIS rules.

## Install

This repository root **is** the Ansible role (Galaxy-style layout). Release metadata and the
scoring helper live under `compliance/` and are not loaded when the role runs.

From **GitHub** (public):

```yaml
# requirements.yml
roles:
  - src: https://github.com/stigready/stigready-baseline
    scm: git
    version: v0.3.51   # or an immutable commit SHA
    name: stigready_baseline
```

```bash
ansible-galaxy role install -r requirements.yml -p ./roles
ansible-playbook -i inventory site.yml   # role: stigready_baseline
```

## Verification status (this release)

This release carries no per-role OpenSCAP score, and no OpenSCAP results ship with it. A container
cannot evaluate most hardening rules, so the role is not scored here. To score a host you hardened
with it, see *Score it yourself*. `compliance/releases/0.3.51/manifest.json` records this.

## Reports & review

- **[reports/index.html](reports/index.html)** — HTML report index
- **[CHANGELOG.md](CHANGELOG.md)** — release notes

## Score it yourself

This repository ships no OpenSCAP results (see *Verification status*). To score a host you
hardened with this role, run `oscap xccdf eval` on it and score the `results.xml` with this role's
exception list: see **[compliance/README.md](compliance/README.md)**.

## Notices

StigForge and StigReady are independent. They are not affiliated with, endorsed by or certified by the Center for Internet Security, Inc. (CIS), the Defense Information Systems Agency (DISA), the U.S. Department of Defense, NIST, Red Hat, Canonical, Amazon, the Rocky Enterprise Software Foundation, the AlmaLinux OS Foundation or the ComplianceAsCode project. "CIS" and "CIS Benchmarks" are trademarks of the Center for Internet Security, Inc. DISA STIGs are DISA publications. Red Hat and Red Hat Enterprise Linux are trademarks or registered trademarks of Red Hat, Inc. or its subsidiaries in the United States and other countries. Ubuntu is a trademark of Canonical Ltd. Amazon Linux is a trademark of Amazon.com, Inc. or its affiliates. Rocky Linux is a trademark of the Rocky Enterprise Software Foundation. AlmaLinux is a trademark of the AlmaLinux OS Foundation. Linux is the registered trademark of Linus Torvalds in the U.S. and other countries. All trademarks are the property of their respective owners. This role is hand-written: it is not generated from ComplianceAsCode/SSG and is not a CIS or STIG profile. An OpenSCAP score is a measurement, not a certification.

## License

Copyright (c) 2026 StigReady.

- **[LICENSE](LICENSE)** (MIT) — StigForge export packaging
- **[NOTICE](NOTICE)** — ComplianceAsCode / BSD-3-Clause task body attribution

## Catalog

- [https://stigready.com/#stigforge](https://stigready.com/#stigforge)

