Summary of the mox setup

## Stack

- Namecheap as the domain registrar.
- Cloudflare DNS, for zonefile export/import.
- Hetzner ARM64 VPS (Debian 13)
- mox (SMTP/IMAP/Webmail/Admin) with Let’s Encrypt certificates and Unbound as a local DNSSEC-validating resolver

### On Hetzner (server):

- Hostname and A/AAAA to your public IPv4/IPv6.
- Reverse DNS (PTR) for both IPs set to hostname.
- Unbound installed + validating (`/etc/resolv.conf` → `127.0.0.1` + `options trust-ad`).
- mox installed at `/usr/local/mail`, systemd service created, listeners on 25/465/993/80/443 (IPv4+IPv6).
- ACME/Let’s Encrypt enabled in `mox.conf` (certs issued OK).
- UFW opened the mail/Web ports.
