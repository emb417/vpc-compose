# vpc-compose

This repository contains the Docker Compose configuration files for the VPC stack.

- [vpc-nginx](https://github.com/emb417/vpc-nginx): Nginx Reverse Proxy
  - depends on vpc-next, vpc-data, vps-data
  - requires certs in `/data/certbot/conf`
- [certbot](https://github.com/certbot/certbot): Certbot for SSL certificates
- [vpc-next](https://github.com/emb417/vpc-next): VPC Next.js App
  - requires vpc-next.env
  - depends on vpc-data, vps-data
- [vpc-bot](https://github.com/ericfaris/vpc-bot): VPC Discord Bot
  - requires vpc-bot.env
- [vpc-data](https://github.com/ericfaris/vpc-data): VPC Data API
  - requires vpc-data.env
  - depends on vps-data
- [vps-data](https://github.com/ericfaris/vps-data): VPS Data API
  - requires vps-data.env
- [vpw-bot](https://github.com/ericfaris/vpw-bot): VPW Discord Bot
  - requires vpw-bot.env
- [vpw-data](https://github.com/ericfaris/vpw-data): VPW Data API
  - requires vpw-data.env

The `docker-compose.yml` file is the primary configuration file for the production VPC stack.

The `docker-compose-refactor.yml` file is the primary configuration file for the refactored VPC stack, currently in production as of January 28, 2026, working out all of the kinks before locking in the versions and promoting them to `docker-compose.yml`.

The `docker-compose-local.yml` file is used for local development and testing.

## Usage

### Local Development

To build the local development environment, run the following commands from the `vpc-compose` directory:

```bash
docker compose -f docker-compose-local.yml build
```

To run the local development environment, run the following commands from the `vpc-compose` directory:

```bash
docker compose -f docker-compose-local.yml up -d
```

### Production Refactor

To pull built images for the production refactor environment, run the following commands from the `vpc-compose` directory:

```bash
docker compose -f docker-compose-refactor.yml pull
```

To run the production refactor environment, run the following commands from the `vpc-compose` directory:

```bash
docker compose -f docker-compose-refactor.yml up -d
```

### Production

To pull built images for the production environment, run the following commands from the `vpc-compose` directory:

```bash
docker compose -f docker-compose.yml pull
```

To run the production environment, run the following commands from the `vpc-compose` directory:

```bash
docker compose -f docker-compose.yml up -d
```

### Notes

Certbot will need certs copied from server A to server B if migrating, otherwise use the `init-letsencrypt.sh` script to generate them.

## Environment Variables

The following variables are expected in the `vpc-data.env`, `vpc-data-local.env`, `vpw-data.env`, and `vpw-data-local.env` files:

| Variable    | Description       |
| ----------- | ----------------- |
| DB_NAME     | Database name     |
| DB_USER     | Database username |
| DB_PASSWORD | Database password |
| PORT        | Port number       |

The following variables are expected in the `vpc-next.env` and `vpc-next-local.env` files:

| Variable                  | Description                           |
| ------------------------- | ------------------------------------- |
| SSR_BASE_URL              | Base URL for SSR                      |
| CSR_BASE_URL              | Base URL for CSR                      |
| VPC_API_PATH              | Path to VPC API                       |
| VPC_API_RECENT_WEEKS      | Path to VPC API for recent weeks      |
| VPC_API_COMPETITION_WEEKS | Path to VPC API for competition weeks |
| VPC_API_SEASON_WEEKS      | Path to VPC API for season weeks      |
| VPC_API_RECENT_TABLES     | Path to VPC API for recent tables     |
| VPC_API_SCORES_PATH       | Path to VPC API for scores            |
| VPS_API_TABLES_PATH       | Path to VPS API for tables            |
| VPS_API_GAMES_PATH        | Path to VPS API for games             |

The following variables are expected in the `vpc-bot.env` and `vpc-bot-local.env` files:

| Variable                        | Description                                |
| ------------------------------- | ------------------------------------------ |
| APPLICATION_ID                  | Discord API Application ID                 |
| BOT_TOKEN                       | Discord API Bot Token                      |
| GUILD_ID                        | Discord Server ID                          |
| BOT_COMPETITION_ADMIN_ROLE_NAME | Name of the Competition Corner Mod role    |
| BOT_HIGH_SCORE_ADMIN_ROLE_NAME  | Name of the High Score Corner Mod role     |
| COMPETITION_CHANNEL_NAME        | Name of the Competition Corner channel     |
| COMPETITION_CHANNEL_ID          | ID of the Competition Corner channel       |
| COMPETITION_WEEKLY_POST_ID      | ID of the weekly competition post          |
| COMPETITION_SEASON_POST_ID      | ID of the season competition post          |
| HIGH_SCORES_CHANNEL_NAME        | Name of the High Score Corner channel      |
| HIGH_SCORES_CHANNEL_ID          | ID of the High Score Corner channel        |
| BRAGGING_RIGHTS_CHANNEL_NAME    | Name of the Braggin' Rights channel        |
| BRAGGING_RIGHTS_CHANNEL_ID      | ID of the Braggin' Rights channel          |
| COMMANDS_DIR                    | Directory of command files                 |
| SECONDS_TO_DELETE_MESSAGE       | Number of seconds to delete messages after |
| VERSION                         | Version of the bot                         |
| DB_NAME                         | Name of the MongoDB database               |
| DB_USER                         | Username for the MongoDB database          |
| DB_PASSWORD                     | Password for the MongoDB database          |
| COMPETITIONS_URL                | URL of the competitions endpoint           |
| HIGH_SCORES_URL                 | URL of the high scores endpoint            |
| VPC_DATA_SERVICE_API_URI        | URL of the VPC Data service API            |
| VPS_DATA_SERVICE_API_URI        | URL of the VPS Data service API            |

The following variables are expected in the `vpw-data.env` and `vpw-data-local.env` files:

| Variable                  | Description                                |
| ------------------------- | ------------------------------------------ |
| APPLICATION_ID            | Discord API Application ID                 |
| BOT_TOKEN                 | Discord API Bot Token                      |
| GUILD_ID                  | Discord Server ID                          |
| COMMANDS_DIR              | Directory of command files                 |
| SECONDS_TO_DELETE_MESSAGE | Number of seconds to delete messages after |
| VERSION                   | Version of the bot                         |
| VPW_DATA_SERVICE_API_URI  | URL of the VPW Data service API            |

The following variables are expected in the `vps-data.env` file:

| Variable | Description                 |
| -------- | --------------------------- |
| VPS_URL  | URL of the VPS DB JSON file |
