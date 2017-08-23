Four Distributed Systems Architectural Patterns by Tim Berglund
------------------------------------------------------------------

### 3 Tier Architecture
- presentation tier
- business tier
- data tier

### modern 3 tier system

```txt
presentation   |     business                      |   data
---------------|-----------------------------------|------------

ReactJS                               /  NodeJS \          
         \                            /  NodeJS \       
ReactJS --  internet -- NGINX/ELB     -- NodeJS ---- cassabdra cluster     
         /                            \  NodeJS /
ReactJS                               \  NodeJS /

```

Cassandra
- token
- ring
- multi copies

Strengths
- rich front-end (scale)
- scalable middle tier
- bsically infinitely scalable data tire

Weaknesses
- state in he middle tier


### Sharded Architecture

```txt
// simple
Client <-> Complete Application <-> Data

// complex

Client                   Complete Chunk of Application  --> Data  
        \            / 
Client --- Router   --   Complete Chunk of Application  --> Data  
        /            \
Client                   Complete Chunk of Application  --> Data  

```

e.g. Slack, sharding by name: a-f, g-n, o-z

- master and slaves, e.g. zookeeper
- hard to resolve partition
- decide up front how many shards

Strengths
- client isolation

Weaknesses
- monitoring and logging
- no comprehensive view of daa (ETL)
- oversized shards


### Lambda Architecture
Lambda architecture is a data-processing architecture designed to handle massive quantities of data by taking advantage of both batch- and stream-processing methods.


- Streaming vs Batch
- Bounded vs Unbounded

Lambda assumes unbounded, immutable data

High latency bounded analysis
- long term storage

Low latency unbounded analysis
- temporary queuing

```txt
        Cassandra + Spark    
      /                      \
kafka                            Cassandra
      \                      /
        Event Framework       

```

Messaging
- kafka
- producer, consumer, topic

Lambda Strenghts
- build both: streaming and batch

Streaming

Integration

Kafka like a Message Bus, problems?

Eventvs vs Request/Response





### Architecture



### Ref

https://www.youtube.com/watch?v=tpspO9K28PM&t=33s&index=22&list=PL0OZE4KoHlVuBT1Z9lL1yAHNclm78GuHy
