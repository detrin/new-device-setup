# Coolify Server Setup Guide (AlmaLinux 9)

This document covers the non-obvious configuration steps required to run Coolify reliably on AlmaLinux 9 with a custom Docker bridge network.

---

## 1. Install SSH Server

AlmaLinux 9 may not have `openssh-server` installed by default.

```bash
dnf install -y openssh-server
systemctl enable --now sshd
```

---

## 2. Configure Docker Daemon

Create or edit `/etc/docker/daemon.json`:

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "default-address-pools": [
    {"base":"10.0.0.0/8","size":24}
  ],
  "host-gateway-ip": "10.0.0.1"
}
```

> **Why `host-gateway-ip`?** Coolify's main container has `host.docker.internal` mapped via `--add-host`, but the ephemeral helper containers it spawns for deployments do not. Setting `host-gateway-ip` in daemon.json ensures the IP is resolvable system-wide.

---

## 3. Add `host.docker.internal` to `/etc/hosts`

Docker injects `/etc/hosts` entries into all new containers, including ephemeral ones. This is what makes Coolify's deployment helper containers able to reach the host over SSH.

```bash
echo "10.0.0.1 host.docker.internal" >> /etc/hosts
```

> **Note:** Replace `10.0.0.1` with your actual `docker0` bridge IP if different (`ip addr show docker0 | grep inet`).

---

## 4. Configure Firewall (ufw)

Allow SSH from the Docker bridge network so Coolify containers can connect to the host:

```bash
ufw allow 22/tcp
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow in on docker0 to any port 22
ufw reload
```

> **Why the `docker0` rule?** By default ufw blocks connections from Docker bridge networks to the host. Without this rule, SSH connections from Coolify containers to `host.docker.internal` are refused even though sshd is running.

---

## 5. Restart Docker

Apply all daemon config changes:

```bash
systemctl restart docker
cd /data/coolify/source && docker compose down && docker compose up -d
```

---

## 6. Verify Everything Works

```bash
# Confirm host.docker.internal resolves inside containers
docker run --rm alpine ping -c1 host.docker.internal

# Confirm SSH is reachable from Docker network
docker run --rm alpine nc -zv 10.0.0.1 22

# Confirm sshd is running and enabled
systemctl status sshd
```

---

## 7. Coolify SSH Key

After Coolify is installed, make sure its public key is in `~/.ssh/authorized_keys` on the host. Coolify stores its keys in `/data/coolify/ssh/keys/`. Extract public keys with:

```bash
for key in /data/coolify/ssh/keys/*; do
  ssh-keygen -y -f "$key"
done
```

Compare output with `~/.ssh/authorized_keys` and add any missing keys.

---

## Summary of Non-Obvious Gotchas

| Issue | Root Cause | Fix |
|-------|-----------|-----|
| `ssh: connect to host host.docker.internal: Connection refused` | ufw blocking Docker bridge | `ufw allow in on docker0 to any port 22` |
| `host.docker.internal` not resolving in helper containers | Ephemeral containers don't get `--add-host` | Add to `/etc/hosts` + `host-gateway-ip` in daemon.json |
| `mysqld: command not found` in MariaDB 11.8+ | Binary renamed | Use `mariadbd` instead of `mysqld` |
| `mysqladmin ping` auth failure during init | Root password not set yet during entrypoint init | Use `mariadb-admin ping` without `--password` flag |
