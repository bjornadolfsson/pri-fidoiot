# Copyright 2022 Intel Corporation
# SPDX-License-Identifier: Apache 2.0

FROM ubuntu:22.04
ARG _JAVA_OPTIONS
ENV _JAVA_OPTIONS=${_JAVA_OPTIONS}
RUN apt-get update && apt-get install -y openjdk-17-jre wget --no-install-recommends

RUN useradd -ms /bin/bash fdo
WORKDIR /home/fdo/
RUN mkdir -p /home/fdo/lib /home/fdo/WEB-INF /home/fdo/app-data

# Setup service dependencies
COPY ./lib ./lib/
COPY ./WEB-INF ./WEB-INF
COPY log4j2.xml .
COPY aio.jar .
COPY hibernate.cfg.xml .
COPY service.yml .
COPY context.xml .
COPY tomcat-users.xml .
COPY secrets.run.path /home/fdo/secrets.path


RUN chown -R fdo:fdo /home/fdo
USER fdo

# Configure and start all-in-one
CMD ["/usr/lib/jvm/java-17-openjdk-amd64/bin/java", "-jar", "aio.jar"]

#Docker image healthcheck
HEALTHCHECK --interval=30s --timeout=30s CMD wget --spider -Y off http://localhost:8040/health || exit 1