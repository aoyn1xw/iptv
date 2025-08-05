# IPTV Server Setup

This VPS hosts two IPTV playlists using Nginx as a reverse proxy to the [iptv-org](https://github.com/iptv-org/iptv) public playlist repository.

---

## URLs

- **All Channels**

```
http://tv.ayon1xw.me/all.m3u
```

Loads all available channels from iptv-org.

- **Family Channels (Germany, Kosovo, Albania, Turkey)**

```
http://tv.ayon1xw.me/familytv.m3u
```
Loads only channels from:
- Germany (`de`)
- Kosovo (`xk`)
- Albania (`al`)
- Turkey (`tr`)

---

## Nginx Config File Location

- Path: `/etc/nginx/sites-available/iptv.conf`
- Linked to: `/etc/nginx/sites-enabled/iptv.conf`

---

## Nginx Config Snippet

```
server {
  listen 80;
  server_name tv.ayon1xw.me;

  # All channels
  location /all.m3u {
      proxy_pass https://iptv-org.github.io/iptv/index.m3u;
      proxy_set_header Host iptv-org.github.io;
      proxy_ssl_server_name on;
  }

  # Family channels (Germany, Kosovo, Albania, Turkey)
  location /familytv.m3u {
      proxy_pass https://iptv-org.github.io/iptv/index.country.m3u?country=de,xk,al,tr;
      proxy_set_header Host iptv-org.github.io;
      proxy_ssl_server_name on;
  }
}
