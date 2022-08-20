&leftarrow; [back to Book reviews](index.md)

**Designing Data-Intensive Applications - by Martin Kleppman, 2017**

![alt text](ddia.png "Cover")

## Main ideas and overview of the book:
Many applications today are Data-intesive vs Compute-intensive:
 - raw CPU power is the not limiting factor
 - the primary challenge is data - data quantity, data procesing speed, data complexity

The books consists of three parts:
 - Part 1 - focused on data storage & retrieval on a single machine (Chapters 1-4)
 - Part 2 - data storage & retrieval on multiple machines (Chapters 5-9)
 - Part 3 - building derived data systems - with batch & stream processing (Chapters 10-12)

Big questions/hard problems:
1. How to handle load?
 - indexes to speed up reads (support range queries? hot indexes?)
 - partitioning to speed up read/writes (hot partitions? rebalancing?)
2. How handle failures?
 - replication (lag? consistency?)
	
## Chapter1 - Reliability/Scalability

Describing Load:
 - specific to your system
 - e.g: volume of reads/writes, volume of data to store, distribution of followers per user

Describing Performance:
 - Throughput - in offline/batch systems (nr of records per second, total processing time of dataset)
 - Response time - in online/interactive systems - percentiles - median p50, p95, p999 (1 in 1000) - tail latencies
  
## Chapter2 - Data Models & Query Languages

Declarative Query Language (SQL) vs Imperative Query API
 - more concise
   - hides implementation details, allows for database system to intorduce improvements
   - decouples client code from database engine details
   - more limited in funcionality, which give  more room for optimizations in database

MapReduce a hybrid - selection query, with map and reduce user specified by code
 - map(record) -> emit(key, value)
 - group and sort by key
 - reduce(key, value) -> for each key called once, to produce final result


Relational Model - classic approach
 - if self-contianed data with mostly one-to-many relationships - Document Model better
   - less data locality, needs to join information
 - if lost of many-to-many relationship, anything can be connected - Graph Model better
   - complicated queries, several levels of joins needed, number of joins not fixed
 - schema-on-write - table update/backfill needed for schema change


Document Model
 - suitable for tree-like structure (mostly one-to-many relationships)
   - easy to represent: one-to-many relationships
   - hard to represent: many-to-on, many-to-many - data duplication, manual/client side joins - reference resolution
 - data locality
 - schema-on-read - easy to incorporate schema changes


Graph-Like Model
 - suitable for lots of many-to-many relationships
 - flexiblity to add edges, vertices
 Property Graph Model
  - vertices (properties) + edges (properties - head - tail - label)
Triple Store  (RDF - Resource Description Framework)
  - (subject, predicate, object) - does not distinguish between properties and edges - predicates used for both
    - subject (vertex) + prediate (property name | edge label) + object (property value | vertex)

## Chapter3 - Storage and Retrieval

* For efficient lookups we need an index
* For efficient range lookups, we need a sorted index
* But each index comes with write overhead cost
* To recover from crases - write-ahead log or redo log


Storage engines:
```
	- in-memory only (limited in size)
	- log-strcutured storage engine
		- in memory hash index or sorted/sparse index
		- in-memory memtable - balanced tree to sort
		- disk based sorted log database (SSTable, LSM tree) - append only
	- page-structured storage engines
		- disk based B-tree index
		- disk based database - in-place updates
```
In-memory hash-index 
 - simple index, append only writes
 - cons: if many keys, it does not fit in RAM
 - cons: no support for range queries, only exact queries

SSTable/LSM-Tree 
 - sorted, sparse, index, write firts to memory - updates appended
 ```
		- in-memory writes to a sorted+balanced tree - periodcally write to disk (sorted+compacted)
		- keep in-memory spare index for each segment written to disk - sparse is enough because values are sorted
		- values can be repeated, needs to check in specific order, from more recent to least recent segements
		- bloomfilter for quickly discarting non-members
```
B-tree 
- sorted index, write to disk - update-in-place, overwrite
```
		- works with fixed-sized blocks/pages
		- each page stores key range boundaries and references to child pages - starts from root page
		- high branching factor -100-500, few levels
		- easy to add new key/split a page, tricky to delete
		- pros: keys are stored in one place, easier to lock/make transactional
```

Write Performance - LSM-tree wins
 - lower write amplification and sequential operations/compression
 - B-tree higher write amplification (redo log, page, split/parent page)

Read Performance - B-tree wins 
 - fewer operations (˜4 page reads, 4 deep B-trees)
 - LSM-tree might need to check several data structure/SSTables in compaction

Access Pattern differences
 - OLTP - transactions - low latency, small nr of records
 - OLAP - analaytics - bulk, aggregates over large nr of records

Column-Oriented Stotage
 - if rows are a large (100+ columns) and we need to aggregate and extract a couple of columns (3) - row based storage is highly inefficient
 - compression
 - different sorting order (write have to go first to memory, and merged to disk later)
