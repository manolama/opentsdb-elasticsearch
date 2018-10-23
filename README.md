       ___                 _____ ____  ____  ____
      / _ \ _ __   ___ _ _|_   _/ ___||  _ \| __ )
     | | | | '_ \ / _ \ '_ \| | \___ \| | | |  _ \
     | |_| | |_) |  __/ | | | |  ___) | |_| | |_) |
      \___/| .__/ \___|_| |_|_| |____/|____/|____/
           |_|    The modern time series database.


[![Build Status](https://travis-ci.org/OpenTSDB/opentsdb-elasticsearch.svg?branch=next)](https://travis-ci.org/OpenTSDB/opentsdb-elasticsearch) [![Coverage Status](https://coveralls.io/repos/github/OpenTSDB/opentsdb-elasticsearch/badge.svg?branch=next)](https://coveralls.io/github/OpenTSDB/opentsdb-elasticsearch?branch=next)
 
Search plugin for OpenTSDB

##Installation

* Compile the plugin via ``mvn package``.
* Create a plugins directory for your TSD
* Copy the plugin from the ``target`` directory into your TSD's plugin's directory.
* Add the following configs to your ``opentsdb.conf`` file.
    * Add ``tsd.core.plugin_path = <directory>`` pointing to a valid directory for your plugins.
    * Add ``tsd.search.enable = true``
    * Add ``tsd.search.plugin = net.opentsdb.search.ElasticSearch`` 
    * Add ``tsd.search.elasticsearch.host = <host>`` The HTTP protocol, host and port for an ES host or VIP in the format ``http[s]://<host>[:port]``.
* Add a mapping for each JSON file in the ./scripts folder via:
  (NOTE: It's important to do this BEFORE starting a TSD that would index data as you can't modify the mappings for documents that have already been indexed [afaik])

```  
  curl -H "Content-Type: application/json" -X PUT -d @scripts/schema_mapped_and_analyzed/opentsdb_index.json http://<eshost>/opentsdb/
  curl -H "Content-Type: application/json" -X PUT -d @scripts/schema_mapped_and_analyzed/tsmeta_map.json http://<eshost>/opentsdb/tsmeta/_mapping
  curl -H "Content-Type: application/json" -X PUT -d @scripts/schema_mapped_and_analyzed/uidmeta_map.json http://<eshost>/opentsdb/uidmeta/_mapping
  curl -H "Content-Type: application/json" -X PUT -d @scripts/schema_mapped_and_analyzed/annotation_map.json http://<eshost>/opentsdb/annotation/_mapping
```

* Optionally add ``tsd.core.meta.enable_tracking = true`` to your TSD config if it's processing incoming data
* Turn up the TSD OR...
* ... if you have existing data, run the ``uid metasync`` utility from OpenTSDB
