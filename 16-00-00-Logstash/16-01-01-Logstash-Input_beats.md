# Logstash - Input "beats" #

This plugin wait for receiving data from remote beats services. It use tcp
/5044 port for communication:

                input {
                        beats {
                                port => 5044
                        }
                }
