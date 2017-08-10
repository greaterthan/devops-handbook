# What's missing

* Check how we can have install using different rails versions at the same time. Read [Rails deployment](https://www.ralfebert.de/tutorials/rails-deployment/ "Rails deploymnt")

* Change ruby installation from being installed under a user to be installed globally
  * Change the appropriate places in the systemd script
* Modify nginx install to enable gzip
* Install ssh certificates using `certbot`
  * Change the `force_ssl` variable back to be set to `true`
* Describe how to update to a new version using the blue/green deployment
* Deploy using Travis \(including testing\)





