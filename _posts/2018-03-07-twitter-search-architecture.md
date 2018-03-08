Twitter's Search Architecture - 2013
https://www.youtube.com/watch?v=AguWva8P_DI


- use LUCENE with modifications
- 230 million active users - 500 million tweets per month
- 33388 TPS per second
- 2 billion search queries per day

2010 - Apache Lucene - Inverted Indexes
2011 - search image and videos
		index compression
		relevance search in time-sorted index

2013 - SSD with Vanilla Lucene
2014 - 


Design - Real time steam of tweet goes into Analyser/ Partitioner 

Analyser/ Partitioner  - pre-process tweet for indexing


Twitter Blender - Thrift Service Aggregator - Queries mutiple Earlybirds, merge results.

Snowflake - distributed id generator

Inverted Indexing - 