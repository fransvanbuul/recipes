# Postgres cheatsheet

### Running the docker image

```
docker run -d --rm --name postgres -p 5432:5432 -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=secret postgres:9.6
```

(Alternatively, `-it` instead of `-d`)

### Installing Postgres 9.6 on Ubuntu 18.04

Adding the repo:
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
sudo apt-get update
```

Server:
```
sudo apt-get install postgresql-9.6 postgresql-contrib-9.6
sudo service postgresql start
```

Client-only:
```
sudo apt-get install postgresql-client-9.6
```

### Accessing it locally
```
sudo su - postgres
psql
```

### Making it accessible over the network

In `/etc/postgresql/9.6/main/postgresql.conf`:
* `listen_addresses = '*'`

In `/etc/postgresql/9.6/main/pg_hba.conf`:
* `host all all 0.0.0.0/0 md5`

From psql prompt:
* `ALTER USER postgres WITH PASSWORD 'secret';`

### Elementary tuning

In `/etc/postgresql/9.6/main/postgresql.conf`:
* `shared_buffers = `<25% of RAM>
* Optionally (may lead to small loss of data on crash, without corruption): `synchronous_commit = off`

### Some basic psql commands

* `psql -h localhost -U postgres` (`-p` for port, `-d` to connect to chosen db, `-c` for single cmd)
* List databases: `\l`
* Use database: `\c dbname` (already connected to a database after logging in)
* Help: '\?'
* Quit: '\q'
* List all tables, views: `\d` (to include system tables, add `S`)
* Describe single table: `\d tablename`

### Java/JDBC/JPA/Spring Boot

```
spring.datasource.url=jdbc:postgresql://localhost/test
spring.datasource.username=dbuser
spring.datasource.password=dbpass
spring.datasource.driver-class-name=org.postgresql.Driver
```

(When custom-configuring the datasource bean, `jdbcUrl` instead of `url` for the Hikari
connection pool.)

```
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
```
