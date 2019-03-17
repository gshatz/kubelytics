ARG DOCKER_USER
ARG HADOOP_VERSION
ARG SPARK_VERSION
FROM ${DOCKER_USER}/spark:${SPARK_VERSION}-${HADOOP_VERSION}

ARG MIRROR="http://archive.apache.org/dist"
ARG ZEPPELIN_VERSION="0.7.3"
ARG ZEPPELIN_BIN="${MIRROR}/zeppelin/zeppelin-${ZEPPELIN_VERSION}/zeppelin-$ZEPPELIN_VERSION-bin-all.tgz"
ARG ART_URL="http://iguazio_ro:AP6zvYnUy6UqWtgfE7uidNWhFF5@artifactory.iguazeng.com:8081/artifactory"

ENV PYTHONPATH "$SPARK_HOME/python"

USER root

ENV ZEPPELIN_HOME "/zeppelin"
ENV SPARK_3RD_PARTY "$SPARK_HOME/3rd_party"

RUN set -x && \
    mkdir $ZEPPELIN_HOME && \
    wget --quiet $ZEPPELIN_BIN && \
    tar xzf zeppelin-$ZEPPELIN_VERSION-bin-all.tgz && \
    rm -rf zeppelin-$ZEPPELIN_VERSION-bin-all.tgz && \
    mv zeppelin-$ZEPPELIN_VERSION-bin-all/* $ZEPPELIN_HOME && \
    rm -rf zeppelin-$ZEPPELIN_VERSION-bin-all && \
    for interperter in "alluxio" "flink" "scio" "pig" "ignite" ; do rm -rf ${interperter}; done; \
    chown -R ${CONTAINER_USER}:${CONTAINER_USER} $ZEPPELIN_HOME && \
    mkdir -p $SPARK_3RD_PARTY && cd $SPARK_3RD_PARTY && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/mssql-jdbc-7.2.1.jre8.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/mysql-connector-java-8.0.13.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/ojdbc8.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/postgresql-42.2.5.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/spark-redshift_2.10-3.0.0-preview1.jar && \
    chown -R ${CONTAINER_USER}:${CONTAINER_USER} $SPARK_3RD_PARTY


WORKDIR $ZEPPELIN_HOME

ADD docker-zeppelin.sh /
RUN chmod +x /docker-* && chown -R ${CONTAINER_USER}:${CONTAINER_USER} /docker-*

USER $CONTAINER_USER
