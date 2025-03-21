Sets up a new server with Coolify, decent security, and a few other things.

```bash
apt update && apt install git unzip -y && git clone https://github.com/BOOST-Creative/coolify-setup.git --depth 1 /tmp/cs && /tmp/cs/setup.sh
```

You should only use ed25519 keys to connect to the server via SSH.

## Coolify setup

The Coolify GUI should be accessible at http://localhost:8000 when connected via SSH.

Create your account, then go to Servers > localhost > Configuration and change the port to your chosen port.

If the SSH settings show errors, try restarting the server. You may need to add the Coolify public key to `/root/.ssh/authorized_keys`.

In the Proxy tab, stop the proxy and switch to use Caddy.

Then go to Settings > General and change the instance domain if you need to access it from outside the server.

## Firewall

This setup uses UFW with [ufw-docker](https://github.com/chaifeng/ufw-docker).

The firewall is configured to allow HTTP (port 80), HTTPS (port 443), and SSH (your chosen port).

If you need to allow ingress on other ports, do this:

```bash
sudo ufw allow <port>
sudo ufw route allow proto tcp from any to any port <port>
```

## TODO

- It would be nice to have a global WAF. We'd need to integrate it with caddy-docker-proxy, which Coolify uses, and make sure it uses the correct headers for Cloudflare.
