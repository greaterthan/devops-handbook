# Notes

Set up an appropriate instance. I've used a copy of the existing server running cobudget, a t2.small.

These notes are work in progress and not to be trusted!!!

## Install docker

``` 
apt-get update
apt-get install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install -y docker-ce
```

Make a `dbdump` directory to contain the database dumps we want to reload

Login to the relevant docker repo

Get the cobudget docker image

```
sudo docker pull 041454725975.dkr.ecr.us-west-2.amazonaws.com/cobudget
```

Create a network, start the database and the cobudget container

```
sudo docker network create --driver bridge cobudget
sudo docker run --network=cobudget --name=db -v /vagrant:/dbdump -e POSTGRES_DB=cobudget_development -e POSTGRES_USER=ubuntu -e POSTGRES_PASSWORD=<password> -d postgres:9.6
```

Load 


