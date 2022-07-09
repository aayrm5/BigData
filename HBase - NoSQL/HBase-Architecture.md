# HBase Architecture

1. Region Server
2. Region
3. Memstore
4. Wal
5. Block Cache
6. HFile
7. Zookeeper
8. HMaster

Let's say we have a table `employee`, which contains 2 column families and each column family contains 3 columns each. The row-key of this table `employee` is `employee id`.
There are 10,000 records in the `employee` table.

These 10K records would be stored in multiple `regions`. 

region1 => 1-2500 <br>
region2 => 2501-5000 <br>
region3 => 5001-7500 <br>
region4 => 7501-10000 <br>

Each `region-server` holds multiple `regions`. Each `region` holds the data sorted based on row-keys.

region-server1 => region1 & region2 <br>
region-server2 => region3 & region4

*Region Servers typically have one-to-one mapping with data nodes.* <br>
i.e. if we have 4 data-nodes, we will have 4 region-servers, each running on one data-node.

Note: Column families will also be stored in separate files. <br>

<img src="HBase - NoSQL\HBase Table.png"
    alt = "HBase Table storage"
    style ="float: left; margin-right:10px;"/>
