# HBase Read/Write Operations (21:00)

### The common steps involved in HBase read/write operations are as follows:

**Step1**: The client contacts the zookeeper to fetch the location of the META table (if the client doesn’t have the latest version of the META table already cached)

**Step2**: The client queries the META table to find the location of the region server, which has the region containing the row-key that the client is looking for.

**Step3**: The client caches the identified region server information as well as the META table location for future interactions.

**Step4**: The client can now communicate with the specific region server, identified in the step 2 above. This region server assigns the request to the specific region, where the read/write operations can be executed.

