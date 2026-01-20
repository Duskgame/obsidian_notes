[Room](https://developer.android.com/topic/libraries/architecture/room) is a persistence library that's part of Android [Jetpack](https://developer.android.com/jetpack/androidx/explorer?case=data). Room is an abstraction layer on top of a [SQLite](https://developer.android.com/training/data-storage/sqlite) database. SQLite uses a specialized language (SQL) to perform database operations. Instead of using SQLite directly, Room simplifies the chores of database setup, configuration, and interactions with the app. Room also provides compile-time checks of SQLite statements.

An _abstraction layer_ is a set of functions that hide the underlying implementation/complexity. It provides an interface to an existing set of functionality, like SQLite in this case.

The image below shows how Room, as a data source, fits in with the overall architecture recommended in this course. Room is a Data Source.

![[image-29.png|296x264]]