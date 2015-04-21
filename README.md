# vagrant-docker-registry

Run docker-registry locally:

```
$ export AWS_ACCESS_KEY_ID="..."
$ export AWS_SECRET_ACCESS_KEY="..."
$ export AWS_REGION="..."
$ export DOCKER_REGISTRY_AWS_BUCKET="..."
$ export DOCKER_REGISTRY_STORAGE_PATH="..."
$ vagrant up
$ vagrant ssh -c 'ip addr | grep inet | grep eth1'
    inet 172.28.128.3/24 brd 172.28.128.255 scope global eth1
$ curl 172.28.128.3
"\"docker-registry server\""
```

If you use boot2docker:

```
$ boot2docker ssh
docker@boot2docker:~$ echo '172.28.128.3 your-docker-registry' | sudo tee -a /etc/hosts
docker@boot2docker:~$ echo 'EXTRA_ARGS="--insecure-registry your-docker-registry:80"' | sudo tee -a /var/lib/boot2docker/profile
docker@boot2docker:~$ sudo /etc/init.d/docker restart
```

You can access your images:

```
$ docker pull your-docker-registry:80/your-repo:latest
```

