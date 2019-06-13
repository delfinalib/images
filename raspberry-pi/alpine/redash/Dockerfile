FROM balenalib/raspberry-pi-alpine-node as builder

ENV VERSION 7.0.0

RUN [ "cross-build-start" ]

RUN wget https://github.com/getredash/redash/archive/v${VERSION}.tar.gz \
    && tar -zxf v${VERSION}.tar.gz \
    && rm v${VERSION}.tar.gz \
    && apk add --no-cache git \
    && cd /redash-${VERSION} \
    && rm package-lock.json
RUN npm install \
    && npm run build \
    && rm -rf node_modules \
    && mv /redash-${VERSION} /redash

RUN [ "cross-build-end" ]

FROM balenalib/raspberry-pi-alpine-python:3.7

COPY --from=builder /redash /app

RUN [ "cross-build-start" ]

RUN apk add --no-cache \
        postgresql-dev \
        mariadb-dev \
        alpine-sdk \
        libffi-dev \
        python2-dev \
        musl-dev \
    && pip install cryptography==2.0.2 \
    && cd /app \
    && pip install -r requirements.txt \
                   -r requirements_dev.txt
# If we need more data source support
#                   -r requirements_all_ds.txt

RUN [ "cross-build-end" ]

ENTRYPOINT ["/app/bin/docker-entrypoint"]
CMD ["server"]
