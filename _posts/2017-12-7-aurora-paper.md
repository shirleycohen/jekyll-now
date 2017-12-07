---
layout: post
title: Paper Review &#35;3
---

<a href="http://www.allthingsdistributed.com/files/p1041-verbitski.pdf">Amazon Aurora: Design Considerations for High
Throughput Cloud-Native Relational Databases</a> by Verbitski et al., SIGMOD 2017. 

**1) Biggest contribution:** A relational database architecture that decouples the database and storage tiers and minimizes the amount of network traffic between those two tiers. More specifically, the Aurora database engine writes only redo log records to the storage nodes, not data blocks like traditional database systems do. The storage nodes receive the log records and write them out to disk. Aurora's durability tracking system ensures that 4/6 copies have been written to disk before a commit is acknowledged. The 6 copies are spread across 3 AZs within an AWS region with each AZ having 2 copies.   

**2) Biggest mistake:** The authors focus on OLTP workloads, which is Aurora's primary use case. However, given Aurora's unique storage design, it seems like it would be provide some performance benefits for small to medium-sized OLAP workloads or even mixed workloads.        

**3) Biggest question:** A natural question is how well does Aurora handle mixed workloads consisting of both OLTP and OLAP as compared to traditional relational database systems. Since Aurora databases can grow up to 64TB in size, the storage capacity would be more adequate for small to medium data warehouses.  