# Getting started with logstash and SQL SERVER

#### Versions
- Logstash 7.9.0
- SQL Server 2019-latest


#### Requirements
- JDK
- Docker
- mssql (driver)

#### Steps

Edit mssql.conf in logstash/mssql.conf

```conf
input {
  jdbc {
    jdbc_driver_class => "com.microsoft.sqlserver.jdbc.SQLServerDriver"
    jdbc_connection_string => "jdbc:sqlserver://YOUR_DB_HOST:YOUR_DB_PORT;databaseName=YOUR_DB_NAME;integratedSecurity=false;"
    jdbc_user => "YOUR_DB_USER"
    jdbc_password => "YOUR_DB_PASS"
    # schedule => "0 6 * * * Europe/Paris"
    schedule => "* * * * *"
    statement => "SELECT * FROM YOUR_TABLE"
  }
}

filter {}

output {
  elasticsearch {
    hosts => "YOUR_HOST_ES"
    index => "YOUR_INDEX"
    document_id => "%{YOUR_ID_REFERENCES}"
  }
}

```

```bash
# Starting containers
$ docker-compose up -d

# Opening bash on container
$ docker exec -it dwh_logstash /bin/bash

# Copy the driver into lib/jars
$ cp /etc/logstash/conf.d/drivers/mssql-jdbc-8.4.1.jre11.jar logstash-core/lib/

# You must restart the container
$ docker container restart dwh_logstash

# After one minute you must check the container log
$ docker logs dwh_logstash

```
<br>
#### Web access
kibana - http://localhost:5601
elasticsearch - http://localhost:9200

Now check the data transfered with kibana, we can do it using <i>"Dev tools"</i> and <i>"Discover"</i>.

<br>

### What is logstash?

Logstash is a tool for managing events and logs. When used generically, the term encompasses a larger system of log collection, processing, storage and searching activities.

The classic definition of Logstash says it’s an open-source, server-side data processing pipeline that can simultaneously ingest data from a wide variety of sources, then parse, filter, transform and enrich the data, and finally forward it to a downstream system.

### How logstash works?

Data flows through a Logstash pipeline in three stages: the input stage, the filter stage, and the output stage.
In each stage, there are plugins that perform some action on the data.

This is shown in the image below.

![workflow-logstash](https://cloudaffaire.com/wp-content/uploads/2020/02/word-image-29.png)


### My references pages
- https://cloudaffaire.com/how-to-create-a-pipeline-in-logstash/
- https://www.elastic.co/guide/en/logstash/current/running-logstash-command-line.html#command-line-flags
- https://logz.io/blog/logstash-tutorial/
- https://codeshare.co.uk/blog/how-to-copy-sql-server-data-to-elasticsearch-using-logstash/#step5
- https://www.elastic.co/blog/logstash_lesson_elasticsearch_mapping
- https://www.elastic.co/guide/en/logstash/current/plugins-inputs-jdbc.html
