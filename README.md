Docker Redis
===========

Builds a docker image for redis. This is available as a [Trusted Build](http://blog.docker.io/2013/11/introducing-trusted-builds/) on the public docker index.

* Public Docker Index Link: [nisaacson/redis](https://index.docker.io/u/nisaacson/redis/)

# Usage

```bash
docker pull nisaacson/docker
ID=$(docker run -d -p 2222:22 -p 6379:6379 nisaacson/redis)
sleep "4s"
docker logs $ID
redis-cli -h localhost -p 6379
```

# Building


To build a docker image, use the included `Vagrantfile` to launch up a virtual machine with docker already installed. The Vagrantfile uses the [vagrant docker provisioner](http://docs.vagrantup.com/v2/provisioning/docker.html) to install docker

**Requirements**

* [Vagrant](http://www.vagrantup.com/)

To launch the vagrant virtual machine

```bash
cd /path/to/this/repo
vagrant up
```

Once the virtual machine is running you can test out the `Dockerfile` via

```bash
# log into the virtual machine
vagrant ssh
# go to the mounted shared folder
cd /vagrant

# build a docker image from the Dockerfile
docker build -t redis .

# ensure that the image exists, you should see the `redis` image in the list output
docker images

# run the container, mapping ports on the host virtual machine to the same ports inside the container
ID=$(docker run -d -p 2222:22 -p 6379:6379 redis)

# wait a few seconds and then check the logs on the container, you should see the output from redis starting up.
docker logs $ID
exit # quit the ssh session on the virtual machine
```

To verify that the server is running, use the `redis-cli` tool to connect to the redis server running inside a container. Execute the following on the host virtual machine

```bash
# log into the virtual machine from your host computer
vagrant ssh
sudo apt-get install -y redis-server # will install an old version of redis such as 2.2.2
echo "INFO" | xargs redis-cli | grep "redis_version"
# should output "redis_version:2.8.2"
exit
```


# Access Running container

If you need to check on the running redis server container, you can open a shell via ssh

```bash
ssh -p 2222 root@localhost
# password: redis
```
