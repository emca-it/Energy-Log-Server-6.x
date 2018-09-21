# Elasticsearch #

Elasticsearch is a NoSQL database solution that is the heart of our
system. Text information send to the system, application and system
logs are processed by Logstash filters and directed to Elasticsearch.
This storage environment creates, based on the received data, their
respective layout in a binary form, called a data index. The Index is
kept on Elasticsearch nodes, implementing the appropriate assumptions
from the configuration, such as:

- Replication index between nodes,
- Distribution index between nodes.

The Elasticsearch environment consists of nodes:

- Data node - responsible for storing documents in indexes,
- Master node - responsible for the supervisions of nodes,
- Client node - responsible for cooperation with the client.

Data, Master and Client elements are found even in the smallest
Elasticsearch installations, therefore often the environment is
referred to as a cluster, regardless of the number of nodes
configured. Within the cluster, Elasticsearch decides which data
portions are held on a specific node.

Index layout, their name, set of fields is arbitrary and depends on
the form of system usage. It is common practice to put data of a
similar nature to the same type of index that has a permanent first
part of the name. The second part of the name often remains the date
the index was created, which in practice means that the new index is
created every day. This practice, however, is conventional and every
index can have its own rotation convention, name convention,
construction scheme and its own set of other features. As a result of
passing document through the Logstash engine, each entry receive a
data field, which allow to work witch data in relations to
time. 

The Indexes are built with elementary part called shards. It is good
practice to create Indexes with the number of shards that is the
multiple of the Elasticsearch data nodes number. Elasticsearch in 6.x version has a new feature called Sequence IDs that guarantee more successful and efficient shard recovery.

Elasticsearch use the *mapping* to describes the fields or properties that documents of that type may have. Elasticsearch in 6.x version restrict indices to a single type.