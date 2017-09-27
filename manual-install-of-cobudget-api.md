# Manual install of cobudget-api

## Introduction

The server will be set up to use blue/green deployments. This will done by making two directories, incidentally named `blue` and `green` and set a symbolic link `current` that points to the current release in production.

A new release can then be prepared in the directory not currently in use and the change can happen almost instantanously.

Rollback to the previous version can happen equally fast \(if there are no incompatible database changes\).

## First time install

Create directories, adjust permissions and clone the `cobudget-api` github repo.

```
cd /opt
sudo mkdir cobudget-api
sudo chown ubuntu cobudget-api
cd cobudget-api
mkdir green
git clone https://github.com/cobudget/cobudget-api.git blue
ln -s /opt/cobudget-api/blue current
```

Get the `env` and `start` files and copy to the installed dir

```
cd
git clone https://github.com/greaterthan/aws-deploy.git
cp aws-deploy/env aws-deploy/start /opt/cobudget-api/current
```

git clone [https://github.com/greaterthan/aws-deploy.git](https://github.com/greaterthan/aws-deploy.git)

Now edit the `env`  and `env-vars`  file. The database variable could be

```
DATABASE_URL="postgres://cobudget:<password>@cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com:5432/cobudget_prod"
```

Install missing Postgresql dev files and install all packages

```
sudo apt-get install postgresql-common libpq-dev
/usr/local/lib/ruby-2.4.0/bin/bundle install
```

### Brief notes on new deployment

* Remove the unused blue of green dir
* Clone the api repo into the unused blue or green dir
* Copy `env` , `env-vars` , `start`  and `config/database.yml` to the new dir

And finally, change the symlink and restart the service

```
rm current; ln -s /opt/cobudget-api/blue/ current
sudo systemctl restart cobudget-api
```

### Running with systemd

Copy the systemd config files

```
sudo cp aws-deploy/cobudget-api.service /etc/systemd/system
sudo cp awd-deploy/cobudget-dj.service /etc/systemd/system
```

Get systemd to read the file, start the server and enable for automatic start at boot

```
sudo systemctl daemon-reload
sudo systemctl start cobudget-api
sudo systemctl enable cobudget-api
sudo systemctl start cobudget-dj
sudo systemctl enable cobudget-dj
```

You can check if the server has started with

```
sudo systemctl status cobudget-api.service cobudget-dj.service
```



