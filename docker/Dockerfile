# syntax=docker/dockerfile:1
# vim: set filetype=dockerfile
FROM alpine AS builder

ARG APP='null'
ARG VERSION='0'

LABEL org.opencontainers.image.title="Static API Simulator Bundle"
LABEL org.opencontainers.image.description="Minimalist image with NGINX and BusyBox, built from Debian to simulate an API or serve static content with embedded metadata."
LABEL org.opencontainers.image.architecture="${TARGETARCH}"
LABEL org.opencontainers.image.supported.architectures="amd64,arm32v6,arm32v7,arm64v8,i386,ppc64le,riscv64,s390x"
LABEL org.opencontainers.image.supported.platforms="amd64,arm32v6,arm32v7,arm64v8,i386,mips64le,ppc64le,riscv64,s390x,windows-amd64"
LABEL org.opencontainers.image.platform="linux/${TARGETARCH}"
LABEL org.opencontainers.image.source="https://github.com/edelux/django"
LABEL org.opencontainers.image.url="https://github.com/edelux/django"
LABEL org.opencontainers.image.version="${VERSION}"
LABEL org.opencontainers.image.authors="Ernie D'lux (edelux) EDH <edeluquez@hotmail.com>"
LABEL org.opencontainers.image.licenses="MIT"
LABEL org.opencontainers.image.created="$(date -u +%Y-%m-%dT%H:%M:%SZ)"
LABEL org.opencontainers.image.documentation="https://github.com/edelux/django#readme"
LABEL org.opencontainers.image.vendor="edelux"
LABEL org.opencontainers.image.ref.name="${APP}-${VERSION}"

RUN apk --update --no-cache upgrade && apk --no-cache add nginx sed && \
    rm -fr /var/www/localhost /etc/nginx/modules* /etc/nginx/http.d && mkdir -p /etc/nginx/conf.d && \
    mkdir /scratch && printf '{"data":{"app":"'$APP'","version":"'$VERSION'"}}' >/var/www/index.json && \
    tar cf - /run/nginx /usr/sbin/nginx /var/log/nginx /var/lib/nginx /usr/share/nginx /etc/nginx /var/www/ /etc/group \
       /lib/ld-musl-*.so* /usr/lib/libpcre.so* /usr/lib/libssl.so* /usr/lib/libcrypto.so* /usr/lib/libz.so* /usr/lib/libpcreposix* /lib/libc.musl* \
          | (cd /scratch; tar xfp -)

COPY nginx.conf /scratch/etc/nginx/nginx.conf
COPY nginx.default /scratch/etc/nginx/conf.d/default.conf

RUN egrep 'root|^bin|daemon|guest|nginx' /etc/passwd >>/scratch/etc/passwd && \
    egrep 'root|^bin|daemon|guest|nginx' /etc/shadow >>/scratch/etc/shadow && \
    sed -i 's/www-data/nginx/' /scratch/etc/nginx/nginx.conf

FROM scratch
COPY --from=builder /scratch /

HEALTHCHECK CMD ["/usr/sbin/nginx", "-t"] || exit 1
ENTRYPOINT ["/usr/sbin/nginx"]
CMD ["-g", "daemon off;"]
