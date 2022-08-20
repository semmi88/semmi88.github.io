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
  
