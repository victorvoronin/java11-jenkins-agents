FROM openshift/jenkins-slave-maven-centos7:v3.11

MAINTAINER Viktor Voronin <viktor_voronin@epam.com>

ENV GRADLE_VERSION=6.1.1

USER root

RUN curl -skL -o /tmp/gradle-bin.zip https://services.gradle.org/distributions/gradle-$GRADLE_VERSION-bin.zip && \
    mkdir -p /opt/gradle && \
    unzip -q /tmp/gradle-bin.zip -d /opt/gradle && \
    ln -sf /opt/gradle/gradle-$GRADLE_VERSION/bin/gradle /usr/local/bin/gradle

RUN yum remove java-1.8.0-openjdk-headless java-1.7.0-openjdk-headless java-1.7.0-openjdk java-1.7.0-openjdk-devel -y && \
    yum install java-11-openjdk-devel.x86_64 -y

RUN chown -R 1001:0 /opt/gradle && \
    chmod -R g+rw /opt/gradle

USER 1001

