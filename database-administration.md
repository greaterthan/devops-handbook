# Database administration

## Current production database on Heroku

The credentials are

**Host: **ec2-52-87-121-192.compute-1.amazonaws.com

**Database: **d1hlu2cr9ta2p3

**User: **ud6dfgef6bi48k

Password can be found on the database credentials page on [Heroku](https://data.heroku.com/datastores/639d09f0-d0f9-45e9-92fa-74701591ef14)

## New production database on AWS

The credentials are

**Host: **cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com

**Database:** cobudget\_prod

**User:** cobudget

Password is currently only known by Michael

## Moving a database to a new database instance

* Use `pg_dump` to dump the exsting database into a file
* Edit the user name to fit the new database to be used
* use `psql` to load the data into the new database

### Example: Dumping the heroku database and loading it on AWS

First dump the Heroku database

```
pg_dump -h ec2-52-87-121-192.compute-1.amazonaws.com -U ud6dfgef6bi48k -d d1hlu2cr9ta2p3 >db_dump
```

Edit the user name from "ud6dfgef6bi48k" to "cobudget"

```
sed -e 's/Owner: ud6dfgef6bi48k/Owner: cobudget/' -e 's/OWNER TO ud6dfgef6bi48k/OWNER TO cobudget/' db_dump >db_loadable
```

Create the new database

```
createdb -h cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com -U cobudget cobudget_prod
```

Load the new database with the previously dumped data

```
psql -h cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com -U cobudget -d cobudget_prod <db_loadable
```

The load should be executed with no errors.

To look around in the database, use `psql`

```
psql -h cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com -U cobudget -d cobudget_prod
```

In case something goes wrong, you can always delete the database

```
dropdb -h cobudget-prod.cird90svlmvv.us-west-2.rds.amazonaws.com -U cobudget cobudget_prod
```

and restart with creating the new database.



