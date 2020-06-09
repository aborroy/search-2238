# SEARCH-2238 Testing environment

This Docker Compose includes a default configuration to test [SEARCH-2238](https://issues.alfresco.com/jira/browse/SEARCH-2238) ticket.

Following instructions can be applied to every folder in order to test ACS 6.0, 6.1 and 6.2.

Local Docker image for [Search Services master](https://github.com/Alfresco/SearchServices/tree/master/search-services#use-alfresco-search-services-docker-image) needs to be also available in your local computer.

For ACS 6.0 change the Alfresco port from 8080 to 8082.


## Test 1: Creating instances

Start Docker Compose

```
$ cd docker-compose
$ docker-compose up --build --force-recreate
```

**Node 1**

Create three Shards in default SOLR.

```
$ curl --request GET \
--url 'http://localhost:8080/solr/admin/cores?action=newCore&storeRef=workspace%3A%2F%2FSpacesStore&numShards=3&numNodes=1&nodeInstance=1&template=rerank&property.data.dir.root=%2Fopt%2Falfresco-search-services%2Fdata%2Findex&shardIds=0&property.alfresco.host=alfresco&property.alfresco.port=8080&property.alfresco.secureComms=none'

$ curl --request GET \
--url 'http://localhost:8080/solr/admin/cores?action=newCore&storeRef=workspace%3A%2F%2FSpacesStore&numShards=3&numNodes=1&nodeInstance=1&template=rerank&property.data.dir.root=%2Fopt%2Falfresco-search-services%2Fdata%2Findex&shardIds=1&property.alfresco.host=alfresco&property.alfresco.port=8080&property.alfresco.secureComms=none'

$ curl --request GET \
--url 'http://localhost:8080/solr/admin/cores?action=newCore&storeRef=workspace%3A%2F%2FSpacesStore&numShards=3&numNodes=1&nodeInstance=1&template=rerank&property.data.dir.root=%2Fopt%2Falfresco-search-services%2Fdata%2Findex&shardIds=2&property.alfresco.host=alfresco&property.alfresco.port=8080&property.alfresco.secureComms=none'
```

**Node 2**

Create three Shards in secondary SOLR.

```
$ curl --request GET \
--url 'http://localhost:8080/solr-secondary/admin/cores?action=newCore&storeRef=workspace%3A%2F%2FSpacesStore&numShards=3&numNodes=1&nodeInstance=1&template=rerank&property.data.dir.root=%2Fopt%2Falfresco-search-services%2Fdata%2Findex&shardIds=0&property.alfresco.host=alfresco&property.alfresco.port=8080&property.alfresco.secureComms=none'

$ curl --request GET \
--url 'http://localhost:8080/solr-secondary/admin/cores?action=newCore&storeRef=workspace%3A%2F%2FSpacesStore&numShards=3&numNodes=1&nodeInstance=1&template=rerank&property.data.dir.root=%2Fopt%2Falfresco-search-services%2Fdata%2Findex&shardIds=1&property.alfresco.host=alfresco&property.alfresco.port=8080&property.alfresco.secureComms=none'

$ curl --request GET \
--url 'http://localhost:8080/solr-secondary/admin/cores?action=newCore&storeRef=workspace%3A%2F%2FSpacesStore&numShards=3&numNodes=1&nodeInstance=1&template=rerank&property.data.dir.root=%2Fopt%2Falfresco-search-services%2Fdata%2Findex&shardIds=2&property.alfresco.host=alfresco&property.alfresco.port=8080&property.alfresco.secureComms=none'
```

Check that every Shard is available in the Web Console.

http://127.0.0.1:8080/alfresco/s/enterprise/admin/admin-flocs


# Test 2: Grouping instances

Start Docker Compose

```
$ cd docker-compose
$ docker-compose up --build --force-recreate
```

**Node 1**

```
curl --location --request POST 'http://127.0.0.1:8080/alfresco/s/enterprise/admin/solrproxy' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic YWRtaW46YWRtaW4=' \
--header 'Content-Type: application/json' \
--data-raw '{
    "server": "http://solr6:8983/solr",
    "action": "newCore",
    "storeRef": "workspace://SpacesStore",
    "numShards": "2",
    "nodeInstance": 1,
    "replicationFactor": "1",
    "numNodes": 2,
    "template": "rerank",
    "coreName": "alfresco"
}'
```

**Node 2**

```
curl --location --request POST 'http://127.0.0.1:8080/alfresco/s/enterprise/admin/solrproxy' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic YWRtaW46YWRtaW4=' \
--header 'Content-Type: application/json' \
--data-raw '{
    "server": "http://solr6secondary:8983/solr",
    "action": "newCore",
    "storeRef": "workspace://SpacesStore",
    "numShards": "2",
    "nodeInstance": 2,
    "replicationFactor": "1",
    "numNodes": 2,
    "template": "rerank",
    "coreName": "alfresco"
}'
```

Check that every Shard is available in the Web Console.

http://127.0.0.1:8080/alfresco/s/enterprise/admin/admin-flocs

![expected result](https://github.com/Alfresco/search-2238/expected-result.png?raw=true)
