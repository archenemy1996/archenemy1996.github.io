---
layout: post
title: Database Design and Performance 
categories: [Database, PostgreSQL]
tags: [Database, PostgreSQL, Queries]
featured-image:  Linux Modules/icon.jpeg
featured-image-alt: none 
---

# Part-1 : DB design and normalization

### Designing tables

At the start of a new project, or a new feature in the project, one of the first things that we are required to define or design are the entities that interact with the system. In our library project, we had users, addresses, books as entities and we designed tables and relationships based on how we assumed these entities would interact. As the project evolves and requirements change, the design of our tables may result in inconsistencies and anomalies when running queries.

Take a look at our books table. A book can have one and only one author, but many books are written collaboratively.

``` sql
library=# \d books
                                       Table "public.books"
     Column     |            Type             |                     Modifiers
----------------+-----------------------------+----------------------------------------------------
 id             | integer                     | not null default nextval('books_id_seq'::regclass)
 title          | character varying(100)      | not null
 author         | character varying(100)      | not null
 published_date | timestamp without time zone | not null
 isbn           | integer                     |
Indexes:
    "books_pkey" PRIMARY KEY, btree (id)
    "books_isbn_key" UNIQUE CONSTRAINT, btree (isbn)
Referenced by:
    TABLE "reviews" CONSTRAINT "reviews_book_id_fkey" FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE
    TABLE "users_books" CONSTRAINT "users_books_book_id_fkey" FOREIGN KEY (book_id) REFERENCES books(id) ON UPDATE CASCADE
```

For example how do we insert the book "http://www.amazon.com/gp/product/1937077632" Smart Money Smart Kids, by Dave Ramsey and Rachel Cruz.

``` sql
library=# INSERT INTO books(title, author, published_date) VALUES ('Smart Money Smart Kids', 'Dave Ramsey', '4/29/2014');
INSERT 0 1
library=# INSERT INTO books(title, author, published_date) VALUES ('Smart Money Smart Kids', 'Rachel Cruz', '4/29/2014');
INSERT 0 1
```  

Now we have duplicate data in our database

``` sql
library=# SELECT * FROM books WHERE title = 'Smart Money Smart Kids';
 id |         title          |   author    |   published_date    | isbn
----+------------------------+-------------+---------------------+------
 23 | Smart Money Smart Kids | Dave Ramsey | 2014-04-29 00:00:00 |
 24 | Smart Money Smart Kids | Rachel Cruz | 2014-04-29 00:00:00 |
(2 rows)
```

Not only do we have duplicate id's for the same book, but this anomaly could be carried into the users table when checking out a book. These database inconsistencies occur all the time in real life work situations, database developers and administrators have to work together to redesign/normalize the database and also clean up the data.

The other way we could have done it was to add another author field call it author_2. In this case, we would have the same book_id, but multiple authors

``` sql
Table "public.books"
Column         |            Type             |                     Modifiers
---------------+-----------------------------+----------------------------------------------------
id             | integer                     | not null default nextval('books_id_seq'::regclass)
title          | character varying(100)      | not null
author         | character varying(100)      | not null
author_2       | character varying(100)      | not null
published_date | timestamp without time zone | not null
isbn           | integer                     |


 id |         title          |   author    |   author_2    |   published_date    | isbn
----+------------------------+-------------+---------------+---------------------+------
 23 | Smart Money Smart Kids | Dave Ramsey |  Rachel Cruz  | 2014-04-29 00:00:00 |

```

### Normalizing a database
