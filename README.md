# Docker Swarm Example

SSH into the first node via `vagrant ssh`, then call this to initialize it as the first node in the Swarm:

```
sudo docker swarm init --advertise-addr 192.168.2.3
sudo docker swarm join-token manager
```

Copy the command you got from calling `docker swarm join-token` and run it on *node2*. Then, on *node2*, run:

```
cd /vagrant
sudo docker stack deploy --compose-file ./docker-compose.yml web
```

This will start a .NET Core MVC web application that can be accesses at 192.168.2.3:8080 or 192.168.2.4:8080.