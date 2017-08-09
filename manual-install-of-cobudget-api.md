# Manual install of cobudget-api

## First time install

Create directy, adjust permissions and clone the `cobudget-api` github repo.

```
cd /opt
sudo mkdir cobudget-api
sudo chown ubuntu cobudget-api
git clone https://github.com/cobudget/cobudget-api.git
```

Get the `env` and `start` files and copy to the installed dir

```
cd
git clone https://github.com/greaterthan/aws-deploy.git
cp aws-deploy/env aws-deploy/start /opt/cobudget-api
```

git clone https://github.com/greaterthan/aws-deploy.git

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



