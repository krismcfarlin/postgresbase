<p align="center">
    <a href="https://pocketbase.io" target="_blank" rel="noopener">
        <img src="https://i.imgur.com/5qimnm5.png" alt="PocketBase - open source backend in 1 file" />
    </a>
</p>

## Fork Explanation ❤️ Pocketbase  
[Pocketbase Documentation](https://pocketbase.io/docs)  

Pocketbase is a great product and very efficient for small-mid projects. It has no any additional setup for any other features and it is very easy to use on a single server.  

In our use-case we really need to use postgres as a main database and operate it manually. Also we love what Pocketbase does with CRUD operation and RBAC implementations via simple notations. So we want to use it. Thus we forked it and make it compatible with postgres using its own library called ["pocketbase/dbx"](https://github.com/pocketbase/dbx).  

We are still working on it and we will update the documentation as soon as we finish the project.  

We just added a following features additinonally to the Pocketbase:
- We use [Twitter Snowflake for ID generation](https://github.com/AlperRehaYAZGAN/postgresbase/blob/master/migrations/1640988000_init.go#L48). Every table ID is generated by postgres function as data type varchar(32)  
- We converted [created and updated columns](https://github.com/AlperRehaYAZGAN/postgresbase/blob/master/migrations/1640988000_init.go#L73-L74) to postgres native date types `TIMESTAMPTZ` to support native date operations  
- We write [json functions for postgres](https://github.com/AlperRehaYAZGAN/postgresbase/blob/master/migrations/1640988000_init.go) in migration files to support json equivalent operations from Pocketbase.  


## Usage  
You can easily fork and setup the project.  

```bash
# clone and download libraries
git clone https://github.com/AlperRehaYAZGAN/postgresbase
cd postgresbase
go mod download

# docker-compose has 3 service for test pocketbase all features:
# 1. Postgres: runs on port 5432
# 1. postgres://user:pass@localhost/logs?sslmode=disable
# 2. minio: UI runs on port 9001 and API on 9000  (minio123:minio123)
# 2. s3://minio123:minio123@localhost:9000/public
# (dont forget to manually create bucket called "public" via web ui to establish s3 connection from pocketbase)
# 3. mailhog: port: SMTP-1025 and UI-8025
# 3. smtp://localhost:1025 - http://localhost:8025
docker-compose up -d

# run the project with postgres connection info
CGO_ENABLED=0 go build -tags pq -o server \
    github.com/pocketbase/pocketbase/examples/base \
    env \
    LOGS_DATABASE="postgresql://user:pass@localhost/logs?sslmode=disable" \
    DATABASE="postgresql://user:pass@localhost/postgres?sslmode=disable" \
    ./server serve  

```
