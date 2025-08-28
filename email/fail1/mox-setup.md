Email is insane. There is not one, or two, there are **3 protocols** in email, technically more.

- **Extras**: Mailing lists, CalDAV/CardDAV/ActiveSync, migration/archiving
- **Protocols**: SMTP, IMAP, POP3, LMTP, JMAP
- **DNS**: MX/A/AAAA, PTR, SPF, DKIM, DMARC, ARC, BIMI
- **TLS**: STARTTLS/SMTPS, TLS 1.2/1.3, ACME/Let’s Encrypt, DANE/TLSA
- **Routing**: Smart-host, relays, rewriting, rate limits

There are entire packages (mail server stacks), just to make it simpler and simpler to run email. Some Examples include

1. docker-mailserver
2. mail-in-a-box
3. mox

https://www.xmox.nl/ is the best solution i believe, it does everything, and then, prints your DNS zone config to copy paste, saving you another hour of setting up email DNS records. Check out the video!

## Roadblock; email anti-spam culture

> Heads-up for Hetzner Cloud: outbound SMTP port 25 & 465 are blocked by default for ~the first month. You can request an unblock in the Cloud Console after your first invoice, or temporarily relay through a smarthost (I show how below).

If you already paid an https://hetzner.com invoice, unblock outbound port `25` and `465` at https://console.hetzner.com/limits by sending this message, the automated system will instantly unblock them.

```
Please unblock outbound SMTP ports 25 and 465 for my Cloud project.
Use case: personal email for domain $MAIL_HOSTNAME running mox on Debian 13.
IPs: $IP and IPv6: $IP6.
Volume: low (personal mail only, no marketing/bulk).
Anti-abuse: SPF, DKIM, DMARC, and PTR/rDNS will be configured; no open relays; submission on 587 for clients.
```

## The Chosen One `mox`

Unfortunally, `mox` does not set up `unbound` for DNSSEC, you still have to do that manually, which takes about half an hour. there is no mail stack package afaik that also takes care of this.

Just keep hacking the box, and rerun `mox quickstart -hostname $EMAIL_SERVER $EMAIL` whenever something starts working. keep updating DNS with the new 

```bash
EMAIL="hermann@stanew.name"
EMAIL_SERVER="host.stanew.name"
IP4="142.132.230.251"
IP6="2a01:4f8:c17:7b0c::/64"
```

install dependencies

```bash
hostnamectl set-hostname $EMAIL_SERVER
apt update
apt install golang-go ca-certificates unzip ufw unbound --yes
```

install mox

```bash
GOBIN=/usr/local/bin CGO_ENABLED=0 go install github.com/mjl-/mox@latest
useradd -m -d $HOME/mail mox
```

If outbound port `25` and `465` are not yet unblocked on the VPS also pass `-skipdial`.

```bash
cd ~/mail/
mox quickstart -hostname $EMAIL_SERVER $EMAIL
mox config dnsrecords stanew.name > name.zone
```

the nightmare continues

- fix: hostname, hostname cloud config, and DNS resolving
- Enable DNSSEC
- un-firewall `ufw` ports, **dont** firewall SSH (go back to the beginning, do not collect 200)
- link systemd unit file, enable and start the service
   - link binary into PWD
- upload DNS, drop records that chatgpt suggested
- Set PTR/rDNS at Hetzner to servername for both IPs.
- fix directory perms `mox:mox`
- edit `mox` configs, patch DNS, flush `unbound`, etc etc

## todo

and we are still not done, namecheap somehow struggles with DNS

- Publish DANE/TLSA
- Tighten MTA-STS policy (`testing` -> `enforce`)
- Debian enable unattended upgrades
