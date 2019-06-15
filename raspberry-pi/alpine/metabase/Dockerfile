FROM metabase/metabase as prebuilt

FROM balenalib/raspberry-pi-alpine-openjdk as runner

RUN [ "cross-build-start" ]

WORKDIR /app

ENV FC_LANG en-US
ENV LC_CTYPE en_US.UTF-8

# dependencies
RUN apk add --update bash ttf-dejavu fontconfig

# add fixed cacerts
COPY --from=prebuilt /usr/lib/jvm/default-jvm/jre/lib/security/cacerts /usr/lib/jvm/default-jvm/jre/lib/security/cacerts

# add Metabase script and uberjar
RUN mkdir -p bin target/uberjar
COPY --from=prebuilt /app/target/uberjar/metabase.jar /app/target/uberjar/
COPY --from=prebuilt /app/bin/start /app/bin/

# create the plugins directory, with writable permissions
RUN mkdir -p /plugins
RUN chmod a+rwx /plugins

RUN [ "cross-build-end" ]

# expose our default runtime port
EXPOSE 3000

# run it
ENTRYPOINT ["/app/bin/start"]
