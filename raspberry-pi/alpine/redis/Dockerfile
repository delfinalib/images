FROM balenalib/raspberry-pi-alpine

RUN apk add --no-cache redis && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /data/redis && \
    chown -R redis:redis /data/redis

COPY redis*.conf /etc/

EXPOSE 6379
CMD ["redis-server", "/etc/redis.conf"]
