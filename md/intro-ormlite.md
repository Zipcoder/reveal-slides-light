# Intro ORMLite

-
-

# Persistance

Persistence simply means that we would like our application’s data to outlive the applications process

_

## How do we save save data?

-

* simple text file - CSV
* spreadsheets
* databases

-

## Different types of databases

-

## Relational
* mysql
* h2
* sqlite
* oracle
* ms sql server

-

## Non-relational
* dynamoDb
* mongoDb
* firebase

-
-

# CRUD
* Create
* Read
* Update
* Delete

The four basic functions that models should be able to do, at most.

_

When we are building APIs, we want our models to provide four basic types of functionality.

-

The CRUD paradigm is common in constructing web applications, because it provides a memorable framework for reminding developers of how to construct full, usable models.

-
-

# ORM
## Object Relational Mapping

-

The technique to convert data between object model and relational database is known as object-relational mapping (ORM, O/RM and O/R mapping).

-

### Object State vs Table Schema

-

```
class Person(){
	int id;
	String firstName;
	String lastName;
	String dob;
	int height;
	int weight;
	String occupation;
}
```

-

| id  | first_name  | last_name | dob     | height | weight | occupation |
| --- |-------------| --------- | ------- | ------ | ------ | ---------- |
| 1   | Jon         | Lennon    | 10/9/40 | 5'10   | 160    | car sales  |
| 2   | Paul        | McCartney | 6/18/42 | 5'11   | 175    | pilot      |
| 3   | Ringo       | Starr     | 7/7/40  |	5'9    | 165    | dog groomer|

-

### Java ORMs

* Hibernate (widely used)
* AcitveJPA (open-source)
* Athena Framwork (Adobe)
* ORMLite (light weight ORM implementation)

-
-

# ORMLite

-

Object Relational Mapping Lite (ORM Lite) provides some simple, lightweight functionality for persisting Java objects to SQL databases while avoiding the complexity and overhead of more standard ORM packages.

-

To use ORMLite in your project if you are using maven, then you should add the following configuration stanza to your pom.xml file.

```
<!-- this has a dependency on ormlite-core -->
<dependency>
		<groupId>com.j256.ormlite</groupId>
		<artifactId>ormlite-jdbc</artifactId>
		<version>5.1</version>
</dependency>
```

-

## Anotations

-

### Annotation is Metadata

Metadata is data about data. So Annotations are metadata for code

```
@Override
public String toString() {
return "This is String Representation of current object.";
}
```

-

To setup your classes to be persisted you need to do the following things:

* Add the @DatabaseTable annotation to the top of each class. You can also use @Entity.
* Add the @DatabaseField annotation right before each field to be persisted. You can also use @Column and others.
* Add a no-argument constructor to each class with at least package visibility.

-

ORMLite also allows for JPA Annotations

```
@Entity(name = "accounts")
public class Account {

    @Id
    private String name;

    @Column(nullable = false)
    private String password;
```

-

All persisted classes must define a no-argument constructor with at least package visibility.

```
public Account() {
		// ORMLite needs a no-arg constructor
}

public Account(String name, String password) {
		this.name = name;
		this.password = password;
}
```

-

### Connections Sources

To use the database and the DAO objects, you will need to configure what JDBC calls a DataSource (see the javax.sql.DataSource class) and what ORMLite calls a ConnectionSource. A ConnectionSource is a factory for connections to the physical SQL database.

```
// single connection source example for a database URI
ConnectionSource connectionSource =
  new JdbcConnectionSource("jdbc:h2:mem:account");
```

-

### DAO
### Data Access Object

The Data Access Object (DAO) pattern is a structural pattern that allows us to isolate the application/business layer from the persistence layer (usually a relational database, but it could be any other persistence mechanism) using an abstract API.

-

Once you have annotated your classes and defined your ConnectionSource you will need to create the Data Access Object (DAO) class(es), each of which will handle all database operations for a single persisted class

```
Dao<Account, String> accountDao = DaoManager.createDao(connectionSource, Account.class);
Dao<Order, Integer> orderDao = DaoManager.createDao(connectionSource, Order.class);
```
-

### Identity columns

Database rows can be identified by a particular column which is defined as the identity column. Rows do not need to have an identity column but many of the DAO operations (update, delete, refresh) require an identity column.


* Fields With id	  	Using fields with id = true.
* Fields With generatedId	  	Using fields with generatedId = true.

-

### DAO Usage

The following database operations are easily accomplished by using the DAO methods:

```
Account account = new Account();
account.name = "Jim Coakley";
accountDao.create(account);
```

```
Account account = accountDao.queryForId(name);
if (account == null) {
  account not found handling …
}
```

```
account.password = "_secret";
accountDao.update(account);
```

```
accountDao.delete(account);
```
-

## Raw SQL statements

The built-in methods available in the Dao interface and the QueryBuilder classes don’t provide the ability to handle all types of queries. For example, aggregation queries (sum, count, avg, etc.) cannot be handled as an object since every query has a different result list.

-

```
// find out how many orders account-id #10 has
GenericRawResults<String[]> rawResults =
  orderDao.queryRaw(
    "select count(*) from orders where account_id = 10");
// there should be 1 result
List<String[]> results = rawResults.getResults();
// the results array should have 1 value
String[] resultArray = results.get(0);
// this should print the number of orders that have this account-id
System.out.println("Account-id 10 has " + resultArray[0] + " orders");
```

-

```
QueryBuilder<Account, Integer> qb = accountDao.queryBuilder();
qb.where().ge("orderCount", 10);
results = accountDao.queryRaw(qb.prepareStatementString());
```

-

## Foriegn Objects

TB/NTB
