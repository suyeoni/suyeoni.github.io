---
title: "JSON parsing to make RDD in spark"
date: 2019-05-10 16:05:00 -0900
categories:
  - spark
  - json
  - rdd
tags:
  - spark
  - json
  - rdd
---
SQLContext(spark 1.x~) SparkSession(spark 2.x~) has methods which make 'DataFrame' from JSON data. (you can check what 'DataFrame' is, here)

Before making DataFrame from JSON, Spark reads data (files, or string value etc..) as a RDD, which is a distributed data model defined Spark. 

Especially, RDD, which is produced by reading a text file, is made by each line(parsing by \n).

In my case, I had a problem with data loss when testing for checking the difference between TSV and JSON when making DataFrame.

There are many TSV log files in HDFS, so I copied TSV logs files after converting JSON format. data size was about 40m.

But when checking with Spark application, the number of row in JSON files is smaller than TSV files about 8000 rows.

The problem is miss the 'new line' when converting to JSON, like **{json}\n{json}**, not **{json}\n{json}\n**.

So I did some tests about making 'Dataframe' with JSON formats.

#### JSON Objects, not List  
{% capture fig_img %}
![JSON Objects, not List]({{ "meta/2019-05-11-json-parsing-in-spark_1.png" | relative_url }})
{% endcapture %}

#### JSON Objects in List  
{% capture fig_img %}
![JSON Objects in List]({{ "meta/2019-05-11-json-parsing-in-spark_2.png" | relative_url }})
{% endcapture %}

#### Mixed
{% capture fig_img %}
![Mixed]({{ "meta/2019-05-11-json-parsing-in-spark_3.png" | relative_url }})
{% endcapture %}