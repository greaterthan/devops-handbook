# Setting up EC2 instance

* Add ssh keys to `.ssh/autohorized_keys` for standard user `ubuntu`
* Install Ruby \(see below\)
* Install and configure Nginx \(see below\)

## Install Ruby

Ruby is installed under `/usr/local/lib`with each ruby version in it's own directory

Install dependencies:

```
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs
```

Add the the current user `ubuntu`to the `staff` group

```
sudo usermod -a -G staff ubuntu
```

Now log out and log back in for new group to take effect.

Install the latest version of [ruby-install](https://github.com/postmodern/ruby-install)

Installation instructions for version 0.6.1 is replicated here

```
wget -O ruby-install-0.6.1.tar.gz https://github.com/postmodern/ruby-install/archive/v0.6.1.tar.gz
tar -xzvf ruby-install-0.6.1.tar.gz
cd ruby-install-0.6.1/
sudo make install
```

### Installing ruby 2.4.0

We will use `/usr/local/lib/ruby-2.4.0`

Prepare the directory

```
sudo mkdir /usr/local/lib/ruby-2.4.0
sudo chgrp staff /usr/local/lib/ruby-2.4.0
sudo chmod 775 /usr/local/lib/ruby-2.4.0
```

Install ruby

```
ruby-install --install-dir /usr/local/lib/ruby-2.4.0 ruby 2.4.0
```

And finally install `bundler`

```
/usr/local/lib/ruby-2.4.0/bin/gem install bundler
```

## Install and configure Nginx

Install nginx

```
sudo apt install nginx
```

Get the nginx configuration files and copy them in place

```
cd
git clone https://github.com/greaterthan/aws-deploy.git
sudo cp aws-deploy/cobudget /etc/nginx/sites-available/
sudo cp aws-deploy/cobudget_trace.conf /etc/nginx/conf.d/
cd /etc/nginx/sites-enabled
sudo rm default
sudo ln -s /etc/nginx/sites-available/cobudget cobudget
```

Restart the nginx server

```
sudo systemctl restart nginx
```

## Install SSL certificates using certbot

Use the instructions from [certbot](https://certbot.eff.org/#ubuntuxenial-nginx)

As the mail address for notifications, use `devops@greater.finance`

Add HTTP2.0 capability by following [these instructions](https://www.bjornjohansen.no/enable-http2-on-nginx).



