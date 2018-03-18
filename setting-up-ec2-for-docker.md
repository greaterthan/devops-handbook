# Setting up EC2 instance

* Add ssh keys to `.ssh/autohorized_keys` for standard user `ubuntu`
* [Install docker-ce](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce)
* Disable Nginx \(see below\)
* Install SSL certificates \(see below\)

## Disable Nginx

```
sudo systemctl stop nginx 
sudo systemctl disable nginx 
```

## Install SSL certificates using certbot

Use the instructions from [certbot](https://certbot.eff.org/#ubuntuxenial-nginx)

As the mail address for notifications, use `devops@greater.finance`

Add HTTP2.0 capability by following [these instructions](https://www.bjornjohansen.no/enable-http2-on-nginx).



