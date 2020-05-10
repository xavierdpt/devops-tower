# devops-tower

In this project, we will create a full devops stack.

## Vagrant setup

```
vagrant init ubuntu/bionic64
```

Configure box to add more memory

```
vagrant up
vagrant ssh-config > ssh-config
ssh -F ssh-config -X default
```

## Install firefox

apt-get update
apt-get upgrade
apt-get install firefox

## Demo project

Install java

```
sudo apt-get install openjdk-11-jdk
```

Install eclipse and Spring Tools

Initialize demo project with a simple GET handler.

## Install docker

Follow recommended instructions on docker website for Ubuntu

## Create git server

docker run -it -p 2222:22 --name git ubuntu

```
apt-get update
apt-get upgrade
apt-get install openssh-server git
service ssh start
```

Create git user (as root then git in docker container):
```
adduser git
su git
cd
mkdir .ssh
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

Create git directory (as root in docker container)
```
mkdir /srv/git
chown git:git /srv/git
```

Create ssh key pair (as vagrant in vagrant box)
```
ssh-keygen -t rsa
scp -P 2222 .ssh/id_rsa.pub git@localhost:
```

Accept ssh public key (as git in docker container)
```
cat id_rsa.pub >>.ssh/authorized_keys
rm id_rsa.pub
```

Init project directory (as git in docker container)
```
cd /srv/git
mkdir demo-project.git
cd demo-project.git
git init --bare
```

Add existing project to source control (as vagrant in vagrant box)
```
cd ~/eclipse-workspace/demo
git init
git config user.name developer
git config user.email developer@localhost
git add .
git commit -m "Initial commit"
git remote add origin "ssh://git@localhost:2222/srv/git/demo-project.git/"
git push -u origin master
```

Stop ssh server, and leave container (as root in docker container)
```
service ssh stop
exit
```

Stop container, commit changes, set entrypoint to sshd server, and start container
```
sudo docker container ls -a # Check that git container is stopped
sudo docker commit -c 'CMD ["/usr/sbin/sshd"]' git git
sudo docker container rm git
sudo docker run -p 2222:22 git
```

TODO: Set up volume mounts to not have the mutable data part of the image.


