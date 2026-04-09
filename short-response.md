# Short Response: Schema Design and Normalization

Answer each question below. Write in complete sentences (3–5 per answer).

---


## Question 1

The table below stores data for a library's checkout system. Identify every normalization rule it violates and describe how you would fix the schema. You do not need to write SQL — describe the tables you would create and why.

| checkout_id | patron_id | patron_name | patron_email     | book_id | book_title                | author_name       | genres                   |
| ----------- | --------- | ----------- | ---------------- | ------- | ------------------------- | ----------------- | ------------------------ |
| 1           | 10        | Maya Patel  | maya@email.com   | 201     | The Left Hand of Darkness | Ursula K. Le Guin | Science Fiction, Fantasy |
| 2           | 11        | Jordan Kim  | jordan@email.com | 202     | Beloved                   | Toni Morrison     | Fiction, Historical      |
| 3           | 10        | Maya Patel  | maya@email.com   | 202     | Beloved                   | Toni Morrison     | Fiction, Historical      |
| 4           | 12        | Sam Torres  | sam@email.com    | 201     | The Left Hand of Darkness | Ursula K. Le Guin | Science Fiction, Fantasy |

**Your answer:**

This flat table violates two normalization rules:
**Normalization Rule #2: Atomic Values**: The `genres` column has many values in a cell, which means it isn’t atomic because each cell should store one value.
**Normalization Rule #3: Primary Key Dependency**: Columns like `patron_name`, `patron_email`, `book_title`, etc. are not dependent on the primary key: `checkout_id`. For example, `patron_name` and `patron_email` are dependent on `patron_id` and not their`checkout_id`. This also causes data redundancy since Maya Patel’s name and email is repeated twice.

To fix the schema, I’d separate the flat table into four tables.

**Table 1**: A `patrons` table storing `patron_id`, `patron_name`, and `patron_email` since a patron is dependent on `patron_id` and not `checkout_id`.

**Table 2**: A `books` table storing `book_id`, `book_title`, `author_name` since a book is dependent on `book_id` and not `checkout_id`.

**Table 3**: A `checkouts` table storing `checkout_id`, and storing `patron_id` and `book_id` as foreign keys because the `checkouts` table only needs to know the  `patron_id`, and `book_id` not data such as the `patron_name` or `book_title`. This avoids the repeated data from the previous table.

**Table 4**: A `book_genres` association storing `book_id` and `genre` to handle the many to many relationship between books and genres. This avoids storing non-atomic values like the previous table. 

## Question 2

Explain the difference between one-to-many relationships and many-to-many relationships by providing a real world example of each. Then describe how each is represented in a relational database. Use the term "association/bridge" table in your response.

**Your answer:**

A **one to many relationship** is when one row in a table can be referenced by many rows in another table. For example, a single venue can have many events, but a single event can only have one venue. In a relational database, this is represented by storing a foreign key like `venue_id` on the `events` table (the “many” side of the relationship) that references the `venues` table. A **many to many relationship** is when rows in each table can reference many rows in the other table. For example, a book can have many authors, and an author can have many books. In a relational database, this can be represented through an association table like `book_authors` that stores `book_id` and `author_id` as foreign keys to pair a book to its author in each row. Storing two foreign keys allows each row to have more than one pairing.

## Question 3

What is referential integrity? How does PostgreSQL enforce it, and why does this enforcement determine the order in which you must create — and drop — tables?

**Your answer:**

---

## Question 4

Why does an association table need a `UNIQUE (col1, col2)` constraint on its two foreign key columns? What specific problem does this prevent, and why wouldn't making each column individually `UNIQUE` solve it?

**Your answer:**

---
