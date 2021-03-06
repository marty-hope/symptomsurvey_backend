# symptomsurvey_backend

This repository is the API for a website for use by Clackamas County.  These are developer notes.

## setting up your development environment

### Installing Docker

Download Docker for your system at https://www.docker.com/ and install.

### Cloning the repository

Navigate to a repository where you would like to store the source code.  Then run

```bash
git clone https://github.com/codeforportland/symptomsurvey_backend.git
cd symptomsurvey_backend
```

### Private key

The private key is intentionally not checked into version control so that it will remain a secret.

Get the private key for the app from me and create a file to contain it at `WEB/keys/token`.

### Building the container

From the cloned repo directory and with docker running, run

```bash
docker-compose build
```

This may take a while. If it seems to freeze on windows, pressing enter seems to cause the output to update.

### Running / Updating the site locally

From the cloned repo directory and with docker running, run

```bash
docker-compose up -d --build
```

This will build all services on the site, as well as launch them. If the site is already running, it will rebuild any services who's source has changed, as well as relaunch it.

### Stopping the site

From the cloned repo directory and with docker running, run

```bash
docker-compose down
```

## Updating the project

### Adding migrations

While the project is running, you can manage mongodb through the MANAGE service, which runs idle with the appropriate node files. You can access this via
```bash
docker-compose exec manage sh
```
which will open a shell running in the container.

You can then run migrations with either
```bash
npm run up
npm run down
npm run create <migration-name>
```

which will perform migrations as per usual.
The migrations folder is synced between the repo and container, so any new migrations will be duplicated either way. This allows you to create a migration in the container, edit it locally, and sync it to mongo without starting and stopping any containers.

### Code Structure

#### Adding Controllers and Routes

If you want to add a Create, Read, Update, or Delete (CRUD) endpoint for a resource, then it should be added to the controllers directory. To add a new resource create a python file with the resource name, expose a method from it called `add_routes`, and call that method in the `add_routes` method in controllers/routes.
