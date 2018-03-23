FROM php:7.2-zts AS base
LABEL maintainer="Micheal Waltz <ecliptik@gmail.com>"

#Version of minecraft to use
ENV PMMP_VERSION=1.7dev-743

#Install required packages
RUN apt update && apt install --no-install-recommends -y \
        libevent-pthreads-2.0-5 \
        libyaml-0-2 \
        openssl && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

#Build image
FROM base AS build

#Install build-time only packages
RUN apt update && apt install -y \
        build-essential \
        zlib1g-dev \
        libyaml-dev \
        git \
        curl && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

#Install required modules
RUN docker-php-ext-install -j$(nproc) bcmath sockets json zip
RUN pecl install yaml

#Clone and build phthreads module from source since it's outdated in pecl
WORKDIR /src
RUN git clone https://github.com/krakjoe/pthreads.git --depth 1
RUN cd pthreads && phpize && ./configure && make && make install

#Switch to app dir for download
WORKDIR /app

#Download pmmp phar
RUN curl -L https://github.com/pmmp/PocketMine-MP/releases/download/${PMMP_VERSION}/PocketMine-MP.phar -o ./PocketMine-MP.phar

#Runtime image
FROM base AS runtime

#Copy from build
COPY --from=build /usr/local/ /usr/local/
COPY --from=build /app /app

#PHP config to enable modules
COPY ./php.ini /etc/php.ini
ENV PHPRC=/etc/php.ini

#Setup minecraft user
RUN useradd --user-group \
            --no-create-home \
            --home-dir /data \
            --shell /usr/sbin/nologin \
            minecraft

#Volumes
VOLUME /data /home/minecraft

#Ports
EXPOSE 19132

#Make phar owned by minecraft user
RUN chown -R minecraft:minecraft /app

#User and group to run as minecraft
USER minecraft:minecraft

#Set runtime workdir
WORKDIR /data

ENTRYPOINT ["php"]
CMD ["/app/PocketMine-MP.phar"]
