# Setup

## 1. Copy env_example to .env and edit it.

## 2. Setup folders

```Bash
(export $(grep -v '^#' .env | xargs); mkdir -p $NEXTCLOUD_DATA_DIR $NEXTCLOUD_DB_DIR)
(export $(grep -v '^#' .env | xargs); sudo chown -R 33:33 $NEXTCLOUD_DATA_DIR)
(export $(grep -v '^#' .env | xargs); sudo chown -R 999:999 $NEXTCLOUD_DB_DIR)
```

## 3. Start service

```Bash
docker-compose up -d --build
```

## 4. Config cron in web ui

- Once the instance is up, go to Settings > Basic settings in the Nextcloud UI and ensure Background jobs is set to Cron.
