## Introduction to distributed systems:

pizza parlour:

- optimise process and increase throughtput using the same resource -> vertical scaling
- preparing beforehand at non peek hours
- keep backups and avoid single point of failures -> backups (master -> slave)
- hire more resources -> horizontal scaling
- microSerivce architecture
- distributed system (partitioning)
- Load balencer
- decoupling (seperationg of responsibilites - pizza & delivery, tmrw burger)
- logging & metrics
- extensible

Resilence -> recovering from failures quickly

---

#### Horizontal & vertical scaling

##### Horizontal
- load balacing requrired
- resilent
- network calls (RPC) (communication b/w servers)
- data inconsistency (atomic trans, locks all servers)
- scales well as users increase

##### Vertical
- n/a
- single point of failure
- inter process communication (ipc)
- consistent
- hardware limit

In realword we use comb of both taking advatages of both world -> IPC + consitent + scales well as user-increase + resilent


---


#### Monolith vs Microservices

-> in monlith whole logic/businees in one unit
-> in microservice each unit handles each businees/logic 


Monolith
-  good for small teams
-  lesser moving parts (less complex)
-  less duplication
-  faster, no network calls (procedure call)

disAdv
- more context required to understand whole system for new team member
- deployments are complicated
- to much responsibilty on each server (single point of failure - restart all system of that server)


MicroSerivce
- easier to scale of each logic unit
- less context required for one service for new team member
- deployment is easy
- decoupled 
- easier to reason about 
- mostly a gatway is used to assign each service unit 


- a good indiccation if it is not to make microservice is if service-1 is always talking to only service-2, then service-1 & service-2 may can be only in one unit i.e one service, can be convert a rpc call to simple function call (ipc)
- for a larger system we often use microservice architecture


---


#### Load Balancing
-> balancing load evenly on given serviers

h(r1) = m1%n
r1 -> request
h() -> hash function
m1 -> hash func result 
n  -> # of servers


---


#### Sharding (sudo code)

vertical partioning   -> partioning based on columns
horizontal partioning -> partionign based on rows 

== horizontal partioning is known as sharding ==

Shards

- Logical sharding & Physical sharding

partioning on same physical machine - logical sharding
'     '      'different '   '  ' '  - physical ' '


p1 (partition-1) & p2 -> DB1
p3 & p4 -> DB2

- 2 physical shards have 4 logical shards


-> query will be easier by directing to one physincal shard like get data for users-100 to 200
-> place physical shard in different geo-locations 
-> if one shard is down...remaining users data will be another shard (if replication is not there)

Sharding strategies
-> algorithmic sharding
-> dynamic sharding

- alog sharding    -> one func or logic takes userId or city and gives result of shard, either read/write goes to (app itself take care to particular sharding redirect) (static # shards)
- dynamic sharding -> module/another-service decides the shard the query goes to 

-> disAdv
- data may not be evenly distribute, some shards may have gets larger data which becomes hotspot
- join operation is very expensive among shards - very diffcult to come back to non-shard architecture
- choosing a shard key is difficult



- sharding a relational db & non-relational is different

---

##### key based-sharding

-> choosing a shardKey (shardKey may not be primary key always)

-> one way to use a hash function 
   usedId -> hash func -> result which hard to go  
-> this is know as algo-sharding (application code itself know, which shard to route to)
-> calcalating on the go - no storing which user a shard to choose


Adv:
- data is evenly distributed

disAdv:
- when newShard is added hashing func will be changed, then previous users may be redirect to different shards, migrating existing data is expensive & difficult on prod systems
(one solution is consistent hashing)


How to choose a shard key 
- key should be static in nature, not frequently changed in key-based stategy 
- combination of columns can be used for shard key 

---

##### Range Based Sharding

- one way is first 3 months in one shard next in another shard and so..on
- store based on price-range like ecommerce data
- it is algo-sharding

Adv:
- same database schema for all logical & physical shards
- can be add more shard as their is no hasing 
- queries are useful if they have range base condition (like jan-march, nov-dec)

disAdv:
- for ex-jan-feb people are more active and we are getting lot of data for read & write..it becomes a hot-spot (data not evenly distributed)


use-cases:
- usernames divided bases on starting letters a-b, g-n (prob - may become hotspot)

---

##### Directory based sharding

- it is dynamic sharding (means we store  the sharding information in look-up table other than applicaton & db (app db), that lookup table tells where rows are located to particular shard)
- ex - divided our shard based on zones 

- lookUp table
  zone  shard 
   A     1
   B     2
   C     3

adv:
more zones can be added easily without touching previous shards
removing zone is easy without touching previous shard 


- carefully pickup the shard key so lookup table rows are less - (ex- may not pickup userId as shardKey..coz it is unique resulting larger lookup table)

disAdv:
- increase latency  App -> lookup -> db/shard  for every read & write
- lookup table is down  not able to result query ( solution - replica)
- hotspot very less coz most cases directory based sharding is implemneted on pre-knowlede of shard key (cities, zones)



- there are also other stategies, and some may be combination of directory, key or range.
---

##### Geo based Sharding

---