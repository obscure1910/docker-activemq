# syntax=docker/dockerfile:1
FROM openjdk:8-jre-alpine3.9

ENV ACTIVEMQ_VERSION 5.16.2
ENV ACTIVEMQ apache-activemq-$ACTIVEMQ_VERSION
ENV ACTIVEMQ_TCP=61616 ACTIVEMQ_AMQP=5672 ACTIVEMQ_STOMP=61613 ACTIVEMQ_MQTT=1883 ACTIVEMQ_WS=61614 ACTIVEMQ_UI=8161
ENV SHA512_VAL=27bb26786640f74dcf404db884bedffc0af4bfb2a0248c398044ac9a13e19ff097c590b79eb1404e0b04d17a8f85a8f7de87186a96744e19162d70b3c7a9bdde

ENV ACTIVEMQ_HOME /opt/activemq

RUN set -x && \
    mkdir -p /opt && \
    apk --update add --virtual build-dependencies curl && \
    curl http://archive.apache.org/dist/activemq/$ACTIVEMQ_VERSION/$ACTIVEMQ-bin.tar.gz -o $ACTIVEMQ-bin.tar.gz

RUN if [ "$SHA512_VAL" != "$(sha512sum $ACTIVEMQ-bin.tar.gz | awk '{print($1)}')" ];\
    then \
        echo "sha512 values doesn't match! exiting."  && \
        exit 1; \
    fi;

RUN tar xzf $ACTIVEMQ-bin.tar.gz -C /opt && \
    ln -s /opt/$ACTIVEMQ $ACTIVEMQ_HOME && \
    addgroup -S activemq && adduser -S -H -G activemq -h $ACTIVEMQ_HOME activemq && \
    chown -R activemq:activemq /opt/$ACTIVEMQ && \
    chown -h activemq:activemq $ACTIVEMQ_HOME && \
    apk del build-dependencies && \
    rm -rf /var/cache/apk/*

RUN sed -i 's/<property name="host" value="127.0.0.1"\/>/<property name="host" value="0.0.0.0"\/>/' ${ACTIVEMQ_HOME}/conf/jetty.xml

USER activemq

WORKDIR $ACTIVEMQ_HOME
EXPOSE $ACTIVEMQ_TCP $ACTIVEMQ_AMQP $ACTIVEMQ_STOMP $ACTIVEMQ_MQTT $ACTIVEMQ_WS $ACTIVEMQ_UI

CMD ["/bin/sh", "-c", "bin/activemq console"]
