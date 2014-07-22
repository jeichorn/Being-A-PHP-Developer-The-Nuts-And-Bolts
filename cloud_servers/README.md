# Cloud Servers

"The Cloud" such a trendy term these days, but don't let the trendiness fool you, there is a lot of power in the cloud.  You get an API to manage your servers and in some cases associated services.  You get to pay by the hour with no long term contracts (in most cases).  You get great pricing on a basic server from providers like Digital Ocean.

The **most important features** of cloud hosting are API control and hourly pricing.  Having an API to create and destroy servers allows you to treat hardware as code and control it with configuration.  Hourly pricing lets you spin up servers to run tests, to demo applications to clients, or as short term dev environments.  You can turn servers on as you need them and turn them off when you don't.

Even if the cloud isn't the right fit for every project, its easy to see how a $10 a month server with API control can be the right fit for testing and staging environments.  It might even make sense to use it for development rather then buying more hardware for all the VM's you find yourself running.

Lets set one up at Digital Ocean so our next tutorial on deployment has a destination.

## Tutorial
The assumption here is that you are using this server for a remote development environment or staging environment.  Thats a great use case for reusing the same PuPHPet config that we are using in development.  In this case we will just be using PHP script to create the server and then kick off the PuPHPet code.  If this was for a production environment you will at least want to learn how customize the PuPHPet code before reusing it.  Your provider choice might change as well, since extra features can start to play a larger part then price.

For a development/testing/staging environment its hard to beat the $10 a month instances offered by [Linode](https://www.linode.com/?r=6116e4762c91140ac2226d23dfa53a7cfdd7971e) or [Digital Ocean](https://www.digitalocean.com/?refcode=b7452ac8079b).  They both offer nice APIS and similar performance.  Digital Ocean also offers a $5 server, though at 512m its less useful as a general instance.  In this tutorial we are going to use Digital Ocean.

1. Clone the [example repo](https://github.com/jeichorn/PHP-Developer-Example).
2. [Signup for a Digital Ocean account](https://www.digitalocean.com/?refcode=b7452ac8079b).
3. Create a [Read/Write API key](https://cloud.digitalocean.com/settings/tokens/new).
4. Create a config.php file in the DigitalOcean directory of the Example repo
```php
return [
    'token' => 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX',
    'image' => 'ubuntu-14-04-x64',
    'ssh-identify' => 'ubuntu-14-04-x64',
];
```
5. Add the following stub config to puphpet/config.yml (we are disabling the port forwarding in our local config, but otherwise sharing everything else)
```
digital_ocean:
    vm:
        network:
            private_network: 192.168.56.101
            forwarded_port: {  }
```
6. Check out create-server.php
 * We are using https://github.com/toin0u/DigitalOceanV2 to talk to the API
 * The script is simple but does some nice things like create or lookup server info if the server is already running
 * It supports all the Images that DigitalOcean does
 * A server from an API, :-) nice isn't it
7. Run ```composer install``` in the DigitalOcean directory
8. Run ```php create-server.php```
9. Run ```php create-server.php go```
 * We run it on a 2nd run since the server needs to be booted before we can run the PuPHPet stuff
 * If you don't already have a PuPHPet config from an earlier step this will blow up on you, its just reusing that config
10. Update your hosts file to point awesome.dev to your new server
 * You could also add additional names to your vhost config and point real DNS to the server.  For staging etc this can be handy.
 * Digital Ocean provides DNS and has an API for it, so you could automate the DNS part as well
11. Run ```ssh root@ipofewserver``` then ```tail -f /var/log/apache2/*.log```
12. Open awesome.dev in your web browser
 * You won't see anything since its just an empty directory, but we can take care of that in the deployment chapter
