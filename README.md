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

## Demo project

Install java

```
sudo apt-get install openjdk-11-jre
```

Install eclipse and Spring Tools

Initialize demo project with a simple GET handler.

## Install docker

Follow recommended instructions on docker website for Ubuntu

## Create git server

docker run -it -p 2222:22 --name git ubuntu

Create git user, initialize keys.
