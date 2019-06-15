FROM metabase/metabase:v0.32.8 as prebuilt

FROM balenalib/raspberry-pi-alpine-openjdk as runner

RUN [ "cross-build-start" ]

WORKDIR /app

ENV FC_LANG en-US
ENV LC_CTYPE en_US.UTF-8

# add fixed cacerts
COPY --from=prebuilt /usr/lib/jvm/default-jvm/jre/lib/security/cacerts /usr/lib/jvm/default-jvm/jre/lib/security/cacerts
COPY --from=prebuilt /app/ /app/

# create the plugins directory, with writable permissions
RUN apk add --update bash ttf-dejavu fontconfig \
    && mkdir -p /plugins \
    && chmod a+rwx /plugins

RUN [ "cross-build-end" ]

# expose our default runtime port
EXPOSE 3000

# run it
ENTRYPOINT ["/app/run_metabase.sh"]
