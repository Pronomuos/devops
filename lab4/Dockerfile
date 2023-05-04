FROM alpine AS openssl

RUN apk add --no-cache openssl

WORKDIR /certs

COPY ../generate-certs.sh ./

CMD ["sh", "/certs/generate-certs.sh"]



FROM alpine:latest as build

ARG VERSION=1.23.3
ARG NGINX_DOWNLOAD_URL=http://nginx.org/download/nginx-${VERSION}.tar.gz

VOLUME /dependencies

WORKDIR /tmp
RUN apk --no-cache --update add build-base  \
    gnupg  \
    pcre-dev  \
    wget  \
    zlib-dev  \
    linux-headers  \
    libressl-dev  \
    zlib-static


RUN set -x  && wget -q $NGINX_DOWNLOAD_URL && tar -xf nginx-${VERSION}.tar.gz && echo ${VERSION}

WORKDIR /tmp/nginx-${VERSION}
RUN ./configure --with-ld-opt="-static" --with-http_ssl_module && make && make install &&  \
    strip /usr/local/nginx/sbin/nginx


RUN ln -sf /dev/stdout /usr/local/nginx/logs/acess.log && \
    ln -sf /dev/stderr /usr/local/nginx/logs/error.log

RUN cp -r /usr/local/nginx /dependencies

FROM scratch as app

VOLUME /dependencies

COPY --from=build /dependencies /usr/local/nginx
COPY --from=build /etc/passwd /etc/group /etc/

STOPSIGNAL SIGQUIT

EXPOSE 7070 443

ENTRYPOINT ["/usr/local/nginx/sbin/nginx"]

CMD ["-g", "daemon off;"]