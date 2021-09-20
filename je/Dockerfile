FROM openjdk:8-jre AS base
LABEL maintainer="PPB <pp.bariller@gmail.com>"

#Version of minecraft to use
ENV MC_VERSION=1.17.11

#Set inital workdir
WORKDIR /app

#Install required packages
RUN apt update && apt install --no-install-recommends -y \
        openssl \
        python && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

#Build image
FROM base AS build

#Install build-time only packages
RUN apt update && apt install -y \
        curl \
        python-pip && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

#Install mcstatus
RUN pip install mcstatus

#Download minecraft jar
RUN curl -L https://s3.amazonaws.com/Minecraft.Download/versions/${MC_VERSION}/minecraft_server.${MC_VERSION}.jar \
         -o ./minecraft.jar

#Runtime image
FROM base AS runtime

#Copy from build
COPY --from=build /usr/local/ /usr/local/
COPY --from=build /app /app

#Setup minecraft user
RUN useradd --user-group \
            --no-create-home \
            --home-dir /data \
            --shell /usr/sbin/nologin \
            minecraft

#Volumes
VOLUME /data /home/minecraft

#Ports
EXPOSE 25565 25575

#Setup healthcheck
HEALTHCHECK CMD mcstatus localhost ping

#User and group to run as
USER minecraft:minecraft

#Set runtime workdir
WORKDIR /data

ENTRYPOINT ["java"]
CMD ["-Xms1G", "-Xmx1G", "-jar", "/app/minecraft.jar", "nogui"]
