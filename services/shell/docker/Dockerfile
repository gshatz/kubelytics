ARG DOCKER_USER
ARG PRESTO_VERSION
ARG HADOOP_VERSION
ARG SPARK_VERSION
FROM $DOCKER_USER/presto:${PRESTO_VERSION}-${HADOOP_VERSION} as presto

FROM $DOCKER_USER/spark:${SPARK_VERSION}-${HADOOP_VERSION} as spark
ARG TTYD_VERSION
ARG ART_URL="http://iguazio_ro:AP6zvYnUy6UqWtgfE7uidNWhFF5@artifactory.iguazeng.com:8081/artifactory"

USER root

ENV SPARK_3RD_PARTY "$SPARK_HOME/3rd_party"

COPY --from=presto /presto/bin/presto /usr/local/bin/presto

RUN set -x && \
    wget --quiet https://github.com/tsl0922/ttyd/releases/download/$TTYD_VERSION/ttyd_linux.x86_64 \
    && mv ttyd_linux.x86_64 /usr/local/bin/ttyd \
    && chmod +x /usr/local/bin/ttyd && \
    mkdir -p $SPARK_3RD_PARTY && cd $SPARK_3RD_PARTY && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/mssql-jdbc-7.2.1.jre8.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/mysql-connector-java-8.0.13.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/ojdbc8.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/postgresql-42.2.5.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/spark-redshift_2.10-3.0.0-preview1.jar && \
    chown -R ${CONTAINER_USER}:${CONTAINER_USER} $SPARK_3RD_PARTY

EXPOSE 7681

USER ${CONTAINER_USER}

CMD [ "ttyd", "bash" ]