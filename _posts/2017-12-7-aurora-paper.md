---
layout: post
title: Paper Review &#35;3
---

<a href="http://www.allthingsdistributed.com/files/p1041-verbitski.pdf">Amazon Aurora: Design Considerations for High
Throughput Cloud-Native Relational Databases</a> by Verbitski et al., SIGMOD 2017. 

<!--more-->

**1) Biggest contribution:** A relational database architecture that decouples the database and storage tiers and minimizes the amount of network traffic between the two tiers. More specifically, the Aurora database engine writes only redo log records to the storage nodes, not data blocks like traditional database systems do. The storage nodes receive the log records and persist them to disk. Aurora's durability tracking system ensures that 4/6 copies of log records have been written to disk before a commit is acknowledged. The 6 copies are spread across 3 AZs within an AWS region with each AZ having 2 copies.   

**2) Biggest mistake:** The paper focuses exclusively on OLTP workloads, which is Aurora's primary use case. However, I think that small OLAP workloads and mixed workloads, both of which are quite common for small to medium-sized production databases, could potentially see some benefits from Aurora's unique storage design too. In particular, Aurora's read replicas access the same underlying storage nodes as the primary, thus eliminating the need for an asyn replication process that updates the replica's separate storage. This implies that Aurora does not suffer from the same type of replication lags that occur when a primary instance gets overwhelmed, something that makes replicas fragile in traditional database systems. That said, there is still a need to update the replica's stale blocks in memory, so the problem doesn't entirely go away.   

**3) Biggest question:** A natural question that comes to mind is how well does Aurora handle mixed workloads consisting of both OLTP and OLAP as compared to traditional relational database systems? Since Aurora databases can grow to 64TB in size, the storage capacity would be adequate for many small and medium-sized data warehouses. In addition, adding more replicas should result in a linear increase in read throughput, up to 15 read replicas are allowed per Aurora cluster. In short, I think that benchmarking some realistic hybrid workloads would be an interesting area to explore. 