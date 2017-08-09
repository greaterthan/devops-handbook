# AWS Setup

We're currently running a very simple setup on AWS. One application server \(EC2\) and one database server \(RDS\)

All instances are setup on us-west \(Oregon\)

### EC2 instance

Type: t2.small running ubuntu 16.04 \(LTS\) with 30GB SSD mounted as root volume.

![](/assets/170809-GT EC2 config.png)

### RDS instance

Type: db.t2.small with 50GB SSD disk, running postgresql 9.6.2, single AZ, no public IP

![](/assets/170809-GT RDS config.png)

## Security

All instances are running in the same security group called `Cobudget-prod`. The group has the following inbound rules:

* SSH from everywhere
* HTTP from everywhere
* HTTPS from everywhere
* PostgreSQL from own security group `Cobudget-prod`



