# docker-minecraft
Minecraft Server in Docker

je/ - Minecraft Jave Edition Server
nukkit/ - Nukkitx Bedrock Server
pmmp/ - Pocket Minecraft MP Server

#Build

```
docker build -t minecraft .
```

#Configure
Created a server.properties and pocketmine.yml configuration file in `data/` directory that is mounted into the container.

#Run

```
docker run -it --rm -p 19132:19132 -p 19132:19132/udp -v `pwd`/data:/data minecraft
```

#Using docker-compose

```
docker-compose build
docker-compose up -d
```
