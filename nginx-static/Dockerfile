FROM alpine:3.21.2@sha256:56fa17d2a7e7f168a043a2712e63aed1f8543aeafdcee47c58dcffe38ed51099 AS build

WORKDIR /build

#Install build dependencies
RUN apk update && \
    apk add git mercurial curl g++ make linux-headers pcre-dev zlib-dev perl

#Clone nginx repo and switch to latest version
RUN hg clone https://hg.nginx.org/nginx/ \
        && hg pull -R nginx \
        && hg update -R nginx \
        && REV=$(hg tags -R nginx | grep -o ^release-[0-9\.]* | sort -V | tail -n 1) \
        && hg update -R nginx -r $REV

#Fetch additional modules
WORKDIR /build/modules
RUN wget -qO- $(curl -s https://api.github.com/repos/aperezdc/ngx-fancyindex/releases/latest | grep "tarball_url" | cut -d\" -f4) | tar xz


#Build nginx
WORKDIR /build/nginx
RUN MODULEARGS=$(ls /build/modules/ | sed -e 's/^/--add-module=\/build\/modules\//') \
        && ./auto/configure \
                --with-cc-opt='-O2 -ffile-prefix-map=/build/nginx=. -fstack-protector-strong -Wformat -Werror=format-security -Wdate-time -D_FORTIFY_SOURCE=2' \
                --with-ld-opt=-Wl,-z,relro \
                --prefix=/usr/local/nginx \
                --conf-path=/etc/nginx/nginx.conf \
                --http-log-path=/dev/stdout \
                --error-log-path=/dev/stderr \
                --lock-path=/var/lock/nginx.lock \
                --pid-path=/run/nginx.pid \
                --http-client-body-temp-path=/var/lib/nginx/body \
                --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
                --http-proxy-temp-path=/var/lib/nginx/proxy \
                --http-scgi-temp-path=/var/lib/nginx/scgi \
                --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
                --with-file-aio \
                --with-threads \
                --with-pcre-jit \
                --without-http_charset_module \
                --without-http_gzip_module \
                --without-http_ssi_module \
                --without-http_userid_module \
                --without-http_access_module \
                --without-http_auth_basic_module \
                --without-http_mirror_module \
                --without-http_autoindex_module \
                --without-http_geo_module \
                --without-http_map_module \
                --without-http_split_clients_module \
                --without-http_referer_module \
                --without-http_proxy_module \
                --without-http_fastcgi_module \
                --without-http_uwsgi_module \
                --without-http_scgi_module \
                --without-http_grpc_module \
                --without-http_memcached_module \
                --without-http_limit_conn_module \
                --without-http_limit_req_module \
                --without-http_empty_gif_module \
                --without-http_browser_module \
                --without-http_upstream_hash_module \
                --without-http_upstream_ip_hash_module \
                --without-http_upstream_least_conn_module \
                --without-http_upstream_random_module \
                --without-http_upstream_keepalive_module \
                --without-http_upstream_zone_module \
                --without-mail_pop3_module \
                --without-mail_imap_module \
                --without-mail_smtp_module \
                $MODULEARGS \
        && make -j \
        && make -j install

#Final Container
FROM alpine:3.21.2@sha256:56fa17d2a7e7f168a043a2712e63aed1f8543aeafdcee47c58dcffe38ed51099

#Install runtime dependencies, create user/group and create directories
RUN apk --no-cache add pcre && mkdir -p /var/lib/nginx && mkdir -p /etc/nginx/conf.d;

#Copy files
COPY --from=build /usr/local/nginx /usr/local/nginx
COPY --from=build /etc/nginx /etc/nginx

RUN echo -e "\
worker_processes 1;\
events {\
  worker_connections 1024;\
}\
\
http {\
    include       mime.types;\
    default_type  application/octet-stream;\
    sendfile        on;\
    keepalive_timeout  65;\
    \
    server {\
        listen       80;\
        root   /var/www/\$host;\
    }\
    \
    include /etc/nginx/conf.d/*.conf;\
}\
" > /etc/nginx/nginx.conf

#Config
EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]
