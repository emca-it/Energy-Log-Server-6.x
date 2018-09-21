# Elasticsearch API #


The Elasticsearch has a typical REST API and data is received in JSON format after the HTTP protocol.
By default the tcp/9200 port is used to communicate with the Elasticsearch API.
For purposes of examples, communication with the Elasticsearch API will be carried out using the *curl*
application.

Program syntax:

    # curl -XGET -u login:password '127.0.0.1:9200'
    
Available methods:

- PUT - sends data to the server;
- POST - sends a request to the server for a change;
- DELETE - deletes the index / document;
- GET - gets information about the index /document;
- HEAD - is used to check if the index / document exists.

Avilable APIs by roles:

- Index API - manages indexes;
- Document API - manges documnets;
- Cluster API - manage the cluster;
- Search API - is userd to search for data.
