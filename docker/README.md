# Docker

The docker image will run matrix-reminder-bot with a SQLite database and
end-to-end encryption dependencies included. For larger deployments, a
connection to a Postgres database backend is recommended.

## Setup

### The `/data` volume

The docker container expects the `config.yaml` file to exist at
`/data/config.yaml`. To easily configure this, it is recommended to create a
directory on your filesystem, and mount it as `/data` inside the container:

```
mkdir data
```

We'll later mount this directory into the container so that its contents
persist across container restarts.

### Creating a config file

Copy `sample.config.yaml` to a file named `config.yaml` inside of your newly
created `data` directory. Fill it out as you normally would, with a few minor
differences:

* The bot store directory should reside inside of the data directory so that it
  is not wiped on container restart. Change it from the default to `/data/store`.
  There is no need to create this directory yourself, matrix-reminder-bot will
  create it on startup if it does not exist.

* Choose whether you want to use SQLite or Postgres as your database backend. If
  using SQLite, ensure your database file is stored inside the `/data` directory:

  ```
  database: "sqlite:///data/bot.db"
  ```

  If using postgres, point to your postgres instance instead:

  ```
  database: "postgres://username:password@postgres/matrix-reminder-bot?sslmode=disable
  ```

Change any other config values as necessary. For instance, you may also want to
store log files in the `/data` directory.

## Running

First, create a volume for the data directory created in the above section:

```
docker volume create \
  --opt type=none \
  --opt o=bind \
  --opt device="/path/to/data/dir" data_volume
```

Run the following to start the bot:

```
docker-compose up matrix-reminder-bot
```

This will start the bot and log the bot's output. You can instead run the container detached
with the `-d` flag:

```
docker-compose up -d matrix-reminder-bot
```

This will use the `matrix-reminder-bot:latest` tag from
[Docker Hub](https://hub.docker.com/anoa/matrix-reminder-bot).

If you would rather run from the checked out code, you can run:

```
docker-compose up local-checkout
```

**Note:** If you are trying to connect to a Synapse instance running on the
host, you need to allow the IP address of the docker container to connect. This
is controlled by `bind_addresses` in the `listeners` section of Synapse's
config. If present, either add the docker internal IP address to the list, or
remove the option altogether to allow all addresses.

## Updating

To update the container, navigate to the bot's `docker` directory and run:

```
docker-compose pull matrix-reminder-bot
```

Then restart the bot.

## Systemd

A systemd service file is provided for your convenience at
[matrix-reminder-bot.service](matrix-reminder-bot.service). The service uses
`docker-compose` to start and stop the bot.

Copy the file to `/etc/systemd/system/matrix-reminder-bot.service` and edit to
match your setup. You can then start the bot with:

```
systemctl start matrix-reminder-bot
```

and stop it with:

```
systemctl stop matrix-reminder-bot
```

To run the bot on system startup:

```
systemctl enable matrix-reminder-bot
```

## Building the image

To build the image from source, use the following `docker build` command from
the repo's root:

```
docker build -t anoa/matrix-reminder-bot:latest -f docker/Dockerfile .
```
