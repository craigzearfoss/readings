# An Elasicsearch Tutorial: Getting Started

- logz.io
- [An Elasticsearch Tutorial: Getting Started](https://logz.io/blog/elasticsearch-tutorial/)
- [FUN PHP - Higher order functions](https://www.youtube.com/watch?v=sUJ8j2UR5kU)
- Daniel Berman
- Updated Apr 5, 2020
- Completed June 30, 2020

## What is Elasticsearch?
- NoSQL database.
- Initially released in 2010.
- Based on Apache Lucene,
- Open source and built with Java.
- [Elasticsearch API 101](https://logz.io/blog/elasticsearch-api/)
- Often used together with **ELK Stack**, **Logstash** and **Kibana**.
- Elasticsearch behaves like a REST API, so you can use either the POST or the PUT method to add data to it. 

## Installation
- Requirements
  - Java 8 (specific version recommended: Oracle JDK version 1.8.0_131)
  - You can install it as a standalone system or using apt and yum repositories.

### Installation Steps
1. Add Elastic’s signing key:
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```
2. For Debian, we need to then install the apt-transport-https package:
```
sudo apt-get install apt-transport-https
```
3. Add the repository definition to your system:
```
echo "deb https://artifacts.elastic.co/packages/6.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-6.x.list
```
4. Update your repositories and install Elasticsearch:
```
sudo apt-get update
sudo apt-get install elasticsearch
```

## Configuration
- Configuration file location depends on your operating system.
- Default is usally fie for development and testing.
- If installing on the cloud, it is best practice to bind Elasticsearch to either a private IP or localhost:
```
sudo vim /etc/elasticsearch/elasticsearch.yml
network.host: "localhost"
http.port:9200
```

## Running
- Starting Elasticsearch
```
sudo service elasticsearch start
```
- To confirm everything is working, point curl or your browser to *http://localhost:9200* and you should see output like:
```
{
  "name" : "33QdmXw",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "mTkBe_AlSZGbX-vDIe_vZQ",
  "version" : {
    "number" : "6.1.2",
    "build_hash" : "5b1fea5",
    "build_date" : "2018-01-10T02:35:59.208Z",
    "build_snapshot" : false,
    "lucene_version" : "7.1.0",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```
- Log files located (on Deb) in */var/log/elasticsearch/*.

## Creating an Elasticsearch Index
- You can use either the POST or the PUT method to add data to it. 
- Example:
```
curl -X PUT 'localhost:9200/app/users/4' -H 'Content-Type: application/json' -d '
{
  "id": 4,
  "username": "john",
  "last_login": "2018-01-25 12:34:56"
}
```
- The response looks like:
```
{"_index":"app","_type":"users","_id":"4","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":0,"_primary_term":1}
```
- You can define Elasticsearch mappings according to data types.
- To see a list of Elasticsearch indices:
```
curl -XGET 'localhost:9200/_cat/indices?v&pretty'
health status index               uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   logstash-2018.01.23 y_-PguqyQ02qOqKiO6mkfA   5   1      17279            0      9.9mb          9.9mb
yellow open   app                 GhzBirb-TKSUFLCZTCy-xg   5   1          1            0      5.2kb          5.2kb
yellow open   .kibana             Vne6TTWgTVeAHCSgSboa7Q   1   1          2            0      8.8kb          8.8kb
yellow open   logs                T9E6EdbMSxa8S_B7SDabTA   5   1          1   
```

## Elasticseach Querying
- URI searches:

### Fetch a single item:
```
curl -XGET 'localhost:9200/app/users/4?pretty'
```
```
{
  "_index" : "app",
  "_type" : "users",
  "_id" : "4",
  "_version" : 1,
  "found" : true,
  "_source" : {
    "id" : 4,
    "username" : "john",
    "last_login" : "2018-01-25 12:34:56"
  }
}
```
- Fields starting with an underscore are all meta fields of the result.
- The _source object is the original document that was indexed.

### Search for items:
```
curl -XGET 'localhost:9200/_search?q=logged'
```
```
curl -XGET 'localhost:9200/_search?q=logged'
{"took":173,"timed_out":false,"_shards":{"total":16,"successful":16,"skipped":0,"failed":0},"hits":{"total":1,"max_score":0.2876821,"hits":[{"_index":"logs","_type":"my_app","_id":"ZsWdJ2EBir6MIbMWSMyF","_score":0.2876821,"_source":
{
    "timestamp": "2018-01-24 12:34:56",
    "message": "User logged in",
    "user_id": 4,
    "admin": false
}
}]}}
```
Results:
- **took**: The time in milliseconds the search took.
- **timed_out**: If the search timed out.
- **shards**: The number of Lucene shards searched, and their success and failure rates.
- **hits**: The actual results, along with meta information for the results
- Lucene queries:
  - **username:johnb**
  - **john\***
  - **john?**

## Elasticsearch Query DSL
- Used for more advanced searchs than URI queries.
- It contains two kinds of clauses: 
  1. **leaf query clauses** that look for a value in a specific field
  2. **compound query clauses** (which might contain one or several leaf query clauses)

### Elasticsearch Query Types
- Geo queries,
- “More like this” queries
- Scripted queries
- Full text queries
- Shape queries
- Span queries
- Term-level queries
- Specialized queries

### Filter Context
- Test documents in a boolean fashion.
- Filters are generally faster than queries but do not calculate a relevance score.

### Query Context
- Calculates a relevance score. (Determines the order of the results.)
- Queries are generally faster than filters.

- Search Example:
```
curl -XGET 'localhost:9200/logs/_search?pretty' -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_phrase": {
      "message": "User logged in"
    }
  }
}
'
```
- Results:
```
{
  "took" : 28,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 1,
    "max_score" : 0.8630463,
    "hits" : [
      {
        "_index" : "logs",
        "_type" : "my_app",
        "_id" : "ZsWdJ2EBir6MIbMWSMyF",
        "_score" : 0.8630463,
        "_source" : {
          "timestamp" : "2018-01-24 12:34:56",
          "message" : "User logged in",
          "user_id" : 4,
          "admin" : false
        }
      }
    ]
  }
}
'
```

## Removing Elasticsearch Data
- Delete an index:
```
$ curl -XDELETE 'localhost:9200/logs?pretty'
```
- Delete all indices:
```
$ curl -XDELETE 'localhost:9200/_all?pretty'$
```
- Delete a single document:
```
$ curl -XDELETE 'localhost:9200/index/type/document'
```
- The repsonse to a delete:
```
{
 "acknowledged" : true
}
```