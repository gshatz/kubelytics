ARG DOCKER_USER
ARG HADOOP_VERSION
FROM $DOCKER_USER/hadoop:${HADOOP_VERSION}

ARG MIRROR="http://archive.apache.org/dist"
ARG SPARK_VERSION="spark-2.1.2"
ARG HADOOP_SPARK_VERSION="hadoop2.7"
ARG SPARK_BIN_NAME="${SPARK_VERSION}-bin-${HADOOP_SPARK_VERSION}"
ARG SPARK_BIN="${MIRROR}/spark/${SPARK_VERSION}/${SPARK_BIN_NAME}.tgz"
ARG ART_URL="http://iguazio_ro:AP6zvYnUy6UqWtgfE7uidNWhFF5@artifactory.iguazeng.com:8081/artifactory"

USER root

ENV SPARK_HOME /spark
ENV PATH $PATH:${SPARK_HOME}/bin
ENV SPARK_3RD_PARTY "$SPARK_HOME/3rd_party"

RUN set -x && \
    mkdir $SPARK_HOME && \
    wget --quiet ${SPARK_BIN} && \
    tar xzf ${SPARK_BIN_NAME}.tgz && \
    rm -rf ${SPARK_BIN_NAME}.tgz && \
    mv ${SPARK_BIN_NAME}/* $SPARK_HOME/ && \
    rm -rf ${SPARK_BIN_NAME} && \
    chown -R ${CONTAINER_USER}:${CONTAINER_USER} $SPARK_HOME && \
    mkdir -p $SPARK_3RD_PARTY && cd $SPARK_3RD_PARTY && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/mssql-jdbc-7.2.1.jre8.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/mysql-connector-java-8.0.13.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/ojdbc8.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/postgresql-42.2.5.jar && \
    wget --quiet $ART_URL/iguazio_mvn/io/iguaz/apps/spark-redshift_2.10-3.0.0-preview1.jar && \
    chown -R ${CONTAINER_USER}:${CONTAINER_USER} $SPARK_3RD_PARTY

ADD spark-master /
ADD spark-worker /

RUN chmod +x /spark-* && chown -R ${CONTAINER_USER}:${CONTAINER_USER} /spark-*

USER ${CONTAINER_USER}
