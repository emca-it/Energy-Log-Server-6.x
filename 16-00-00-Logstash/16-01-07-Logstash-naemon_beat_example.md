# Logstash pluging for "naemon beat" #

This Logstash plugin has example of complete configuration for integration with *naemon* application:

    input {
        beats {
            port => FILEBEAT_PORT
            type => "naemon"
        }
    }

    filter {
        if [type] == "naemon" {
            grok {
                patterns_dir => [ "/etc/logstash/patterns" ]
                match => { "message" => "%{NAEMONLOGLINE}" }
                remove_field => [ "message" ]
            }
            date {
                match => [ "naemon_epoch", "UNIX" ]
                target => "@timestamp"
                remove_field => [ "naemon_epoch" ]
            }
        }
    }

    output {
        # Single index
    #    if [type] == "naemon" {
    #        elasticsearch {
    #            hosts => ["ELASTICSEARCH_HOST:ES_PORT"]
    #            index => "naemon-%{+YYYY.MM.dd}"
    #        }
    #    }

        # Separate indexes
        if [type] == "naemon" {
            if "_grokparsefailure" in [tags] {
                elasticsearch {
                    hosts => ["ELASTICSEARCH_HOST:ES_PORT"]
                    index => "naemongrokfailure"
                }
            }
            else {
                elasticsearch {
                    hosts => ["ELASTICSEARCH_HOST:ES_PORT"]
                    index => "naemon-%{+YYYY.MM.dd}"
                }
            }
        }
    }
