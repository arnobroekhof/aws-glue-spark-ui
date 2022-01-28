# syntax=docker/dockerfile:1
FROM --platform=$BUILDPLATFORM amazonlinux:2
FROM --platform=$BUILDPLATFORM amazoncorretto:8
FROM --platform=$BUILDPLATFORM maven:3.6-amazoncorretto-8

RUN yum install -y procps

WORKDIR /tmp/
ADD pom.xml /tmp
RUN curl -o ./spark-2.4.3-bin-without-hadoop.tgz https://archive.apache.org/dist/spark/spark-2.4.3/spark-2.4.3-bin-without-hadoop.tgz
RUN tar -xzf spark-2.4.3-bin-without-hadoop.tgz && \
    mv spark-2.4.3-bin-without-hadoop /opt/spark && \
    rm spark-2.4.3-bin-without-hadoop.tgz
RUN mvn dependency:copy-dependencies -DoutputDirectory=/opt/spark/jars/
RUN rm /opt/spark/jars/servlet-api-2.5.jar && \
    rm /opt/spark/jars/jsr305-1.3.9.jar && \
    rm /opt/spark/jars/jersey-*-1.9.jar

RUN echo $'\n\
spark.eventLog.enabled                      true\n\
spark.history.ui.port                       18080\n\
' > /opt/spark/conf/spark-defaults.conf

ENTRYPOINT ["/bin/bash", "-c"]:
