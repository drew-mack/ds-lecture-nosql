NoSQL Databases
===============

### Introduction

Before databases, computers would store data in formats such as files on disk,
or even paper with holes punched in it very early on. The emergence of the
database was a revolution in the field of data storage. At the time of their inception,
Structured Query Language, or SQL style databases quickly dominated the database space.
Data within SQL databases can only be stored once a relationship or relationships between
the data have been defined. For that reason, SQL databases are referred to as *relational*
databases. But just as storage media before them, the concept could be iterated upon
to better suit the needs of some tasks. To that end, many projects in the world of
software development have taken to employing NoSQL databases.

Exactly how they sound, NoSQL databases do not require the definition or a relational structure
in order to start storing data. This freedom grants many of strengths present in NoSQL
databases that their SQL sisters lack. The two critical advantages for NoSQL are
syntax and scalability. Although quality SQL syntax resources are plentiful[^1], the
necessary notation for describing and working within the pre-defined schema of a
relational database can feel restrictive. As far as scalability, SQL databases scale
vertically, whereas NoSQL databases scale horizontally. What that essentially means is
SQL databases need a more powerful server in order to serve more clients, but a NoSQL
database only needs more servers at all. This flexibility allows NoSQL databases to
potentially expand much more rapidly than SQL servers, thus accomodating datasets that
increase in size more rapidly[^2].

Many NoSQL database frameworks have been developed by various software manufacturers,
and each comes with its own strengths and weaknesses. Some of the most popular are
MongoDB, Redis, HBase, DynamoDB, Neo4j, InfoGrid, and the list goes on. Aside from
implementation, the necessity of different frameworks comes from the desire for different
data storage structures. Whereas SQL databases typically only come in one storage structure,
tables, NoSQL databases have variety. Some of these structures are document storage as in
MongoDB[^3], coulumn storage as in HBase[^4], graph storage as in InfoGrid, and key value storage
as in Redis[^5]. Each has its ideal use case, and each could stand as a section of its own.
For the purpose of introduction, we have elected to use Redis as its base functionality is
relatively simple, but its full potential is vast.

![redis logo](./images/redis_logo.png)

Redis is a NoSQL database focused on efficiency. Based on this focus, it operates mainly in memory
rather than on disk in order to reduce write and read times. On disk data representations are supported
by means of dumping, or by maintaining a log of actions that can be used to replicate a failed instance.
Its key value data storage supports values stored as strings, lists, sets, bitmaps, hyperloglogs, and
geospatial indicies[^5]. In addition to serving as a cache in this way, redis provides a host of
other features such as providing a publisher/subscriber messenger service, server-based Lua script
execution, and built in time to live support.

### Installing Redis

As officially recommended by Redis Labs, the following is the standard installation method for redis on
any Debian based Linux distrobution, including Ubuntu, with Apt. Many other dependency resolvers such as
YUM or ZYpp standard in other Linux distrobutions will also cleanly install Redis.

~~~~~
sudo apt-get install redis-server
~~~~~

After installing, to run an instance of Redis one would send the following command.

~~~~~
sudo service redis-server start
~~~~~

### Getting Started

Now that one has an instance of the server up and running on the default host and port, `127.0.0.1`,
and `6379` respectively, one can connect to it. The command for such is as follows.

~~~~~
redis-cli
~~~~~

After sending that command, bash will switch from prompting the user with `$PS1` on new lines, to prompting with
`127.0.0.1:6379>` on new lines in this case. It is possible to connect to a different instance of Redis server
in this way, or to initiate an instance of the Redis server in a non-default location but for demonstration
purposes, we will keep it simple.

At this point, since the server instance is brand new, nothing is in it. One can verify this with the following
command.

~~~~~
127.0.0.1:6379> KEYS *
(empty list or set)
~~~~~

Redis functions that support finding keys such as `KEYS` and `SCAN` use glob-style pattern matching. Supported
pattern syntax is as follows[^6]:
* h?llo matches hello, hallo and hxllo
* h*llo matches hllo and heeeello
* h[ae]llo matches hello and hallo, but not hillo
* h[^e]llo matches hallo, hbllo, ... but not hello
* h[a-b]llo matches hallo and hbllo
* \\\]\\\^\\\*\\\? matches \]\^\*\?
So in the previous example, `KEYS *` returns every possible key as the `*` pattern means any combination of
characters.

[^1]: Refsnes Data, 2019, https://www.w3schools.com/sql/  
[^2]: Laura Shiff, 2018, https://www.bmc.com/blogs/sql-vs-nosql/  
[^3]: Mongo DB, 2019, https://www.mongodb.com/what-is-mongodb  
[^4]: Apache Software Foundation, 2019, http://hbase.apache.org/  
[^5]: Redis Labs, 2019, https://redis.io/topics/introduction
[^6]: Redis Labs, 2019,  https://redis.io/commands/keys
