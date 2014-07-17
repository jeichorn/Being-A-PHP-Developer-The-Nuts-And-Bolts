# Composer

Now that we have a development environment lets write some code.  We could start with an apache or nginx config and an empty PHP file and go from there, or we could start looking for libraries and frameworks to get the basics done for us.

Everyone has a different philosophy on what they are looking for, from a big framework that does everything, to a special purpose library to solve a specific task.  The PHP community offers this and everything in between.  [Composer](https://getcomposer.org/) and the [Packagist](https://packagist.org/) repository give you tools to help you find, install, and manage these.

## Installing Composer
If you use puphpet you can have it automatically install composer for you.  But if not its easy to install.  Just follow the instructions in the manual: https://getcomposer.org/doc/00-intro.md#installation-nix

## Our example project
Our example project is https://github.com/jeichorn/PHP-Developer-Example, you can fork it to skip some copy paste, or just refer to it if you get stuck.  We will be building a small [REST](http://en.wikipedia.org/wiki/Representational_state_transfer) api that returns gibberish.  We are going to be using the [Aura v2 framework](http://auraphp.com/packages/v2), and building a test client using [Guzzle](http://guzzle.readthedocs.org/en/latest/).

## composer.json
This is a config file that sits in the root of your repo.  It describes your project, its dependencies and can be used to configure an autoloader.

Our dependencies:
* https://packagist.org/packages/guzzlehttp/guzzle
* https://packagist.org/packages/aura/framework-project

Here is a annotated version of the one from our test project.
```js
{
    /* A standard project description, you can skip this if you want */
    "name": "PHP Developer Example",
    "description": "Example code that goes along with: https://www.gitbook.io/book/jeichorn/being-a-php-developer-the-nuts-and-bolts",
    "license": "MIT",
    "authors": [
        {
            "name": "Joshua Eichorn",
            "email": "josh@bluga.net"
        }
    ],
    /* Our dependencies */
    "require": {
        "guzzlehttp/guzzle": "4.*",
        "aura/web": "2.0.*@dev"
    },
    /* Our autoload rules */
    "autoload": {
        "psr-4": {
            /* Anything with the namespace on the left, we look for in the directory on the right, following the PSR-4 rules*/
            "jeichorn\\DevExample\\": "src/DevExample"
        }
    }
}
```

If you are looking for everything you can do in composer.json, check out its [manual](https://getcomposer.org/doc/04-schema.md).


## Installing our deps
Its really simple, just run "composer update" or if you downloaded composer.phar into your project dir "php composer.phar update".  It will resolve your dependencies, install them into the vendor dir and build an autoloader at "vendor/autoload.php"

To use the libraries you've installed just include the vendor autoload file and use the code like normal.

## Source control and the vendor dir
You may wonder what to do about GIT and this vendor dir.  The easiest thing to do is to add it to your .gitignore file, and then whenever you checkout the repo in a new place run "composer install", which will give you the same versions of dependencies as the last "composer update" run (this information is stored in composer.lock).

Depending on how you are deploying code, you might want to check it in to your repo.  If you do this, you will need to clear our any ".git" directories from the vendor dir before you run git add.  You might find the following shell script helpful in that circumstance.

```
#!/bin/bash
find vendor -type d -name .git | xargs rm -rf
```


## Whats this psr-4 stuff
The [PHP Framework Interop Group](http://www.php-fig.org/) has created standards on basic things like autoloading that different php code needs to follow to work together.  [PSR-4](http://www.php-fig.org/psr/psr-4/) is the newest autoload standard and is focused on how to manage the autloading of [namespaced code](http://us1.php.net/namespaces).
