# docker-minecraft
Minecraft Server in Docker

#Build

```
docker build -t pmmp .
111

#Configure
Created a server.properties and pocketmine.yml configuration file in `data/` directory that is mounted into the container.

#Run

```
docker run -it --rm -p 19132:19132 -p 19132:19132/udp -v `pwd`/data:/data pmmp:latest
```

#Using docker-compose

```
docker-compose build
docker-compose up -d
```
