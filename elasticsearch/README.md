
# Commands for Elasticsearch

- DELETE an index

```
curl -X DELETE "elasticsearch.example.com:9200/es-index-example?pretty"
```

- List all indices

```
curl -X GET "elasticsearch.example.com:9200/_cat/indices?v"
```

```
$ curl -X GET "elasticsearch.example.com:9200/_cat/indices?v"

health status index               uuid                   pri rep docs.count docs.deleted store.size pri.store.size
yellow open   telegraf-2018.11.14 9irYcn5rRhuCpO3SvCOxyw   5   1     385970            0     63.9mb         63.9mb
```

- List all indices in JSON

```
curl -X GET "elasticsearch.example.com:9200/_cat/indices?format=json&pretty=true"
```

- Get cluster Health Status

```
curl -X GET "elasticsearch.example.com:9200/_cluster/health"
```

- Get cluster Health Status with prettified output

```
curl -X GET "elasticsearch.example.com:9200/_cluster/health?pretty"
```
