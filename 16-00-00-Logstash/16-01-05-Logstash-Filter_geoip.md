# Logstash - Filter "geoip" #

This filter processing an events data with IP address and check localization:

	filter {
	     if [src][locality] == "public" {

			geoip {
				source => "[src][ip]"
				target => "[src][geoip]"
				database => "/etc/logstash/geoipdb/GeoLite2-City.mmdb"
				fields => [ "city_name", "country_name", "continent_code", "country_code2", "location" ]
				remove_field => [ "[src][geoip][ip]" ]
			}

			geoip {
				source => "[src][ip]"
				target => "[src][geoip]"
				database => "/etc/logstash/geoipdb/GeoLite2-ASN.mmdb"
				remove_field => [ "[src][geoip][ip]" ]
			}

	     }

	     if [dst][locality] == "public" {

			geoip {
				source => "[dst][ip]"
				target => "[dst][geoip]"
				database => "/etc/logstash/geoipdb/GeoLite2-City.mmdb"
				fields => [ "city_name", "country_name", "continent_code", "country_code2", "location" ]
				remove_field =>  [ "[dst][geoip][ip]" ]
			}

			geoip {
				source => "[dst][ip]"
				target => "[dst][geoip]"
				database => "/etc/logstash/geoipdb/GeoLite2-ASN.mmdb"
				remove_field => [ "[dst][geoip][ip]" ]
			}
	     }

	}
