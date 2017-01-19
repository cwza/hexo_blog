title: USE TIMESTAMPTZ IN POSTGRES NOT TIMESTAMP
categories: database
tags: [postgres, database]
date: 2017-01-19 18:53:57
---

ALWAYS USE TIMESTAMPTZ NOT USE TIMESTAMP IN POSTGRES.
SET timezone = 'UTC' IN pgsql/data/postgresql.conf.

<!--more-->
timestamptz is accepted as an abbreviation for timestamp with time zone.

USE THIS:
``` sql
CREATE TABLE IF NOT EXISTS users
(
    id serial PRIMARY KEY,
    mail text NOT NULL UNIQUE,
    picture text,
    create_time timestamptz default current_timestamp,
    update_time timestamptz default current_timestamp
);
```

NOT THIS:
``` sql
CREATE TABLE IF NOT EXISTS users
(
    id serial PRIMARY KEY,
    mail text NOT NULL UNIQUE,
    picture text,
    create_time timestamp default current_timestamp,
    update_time timestamp default current_timestamp
);
```
