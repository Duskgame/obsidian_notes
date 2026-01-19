## What is a database?

If you are familiar with a spreadsheet program like Google Sheets, you are already familiar with a basic analogy for a database.

A spreadsheet consists of separate data tables, or individual spreadsheets in the same workbook.

Each table consists of columns that define what the data represents and rows that represent individual items with values for each column. For example, you might define columns for a student's ID, name, major, and grade.

![[image-24.png]]

Each row contains data for a single student, with values for each of the columns.

![[image-25.png]]

A relational database works the same way.

- Tables define high-level groupings of data you want to represent, such as students and professors.
- Columns define the data that each row in the table contains.
- Rows contain the actual data that consist of values for each column in the table.

The structure of a relational database also mirrors what you already know about classes and objects in Kotlin.

```
data class Student(
    id: Int,
    name: String,
    major: String,
    gpa: Double
)
```

- Classes, like tables, model the data you want to represent in your app.
- Properties, like columns, define the specific pieces of data that every instance of the class should contain.
- Objects, like rows, are the actual data. Objects contain values for each property defined in the class, just as rows contain values for each column defined in the data table.

Just as a spreadsheet can contain multiple sheets and an app can contain multiple classes, a database can contain multiple tables. A database is called a relational database when it can model relationships between tables. For example, a graduate student might have a single professor as a doctoral advisor whereas that professor is the doctoral advisor for multiple students.

![[image-26.png|745x216]]

Every table in a relational database contains a unique identifier for rows, such as a column where the value in each row is an automatically incremented integer. This identifier is known as the primary key.

When a table references the primary key of another table, it is known as a foreign key. The presence of a foreign key means there's a relationship between the tables.

**Note:** Like with Kotlin classes, the convention is to use the singular form for the name of database tables. For the example above, that means you name the tables `teacher`, `student`, and `course`, not the plural forms of `teachers`, `students`, and `courses`.

