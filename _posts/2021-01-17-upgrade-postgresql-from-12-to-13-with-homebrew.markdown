---
layout: post
title: "Upgrade PostgreSQL from 12 to 13 with Homebrew"
category: journal
tag: daily
icon: "🆙"
cover: /images/journal/upgrade-postgresql-from-12-to-13-with-homebrew/cover.jpg
original_url: https://quaran.to/upgrade-postgresql-from-12-to-13-with-homebrew
---

I wanted to point out some straightforward commands for doing this database upgrade on your Mac, if you, like me, have ignored doing so. [This post from Olivier Lacan](https://olivierlacan.com/posts/migrating-homebrew-postgres-to-a-new-version/) was a great help and I just wanted to make it even clearer that it's this easy:

```shell
brew services stop postgresql
brew postgresql-upgrade-database
brew services start postgresql
```

Glorious! Here's what the original error looked like for me, for SEO juice and future reference:

```shell
💚 pg_ctl -D /usr/local/var/postgres start
waiting for server to start....2021-01-17 17:03:03.686 EST [13710] FATAL:  database files are incompatible with server
2021-01-17 17:03:03.686 EST [13710] DETAIL:  The data directory was initialized by PostgreSQL version 12, which is not compatible with this version 13.1.
 stopped waiting
pg_ctl: could not start server
Examine the log output
```

So, I ran the above commands - here's the full output in case you're curious.

```shell
💚 brew services stop postgresql
Stopping `postgresql`... (might take a while)
==> Successfully stopped `postgresql` (label: homebrew.mxcl.postgresql)

💚  brew postgresql-upgrade-database
==> brew install postgresql@12
==> Downloading https://homebrew.bintray.com/bottles/postgresql%4012-12.5.big_sur.bottle.tar.gz
==> Downloading from https://d29vzk4ow07wi7.cloudfront.net/6e1717130028267c3f8c3910e10909fde608892da4af2669842da0dca386392a?response-content-disposition=attac
######################################################################## 100.0%
==> Pouring postgresql@12-12.5.big_sur.bottle.tar.gz
==> /usr/local/Cellar/postgresql@12/12.5/bin/initdb --locale=C -E UTF-8 /usr/local/var/postgresql@12
==> Caveats
Previous versions of this formula used the same data directory as
the regular PostgreSQL formula. This causes a conflict if you
try to use both at the same time.

In order to avoid this conflict, you should make sure that the
postgresql@12 data directory is located at:
  /usr/local/var/postgresql@12

This formula has created a default database cluster with:
  initdb --locale=C -E UTF-8 /usr/local/var/postgresql@12
For more details, read:
  https://www.postgresql.org/docs/12/app-initdb.html

postgresql@12 is keg-only, which means it was not symlinked into /usr/local,
because this is an alternate version of another formula.

If you need to have postgresql@12 first in your PATH run:
  echo 'export PATH="/usr/local/opt/postgresql@12/bin:$PATH"' >> /Users/qrush/.bash_profile

For compilers to find postgresql@12 you may need to set:
  export LDFLAGS="-L/usr/local/opt/postgresql@12/lib"
  export CPPFLAGS="-I/usr/local/opt/postgresql@12/include"

For pkg-config to find postgresql@12 you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/postgresql@12/lib/pkgconfig"


To have launchd start postgresql@12 now and restart at login:
  brew services start postgresql@12
Or, if you don't want/need a background service you can just run:
  pg_ctl -D /usr/local/var/postgres@12 start
==> Summary
🍺  /usr/local/Cellar/postgresql@12/12.5: 3,224 files, 41.6MB
==> Upgrading postgresql data from 12 to 13...
waiting for server to start....2021-01-17 17:06:21.437 EST [14867] LOG:  starting PostgreSQL 12.5 on x86_64-apple-darwin20.1.0, compiled by Apple clang version 12.0.0 (clang-1200.0.32.27), 64-bit
2021-01-17 17:06:21.439 EST [14867] LOG:  listening on IPv6 address "::1", port 5432
2021-01-17 17:06:21.439 EST [14867] LOG:  listening on IPv4 address "127.0.0.1", port 5432
2021-01-17 17:06:21.440 EST [14867] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"
2021-01-17 17:06:21.477 EST [14868] LOG:  database system was shut down at 2021-01-01 20:11:19 EST
2021-01-17 17:06:21.484 EST [14867] LOG:  database system is ready to accept connections
 done
server started
waiting for server to shut down....2021-01-17 17:06:21.900 EST [14867] LOG:  received fast shutdown request
2021-01-17 17:06:21.900 EST [14867] LOG:  aborting any active transactions
2021-01-17 17:06:21.901 EST [14867] LOG:  background worker "logical replication launcher" (PID 14874) exited with exit code 1
2021-01-17 17:06:21.902 EST [14869] LOG:  shutting down
2021-01-17 17:06:21.908 EST [14867] LOG:  database system is shut down
 done
server stopped
==> Moving postgresql data from /usr/local/var/postgres to /usr/local/var/postgres.old...
==> Creating database...
The files belonging to this database system will be owned by user "qrush".
This user must also own the server process.

The database cluster will be initialized with locale "C".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /usr/local/var/postgres ... ok
creating subdirectories ... ok
selecting dynamic shared memory implementation ... posix
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting default time zone ... America/New_York
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok
syncing data to disk ... ok

initdb: warning: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    /usr/local/opt/postgresql/bin/pg_ctl -D /usr/local/var/postgres -l logfile start

==> Migrating and upgrading data...
Performing Consistency Checks
-----------------------------
Checking cluster versions                                   ok
Checking database user is the install user                  ok
Checking database connection settings                       ok
Checking for prepared transactions                          ok
Checking for reg* data types in user tables                 ok
Checking for contrib/isn with bigint-passing mismatch       ok
Creating dump of global objects                             ok
Creating dump of database schemas
                                                            ok
Checking for presence of required libraries                 ok
Checking database user is the install user                  ok
Checking for prepared transactions                          ok
Checking for new cluster tablespace directories             ok

If pg_upgrade fails after this point, you must re-initdb the
new cluster before continuing.

Performing Upgrade
------------------
Analyzing all rows in the new cluster                       ok
Freezing all rows in the new cluster                        ok
Deleting files from new pg_xact                             ok
Copying old pg_xact to new server                           ok
Setting next transaction ID and epoch for new cluster       ok
Deleting files from new pg_multixact/offsets                ok
Copying old pg_multixact/offsets to new server              ok
Deleting files from new pg_multixact/members                ok
Copying old pg_multixact/members to new server              ok
Setting next multixact ID and offset for new cluster        ok
Resetting WAL archives                                      ok
Setting frozenxid and minmxid counters in new cluster       ok
Restoring global objects in the new cluster                 ok
Restoring database schemas in the new cluster
                                                            ok
Copying user relation files
                                                            ok
Setting next OID for new cluster                            ok
Sync data directory to disk                                 ok
Creating script to analyze new cluster                      ok
Creating script to delete old cluster                       ok

Upgrade Complete
----------------
Optimizer statistics are not transferred by pg_upgrade so,
once you start the new server, consider running:
    ./analyze_new_cluster.sh

Running this script will delete the old cluster's data files:
    ./delete_old_cluster.sh
==> Upgraded postgresql data from 12 to 13!
==> Your postgresql 12 data remains at /usr/local/var/postgres.old

💚 brew services start postgresql
==> Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```

I hope that helps someone in the future!

## *P.S.: Cleanup!*

If your app works well once you've upgraded - your "old" Postgres 12 might take up a ton of space:

```shell
💚 du -ksh /usr/local/var/postgres.old
672M	/usr/local/var/postgres.old
```

You can clean that up with:

```shell
💚 rm -rf /usr/local/var/postgres.old
```

---

¤ [@qrush](https://twitter.com/qrush)
