# Cassandra

Extract Apache Cassandra detailed information.

## Cassandra Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":9042, "protocol":"tcp", "modules":["cassandra"]}]}]}' -H "X-Token:<Token>"
```

## Schema

### Cassandra Event Schema

```json
{
    "result": {
        "data": {
            "keyspaces": {
                "keyspace_name": {
                    "name": "string",
                    "tables": ["string"]
                }
            },
            "cluster": [{
                "address": "string", 
                "datacenter": "string", 
                "cassandraVersion": "string", 
                "rack": "string"
            }], 
            "info": [{
                "server_id": "string", 
                "cql_version": "string", 
                "listen_address": "string", 
                "dse_version": "string", 
                "data_center": "string", 
                "thrift_version": "string", 
                "host_id": "string", 
                "native_protocol_version": "string", 
                "broadcast_address": "string", 
                "release_version": "string", 
                "graph": "boolean", 
                "workload": "string", 
                "cluster_name": "string", 
                "rpc_address": "string", 
                "key": "string", 
                "rack": "string"
            }]
        }
    }
}
```

### Contents of the fields:

* keyspaces - Information regarding all keyspaces
  * keyspace_name - Keyspace name
  * tables - List of tables in the keyspace
* cluster - Information regarding all nodes of the cluster
  * address - Registered address of the cluster node
  * datacenter - Datacenter of the cluster node
  * cassandraVersion - Cassandra version of the cluster node
  * rack - Rack of the cluster node
* info - Information regarding local nodes of the cluster
  * server_id -  single identifier of the machine running a DSE instance
  * cql_version - CQL version 
  * listen_address - Address the server is listening to
  * data_center - Datacenter of the node
  * thrift_version - Thrift version
  * host_id - main identifier used by Cassandra on the server for internal communication (gossip)
  * native_protocol_version - Native protocol version
  * broadcast_address - Broadcast address
  * release_version - Cassandra release version
  * dse_version - DSE version
  * graph - If DSE: Graph enabled
  * workload - If DSE: List of Workloads
  * cluster_name - Cluster name
  * rpc_address - RPC address
  * key
  * rack - Rack of the node

## Cassandra Event Example

```json
{
    "result": {
        "data": {
            "keyspaces": {
                "dse_security": {
                    "name": "dse_security",
                    "tables": ["digest_tokens", "role_options", "spark_security"]
                }, 
                "system_traces": {
                    "name": "system_traces", 
                    "tables": ["events", "sessions"]
                }, 
                "journalist": {
                    "name": "journalist",
                    "tables": ["address_e", "address_p", "discussion_e", "discussion_p", "id_allocation", "journalist_e", "journalist_p", "organization_e", "organization_p", "project_e", "project_p", "reply_e", "reply_p", "requirement_e", "requirement_p", "tag_e", "tag_p"]
                },
                "dse_perf": {
                    "name": "dse_perf",
                    "tables": ["graph_event_log", "node_slow_log", "schema_migration_log", "slow_transaction_log"]
                }, 
                "OpsCenter": {
                    "name": "OpsCenter", 
                    "tables": ["backup_reports", "bestpractice_results", "events", "events_timeline", "pdps", "rollup_state", "rollups300", "rollups60", "rollups7200", "rollups86400", "settings"]
                }, 
                "dse_system": {
                    "name": "dse_system", 
                    "tables": ["encrypted_keys", "leases", "shared_data", "shared_data_versions"]
                }, 
                "system_schema": {
                    "name": "system_schema", 
                    "tables": ["aggregates", "columns", "dropped_columns", "functions", "indexes", "keyspaces", "tables", "triggers", "types", "views"]
                }, 
                "journalist_pvt": {
                    "name": "journalist_pvt", 
                    "tables": []
                }, 
                "solr_admin": {
                    "name": "solr_admin", 
                    "tables": ["solr_resources"]
                }, 
                "system_auth": {
                    "name": "system_auth", 
                    "tables": ["resource_role_permissons_index", "role_members", "role_permissions", "roles"]
                },
                "system": {
                    "name": "system", 
                    "tables": ["IndexInfo", "available_ranges", "batches", "batchlog", "built_views", "compaction_history", "hints", "local", "paxos", "peer_events", "peers", "range_xfers", "size_estimates", "sstable_activity", "views_builds_in_progress"]
                }, 
                "dse_leases": {
                    "name": "dse_leases", 
                    "tables": ["leases", "logs"]
                }, 
                "jouture": {
                    "name": "jouture", 
                    "tables": ["activity", "activity_counter", "activity_notification", "activity_notification_counter", "activity_task", "connection", "grants", "match", "match_counter", "matchobject", "message", "message_counter", "message_thread", "tag", "user", "user_setting"]
                }, 
                "journalist_system": {
                    "name": "journalist_system", 
                    "tables": ["dseg_variables", "shared_data", "shared_data_versions"]
                }, 
                "system_distributed": {
                    "name": "system_distributed", 
                    "tables": ["parent_repair_history", "repair_history"]
                }
            },
            "cluster": [{
                "address": "xxx.xxx.xxx.xxx:9042", 
                "datacenter": "dc1", 
                "cassandraVersion": "3.0.12.1656", 
                "rack": "rack1"
            }, {
                "address": "xxx.xxx.xxx.xxx:9042", 
                "datacenter": "dc1", 
                "cassandraVersion": "3.0.12.1656", 
                "rack": "rack1"
            }, {
                "address": "xxx.xxx.xxx.xxx:9042", 
                "datacenter": "dc1", 
                "cassandraVersion": "3.0.12.1656", 
                "rack": "rack1"
            }], 
            "info": [{
                "server_id": "42-01-0A-8E-00-03", 
                "cql_version": "3.4.0", 
                "listen_address": "xxx.xxx.xxx.xxx", 
                "dse_version": "5.0.8", 
                "data_center": "dc1", 
                "thrift_version": "20.1.0", 
                "host_id": "ea6eefde-bdbf-460f-afbf-5351d629dfc9", 
                "native_protocol_version": "4", 
                "broadcast_address": "xxx.xxx.xxx.xxx", 
                "release_version": "3.0.12.1656", 
                "graph": true, 
                "workload": "Search", 
                "cluster_name": "Kulectiv", 
                "rpc_address": "0.0.0.0", 
                "key": "local", 
                "rack": "rack1"}]
            }
        }
    }
}
```
