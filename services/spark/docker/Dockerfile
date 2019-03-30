ARG DOCKER_USER
ARG HADOOP_VERSION
FROM $DOCKER_USER/hadoop:${HADOOP_VERSION}

ARG MIRROR="http://archive.apache.org/dist"
ARG SPARK_VERSION="spark-2.1.2"
ARG HADOOP_SPARK_VERSION="hadoop2.7"
ARG SPARK_BIN_NAME="${SPARK_VERSION}-bin-${HADOOP_SPARK_VERSION}"
ARG SPARK_BIN="${MIRROR}/spark/${SPARK_VERSION}/${SPARK_BIN_NAME}.tgz"
ARG S3_REPO_URL="https://iguazio-files.s3.amazonaws.com/spark-3rd-party"

USER root

ENV SPARK_HOME /spark
ENV PATH $PATH:${SPARK_HOME}/bin
ENV SPARK_3RD_PARTY "$SPARK_HOME/3rd_party"

RUN set -x && \
    mkdir $SPARK_HOME && cd $SPARK_HOME && \
    wget --quiet ${SPARK_BIN} && \
    tar xzf ${SPARK_BIN_NAME}.tgz && \
    rm -rf ${SPARK_BIN_NAME}.tgz && \
    mv ${SPARK_BIN_NAME}/* $SPARK_HOME/ && \
    rm -rf ${SPARK_BIN_NAME} && \
    mkdir -p $SPARK_3RD_PARTY && cd $SPARK_3RD_PARTY && \
    wget --quiet $S3_REPO_URL/mssql-jdbc-7.2.1.jre8.jar && \
    wget --quiet $S3_REPO_URL/mysql-connector-java-8.0.13.jar && \
    wget --quiet $S3_REPO_URL/ojdbc8.jar && \
    wget --quiet $S3_REPO_URL/postgresql-42.2.5.jar && \
    wget --quiet $S3_REPO_URL/spark-redshift_2.10-3.0.0-preview1.jar && \
    cd $SPARK_HOME/jars/ && \
    wget --quiet $S3_REPO_URL/asm-1.0-RC3.jar && \
    wget --quiet $S3_REPO_URL/json-smart-2.0-RC3.jar && \
    wget --quiet $S3_REPO_URL/jsonevent-layout-1.7.jar && \
    chown -R ${CONTAINER_USER}:${CONTAINER_USER} $SPARK_HOME

COPY log4j.properties $SPARK_HOME/conf/log4j.properties

ADD spark-master /
ADD spark-worker /

RUN chmod +x /spark-* && chown -R ${CONTAINER_USER}:${CONTAINER_USER} /spark-*

USER ${CONTAINER_USER}
