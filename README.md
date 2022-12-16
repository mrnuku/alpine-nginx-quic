# alpine-nginx-quic
**nginx quic http3 with libressl**

*use this release for testing purposes only*

## installation

```
curl -LO https://github.com/mrnuku/alpine-nginx-quic/releases/download/v1.23.2-r0/nginx-1.23.2-r0.apk
apk add --allow-untrusted nginx-1.23.2-r0.apk
```

## setup

nginx.conf

*this can also go in any vhost if u dont like it to work globally*

```
http {
    # 0-RTT QUIC connection resumption
    ssl_early_data on;

    # Add Alt-Svc header to negotiate HTTP/3.
    add_header alt-svc 'h3=":443"; ma=86400; h3-27=":443"; ma=86400, h3-28=":443"; ma=86400, h3-29=":443"; ma=86400';
    add_header QUIC-Status $http3;     # Sent when QUIC was used
}
```
*nginx will give you funky errors if u use reuseport in any sub vhost*

default vhost
```
server {
    listen 443 http3 reuseport;
    listen [::]:443 http3 reuseport;
    listen [::]:443 ssl http2 default_server;
    listen 443 ssl http2 default_server;
}
```

other vhost
```
server {
    listen 443 http3;
    listen [::]:443 http3;
    listen [::]:443 ssl http2 ipv6only=on;
    listen 443 ssl http2;
}
```
