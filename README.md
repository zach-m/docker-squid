# zachm/squid

## Docker Installation

Install `docker` according to the [online documentation](https://docs.docker.com/engine/installation/).

## Squid Installation

Download this repository:
```bash
git clone https://github.com/zach-m/docker-squid.git
```

Setup username and password for the proxy:
```bash
sudo apt-get install apache2-utils
mkdir /var/squid3
cp docker-squid/squid.conf /var/squid3
htpasswd -c /var/squid3/passwords username
```

Build the docker:
```bash
docker build -t zachm/squid docker-squid/
```

## Running the docker as an auto-restart daemon

```bash
docker run --name squid -d --restart=always \
  --publish 3128:3128 \
  --volume /var/squid3/squid.conf:/etc/squid3/squid.conf \
  --volume /var/squid3/passwords:/etc/squid3/passwords \
  --volume /srv/docker/squid/cache:/var/spool/squid3 \
  zachm/squid
```

## Changing configuration

Edit the configuration files at (your local folder):
``` bash
/var/squid3
```
Restart the docker
```bash
docker kill -s HUP squid
```

## Logs

To access the Squid logs, located at `/var/log/squid3/`, you can use `docker exec`. For example, if you want to tail the access logs:

```bash
docker exec -it squid tail -f /var/log/squid3/access.log
```

## Shell Access

For debugging and maintenance purposes you may want access the containers shell. If you are using Docker version `1.3.0` or higher you can access a running containers shell by starting `bash` using `docker exec`:

```bash
docker exec -it squid bash
```
