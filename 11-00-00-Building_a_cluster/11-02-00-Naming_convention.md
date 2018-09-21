# Naming convention #

Elasticsearch require little configuration before before going into work.

The following settings must be considered before going to production:

- **path.data** and **path.logs** - default locations of these files are:
`/var/lib/elasticsearch`and `/var/log/elasticsearch`.
- **cluster.name** - A node can only join a cluster when it shares its 
`cluster.name` with all the other nodes in the cluster. The default name 
is "elasticsearch", but you should change it to an appropriate name which 
describes the purpose of the cluster. You can do this in `/etc/elasticsearch/elasticsearch.yml` file.
- **node.name** - By default, Elasticsearch will use the first seven characters of the randomly 
generated UUID as the node id. Node id is persisted and does not change when a node restarts.
It is worth configuring a more human readable name: `node.name: prod-data-2`
in file `/etc/elstaicsearch/elasticsearch.yml`
- **network.host** - parametr specifying network interfaces to which Elasticsearch can bind. 
Default is `network.host: ["_local_","_site_"]`.
- **discovery** - Elasticsearch uses a custom discovery implementation called "Zen Discovery".
There are two important settings:
    - `discovery.zen.ping.unicast.hosts` - specify list of other nodes in the cluster that are 
    likely to be live and contactable;
    - `discovery.zen.minimum_master_nodes` - to prevent data loss, you can configure this setting
    so that each master-eligible node knows the minimum number of master-eligible nodes that must
    be visible in order to form a cluster.
- **heap size** - By default, Elasticsearch tells the JVM to use a heap with a minimum (Xms) and maximum (Xmx)
size of 1 GB. When moving to production, it is important to configure heap size to ensure that 
Elasticsearch has enough heap available
