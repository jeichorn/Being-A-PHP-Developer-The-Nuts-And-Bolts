# Development Environments
A couple years ago the height of a development environment was a fancy IDE and a php/mysql/apache installer like [Wamp](http://www.wampserver.com/en/).  Today there are a lot better options.  Virtual machines give you the ability have multiple pristine environment that can exactly match your production environment.  They give you the ability to try out new versions of PHP or Mysql and easily rollback, and they let you work on multiple projects with different PHP requirements at the same time.  They also save you from spending hours setting up a dev environment by hand, or day's fixing things after everything has blow up.

The core of this functionality is provided by 2 projects:
* [Virtualbox](https://www.virtualbox.org/) - a free virtualization product
* [Vagrant](http://www.vagrantup.com/) - a vm manger with built in support for virtualbox

With just these basics you can install a linux distro that matches your dev servers, clone it and get to a fairly reproducable state, but we can do better.

There are many configuration mangement tools out there, these allow you to automate the installation and configuration of a VM (or any other server).  Luckily we don't have to learn one to get its advantages for a basic dev environment.  There are projects like [PuPHPet](https://puphpet.com/) which combine the vargrant configuration with a puppet configuration to give you a configurable full blown development environment.
