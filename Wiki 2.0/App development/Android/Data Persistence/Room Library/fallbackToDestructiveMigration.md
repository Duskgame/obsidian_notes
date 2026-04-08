`fallbackToDestructiveMigration()` is a Room database builder option that tells Room to wipe and recreate the database whenever it detects a schema version change that has no written migration path.

```kotlin
Room.databaseBuilder(context, KwizzDatabase::class.java, "kwizz_database")
    .fallbackToDestructiveMigration()
    .build()
```

Without it, Room throws an `IllegalStateException` on launch if the database version was bumped but no migration was provided.

**When to use:** During development, when you don't have real user data to preserve and don't want to write migrations for every schema change.

**When to remove:** Before shipping to production — at that point you need proper migrations so users don't lose their data on app update. See [[Database Instance]] for migration setup.
