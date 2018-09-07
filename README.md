Simple static reverse proxy for Cloud Foundry
=============================================
This repo contains all files necessary to create a really simple, static and
stateless API-Gatway on top of Cloud Foundry. We use nginx’s ability to act as
reverse proxy.

The routes are defined in the server section and can point to other CF routes or
external routes alike:

```
  server {
    listen {{port}}; # This will be replaced by CF magic. Just leave it here.

    index index.html index.htm Default.htm;

    location /s1/ {
      proxy_pass http://s1.local.pcfdev.io/;
    }

    location /s2/ {
      proxy_pass http://s2.local.pcfdev.io/;
    }
  }
```

This is a minimal configuration. Other nginx options apply as usual.

Deployment
==========

Cloud Foundry offers an nginx buildpack: https://github.com/cloudfoundry/nginx-buildpack.git
The attached config file is derived from their example in fixtures/mainline.

The buildpack includes the server itself, so all you have to do is cd into the
directory where the config files are located (this repo) and run

`cf push api-gateway -b https://github.com/cloudfoundry/nginx-buildpack.git`

Of course you can give it any other name as well. Afterwards check `cf apps`,
copy the route and direct your browser (or HTTP client) to it. Your simplistic
API gateway is up and running. Since it is completely stateless, you can scale
it with `cf scale` to meet your requirements.

