# Deployment

So you have source control, a development environment, and a server running in the cloud.  The next step is to close the loop and deploy code.  If you are using a framework like Symfony or Laravel you will likely want to start with the tooling that they provide (this is especially useful for database changes since they provide support for managing this).  In our case, this is a small standalone PHP project so we can get away with a simple solution.  Its generally a good idea to start simple, feel some pain and then look for tools to solve the problems you are having.  Deploying tends to vary from app to app so it better to find pain points and solve them then imagine pain points and try to solve everything.

## Tutorial
Our project is hosted at github: https://github.com/jeichorn/PHP-Developer-Example.  We use composer for dependencies, so we need to update them.  We don't have to worry about a database.

1. Create a new release using the Github website.
![Github release](github-release.png)
2. Create a personal access token https://github.com/settings/tokens/new, that way we can have a roll user for deployment access.  (Make sure to copy the token right away, once you reload the page its gone.)
![Github create an access token](github-access-token.png)
3. Take a look deploy.php
 * It uses the github api to find the newest deploy
 * Pre-Releases can be ignored based on config
 * It goes to our configured releases dir
 * Runs ```git fetch```
 * Then it runs ```git checkout $tag``` to switch to our release
 * Then it runs ```composer install``` to get the newest dependencies specified by the lock file
**TODO All of these steps below should be automated and setup with puppet**
4. Create a role user on our target server ```adduser web-deployer --gid 33```
5. Copy deploy.php to our target server, and copy it to /home/web-deployer
6. Change users to web-deployer ```sudo su - web-deployer```
7. Create a .release-config.php file
```php
<?php
return [
  'github-url' => 'https://github.com/jeichorn/PHP-Developer-Example',
  'token' => 'XXXXXXXXXXXXXXXXXXXXXXXXXXX',
  'deploy-dir' => '/var/www/',
  'no-pre-release' => false,
  'email' => 'you@you.com',
];
```
8. Edit web-deployer's crontab and run deploy.php every minute
```crontab -e```
```
# m h  dom mon dow   command
  * *  *   *   *     /usr/bin/php /home/web-deployer/deploy.php
```

## Use the right tool for the job
Git based deployment is great for staging, testing, and production, but its not the solution if you want to do remote development.  In a case like that either use an editor that runs on the server like vim, an editor with transparent sftp support, or rsync.  There are even tools (or you can write a quick script to do it yourself) that watch for changed files and use rsync to update those changes.
