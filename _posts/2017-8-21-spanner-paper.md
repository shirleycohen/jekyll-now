---
layout: post
title: Paper Review #2
---

<a href="https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/46103.pdf">Spanner: Becoming a SQL System</a> by Bacon et al., SIGMOD 2017. 

**1) Biggest contribution:** The paper describes <i>Ressi</i>, Spanner's new layout for data records. Ressi is based on <a href="http://research.cs.wisc.edu/multifacet/papers/vldbj02_pax.pdf"><i>PAX</i> (Partition Attributes Across)</a>. PAX was originally designed to improve cache performance in OLAP workloads. Interestingly, Google found this format superior to <i>SSTable</i> not just for OLAP workloads, but also mixed OLTP/OLAP such as Google Play. Ressi also has the concept of time-based partitioning, keeping current values in physically separate files from previous values.   

**2) Biggest mistake:** I'm surprised that this paper didn't contain a performance evaluation of Spanner. For a systems paper, it is quite unusual not to provide experimental results. This omission is particularly surprising in light of the fact that Spanner is a newly launched a cloud service. Potential customers would want to see some basic benchmark results prior to investing significant development resources into moving their databases to Spanner.   

**3) Biggest question:** What are the top schema design best practices for Spanner? The authors mention that Spanner engineers routinely review the schema designs for internal Google applications to avoid production problems down the road, but they don't provide more details on these design reviews. Presumably, they are needed to avoid performance problems as opposed to data integrity issues. Would be nice to have some concrete guidelines for using the <code>INTERLEAVE IN PARENT</code> directive, particularly since this is unique to Spanner and it is used as a join optimization rather than a foreign key constraint. Also, would be nice to know how the Spanner query optimizer treats complex types such as Protocol Buffers. In other words, is there a performance penalty to using complex types and what it is?