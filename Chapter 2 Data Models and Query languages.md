
## Chapter 2 Data Models and Query languages

### NoSQL

NoSQL -> Not Only SQL

- Greater scalability
- Open Source
- Special query operations
- Dynamic data model

-- 


***Polyglot persistance*** -> multiple persitence approaches

***Impedance mismatch*** -> Disconect between database models and application models

-- 

Document oriented DBs:

- MongoDB
- RethinkDB
- CouchDB
- Espresso

### 1-M & M-N relationships

- Consistency of data
- Spelling
- Avoid ambiguity
- Ease of update
- Localization support
- Better search

Duplication of human meaningful information on every record if not extracted to table.

> Removing duplication => normalization

If database does not support joins normally application code needs to deal with the emulation of joins so the work of making them gets shifted from the DB to the app.

### Document DBs repeating history?

IMS 1968 -> Hierarchical model very similar to JSON = tree of records nested with records

This model supported many to one relationships but joins where no supported, this caused to solve the issue with 2 alternatives which where **relational model(SQL)** and **network-model**

#### Network model
CODASYL model


Main differente was that in the hierarchical model every record had exactly one parent but in the network model each record could have multiple parents tied to it. This allowed many to one and many to many relationships.

Links between records where more like programming points and not like foreign keys.

The way to access the data was to follow a path from root record along these chains (**access path**), similar to traversing a linked list.

Code to query was complicated and inflexible since queries worked like moving a cursor and had to keep track of all parents of a record to find the desired one.

#### Relational model

Lay data in the open, create a relation table.

Query optimizer decides which parts of the query to execute first and which indexes to use those choices are essentially the **access path** that was required in the network model, but the biggest difference here is that this was done by the optimizer and no by the developer.

#### Relations in relational and document databases
Both support many to many relationships really the only difference is that relational databases use ***foreign keys*** to do this and in the document databases ***document references*** are used at read time and they have not repeated the path of CODASYL.









