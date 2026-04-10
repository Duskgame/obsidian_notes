**JDBC** (Java Database Connectivity) is the standard interface that Java/Kotlin uses to talk to databases.

## The problem it solves

Every database (PostgreSQL, MySQL, H2, SQLite) has its own internal communication protocol. These protocols are all different and incompatible. Without a standard, you'd have to write completely different code for every database you wanted to use.

JDBC defines a standard set of interfaces (`connect()`, `executeQuery()`, etc.) that every database driver must implement. Your application code talks to JDBC — never directly to a specific database.

## Database drivers

A **database driver** is the adapter between JDBC and a specific database. It translates the standard JDBC calls into the database's own protocol.

```
Your code (e.g. Exposed)
      ↓ calls standard JDBC interface
Database driver (e.g. h2.jar, postgresql.jar)
      ↓ translates to database-specific protocol
Actual database
```

Each database has its own driver you add as a dependency:

```kotlin
implementation("com.h2database:h2:2.2.224")              // H2
implementation("org.postgresql:postgresql:42.7.3")        // PostgreSQL
implementation("org.xerial:sqlite-jdbc:3.45.1.0")         // SQLite
```

## Connection URL

The JDBC connection URL tells the driver where the database is and how to connect:

```
jdbc:h2:file:./data/kwizz      → H2, stored as a file at ./data/kwizz.mv.db
jdbc:postgresql://localhost:5432/kwizz → PostgreSQL server on port 5432
```

Format: `jdbc:<database-type>:<location>`

## Why you rarely touch JDBC directly

Libraries like **Exposed** (Kotlin) or **Room** (Android) sit on top of JDBC and give you a Kotlin API. They handle the low-level JDBC calls internally. You mostly just need to know JDBC exists so you understand what "driver" and "connection URL" mean when configuring these libraries.

## Related

- [[Database]]
- [[Exposed]]
- [[Room]]
