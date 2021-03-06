# HBase Read/Write Operations (21:00)

### The common steps involved in HBase read/write operations are as follows:

**Step1**: The client contacts the zookeeper to fetch the location of the META table (if the client doesn’t have the latest version of the META table already cached)

**Step2**: The client queries the META table to find the location of the region server, which has the region containing the row-key that the client is looking for.

**Step3**: The client caches the identified region server information as well as the META table location for future interactions.

**Step4**: The client can now communicate with the specific region server, identified in the step 2 above. This region server assigns the request to the specific region, where the read/write operations can be executed.

![Read & Write Operation in HBase](./Images/HBase_Read_Write.png)

## HBase Write Operation

**Step1**: The data first needs to be written to the WAL, which is a write-ahead Log. WAL stores the new or updated data that has not been written to the HDFS and can be used for recovery, incase of a region failure.

**Step2**: Once the data is written to the WAL, it is placed in the MemStore of a region. When the MemStore becomes full, its contents are flushed to HDFS (DataNode) to form a new HFile.

**Step3**: Finally, an acknowledgement is sent back to the client.

![HBase Write Operation](./Images/HBase_Write.png)

## HBase Read Operation:

**Step1**: The region server first checks the block cache of the region server that stores the recently accessed data.

**Step2**: In case the data is not available in the block cache, it checks for the required data in the in-memory store i.e. MemStore

**Step3**: If the MemStore doesn’t contain that particular key-value, the HFile containing that particular key-value pair is identified.

Once the HFile is identified, instead of reading the entire HFile (~GB size), the Data Block Index of the HFile is scanned to get the Data Block with the Key-Value pair, A binary search in this data block finally returns the key-value pair (or null in case it doesn’t exist).

![HBase Read Operation](./Images/HBase_Read.png)

## Compactions:

Need for compaction: The flushes from MemStore to the HDFS create Multiple HFiles, especially during periods of heavy incoming writes. 

**This will lead to two major problems:**

    1. The read efficiency gets low. This is because a large number of HFiles increases the number of disk seeks needed for a read process, thereby adversely impacting the read performance.

    2. It leads to dirty data. A large number of HFiles might result in a lot of redundancy and inconsistency in data.

Compaction comes to our rescue against these challenges. It refers to a process of combining small HFiles to large HFiles, containing merged, sorted, and most recent information.

**Compactions are of two types - Minor and Major**

In minor compaction HBase picks only some of the smaller HFiles and rewrites them to a few larger HFiles.
On the other hand, in Major compaction, all HFiles of a store are picked and rewritten into a single large HFile.

Major compaction is resource intensive because it merges alot of HFiles. Major compaction is done when there is less traffic on the system during non-working hours.

![HBase Compations](./Images/HBase_Compactions.png)


## HBase Update & Delete Operations:

HBase uses HDFS internally, but HDFS is a read-only file system and changes cannot be made to it.

Thus HBase uses timestamps to update the info. HBase creates multiple timestamps and reads the latest one, so whenever user queries the data, latest info is provided which resembles an update.

#### HBase Data Deleltion:
Delete is a special type of update in HBase, where the values for which the delete request is submitted are not deleted immediately. Rather, these values are masked by assigning a tombstone marker to them. Every request to read these values (with  tombstone markers) returns nulls to the client, which gives the client an impression that the values are already deleted.

The reason why HBase does this is because HFiles are immutable. Recall that HDFS doesn't allow modifying the data of a file. All the values with tombstone markers are permanently removed during the next major compaction.


## Incase of Server Failure:
1. If the actuive HMaster fails, the Zookeeper gives the Master's responsibility to one of the inactive HMasters in the cluster.
2. If a region server fails, Zookeeper notifies the HMaster about it. HMaster reassigns the regions of the failed region server to some other region server.

