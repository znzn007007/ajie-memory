## 2026-04-01 — GCP remote ops via gcloud on user-specified instances
- Signal: workaround
- Summary: When operating a GCP VM through `gcloud compute ssh/scp`, first verify the exact `zone` and SSH `user`; instance name alone is not enough, and the default login user may fail while a user like `znzn007007` works.
- Why it matters: Prevents wasted retries on wrong-zone lookups and publickey failures; gives a repeatable sequence for remote changes.
- Evidence: On `instance-20260320-121921`, initial attempts failed because `asia-east1-b` was wrong, then `scp/ssh` as default user hit `Permission denied (publickey)`, while `gcloud compute ssh znzn007007@instance-20260320-121921 --zone=us-west1-b` succeeded.
- Next move: promote later if this repeats

## 2026-04-01 — Safer playbook for gcloud remote execution
- Signal: workaround
- Summary: Preferred order for GCP remote ops: (1) confirm zone, (2) test login with `gcloud compute ssh <user>@<instance> --zone=<zone> --command='whoami'`, (3) use `gcloud compute scp` to push a script, (4) execute the script remotely, instead of packing long multiline shell into one SSH command.
- Why it matters: Reduces quoting breakage and separates connectivity/auth failures from script logic failures.
- Evidence: Long inline commands broke on local quoting and shell parsing; switching to upload-then-run script made the execution path stable, and exposed the real remaining issues (wrong package manager, config path differences).
- Next move: promote later if this repeats

## 2026-04-01 — Cross-distro DB automation on remote hosts
- Signal: workaround
- Summary: For PostgreSQL automation on unknown GCP VMs, detect distro/package manager first (`apt-get` vs `dnf`/`yum`) and do not assume RHEL layout; on Debian, PostgreSQL config file path should be discovered via `SHOW config_file` and `SHOW hba_file` rather than inferred from `data_directory`.
- Why it matters: Avoids failures like `dnf: command not found` and missing `/var/lib/postgresql/.../postgresql.conf` assumptions.
- Evidence: The target VM was Debian Bookworm, not RHEL; package install succeeded only after switching to `apt-get`, and config edits succeeded only after querying PostgreSQL for real config paths.
- Next move: promote later if this repeats

## 2026-04-03 — GCP SSH login assumptions caused repeated false starts
- Signal: correction
- Summary: For GCP instance work (e.g. sub2api-prod), do not assume current local host equals target machine, and do not assume `gcloud compute ssh` will succeed just because `gcloud` is configured. Check target access explicitly first.
- Why it matters: Prevents repeating the same SSH/login dead-end and avoids misleading local-machine conclusions about remote services like PostgreSQL.
- Evidence: User correction on 2026-04-03 after repeated SSH/login mismatch while trying to deploy CLIProxyAPI to sub2api-prod.
- Next move: promote later if this repeats again; for now keep in .learnings.
