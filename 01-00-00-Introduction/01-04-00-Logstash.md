# Logstash #


Logstash is an open source data collection engine with real-time pipelining capabilities. Logstash can dynamically unify data from
disparate sources and normalize the data into destinations of your choice. Cleanse and democratize all your data for diverse advanced
downstream analytics and visualization use cases.

While Logstash originally drove innovation in log collection, its capabilities extend well beyond that use case. Any type of event can be
enriched and transformed with a broad array of input, filter, and output plugins, with many native codecs further simplifying the
ingestion process. Logstash accelerates your insights by harnessing a greater volume and variety of data.

Logstash 6.x version supports native support for multiple pipelines. These pipelines are defined in a *pipelines.yml* file which is loaded by default.
Users will be able to manage multiple pipelines within Kibana. This solution uses Elasticsearch to store pipeline configurations and allows for on-the-fly reconfiguration of Logstash pipelines.