# symfony-selenium

We are using GNU Make for work with CLI while developing. You can see list of all available command using `make` command in project root directory.

## Description

A Docker-based installer and runtime for:
* Symfony web framework (https://symfony.com/doc/6.1/setup.html)
* Selenium (https://github.com/SeleniumHQ/docker-selenium)
* php-webdriver/php-webdriver (https://github.com/php-webdriver/php-webdriver)

## How it works?

Have a look at the `docker-compose.yml` file, here are the `docker-compose` built images:

* `postgres`: This is the PostgreSQL database container. Image: https://hub.docker.com/_/postgres.
* `php-fpm`: This is the PHP-FPM container in which application volume is mounted. DockerFile: `docker/php-fpm/DockerFile`.
* `nginx`: This is the Nginx webserver container in which application volume is mounted. Image: https://hub.docker.com/_/nginx. Configuration: `docker/nginx/default.conf`. 
* `selenium` includes several containers to have possibility to develop with several browsers:
  * * `selenium-hub`: https://hub.docker.com/r/selenium/hub
  * * `chrome`: https://hub.docker.com/r/selenium/node-chrome
  * * `edge`: https://hub.docker.com/r/selenium/node-edge
  * * `firefox`: https://hub.docker.com/r/selenium/node-firefox

Project includes https://github.com/php-webdriver/php-webdriver Php-webdriver library is PHP language binding for Selenium WebDriver, which allows you to control web browsers from PHP. .

Running docker-compose ps should result in the following running containers:

```
CONTAINER ID   IMAGE                                  COMMAND                  CREATED       STATUS             PORTS                              NAMES
bc0f5292155b   nginx:stable-alpine                    "/docker-entrypoint.…"   2 hours ago   Up About an hour   0.0.0.0:80->80/tcp                 symfony_selenium_nginx
16e621f2e31c   symfony-selenium_php-fpm               "docker-php-entrypoi…"   2 hours ago   Up About an hour   0.0.0.0:9000->9000/tcp             symfony_selenium_fpm
54b1e120fd6d   selenium/node-edge:4.3.0-20220726      "/opt/bin/entry_poin…"   2 hours ago   Up 2 hours         5900/tcp                           symfony-selenium-edge-1
447ab57292d0   selenium/node-firefox:4.3.0-20220726   "/opt/bin/entry_poin…"   2 hours ago   Up 2 hours         5900/tcp                           symfony-selenium-firefox-1
c7216f4e2e03   selenium/node-chrome:4.3.0-20220726    "/opt/bin/entry_poin…"   2 hours ago   Up 2 hours         5900/tcp                           symfony-selenium-chrome-1
f2c40688bcdf   selenium/hub:4.3.0-20220726            "/opt/bin/entry_poin…"   2 hours ago   Up 2 hours         0.0.0.0:4442-4444->4442-4444/tcp   selenium-hub
e4f2740a3934   postgres:13-alpine                     "docker-entrypoint.s…"   2 hours ago   Up 2 hours         0.0.0.0:5431->5432/tcp             symfony-selenium-postgres-1
```

## Installation

1. Clone repository

    ```bash
    $ git clone https://github.com/OstrikovG/symfony-selenium
    ```

2. Install application

    ```bash
    $ cd symfony-selenium
    $ make install
    ```

## Usage

Once all the containers are up, our services are available at:

* Symfony app: `http://symfony-selenium.loc:80`
* PostgreSQL server: `symfony-selenium.loc:5432`
* Selenium Grid UI: `http://symfony-selenium.loc:4444/ui` 

## Create a Browser Session 
More information how to use php-webdriver https://github.com/php-webdriver/php-webdriver/blob/main/README.md
```php
// Chromedriver (if started using --port=4444 as above)
$host = 'http://selenium-hub:4444/wd/hub';
$capabilities = DesiredCapabilities::chrome();
$driver = RemoteWebDriver::create($host, $capabilities, 5000);
// Go to URL
$driver->get('https://en.wikipedia.org/wiki/Selenium_(software)');
```