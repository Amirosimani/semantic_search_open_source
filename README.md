# Using Transformers to modernize Elasticsearch

Lexical search has limitations since it looks for literal matches (or variants of it like the stemmed version) of the query words typed by the user. This approach misses the context and doesn’t understand what the whole query really means. For example, when a user searches for “insurance”, a lexical-based searching solution will fail to surface documents that only have “Medicaid” in them. This is probably a significant source of complaints.

To address that issue, I suggest a solution that uses semantic search on top of their existing tech-stack (here, I limit that assumption to elasticsearch).

## Approach

* Using a pre-trained **T5-small** model, I create document-level embeddings for the [**Newsgroup** dataset](https://huggingface.co/datasets/newsgroup)
    * Many pre-trained models are suitable for semantic search and sentence representation in general. Based on the requirements, we can fine-tune the selected model on the company’s own corpus.
* Start a single-node Elasticsearch cluster locally
* Create and index and index the documents and their embeddings.
* Finally, generate query vectors using the same encoder at search time to retrieve the most similar vectors to the query vector.


The provided notebook will do all the above steps except for starting an Elasticsearch cluster. To do so, please follow the steps below.

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