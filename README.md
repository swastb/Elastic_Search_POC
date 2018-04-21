# Elastic_Search_POC
Elastic Search Notes


1. Steps done to populate the elastic search database with the data. 

Create a json file with generating data from https://www.json-generator.com/. Some formatting had to be done on the data for example remove the array symbol. Replace the , by /n . 

How to check if the indexes are present in the elastic search, use the command as 

curl -XGET 'localhost:9200/_cat/indices?v&pretty'

To add data in bulk from a json file execute the command as
curl -H "Content-Type: application/x-ndjson" -XPOST 'localhost:9200/customers/personal/_bulk?pretty&refresh' --data-binary @'customers_full.json'

curl -XPOST "localhost:9200/customers/personal/_bulk?pretty" -H "Content-Type:application/json" --data-binary @customers_full.json

Search Query example with query in the url

Now to search "wyoming" any where in the document try the command as 
curl -XGET 'localhost:9200/customers/_search?q=wyoming&pretty'
to get the result on sorted order of age
curl -XGET 'localhost:9200/customers/_search?q=wyoming&sort=age:desc&pretty'
to seearch wyoming only on the state field
curl -XGET 'localhost:9200/customers/_search?q=state:wyoming&sort=age:desc&pretty'


Seach Query example with query in the request data .. to match all but return 3 size

curl -H "Content-Type:application/json" -XGET 'localhost:9200/customers/_search?pretty' -d '
{
	"query" : {"match_all" :{} },
	"size" : 3
}'

To search by name field where we have "gates"
curl -H "Content-Type:application/json" -XGET 'localhost:9200/customers/_search?pretty' -d '
{
	"query" : {"term" :{"name":"gates"} }
	
}'

Get result without any source
curl -H "Content-Type:application/json" -XGET 'localhost:9200/customers/_search?pretty' -d '
{
	 "_source" : false,
	"query" : {"term" :{"name":"gates"} }
	
}'
The common Terms problem 
