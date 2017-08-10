# Manual install of cobudget-api

## Introduction

The server will be set up to use blue/green deployments. This will done by making two directories, incidentally named `blue` and `green` and set a symbolic link `current` that points to the current release in production.

A new release can then be prepared in the directory not currently in use and the change can happen almost instantanously

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
cp aws-deploy/env aws-deploy/start /opt/cobudget-api/blue
```

git clone [https://github.com/greaterthan/aws-deploy.git](https://github.com/greaterthan/aws-deploy.git)

Now edit the `env` file. The database env could be

```
DATABASE_URL="postgres://cobudget:<password>@cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com:5432/cobudget_prod"
```

Install missing Postgresql dev files and install all packages

```
sudo apt-get install postgresql-common libpq-dev
bundle install
```

And finally, start the server

```
./start
```

That's it. The server is now running.

### What's missing.

* Automatic restart when the machine boot. Build a systemd unit script.
  * Find out how to stop the server.
  * Find out if we need a PID file
* An easy way to update with a new version.
* Deploy using Travis \(including testing\)



