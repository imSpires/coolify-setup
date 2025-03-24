Sets up a new server with Coolify, decent security, and a few other things.

```bash
apt update && apt install git unzip -y && git clone https://github.com/BOOST-Creative/coolify-setup.git --depth 1 /tmp/cs && /tmp/cs/setup.sh
```

**Notes:**

- You should only use ed25519 keys to connect to the server via SSH.
- The server runs Watchtower to keep containers up to date, so it's a good idea to use version tags to keep things from breaking.

## Coolify setup

Create your account, then go to Servers > localhost > Configuration and change the port to your chosen port. This is a bit finicky. If the SSH settings show errors, try restarting the server. You may need to add the Coolify public key to `/root/.ssh/authorized_keys`. Or try `sudo systemctl restart docker`.

In the Proxy tab, stop the proxy and switch to use Caddy. You may need to reboot the server to get it to stick.

Then go to Settings > General and change the instance domain if you need to access it from outside the server.

## Firewall

This setup uses UFW with [ufw-docker](https://github.com/chaifeng/ufw-docker).

The firewall is configured to allow HTTP (port 80), HTTPS (port 443), and SSH (your chosen port).

If you need to allow ingress on other ports, do this:

```bash
sudo ufw allow <port>
sudo ufw route allow proto tcp from any to any port <port>
```

If you need to block an IP range, do this, but be careful not to block Cloudflare IPs:

```bash
ufw prepend deny from 45.135.232.0/24
ufw route prepend deny from 45.135.232.0/24
```

## TLS

We're using the Caddy proxy for Coolify because it's a little easier to run services with self-signed certs (`caddy_0.tls=internal`) and set up redirects. Generally we want to use a self-signed cert and strict SSL through Cloudflare so we don't need to worry about expiring certs.

If a service is not proxied through Cloudflare, removing `caddy_0.tls=internal` will generate a letsencrypt cert.

## TODO

- It would be nice to have a global WAF. We'd need to integrate it with caddy-docker-proxy, which Coolify uses, and make sure it uses the correct headers for Cloudflare.
