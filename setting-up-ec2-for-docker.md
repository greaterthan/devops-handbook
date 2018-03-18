# Setting up EC2 instance

* Add ssh keys to `.ssh/autohorized_keys` for standard user `ubuntu`
* Install docker \(see below\)
* Install SSL certificates \(see below\)

## Install docker and setup a swarm

First [Install docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce)

Then initialize the swarm

```
sudo docker swarm init
```

## Install SSL certificates using certbot

Use the instructions from [certbot](https://certbot.eff.org/#ubuntuxenial-nginx)

Use the `certonly`subcommand

As the mail address for notifications, use `devops@greater.finance`

Add HTTP2.0 capability by following [these instructions](https://www.bjornjohansen.no/enable-http2-on-nginx).



