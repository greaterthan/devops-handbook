# Setting up EC2 instance

* Add ssh keys to `.ssh/autohorized_keys` for standard user `ubuntu`
* Install Ruby \(see below\)
* Install and configure Nginx \(see below\)

## Install Ruby

Ruby is installed by using the \[these instructions\]\([https://gorails.com/deploy/ubuntu/16.04](https://gorails.com/deploy/ubuntu/16.04)\), replicated here.

Install dependencies:

```
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs
```

Install Ruby 2.4.0 using `rbenv`

```
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 2.4.0
rbenv global 2.4.0
ruby -v
```

And finally install `bundler`

```
gem install bundler
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



