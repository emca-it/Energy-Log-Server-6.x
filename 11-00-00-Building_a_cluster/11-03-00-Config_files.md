# Config files #


To configure the Elasticsearch cluster you  must specify some parameters
in the following configuration files on every node that will be connected to the cluster:

- `/etc/elsticsearch/elasticserach.yml`:
    - `cluster.name:name_of_the_cluster` - same for every node;
    - `node.name:name_of_the_node` - uniq for every node;
    - `node.master:true_or_false`
    - `node.data:true_or_false`
    - `network.host:["_local_","_site_"]`
    - `discovery.zen.ping.multicast.enabled`
    - `discovery.zen.ping.unicast.hosts`
    - `index.number_of_shards`
    - `index.number_of_replicas`
- `/etc/elsticsearch/logging.yml`:
  - `logger: action: DEBUG` - for easier debugging.
