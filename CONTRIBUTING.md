# Contributing to matrix-reminder-bot

Thank you for taking interest in this little project. Below is some information
to help you out with that PR!

## Setting up your development environment

### Using `docker-compose`

**It is recommended** to use `docker-compose` to run matrix-reminder-bot while
developing, as all necessary dependencies are handled for you. After installing
`docker-compose`:

1. Create a data directory and config file by following the
   [docker setup instructions](dockers/README.md#setup).

2. Create a docker volume pointing to that directory:

   ```
   docker volume create \
     --opt type=none \
     --opt o=bind \
     --opt device="/path/to/data/dir" data_volume
   ```

Run `docker/start-compose.sh --dev` to start the bot.

### Running natively

If you would rather not or are unable to run docker, please follow the Native
Installation, Configuration and Running sections in the
[project readme](README.md#native-installation).

## Code style

Please follow the [PEP8](https://www.python.org/dev/peps/pep-0008/) style
guidelines and format your import statements with
[isort](https://pypi.org/project/isort/).

Eventually there will be some CI to catch you in the act, but for now it's done
manually :)

## What to work on

Take a look at the [issues
list](https://github.com/anoadragon453/matrix-reminder-bot/issues). What
feature would you like to see or bug do you want to be fixed?

If you would like to talk any ideas over before working on them, you can reach
me at `@andrewm:amorgan.xyz` on matrix.
