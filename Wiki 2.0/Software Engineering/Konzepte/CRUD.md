CRUD stands for Create, Read, Update, and Delete—the four fundamental operations for managing persistent data in databases, applications, and APIs.[](https://www.sumologic.com/glossary/crud)

## CRUD Operations

Create adds new records, such as inserting a user into a [[Database]] table. Read retrieves data, often via queries to fetch [[Lists]] or specific items. Update modifies existing records, like changing an email address. Delete removes data, typically with safeguards like soft deletes to mark records inactive.[](https://www.huntress.com/cybersecurity-101/topic/what-are-crud-operations)

## SQL Examples

Relational databases map CRUD directly to [[SQL Statements]].[](https://www.codecademy.com/article/what-is-crud-explained)

|Operation|SQL Command|Example|
|---|---|---|
|Create|INSERT|INSERT INTO users (name) VALUES ('Alice');[](https://www.huntress.com/cybersecurity-101/topic/what-are-crud-operations)​|
|Read|[[SQL SELECT]]|SELECT * FROM users WHERE id=1;[](https://www.crowdstrike.com/en-us/cybersecurity-101/observability/crud/)​|
|Update|UPDATE|UPDATE users SET name='Bob' WHERE id=1;[](https://www.devart.com/dbforge/sql/sqlcomplete/crud-operations-in-sql.html)​|
|Delete|DELETE|DELETE FROM users WHERE id=1;[](https://www.sumologic.com/glossary/crud)​|

## Common Applications

CRUD powers most data-driven apps, from web forms to [[REST  Android|REST]] APIs (e.g., POST for Create, GET for Read). In [[Model-View-ViewModel|MVVM]] (from prior context), these operations reside in the Model layer, coordinated by [[ViewModel]] commands for [[User Interface|UI]] updates.