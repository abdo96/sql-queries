# A Toy Database

Here is a toy database which models an paper-store.  In this database, we have falling into three categories: information about the company, information about customers and their orders, and information about the products being sold.

You'll be editing the `queries.sql` file, which consists of a series of
"plain English" sentences.  Your job is to translate those sentences into a
single SQL `SELECT` query.  Don't worry if you can't do it — take your best shot!

The database itself is contained in the `papers_store_p&p.db` file.  The `import.sql`
file contains the raw SQL used to generate the toy database.  You don't need to
touch this file, but it's here in case we wind up needing it.  You'll be doing
your work in `queries.sql`.


## ERD
![alt text](https://github.com/abdo96/sql-queries/blob/master/paper_store_p%26p.png)

## Contents

1. [The Plain English Queries](#the-plain-english-queries)
2. [Configuring SQLite3](#configuring-sqlite3)
3. [The Database Tables](#the-database-tables)
  1. [Information About The Products](#information-about-the-products)
  2. [Information About Customers and Their Orders](#information-about-customers-and-their-orders)
  3. [Information About The Company](#information-about-the-company)
4. [One Key Idea](#one-key-idea)

## The Plain English Queries

See the `queries.sql`.  Open the database by running this in your terminal:

```bash
sqlite3 papers_store_p&p.db
```

Once inside, type the following at the SQLite3 prompt

```text
sqlite> .schema
```

to list all the tables in the database.  To see the schema for a specific table,
you can type, e.g., the `accounts` table

```text
sqlite> .schema accounts
```

Don't be afraid to explore the data in the tables to get a better feeling
for how it's formatted.  It's impossible to damage your database as long
as you're only running `SELECT` queries.  For example, what does the data
in the invoices table look like?  Well, let's look at a 5 rows

```sql
SELECT * FROM accounts LIMIT 5;
```

That's what we mean by "explore."

## Configuring SQLite3

The default settings for SQLite3 can make it hard to read the output of `SELECT`
queries.  To change these settings, run the following three commands once you're inside
the SQLite3 shell.  You should just be able to copy and paste these three lines.

```text
.header on
.mode column
.width 20
```

It should look roughly like this: http://cl.ly/image/24431S2W0I3f

If you want to make these settings permanent, copy and paste the following
command into your **terminal** (not the sqlite shell). **Note**: this will not
work on Windows.

```bash
cat <<EOF > ~/.sqliterc
.header on
.mode column
.width 20
EOF
```

You should copy and paste all 5 lines and it should look roughly like this: http://cl.ly/image/3w1Y0l1a0W38

Don't worry about what all the parts of this command are.  The short of it is
that this is adding lines to a file named `.sqliterc`, which SQLite3 uses to
determine its default settings.

## The Database Tables

Remember: relational databases are organized into tables.  Each table has a "schema", which dictates the fields (aka columns) that the data in the table can or must contain.  The data itself is stored as records (aka rows).

Here are the tables, organized by high-level purpose.

### Information About The Products

1. `accounts` are the account of each company buy the paper.
2. `orders` are each company has quantities and prices of each type of paper to be ordered.
3. `region` is the paper company has four regions such as West,NorthWest,etc.
4. `sales_reps` tell us about sales representative in four different regions.
5. `web_events` are list of channels ,like Facebook or Twitter,etc, to attract and advetise for seller companies.

### Information About Customers and Their Orders

1. `total` are the total sales for each seller company.
2. `(standerd-gloss-poster)-amt_usd` are different types of selling papers and their proces.
3. `(standerd-gloss-poster)-qty` are the quantity of each type of paper to be ordered.

## One Key Idea

One of the key ideas in how relational databases like SQLite3 and MySQL organize data is that we try to minimize redundancy by using references to other data rather than duplicating that data between different tables.

For example, every order belongs to one (and only one) account.  An account has its own associated information, like album id,name, and so on.  Each order also has its own associated information, like occurred_at,standard_qty, and standard_amt_usd.  We want to be able to ask questions like "What is the title of the account on which order X appears?"

If you imagine a spreadsheet with a bunch of order listings, one way to achieve this would be to have an "account id" column and to answer this question we would just look at the value in the "account Id" column for order X.  Every order on the same account would have the same value in the "account Id" column, although heaven help us if there are two separate albums with the same id!

This is not how we store information in a relational database.  Rather than storing account-related information in the same table as order-related information, we store account-related information in an "accounts" table and order-related information in a "orders" table.  We assign each order and account a unique id.  In the "orders" table we would when have an `account_id` column containing the unique account ID as a *reference* or *pointer* to the relevant row in the "accounts" table.

Excel can do this, but it is too cumbersome for the most common tasks.  In a relational database like SQLite3 or MySQL, however, it is much easier to deal with.  In fact, SQL (the language) is counting on you storing your data this way.

### Useful Resources

- http://stackoverflow.com/questions/17946221/sql-join-and-different-types-of-joins
- http://en.wikipedia.org/wiki/Join_(SQL)
