# ![Duplicati logo small] Duplicati for FreeBSD

*Install Duplicati.*

[Duplicati logo small]:lib/images/duplicati-logo-small.png

**Only works with FreeBSD.**

![FreeBSD logo medium]

[FreeBSD logo medium]:lib/images/freebsd-logo-medium.png

## Requirements

Run:

- `ansible` >=  `2.7`

## Supported OS

- `FreeBSD` == `11.2`, `12`

## Quick start

Simple example for a standalone Duplicati instance (not recommanded):

```yaml
roles:
  - ansible-for-freebsd-role-duplicati

vars:
  duplicati__listen_interface: "0.0.0.0"  # Not recommanded for production use.
```

Full example with a Nginx reverse proxy (recommanded):

```yaml
roles:
  - ansible-for-freebsd-role-duplicati
  - geerlingguy.nginx

vars:
  nginx_vhosts:
    - listen: "80"
      server_name: duplicati
      extra_parameters: |
        return 301 https://$http_host$request_uri/duplicati;
    - listen: "443 ssl http2"
      server_name: duplicati_ssl
      extra_parameters: |
        location / {
          proxy_pass http://127.0.0.1:8200;
        }
        ssl_certificate           /path/to/cert.pem;
        ssl_certificate_key       /path/to/key.pem;
        ssl_protocols             TLSv1.3;
        ssl_prefer_server_ciphers off;
```

## Variables

|       **Variable**          | **Mandatory** | **Type**  |   **Example**   |
|----------------------------:|:-------------:|:---------:|:----------------|
| duplicati__listen_interface |      no       |  string   | 127.0.0.1       |
| duplicati__port             |      no       |  number   | 8200            |
| duplicati__user             |      no       |  string   | duplicati       |
| duplicati__user_uid         |      no       |  number   | (undefined)     |
