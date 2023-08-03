# Project description
This project includes Drupal 10, Drush 10, Composer install of Drupal Recommended Project and can be used to develop, stage or put into production any Drupal 10 project.

Also included are drush/config-extra and other utilities for CI/CD in terms of a Drupal project - site updates, database schema updates, database backups, push of databases via git, backup of files, replication of production environments and much more.

This is a terminal built environment based on the minimal install profile and the `./drush site:install minimal` command.

The thought behind this approach is to prepare the environment and tools needed to always be running an updated system with a modern proactive approach. The main goal is to have a fully automated production ready installation. Thus this project includes no modules, dependencies or themes not needed for the project beforehand, making it more manageable and minimal in terms of maintenance.

## Development


### Run with Docker

For development purposes this project can be started with:

   ```sh
   $ docker-compose up -d
   ```

Drupal 10 will be available via [localhost:9998](http://localhost:9998/)

phpMyAdmin will for development purposes be available via [localhost:9999](http://localhost:9999/)

#### Basic site configuration
Rename your project or set your frontpage url:

- [go to your basic settings page via GUI](http://localhost:9998/admin/config/system/site-information)
- fill in the field "Default front page" with `/node`
- [visit the frontpage](http://localhost:9998/)

#### Fix file and cache permissions:

   ```sh
   $ chmod 777 -R web/sites/default/files
   ```


### Using Composer

#### Composer dependencies

To install modules or other dependencies strictly use Composer. Installed dependencies are locked to specific versions using the `composer.lock` file. `composer install` will install every package specified in `composer.json` with respect to the pinned versions in `composer.lock`.

#### Adding dependencies

Use `composer require` to add and install new packages. Alternatively add the requirement to `composer.json` and run `composer install`.

   ```sh
    $ docker-compose exec -T drupal composer require "vendor/package:2.*"
   ```

#### Updating dependencies

When a version update is needed, use `composer update vendor/package`.

   ```sh
   $ docker-compose exec -T drupal composer update vendor/package
   ```
#### Rebuilding images and restarting containers
How to rebuild and restart containers after changes to the Dockerfile:

   ```sh
   $ docker-compose up -d --build
   ```

On first run, the `composer.lock` file was generated using `composer update` without further parameters.

#### Running tests (coming soon)

   ```sh

   ```


### Using Drush
Run Drush commands using - this command provides a full list of useful Drush commands:

   ```sh
   $ docker-compose exec -T drupal ./drush
   ```
Full list of commands in [Drush 9](https://drushcommands.com/drush-9x/). A list such as this one is not available yet fir Drush 10 but should suffice.


### Deployment using Github actions
#### Requirements
- Github account
- Github repository
- Github personal access token
- SSH key pair
- SSH public key added to Github repository
- SSH private key added to Github secrets

If you want to use SSH to deploy your Docker Compose Drupal setup to your server with IP whitelisting, you can add an SSH deploy step to your GitHub Actions workflow. Here's an example of how you can do it:

    First, create an SSH key pair (public and private key) on your server (if you haven't already). Make sure to add the public key to the authorized_keys file on your server to allow GitHub to access it via SSH.

    In the GitHub repository, go to "Settings" -> "Secrets" -> "New repository secret." Create two secrets: SSH_PRIVATE_KEY and SSH_USERNAME. The SSH_PRIVATE_KEY should contain the private key generated in step 1, and SSH_USERNAME should be the username you'll use for SSH access (e.g., root, ubuntu, etc.).
