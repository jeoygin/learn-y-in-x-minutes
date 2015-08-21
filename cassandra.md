# Learn Cassandra In X Minutes

## Overview

Apache Cassandra is a massively scalable open source NoSQL database. Cassandra is perfect for managing large amounts of data across multiple data centers and the cloud. Cassandra delivers continuous availability, linear scalability, and operational simplicity across many commodity servers with no single point of failure, along with a powerful data model designed for maximum flexibility and fast response times.

Cassandra has a “masterless” architecture, meaning all nodes are the same. Cassandra provides automatic data distribution across all nodes that participate in a “ring” or database cluster.

Cassandra also provides customizable replication, storing redundant copies of data across nodes that participate in a Cassandra ring.

## Installation

### Prerequisites

* Java 7

### Download

Download [apache-cassandra-2.1.2-bin.tar.gz](http://mirror.bit.edu.cn/apache/cassandra/2.1.2/apache-cassandra-2.1.2-bin.tar.gz) from http://cassandra.apache.org/download/

### Configuration

The Cassandra configuration files can be found in the conf folder.

#### Data Directories

The following settings are in **conf/cassandra.yaml**.

* data_file_directories: /var/lib/cassandra/data (Default: $CASSANDRA_HOME/data/data)
* commitlog_directory: /var/lib/cassandra/commitlog (Default: $CASSANDRA_HOME/data/commitlog)
* saved_caches_directory: /var/lib/cassandra/saved_caches (Default: $CASSANDRA_HOME/data/saved_caches)

#### Log

Log config file is **conf/logback.xml**

### Start

Type the following command to start Cassandra:

```bash
$ bin/cassandra -f
```

Cassandra can be run in the background without the "-f" option.

### cqlsh

**bin/cqlsh** is an interactive command line interface for Cassandra.

Type the following command to connect to Cassandra:

```bash
$ bin/cqlsh
Connected to Test Cluster at 127.0.0.1:9042.
[cqlsh 5.0.1 | Cassandra 2.1.2 | CQL spec 3.2.0 | Native protocol v3]
Use HELP for help.
cqlsh> 
```

In sqlsh, commands are terminated with a semicolon (';'). 'help;' command can access online help.

**Create keyspace:**

Keyspace is a namespace of tables.

```cql
CREATE KEYSPACE test
WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
```

After that, use the new keyspace.

```cql
USE test;
```

**Create table:**

```cql
CREATE TABLE user (
  user_id int PRIMARY KEY,
  first_name text,
  last_name text
);
```

**Insert data:**

```cql
INSERT INTO user (user_id,  first_name, last_name)
  VALUES (1, 'Ming', 'Li');
INSERT INTO user (user_id,  first_name, last_name)
  VALUES (2, 'Hong', 'Zhang');
INSERT INTO user (user_id,  first_name, last_name)
  VALUES (3, 'Lin', 'Li');
```

**Fetch data:**

```cql
cqlsh:test> SELECT * FROM user;

 user_id | first_name | last_name
---------+------------+-----------
       1 |       Ming |        Li
       2 |       Hong |     Zhang
       3 |        Lin |        Li
```

Next, fetch users whose last name is 'Li' by creating an index.

```cql
cqlsh:test> CREATE INDEX ON user (last_name);
cqlsh:test> SELECT * FROM user WHERE last_name = 'Li';

 user_id | first_name | last_name
---------+------------+-----------
       1 |       Ming |        Li
       3 |        Lin |        Li
```

## Use Cassandra in Application

To user Cassandra, you need a database driver for a specific language. We can get CQL drivers from [Data Stax](https://github.com/datastax) and [Cassandra ClientOptions](http://wiki.apache.org/cassandra/ClientOptions).

### Java Example

####Maven Dependency

Please refer to [DataStax Java Driver for Apache Cassandra](https://github.com/datastax/java-driver) for details.

```xml
<dependency>
  <groupId>com.datastax.cassandra</groupId>
  <artifactId>cassandra-driver-core</artifactId>
  <version>2.1.3</version>
</dependency>
```

####Connecting to a Cassandra cluster

```java
import com.datastax.driver.core.Cluster;
import com.datastax.driver.core.Host;
import com.datastax.driver.core.Metadata;

public class SimpleClient {
    private Cluster cluster = null;

    public void connect(String node) {
        cluster = Cluster.builder()
                .addContactPoint(node)
                .build();
        Metadata metadata = cluster.getMetadata();
        System.out.printf("Connected to cluster: %s\n",
                metadata.getClusterName());
        for (Host host : metadata.getAllHosts()) {
            System.out.printf("Datacenter: %s; Host: %s; Rack: %s\n",
                    host.getDatacenter(), host.getAddress(), host.getRack());
        }
    }

    public void close() {
        if (cluster != null) {
            cluster.close();
        }
    }

    public static void main(String[] args) {
        SimpleClient client = new SimpleClient();
        client.connect("127.0.0.1");
        client.close();
    }
}
```

####Using a session to execute CQL statements

1. Modify **SimpleClient** class
    * Add a Session property
    
            private Session session;
                    
    * Get a session from your cluster

            session = cluster.connect();

2. Add a method, **query**, executing a SELECT statement on user table.

```java
import com.datastax.driver.core.Cluster;
import com.datastax.driver.core.Host;
import com.datastax.driver.core.Metadata;
import com.datastax.driver.core.ResultSet;
import com.datastax.driver.core.Row;
import com.datastax.driver.core.Session;

public class SimpleClient {
    private Cluster cluster = null;
    private Session session = null;

    public void connect(String node) {
        cluster = Cluster.builder()
                .addContactPoint(node)
                .build();
        Metadata metadata = cluster.getMetadata();
        System.out.printf("Connected to cluster: %s\n",
                metadata.getClusterName());
        for (Host host : metadata.getAllHosts()) {
            System.out.printf("Datacenter: %s; Host: %s; Rack: %s\n",
                    host.getDatacenter(), host.getAddress(), host.getRack());
        }
        session = cluster.connect();
    }

    public void query() {
        ResultSet results = session.execute("SELECT * FROM test.user " +
                "WHERE last_name = 'Li';");
        System.out.println(String.format("%-30s\t%-20s\t%-20s\n%s", "user_id", "first_name",
                "last_name",
                "-------------------------------+-----------------------+--------------------"));
        for (Row row : results) {
            System.out.println(String.format("%-30s\t%-20s\t%-20s", row.getInt("user_id"),
                    row.getString("first_name"),  row.getString("last_name")));
        }
        System.out.println();
    }

    public void close() {
        if (cluster != null) {
            cluster.close();
        }
        if (session != null) {
            session.close();
        }
    }

    public static void main(String[] args) {
        SimpleClient client = new SimpleClient();
        client.connect("127.0.0.1");
        client.query();
        client.close();
    }
}
```

#### Using bound statements

```java
public void query() {
    PreparedStatement statement = session.prepare(
            "SELECT * FROM test.user WHERE last_name = ?;");
    BoundStatement boundStatement = new BoundStatement(statement);
    ResultSet results = session.execute(boundStatement.bind("Li"));
    System.out.println(String.format("%-30s\t%-20s\t%-20s\n%s", "user_id", "first_name",
            "last_name",
            "-------------------------------+-----------------------+--------------------"));
    for (Row row : results) {
        System.out.println(String.format("%-30s\t%-20s\t%-20s", row.getInt("user_id"),
                row.getString("first_name"),  row.getString("last_name")));
    }
    System.out.println();
}
```

## Data Model

Cassandra is a partitioned row store:

* Model brought from big table, row Key and a lot of columns
* Rows are organized into tables
* Primary key is required
* The first component of a table's primary key is the partition key
* The partition key determines which node stores the data
* Within a partition, rows are clustered by the remaining columns of the key

## CQL

### Data Types

CQL supports a rich set of data types including collection types.

The following table illustrates the native data types:

type|description
:--:|:---------:
ascii|character string
bigint|64-bit signed long
blob|Arbitrary bytes (no validation)
boolean|true or false
counter|Counter column (64-bit signed value). See Counters for details
decimal|Variable-precision decimal
double|64-bit IEEE-754 floating point
float|32-bit IEEE-754 floating point
inet|An IP address. It can be either 4 bytes long (IPv4) or 16 bytes long (IPv6). There is no inet constant, IP address should be inputed as strings
int|32-bit signed int
text|UTF8 encoded string
timestamp|A timestamp. Strings constant are allow to input timestamps as dates, see Working with dates below for more information.
timeuuid|Type 1 UUID. This is generally used as a “conflict-free” timestamp. Also see the functions on Timeuuid
uuid|Type 1 or type 4 UUID
varchar|UTF8 encoded string
varint|Arbitrary-precision integer

Cassandra has three collection types:

* set: set<text>, {'one', 'two'}
* list: list<text>, ['one', 'two']
* map: map<int, text>, {1: 'one', 2: 'two'}

### Functions

CQL3 supports a few functions:

* token()
* uuid()
* now()
* minTimeuuid()
* maxTimeuuid()
* dateOf()
* unixTimestampOf()
* typeAsBlob()
* blobAsType()


## Reference
1. [Getting Started](http://wiki.apache.org/cassandra/GettingStarted)
2. [CQL3](http://cassandra.apache.org/doc/cql3/CQL.html)
3. [DataModel](http://wiki.apache.org/cassandra/DataModel)
4. [DataStax Documentation](http://www.datastax.com/docs)
5. [DataStax Github](https://github.com/datastax)
6. [Cassandra Java Driver Docs](http://www.datastax.com/documentation/developer/java-driver/2.1/index.html)
7. The Data Model is Dead; Long live the Data Model: [Slides](http://www.slideshare.net/patrickmcfadin/the-data-model-is-dead-long-live-the-data-model)
8. Become a Super Modeler: [Slides](http://www.slideshare.net/patrickmcfadin/become-a-super-modeler)
9. The World's Next Top Data Model: [Slides](http://www.slideshare.net/patrickmcfadin/the-worlds-next-top-data-model)
10. Apache Cassandra 2.0: Data Model on Fire: [Slides](http://www.slideshare.net/planetcassandra/c-summit-eu-2013-apache-cassandra-20-data-model-on-fire)