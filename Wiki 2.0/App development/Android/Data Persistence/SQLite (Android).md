SQLite is a common way provided by the [[Android]] SDK for Android apps to persist data. SQLite provides a relational [[Database]] that allows you to represent data in a similar way to how you structure data with [[Kotlin]] classes. SQL—Structured Query Language—, while not an actual programming language, provides a simple and flexible way to read and modify a SQLite database with just a few lines of code.

**Note:** Android apps have a number of ways to store data, including both internal and external storage. This [[Unit]] discusses Room and Preferences Datastore. To learn more about the different methods for storing data on Android, refer to the [Data and file storage overview](https://developer.android.com/training/data-storage).

## What is SQLite?

SQLite is a commonly used relational database. Specifically, SQLite refers to a lightweight C library for relational database management with Structured Query Language, known as SQL and sometimes pronounced as "sequel" for short.

You won't have to learn C or any entirely new programming language to work with a relational database. SQL is simply a way to add and retrieve data from a relational database with a few lines of code.

**Note:** Not all databases are organized into tables, columns, and rows. Other kinds of databases, known as NoSQL, are structured similarly to a [[JSON]] [[Kotlin Object|Object]] with nested pairs of keys and values. Examples of NoSQL databases include Redis or [Cloud Firestore](https://firebase.google.com/docs/firestore).


## Representing data with SQLite

In Kotlin, you're familiar with data types like `Int` and `Boolean`. SQLite databases use data types too! Data table columns must have a specific [[Data Type]]. The following table [[Maps]] common Kotlin data types to their SQLite equivalents.

|   |   |
|---|---|
|**Kotlin data type**|**SQLite data type**|
|`Int`|INTEGER|
|`String`|VARCHAR or TEXT|
|`Boolean`|BOOLEAN|
|`Float`, `Double`|REAL|

The tables in a database and the columns in each table are collectively known as the _schema_. In the next section, you download the starter data set and learn more about its schema.


