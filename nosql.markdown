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
execution, pipelines that support sending many commands at the same time to save on network round
trip time, streams that act like maps with built in history, and built in time to live support
for an instance's keys.

### Getting Started

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

### Lists

To alleviate the lack of members in the Redis server, one could add a list like so.

~~~~~
127.0.0.1:6379> RPUSH good_list "good value"
(integer) 1
~~~~~

It is important to note here that we haven't defined `good_list` at any point before appending the string
`"good value"` to it. This is because of the practicality built-in to Redis' semantic design. Before a list
exists, the only way to add to it is to `RPUSH` a value or values into it. And in case one wants to be sure
that the list they are trying to append to already exists before they do so, `RPUSHX` satisfies that
use case. The other stipulation that `RPUSHX` imposes is that only one value can be added to a list with it,
whereas `RPUSH` allows for appending of any number of items to a list in one command. Redis also supports
 the same operations from the beginning of the list, rather than the end, with `LPUSH` and `LPUSHX`.

A bit of important information that redis gives the user with `RPUSH` is the integer in the return statement,
`(integer) 1`. The parenthesized `integer` in the return is merely a quality of life feature standard in
redis returns. Here its inclusion serves to explicity indicate that the value `1` is an integer and not a
string. The number 1 itself represents the length of the list after `RPUSH`ing `"good_value"`. We will see
the same kind of inclusion with `LRANGE` right now, along with `RPUSH` appending multiple values to `good_list`.

~~~~~
127.0.0.1:6379> RPUSH good_list "another good one" 45
(integer) 3
127.0.0.1:6379> LRANGE good_list 0 -1
1) "good value"
2) "another good one"
3) "45"
~~~~~

In this example, `LRANGE` is used to retrieve a subset of values from `good_list` beginning at index `0` and
ending at index `-1`. Redis supports indexing based on relative position from the front with positive numbers
and 0, as well as from relative position to the back with negative numbers. Given the indicies, the
subset this particular `LRANGE` retrieved is the entire list, as is the case with all `0 -1` retrievals.
On the note of indicies, Redis accomodates those that might be considered invalid. For instance, a starting
index bigger than the ending index will simply return an empty list of results, and an ending index
larger than the actual final index in the list will default to the final index of the list.

Speaking of indicies, Redis also supports index based item retrieval like so.

~~~~~
127.0.0.1:6379> LINDEX good_list 2
"45"
~~~~~

We hope this short demonstration serves as an easy introduction to Redis lists, but the support that Redis
offers for lists doesn't stop there by a long shot. In addition to what is shown here, they support:
* Popping from the left or right, blocking or non-blocking
* Retrieving the length of a list
* Inserting at a given index
* Setting a given index's value
* Removing at a given index
* Trimming to a desired size
* Cycling the member on the back of a list to the front

### More Storage Options

As mentioned before, Redis actually supports quite a few data-structure like models for storing data. Each
of these other storage formats suit the needs of some tasks better than others. Some of the broader-application
models will be expanded upon briefly in this section.

#### Strings

Strings are the most basic data types supported by Redis. Despite their simplicity, they have some
interesting capabilities. They are obatained and set with `GET` and `SET`, but they can also be obtained
and set multiple-at-a-time style with `MGET` and `MSET`. The `GETSET` command will return the current value
and replace it with the value provided. Redis also supports implied integer parsing and incrementation with
`INCR`, and `EXPIRE` and `PERSIST` allow users to control whether a key will eventually delete itself with the
previously mentioned time to live feature. All key value types support this feature with their own
variations of these commands.

#### Hashes

Redis hashes function like, well, hash maps. When defined as follows, they support all of the functionalities
that one might expect of a hash map such as accessing and setting by field.

~~~~~
127.0.0.1:6379> HMSET hash_key field1 value1 field2 value2
OK
127.0.0.1:6379> HGET hash_key field1
"value1"
127.0.0.1:6379> HGETALL hash_key
1) "field1"
2) "value1"
3) "field2"
4) "value2"
~~~~~

These hashes are particularly useful for generally representing objects from any object oriented programming
language.

#### Sets

Redis sets perform in much the same way as mathmatically defined sets with members being unordered and unique.
In addition to accomodating sets, Redis natively supports basic set-oriented operations such as intersections
and unions. Some of their practicality comes from being able to internally track membership of other keys in
certain groups without needing to pull data into a different context to perform useful membership-focused analyses.

#### Sorted Sets

In addition to traditionally implemented sets, Redis supports a bit of a fusion of hashes and sets referred to as
sorted sets. This fusion derives from the unique member guarantee coupled with a "score" attatched to each
member which Redis uses to sort them. Scores are determined by the user at the time of entry, editable at any time, and duplicate scores
are tolerated. To sort identically scored members, Redis evaluates lexigraphical score which is necessarily
unique since members are necessarily unique. Though less efficient in some functionality, sorted sets excel at
tasks where fast score-based retrieval is helpful such as retrieving frequency of contact with friends on a social media website.



[^1]: Refsnes Data, 2019, https://www.w3schools.com/sql/  
[^2]: Laura Shiff, 2018, https://www.bmc.com/blogs/sql-vs-nosql/  
[^3]: Mongo DB, 2019, https://www.mongodb.com/what-is-mongodb  
[^4]: Apache Software Foundation, 2019, http://hbase.apache.org/  
[^5]: Redis Labs, 2019, https://redis.io/topics/introduction  
[^6]: Redis Labs, 2019,  https://redis.io/commands/keys  
