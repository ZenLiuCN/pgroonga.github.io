---
title: Install on Debian GNU/Linux
---

# Install on Debian GNU/Linux

This document describes how to install PGroonga on Debian GNU/Linux.

## Supported versions

Here are supported Debian GNU/Linux versions:

  * [Stretch](#install-on-stretch)

## How to install on Debian GNU/Linux Stretch {#install-on-stretch}

You can use the following instruction to install PGroonga on Debian GNU/Linux Stretch.

Install `apt-transport-https` package:

```console
% sudo apt update
% sudo apt install -y -V apt-transport-https
```

Add APT repository for Groonga:

`/etc/apt/sources.list.d/groonga.list`:

```text
deb [signed-by=/usr/share/keyrings/groonga-archive-keyring.gpg] https://packages.groonga.org/debian/ stretch main
deb-src [signed-by=/usr/share/keyrings/groonga-archive-keyring.gpg] https://packages.groonga.org/debian/ stretch main
```

If you want to use PostgreSQL 10, you need to add [the APT repository by PostgreSQL][postgresql-apt]:

```console
% echo "deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
% wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```

Install `postgresql-9.6-pgroonga` or `postgresql-10-pgroonga` package:

```console
% sudo wget -O /usr/share/keyrings/groonga-archive-keyring.gpg https://packages.groonga.org/debian/groonga-archive-keyring.gpg
% sudo apt update
% sudo apt install -y -V postgresql-9.6-pgroonga
Or
% sudo apt install -y -V postgresql-10-pgroonga
```

If you want to use [MeCab](http://taku910.github.io/mecab/) based tokenizer, you also need to install `groonga-tokenizer-mecab` package:

```console
% sudo apt-get install -y -V groonga-tokenizer-mecab
```

Create a database:

```console
% sudo -u postgres -H psql --command 'CREATE DATABASE pgroonga_test'
```

(Normally, you should create a user for `pgroonga_test` database and use the user. See [`GRANT USAGE ON SCHEMA pgroonga`](../reference/grant-usage-on-schema-pgroonga.html) for details.)

Connect to the created database and execute `CREATE EXTENSION pgroonga`:

```console
% sudo -u postgres -H psql -d pgroonga_test --command 'CREATE EXTENSION pgroonga'
```

That's all!

Try [tutorial](../tutorial/). You can understand more about PGroonga.

[postgresql-apt]:https://www.postgresql.org/download/linux/debian/
