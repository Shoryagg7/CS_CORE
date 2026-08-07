# Linux — Topic Map

> **Block E** (with DBMS). Linux is **on the resume**, so it must be defensible —
> a claimed skill that collapses under questioning is worse than an absent one.
> Sweep-heavy: much of Tier 1 and 2 is already muscle memory from DeliverIQ and
> Docker work, so the real time goes to Tier 3, where namespaces and cgroups sit
> directly on Nutanix's domain.
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

# Tier 1 — Command line fluency

- [ ] Filesystem hierarchy: `/etc`, `/var`, `/proc`, `/dev`, `/tmp`, `/usr`
- [ ] File ops: `ls`, `cd`, `cp`, `mv`, `rm`, `find`, `locate`
- [ ] Text processing: `cat`, `less`, `head`, `tail`, `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `wc`
- [ ] Pipes and redirection; `stdin` / `stdout` / `stderr`; `tee`, `xargs`
- [ ] Permissions: `chmod`, `chown`, rwx octal notation, `umask`
- [ ] **[HOT]** What `chmod 755` means and why it is the common default

---

# Tier 2 — Process and system management

- [ ] `ps`, `top`, `htop`, `kill`, `killall`
- [ ] Signals: SIGTERM, SIGKILL, SIGHUP
- [ ] **[HOT]** SIGTERM vs SIGKILL — why TERM is sent first
- [ ] Background / foreground jobs: `&`, `nohup`, `jobs`, `fg`, `bg`
- [ ] `systemctl` and services; `journalctl`
- [ ] Disk and memory: `df`, `du`, `free`, `lsblk`, `mount`
- [ ] Networking tools: `ping`, `curl`, `wget`, `netstat` / `ss`, `traceroute`, `dig`, `nslookup`
- [ ] Package managers: `apt`, `yum` / `dnf`
- [ ] SSH, key-based auth, `scp`, `rsync`
- [ ] Environment variables; `.bashrc` vs `.bash_profile`; `PATH`

---

# Tier 3 — Systems depth

- [ ] **[HOT]** The `/proc` filesystem — what it exposes, and why it is not a real filesystem
- [ ] File descriptors; everything-is-a-file
- [ ] **[HOT]** `strace` and `ltrace` — observing system calls
- [ ] `lsof`
- [ ] Shell scripting: variables, conditionals, loops, functions, exit codes
- [ ] `cron` and scheduled jobs
- [ ] **[HOT-NX]** Namespaces and cgroups — the mechanism containers are built on
- [ ] Kernel vs user space transitions; system call cost
- [ ] Static vs dynamic libraries; `ldd`, `LD_LIBRARY_PATH`

---

## Resources

- *The Linux Command Line*, William Shotts — free PDF.
- `man` pages — actually read them.
- **Linux Journey** (linuxjourney.com) — structured beginner path.

## Block G drill list — every [HOT] in this file

- [ ] `chmod 755` — what it means, why it is the default
- [ ] SIGTERM vs SIGKILL
- [ ] `/proc` — what it is and is not
- [ ] `strace` — what it shows and when it is reached for
- [ ] **[HOT-NX]** Namespaces and cgroups → how containers are actually built
