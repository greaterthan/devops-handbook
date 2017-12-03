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

(Please note this database will not store data after the container crashes. Should be fixed)
(Please also note it uses user ubuntu, which is foolish. It might as well use a user `cobudget` so it has the same owner as the production database)

Given the `dbdump` directory has a database dump called `cobudget-prod-171129`, restore this to the database

```
sudo docker exec -it db psql -U ubuntu -d cobudget_development -f /dbdump/cobudget-prod-171129
``` 

Start the cobudget container, start delayed jobs and mailcatcher

```
sudo docker run --network=cobudget --name=cobudget -e DATABASE_URL=postgres://ubuntu:pw@db/cobudget_development -p 3000:3000 -p 1080:1080 -d cobudget rails s -b 0.0.0.0
sudo docker exec -it cobudget bin/delayed_job start
sudo docker exec -it cobudget mailcatcher --http-ip 0.0.0.0
```

Cobudget in now running on port 3000 and mailcatcher on port 80

## Installing and running kong from a container

Start the kong database and run migrations

```
sudo docker run -d --name kong-database -p 5432:5432 -e "POSTGRES_USER=kong" -e "POSTGRES_DB=kong" postgres:9.6
sudo docker run --rm --link kong-database:kong-database     -e "KONG_DATABASE=postgres" -e "KONG_PG_HOST=kong-database" -e "KONG_CASSANDRA_CONTACT_POINTS=kong-database" kong:latest kong migrations up
```

(Please note this database will not store data after the container crashes. Should be fixed)

(Please also not this command use links, which is no longer recommended)

Start kong on the host network and make it listen on port 80

```
sudo docker run -d --name kong --network=host -e KONG_DATABASE=postgres -e KONG_PG_HOST=localhost -e KONG_PROXY_LISTEN=0.0.0.0:80 kong:latest
```

Register the cobudget api

```
curl -s -X POST --url http://localhost:8001/apis --data 'name=cobudget-api' --data 'uris=/cb' --data 'upstream_url=http://localhost:3000'
```

If the kong server is restarted and the database still runs, it will have the API registered in the database.

Cobudget is now accessible through this server on `/cb`

(This path can be used for branch-name)

## Mailcatcher

Mailcatcher can be accessed through port 1080. However if we map to a path like this

```
curl -i -X POST --url http://localhost:8001/apis/ --data 'name=mailcatcher' --data 'uris=/mail' --data 'upstream_url=http://localhost:1080'
```

We do get access to `mailcatcher`, but it doesn't work properly because it thinks it's running in the root.
