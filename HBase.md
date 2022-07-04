# Getting started with NoSQL-HBASE

### There are 4 requirements for a database
1. In a database, data is stored in a Structured manner (rows & columns)
2. Database will give you Random Access to your data.
3. Database provides you low latency, takes less time to search a row.
4. Databases offers `ACID` complaince.

**`A`** - Atomicity => A financial transaction occuring should deduct from the sender and credit the receiver. Either both of these should happen or none of these should heppen. <br>
**`C`** - Consistency => The constraints should be met, none of the constraints should be violated. e.g: primary key should be unique. <br>
**`I`** - Isolation => If two persons are operating on a row, making operations on that row, the operation should happen in some defined order/sequence. There should be no dead locks. <br>
**`D`** - Durability => In case of power loss, crashes or errors, the data is not lost.

**Unfortunately, Hadoop is not a database** <br>
The following are the characteristics of Hadoop, which are completely opposite to a traditional database:
- Contains Unstructured Data
- No random access to the records
- Queries are high in latency
- Non ACID Complaint

But we want all the issues to be resolved and want all the benefits of a database in Hadoop.

Google came up with `Big Table` - A distributed storage system for structured data. This is implemented as HBase in Hadoop Eco System. <br>

**`HBase`** - A distributed database management system which runs on top of Hadoop.

*Features*:
- Distributed: Stores data on HDFS.
- Scalable: Capacity directly proportional to number of nodes in the cluster.
- Fault Tolerant: Piggybacks on Hadoop.


