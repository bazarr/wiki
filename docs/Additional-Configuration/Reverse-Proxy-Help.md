# Reverse Proxy Help

## Using Nginx and /bazarr/ base url

 ```php
 location /bazarr/ {
    proxy_pass              http://127.0.0.1:6767/bazarr/;
    proxy_set_header        X-Real-IP               $remote_addr;
    proxy_set_header        Host                    $http_host;
    proxy_set_header        X-Forwarded-For         $proxy_add_x_forwarded_for;
    proxy_set_header        X-Forwarded-Proto       $scheme;
    proxy_http_version      1.1;
    proxy_set_header        Upgrade                 $http_upgrade;
    proxy_set_header        Connection              "Upgrade";
    proxy_redirect off;
    # Allow the Bazarr API through if you enable Auth on the block above
    location /bazarr/api {
        auth_request off;
        proxy_pass http://127.0.0.1:6767/bazarr/api;
    }
 }
 ```

## Using Apache 2.3.12 or greater and /bazarr/ base url

> *Apache 2.3.12 or greater is required to support `AllowEncodedSlashes NoDecode` which is required for Sonarr/Radarr config testing.*

```php
<IfModule mod_ssl.c
<VirtualHost *:443
        ServerAdmin webmaster@localhost
        ServerName localhost
        AllowEncodedSlashes NoDecode


        <Proxy *
                Order deny,allow
                Allow from all
                Satisfy Any
        </Proxy

    ProxyPass "/bazarr/ "http://127.0.0.1:6767/bazarr/"
    ProxyPassReverse "/bazarr/" "http://127.0.0.1:6767/bazarr/"

</VirtualHost
```

### Using Authelia authentication

> *Note: The default buffer_size is 4096, double that seems to fix any loading issues with Bazarr.*

```bash
access_control:
  default_policy:
  rules:
    - domain:
        - bazarr.<domain>.com
      resources:
        - '^/api/.*$'
      policy: bypass

server:
  read_buffer_size: 8192
  write_buffer_size: 8192
  path: authelia
```

### Dockers

Use the [LinuxServer SWAG container](https://hub.docker.com/r/linuxserver/swag/), it has already pre-configured `.conf` files for subfolder and subdomain to make is easy.
