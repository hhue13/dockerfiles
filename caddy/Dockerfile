From alpine:latest
Maintainer thomas.hikade@gmail.com


# https://caddyserver.com/download/linux/amd64?plugins=http.forwardproxy,http.ipfilter,http.prometheus&license=personal
#ADD https://github.com/mholt/caddy/releases/download/v0.10.9/caddy_v0.10.9_linux_amd64.tar.gz /

ADD caddy_bin/caddy /

ADD Caddyfile /

RUN mkdir /data
ADD some_index.html /data

ENV HTTP_PORT=8080 

EXPOSE $HTTP_PORT

CMD [ "/caddy", "-conf", "/Caddyfile" ]
