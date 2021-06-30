# DOCKER

## Install docker

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
apt-cache policy docker-ce
sudo apt-get install -y docker-ce
```

Add _ubuntu_ to docker user group

`sudo usermod -aG docker ubuntu`

Check the version (restart your session to apply usermod changes)

`docker version`

Enable autostart of docker

`sudo systemctl enable docker`

## Install docker-compose

```
sudo curl -L https://github.com/docker/compose/releases/download/1.28.5/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Check the version

`docker-compose version`

## Docker commands

### Clean up

#### Windows

- Delete all containers

  `for /f %i in ('docker ps -aq') do docker rm %i -f`

- Delete all volumes

  `for /f %i in ('docker volume ls -q') do docker volume rm %i -f`

- Delete all images

  `for /f %i in ('docker images -aq') do docker rmi %i -f`

#### Linux

- Delete all containers

  `docker rm $(docker ps -aq) -f`

- Delete all volumes

  `docker volume rm $(docker volume ls -q) -f`

- Delete all images

  `docker rmi $(docker images -aq) -f`

### Fix system clock issue

`docker run --rm --privileged ubuntu hwclock -s`

## Documentation

- docker: <https://docs.docker.com/get-started/>
- docker-compose: <https://docs.docker.com/compose/>
