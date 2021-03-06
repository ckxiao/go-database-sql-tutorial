---
layout: article
title: The Connection Pool
---

There is a basic connection pool in the `database/sql` package. There isn't a
lot of ability to control or inspect it, but here are some things you might find
useful to know:

* Connections are created when needed and there isn't a free connection in the pool.
* By default, there's no limit on the number of connections. If you try to do a lot of things at once, you can create an arbitrary number of connections. This can cause the database to return an error such as "too many connections."
* In Go 1.1 or newer, you can use `db.SetMaxIdleConns(N)` to limit the number of *idle* connections in the pool. This doesn't limit the pool size, though.
* In Go 1.2.1 or newer, you can use `db.SetMaxOpenConns(N)` to limit the number of *total* open connections to the database. Unfortunately, a [deadlock bug](https://groups.google.com/d/msg/golang-dev/jOTqHxI09ns/x79ajll-ab4J) [[fix](https://code.google.com/p/go/source/detail?r=8a7ac002f840)] prevents `db.SetMaxOpenConns(N)` from safely being used in 1.2.
* Connections are recycled rather fast. Setting a high number of idle connections with `db.SetMaxIdleConns(N)` can reduce this churn, and help keep connections around for reuse.

**Previous: [Working with Unknown Columns](varcols.html)**
**Next: [Surprises, Antipatterns and Limitations](surprises.html)**
