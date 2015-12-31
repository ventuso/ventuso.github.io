---
layout: post
title:  "Latency Numbers Every Programmer Should Know"
date:   2015-12-31 8:00:00
categories: guide
published: true
tags: [programming]
---


###Latency Comparison Numbers
--------------------------
| Operation | Time in ns | Time in ms |  Comment | 
|--------------------|----:|:--:|---|---|---|
| L1 cache reference | 0.5 |   |   | 
| Branch mispredict | 5 |    |   |   
| L2 cache reference | 7  |    | 14x L1 cache  |   
| Mutex lock/unlock | 25    |    |   |   
| Main memory reference |  100   |    | 20x L2 cache, 200x  |  
| Compress 1K bytes with Zip |  3,000   |    |   |   
| Send 1K bytes over 1 Gbps network |  10,000   |  0.01  |   |  
| Read 4K randomly from SSD   | 150,000| 0.15 |
| Read 1 MB sequentially from memory | 250,000| 0.25|
| Round trip within same datacenter| 500,000| 0.5|
| Read 1 MB sequentially from SSD| 1,000,000| 1| 4X memory|
| Disk seek | 10,000,000 | 10 | 20x datacenter roundtrip |
| Read 1 MB sequentially from disk| 20,000,000| 20 | 80x memory, 20X SSD|
| Send packet CA->Netherlands->CA| 150,000,000 | 150 | |

###Notes
-----
1 ns = 10^-9 seconds

1 ms = 10^-3 seconds

Assuming ~1GB/sec SSD

###Credit
------
By Jeff Dean: [http://research.google.com/people/jeff/](http://research.google.com/people/jeff/)

Originally by Peter Norvig: [http://norvig.com/21-days.html#answers](http://norvig.com/21-days.html#answers)