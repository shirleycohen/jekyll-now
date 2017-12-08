---
layout: post
title: Paper Review &#35;2
---

<a href="https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/46103.pdf">Spanner: Becoming a SQL System</a> by Bacon et al., SIGMOD 2017. 

<!--more-->

**1) Biggest contribution:** The paper describes <i>Ressi</i>, Spanner's new layout for data records. Ressi is based on <a href="http://research.cs.wisc.edu/multifacet/papers/vldbj02_pax.pdf"><i>PAX</i> (Partition Attributes Across)</a>, which was originally designed to improve cache performance in OLAP workloads. Interestingly, Google found this format to be superior to <i>SSTable</i> not just for processing OLAP workloads, but also for mixed OLTP/OLAP such as Google Play. Ressi also has the concept of time-based partitioning, keeping current values in physically separate files from previous values.   

**2) Biggest mistake:** I'm surprised that this paper didn't provide a performance evaluation. For a systems paper, it is quite unusual to omit experimental results. This omission is particularly surprising given that Spanner is also a newly launched a cloud service. In my experience, customers typically want to see some standard benchmark results before investing any development cycles on yet another new DBMS.     

**3) Biggest question:** What are the top schema design practices for Spanner? The authors mention that Spanner engineers routinely review the schema designs for internal Google apps to avoid production problems down the road, but they don't provide more details on these design reviews. Presumably, they are needed to avoid performance problems as opposed to data integrity issues. For example, it would be nice to know the limits of the <code>INTERLEAVE IN PARENT</code> directive, given that this is unique to Spanner and is a join optimization technique rather than a foreign key constraint. Also, would be nice to know how the Spanner query optimizer treats complex types such as Protocol Buffers. More specifically, what does a query plan look like for a semi-structured schema that uses complex types versus a flattened relational schema with scalar types? In other words, is there still a performance benefit to flattening semi-structured objects to relational tables?