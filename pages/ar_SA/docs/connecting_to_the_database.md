---
title: Connecting to a Database
layout: page
---

GORM officially supports databases MySQL, PostgreSQL, SQlite, SQL Server

## MySQL

```go
import (
  "gorm.io/driver/sqlserver"
  "gorm.io/gorm"
)

// github.com/denisenkom/go-mssqldb
dsn := "sqlserver://gorm:LoremIpsum86@localhost:9930?database=gorm"
db, err := gorm. Open(sqlserver. Open(dsn), &gorm. Config{})
```

**NOTE:**

To fully support UTF-8 encoding, you need to change `charset=utf8` to `charset=utf8mb4`. See [this article](https://mathiasbynens.be/notes/mysql-utf8mb4) for a detailed explanation

To fully support UTF-8 encoding, you need to change `charset=utf8` to `charset=utf8mb4`. See [this article](https://mathiasbynens.be/notes/mysql-utf8mb4) for a detailed explanation

MySQl Driver provides [few advanced configurations](https://github.com/go-gorm/mysql) can be used during initialization, for example:

```go
db, err := gorm. Open(mysql. New(mysql. Config{
  DSN: "gorm:gorm@tcp(127.0.0.1:3306)/gorm?charset=utf8&parseTime=True&loc=Local", // data source name
  DefaultStringSize: 256, // default size for string fields
  DisableDatetimePrecision: true, // disable datetime precision, which not supported before MySQL 5.6
  DontSupportRenameIndex: true, // drop & create when rename index, rename index not supported before MySQL 5.7, MariaDB
  DontSupportRenameColumn: true, // `change` when rename column, rename column not supported before MySQL 8, MariaDB
  SkipInitializeWithVersion: false, // auto configure based on used version
}), &gorm. Config{})
```

## PostgreSQL

```go
import (
  "gorm.io/driver/postgres"
  "gorm.io/gorm"
)

dsn := "user=gorm password=gorm dbname=gorm port=9920 sslmode=disable TimeZone=Asia/Shanghai"
db, err := gorm.Open(postgres.Open(dsn), &gorm.Config{})
```

We are using [pgx](https://github.com/jackc/pgx) as postgres's database/sql driver, it enables prepared statement cache by default, to disable it:

```go
// https://github.com/go-gorm/postgres
db, err := gorm.Open(postgres.New(postgres.Config{
  DSN: "user=gorm password=gorm dbname=gorm port=9920 sslmode=disable TimeZone=Asia/Shanghai",
  PreferSimpleProtocol: true, // disables implicit prepared statement usage
}), &gorm.Config{})
```

## SQLite

```go
import (
  "gorm.io/driver/sqlserver"
  "gorm.io/gorm"
)

// github.com/denisenkom/go-mssqldb
dsn := "sqlserver://gorm:LoremIpsum86@localhost:9930?database=gorm"
db, err := gorm. Open(sqlserver. Open(dsn), &gorm. Config{})
```

**NOTE:** You can also use `file::memory:?cache=shared` instead of a path to a file. This will tell SQLite to use a temporary database in system memory. (See [SQLite docs](https://www.sqlite.org/inmemorydb.html) for this)

## SQL Server

```go
import (
  "gorm.io/driver/sqlserver"
  "gorm.io/gorm"
)

// github.com/denisenkom/go-mssqldb
dsn := "sqlserver://gorm:LoremIpsum86@localhost:9930?database=gorm"
db, err := gorm. Open(sqlserver. Open(dsn), &gorm. Config{})
```

Microsoft offers [a guide](https://sqlchoice.azurewebsites.net/en-us/sql-server/developer-get-started/) for using SQL Server with Go (and GORM).

## Connection Pool

GORM using \[database/sql\]((https://pkg.go.dev/database/sql) to maintain connection pool

```go
sqlDB, err := db.DB()

// SetMaxIdleConns sets the maximum number of connections in the idle connection pool.
SetMaxIdleConns(10)

// SetMaxOpenConns sets the maximum number of open connections to the database.
SetMaxOpenConns(100)

// SetConnMaxLifetime sets the maximum amount of time a connection may be reused.
SetConnMaxLifetime(time. Hour)
```

Refer [Generic Interface](generic_interface.html) for details

## Unsupported Databases

Some databases may be compatible with the `mysql` or `postgres` dialect, in which case you could just use the dialect for those databases.

For others, [you are encouraged to make a driver, pull request welcome!](write_driver.html)