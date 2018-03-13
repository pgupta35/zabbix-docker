Zabbix-Docker
===========================================

Zabbix Docker monitoring integration. Requires cadvisor.

# CAdvisor

```bash
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --volume=/dev/kmsg:/dev/kmsg \
  --publish=127.0.0.1:4560:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

For AWS ECS
```
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/cgroup:/sys/fs/cgroup:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --volume=/dev/disk/:/dev/disk:ro \
  --volume=/dev/kmsg:/dev/kmsg \
  --publish=127.0.0.1:4560:8080 \
  --detach=true \
  --name=cadvisor \
  google/cadvisor:latest
```

# Installation

```
go get
go build
sudo mv zabbix-docker /usr/local/bin
```

# Configuration

```
mkdir ~/.zabbix-docker
cp config.example.yml ~/.zabbix-docker/config.yml
```
Edit ~/.zabbix-docker/config.yml

## On Zabbix UI

- Configuration > Action > Auto registration
Add auto-registration based on host metadata "DHost", Operations: add
  host, Link to templates = Template DHost

# Running

Add to crontab

```
5/* * * * * /usr/local/bin/zabbix-docker -z >/dev/null 2>&1
```
