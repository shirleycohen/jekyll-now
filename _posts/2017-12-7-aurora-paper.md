---
layout: post
title: Paper Review &#35;3
---

<a href="http://www.allthingsdistributed.com/files/p1041-verbitski.pdf">Amazon Aurora: Design Considerations for High
Throughput Cloud-Native Relational Databases</a> by Verbitski et al., SIGMOD 2017. 

**1) Biggest contribution:** A relational database architecture that decouples the database and storage tiers and minimizes the amount of network traffic between those two tiers. More specifically, the Aurora database engine writes only redo log records to the storage nodes, not data blocks like traditional database systems do. The storage nodes receive the log records and write them out to disk. Aurora's durability tracking system ensures that 4/6 copies have been written to disk before a commit is acknowledged. The 6 copies are spread across 3 AZs within an AWS region with each AZ having 2 copies.   

**2) Biggest mistake:** The paper focuses exclusively on OLTP workloads, which is Aurora's primary use case. However, I think that small to medium-sized OLAP workloads and mixed workloads, both of which are quite common in production databases, could also potentially see some substantial benefits from Aurora's unique storage design. In particular, Aurora read replicas access the same underlying storage nodes as the primary instance, thus eliminating the need for an asyn replication process that updates the replica's separate storage. This means the replicas are nearly up-to-date and so the replication lag that typically occurs when a primary instance gets overloaded does not apply. Of course, there is still a need for an asyn replication process that updates the replica's blocks in memory.   

**3) Biggest question:** A natural question is how well does Aurora handle mixed workloads consisting of both OLTP and OLAP as compared with traditional relational database systems? Since Aurora databases can grow up to 64TB in size, the storage capacity would be adequate for most small to medium-sized data warehouses. And the read throughput can be increased by creating more replicas, up-to 15 per Aurora cluster. I think that this would make an interesting performance study.      