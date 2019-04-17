NoSQL Databases
===============

Introduction
------------

Before databases, computers would store data in formats such as files on
disk, or even paper with holes punched in it very early on. The
emergence of the database was a revolution in the field of data storage.
At the time of their inception, Structured Query Language, or SQL style
databases quickly dominated the database space. Data within SQL
databases can only be stored once a relationship or relationships
between the data have been defined. For that reason, SQL databases are
referred to as *relational* databases. But just as storage media before
them, the concept could be iterated upon to better suit the needs of
some tasks. To that end, many projects in the world of software
development have taken to employing NoSQL databases.

Exactly how they sound, NoSQL databases do not require the definition or
a relational structure in order to start storing data. This freedom
grants many of strengths present in NoSQL databases that their SQL
sisters lack. The two critical advantages for NoSQL are syntax and
scalability. Although quality SQL syntax resources are plentiful [1]_,
the necessary notation for describing and working within the pre-defined
schema of a relational database can feel restrictive. As far as
scalability, SQL databases scale vertically, whereas NoSQL databases
scale horizontally. What that essentially means is SQL databases need a
more powerful server in order to serve more clients, but a NoSQL
database only needs more servers at all. This flexibility allows NoSQL
databases to potentially expand much more rapidly than SQL servers, thus
accomodating datasets that increase in size more rapidly [2]_.

Many NoSQL database frameworks have been developed by various software
manufacturers, and each comes with its own strengths and weaknesses.
Some of the most popular are MongoDB, Redis, HBase, DynamoDB, Neo4j,
InfoGrid, and the list goes on. Aside from implementation, the necessity
of different frameworks comes from the desire for different data storage
structures. Whereas SQL databases typically only come in one storage
structure, tables, NoSQL databases have variety. Some of these
structures are document storage as in MongoDB [3]_, coulumn storage as
in HBase [4]_, graph storage as in InfoGrid, and key value storage as in
Redis [5]_. Each has its ideal use case, and each could stand as a
section of its own. For the purpose of introduction, we have elected to
use Redis as its base functionality is relatively simple, but its full
potential is vast.

.. figure:: ./images/redis_logo.png
   :alt: redis logo

   redis logo

.. [1]
   Refsnes Data, 2019, https://www.w3schools.com/sql/

.. [2]
   Laura Shiff, 2018, https://www.bmc.com/blogs/sql-vs-nosql/

.. [3]
   Mongo DB, 2019, https://www.mongodb.com/what-is-mongodb

.. [4]
   Apache Software Foundation, 2019, http://hbase.apache.org/

.. [5]
   Redis Labs, 2019, https://redis.io/topics/introduction
