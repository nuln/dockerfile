# caddy-docker

Docker image for [caddy](https://caddyserver.com/).

## build

```bash
docker build --build-arg VERSION=2.7.6 -t caddy .
```

## usage

```bash
docker run -d --name caddy \
    -p 80:80 -p 443:443 \
    -v /path/to/Caddyfile:/etc/Caddyfile \
    -v /path/to/certs:/root/.caddy \
    -v /path/to/www:/var/www \
    caddy
```


## plugins

* github.com/caddy-dns/cloudflare
* clevergo.tech/caddy-dnspodcn
* github.com/caddy-dns/alidns
* github.com/abiosoft/caddy-exec
* github.com/mholt/caddy-webdav
* github.com/klzgrad/forwardproxy