---
layout: docs
title: Setting up Caddy
permalink: /docs/proxy/caddy/
---

The following configuration works for HTTPS (with an HTTP redirection).

> **NOTE:** Make sure you follow the [prerequisites](/docs/proxy/prerequisites/).

#### Caddy v1 configuration

Create a new virtual host file (assumes `/etc/caddy/caddy.conf` is your main file, and includes all `*.conf` files in `/etc/caddy/caddy.conf.d/` directory):

```
sudo nano /etc/caddy/caddy.conf.d/airsonic.conf
```

Paste the following configuration in the virtual host file:

```caddy
airsonic.mydomain.com {
    proxy / 127.0.0.1:8080 {
        transparent
    }
}
```

Check the Caddy config for validity, and then restart the Caddy service:

```bash
caddy -conf /etc/caddy/caddy.conf -validate
sudo systemctl restart caddy.service
```

#### Caddy v2 configuration

Create or use an existing `Caddyfile` and add the following configuration:

```caddy
airsonic.example.com {
    encode zstd gzip
    reverse_proxy airsonic:4040
}
```

Here is another example of using a context path (in this case `/airsonic`) and subfolder instead of subdomain. You'll need to make sure that your context path matches the subfolder.

```caddy
localhost example.com {
    encode zstd gzip
    reverse_proxy /airsonic/* 192.168.1.101:4040
}
```

> **NOTE:** Feel free to remove or add other settings, as `encode zstd gzip` is not necessary.

To validate the `Caddyfile`, use `caddy validate --config <path>`. If you don't specify the `config` option, then it will look for the `Caddyfile` in the current working directory.

Restart caddy to take on the effects:

```bash
sudo systemctl restart caddy.service
```
