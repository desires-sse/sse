# Structured Search Engine

Structured Search Engine is a search engine which supports real time indexing of documents with some particular structure and searching over them with complex compound query through a RESTful API.

SSE includes real time addition, updation and deletion of documents through a RESTful API and has support for building queries for searching using a Query DSL (similar to Elasticsearch).


For a demo built with structured query engine, see [movie recommendations project](https://github.com/sigir-sse/movie-recommendations).
## Installation

- First we need to install google's snappy which is used for compressing the inverted index.

**DEB-based**: `sudo apt-get install libsnappy-dev`

**RPM-based**: `sudo yum install libsnappy-devel`

**Brew**:  `brew install snappy`

- Recommended Version: `Python >= 3.5`
- Use `pip install -r requirements.txt` to install python dependencies


## Running

- Run using `python -m app.start`

## Configuration

Structured Query Engine creates a `.data` folder into its root directory which has `config` json file with defaults present. You can edit this config file to change default configuration. In default scenario, SSE binds only to `localhost` and thus will not be accessible from outside. Change this setting to bind it to other interface and make it available from outside. Caution: Messing with other files will lead to deletion of indices and loss of data and in result SSE won't start. Delete `.data` folder, start and reindex again in such cases.

## Indexing and Querying

- REST API for SSE is based on ElasticSearch REST API.
- First create an index with proper mappings and settings. See this [example](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-create-index.html#mappings). Nested, primitive, keyword and text datatypes are supported for the fields.
- Add documents with POST for automatic id generation or PUT for specific id. See [this](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#_automatic_id_generation) and [this](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#docs-index_)
- For querying on data, see this [documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html). SSE supports leaf and compound queries at one nested level at the moment.

## Architecture

![Architecture](https://i.imgur.com/tlAG0u7.png)

- You can add nginx load balancer on top of the structured query engine.
- Architecture shows how RESTful API delegates requests to indexer and retriever.
- Memory resources are shared between indexers and retrievers so as to save memory space and avoid duplication
- Indexer uses debounce technique to write to disk so as to lower the number of writes.

## License

Apache License V2
