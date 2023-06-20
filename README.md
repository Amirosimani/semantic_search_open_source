
# Open-source Semantic Search

Use LLMs to search documents in Elasticsearch

link to the [medium article](https://amirhos-imani.medium.com/open-source-semantic-search-a656e4c5483a).

--------------
#### Start a single-node Elasticsearch cluster locally

1. pull the Elasticsearch Docker image `docker pull docker.elastic.co/elasticsearch/elasticsearch:8.7.0
`
2. Create a new docker network for Elasticsearch and Kibana `docker network create elastic`
3. Start Elasticsearch in Docker `docker run --name es01 --net elastic -p 9200:9200 -it docker.elastic.co/elasticsearch/elasticsearch:8.7.0`
4. Copy the http_ca.crt security certificate from your Docker container to your local machine `docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .
`
5. [optional] set up Kibana: `docker run \
    --name kibana \
    --net elastic \
    -p 5601:5601 \
    docker.elastic.co/kibana/kibana:8.2.2`
    note that you will need the enrollment token from step 3
    
6. Test that you have access to your cluster from your jupyter notebook. More info on [Elasticsearch python cli](https://www.elastic.co/guide/en/elasticsearch/client/python-api/current/connecting.html).


For more details on how to set up an instance, check Elasticsearch [website](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)