
# Introduction
Docker Titanic for SilverStripe provides a local development environment for SilverStripe. It is designed to allow anyone working with SilverStripe to start developing locally with minimal knowledge of how Docker works and without having to install additional tools locally.

The only pre-requisite to use this local development environment is to install Docker. No other installations for Composer, PHP, Yarn etc are required on the local host computer.

The core of this library is are auto-generated `docker-compose.yml` and `Dockerfile` and the `titanic` script that is stored at the root of your SilverStripe project project.

The titanic script provides a CLI interface with easy to use commands for common tasks which then intereact with the environment defined by the `docker-compose.yml` file. This environment utilises the silverstripe-docker images(https://github.com/brettt89/silverstripe-docker).

Docker Titanic for SilverStripe has currently been tested on Windows (WSL2) and to a certain extent on macOS. It is also expected to work in any environments that can execute a bash script.

## Credits
Docker Titanic for SilverStripe is heavily inspired by the approach taken by [Vessel](https://github.com/shipping-docker/vessel) by [Chris Fidao](https://github.com/fideloper) as well as [Laravel Sail](https://github.com/laravel/sail). A similar approach has been used but adopted to be SilverStripe-specific.

This tool has been been built upon the [silverstripe-docker repository](https://github.com/brettt89/silverstripe-docker) by Brett Tasker which is used to power the underlying Docker environment. Depending on the PHP version chosen, the `brettt89/silverstripe-web:<PHP-VERSION>-apache` image will be used. This tool adds easy configuration options to quickly spin up a required local SilverStripe environment through user-friendly prompts.

![Example](/img/init.png)

# Guide 
## Installation and Setup
To get started using this tool, simply download the following files from this repository and place them in the root of your SilverStripe project:
- docker-compose-example.yml
- Dockerfile
- titanic

Run the following command and follow the prompts to configure an auto-generated    `docker-compose.yml` and `Dockerfile` for your project such as PHP version and database name.

`bash titanic init`

Once this is completed the example `docker-compose.yml` and `Dockerfile` will be removed.  

You can now run `./titanic start` to start up the environment. 

Once the environment is up and running, you will have to run `composer install` and import an existing database if its an existing project. Please see the specific sections of this README on specific instructions.

## Starting and Stopping the Environment
To start the environment, run:

`./titanic start`

To stop the environment, run:

`./titanic stop`

## Running Composer
To run Composer commands inside the SilverStripe container, simply prefix the composer command with ./titanic.
For example to run `composer install` we can run:

`./titanic composer install`

Alternatively, you can SSH into the running SilverStripe container and run the Composer command there directly. Note if you are using this method, you may have to manually update the owner and permissions of the installed vendor files.

`docker exec -it <container-name> /bin/bash`

### SSH Authentication inside the SilverStripe Container (Beta)
To allow SSH authentication when running Composer related commands, the SSH agent from the local machine socket is passed into the SilverStripe container. 

If you are running Composer commands by SSH into the running container, `ssh-add -l` may have to be run inside the container first before running the Composer command.

*Note that this seems to have some issues with macOSx which is a known issue currently being worked on.

## Database
### Importing a Database into the Environment
An SQL file can be imported into a database by placing it in the root of the SilverStripe project. Once the SQL file has been placed in the root project directory, run:

`./titanic importdb`

Follow the prompts and enter the name of the database to import into and the SQL file you'd like it to import. You will also be provided with an option to create a new database in the event you haven't yet created one.

![Example](/img/import-db.png)

Alternatively you can get import a small SQL file by accessing the database via PHPMyAdmin.


### Exporting a Database
An export of a database can be generated by running: 

`./titanic exportdb`

Follow the prompts and enter the name of the database to export and the SQL file you'd like it to be exported to.

![Example](/img/export-db.png)

Alternatively you can get an export of a database by accessing it via PHPMyAdmin.

### Accessing the database - PHPMYAdmin
This library comes packaged with PHPMyAdmin. By default, this can be accessed by visiting http://localhost:9191 where the username is `root` and the password is left empty. This can be changed by editing the port and/or environment variables in `docker-compose.yml`.

## Visiting the Website in the Browser
By default, the SilverStripe website can be navigated to by visiting http://localhost:8080 in the browser.

To change the port, you can update this in the `docker-compose.yml` file by modifying the ports section of the SilverStripe service.

## Running Yarn commands
To run Yarn commands inside the SilverStripe container, simply prefix the yarn command with ./titanic.
For example to run `yarn dev` we can run:

`./titanic yarn dev`

Alternatively, you can SSH into the running SilverStripe container and run the Yarn command there directly:

`docker exec -it <container-name> /bin/bash`

## Customizing the Docker image
To customise this Docker image, you can manually edit the `docker-compose.yml` or `Dockerfile` that is generated with anything specific.

## Configuring additional SilverStripe Environment Variables
To add additional SilverStripe environment that may be project-specific, simply edit the environment section of the SilverStripe service in the `docker-compose.yml` file.

## Help
To see a list of available commands run:

`./titanic --help`

![Example](/img/titanic-cmds.png)

### Issues
If you have any problems with about how to use this tool, please contact us through a [GitHub issue](https://github.com/brettt89/silverstripe-web/issues). 

## Contributing
You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a GitHub issue, especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.

## License
Docker Titanic for SilverStripe is open-sourced software licensed under the [MIT license](LICENSE.md).
