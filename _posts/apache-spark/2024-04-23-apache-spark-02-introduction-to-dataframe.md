---
title: Apache Spark 02 - Introduction to Dataframe
author: yek
date: 2024-04-23 13:50:07 +03:00
categories: [Blogging, Data Engineering]
tags: [apache spark]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e389b6114248d19c27e1b6c2eeecd4d.jpg
  alt: Raven's Nest overlook at sunset, Acadia National Park, Schoodic Peninsula, Maine, USA
---

<style>
r { color: Crimson }
bl { color: CornflowerBlue }
y { color: Gold }
g { color: SpringGreen }
o { color: OrangeRed }
m { color: Magenta }
</style>

### 02 - Introduction to Dataframe
> Jupyter Notebook
{: .prompt-info }

```py
import findspark
findspark.init("/opt/spark") # spark home

from pyspark.sql import SparkSession
```

##### Create spark session
- application name (required)
- local mod (required)
- 2 core/thread

```py
spark = SparkSession.builder \
.appName("Introduction to Dataframe") \
.master("local[2]") \
.getOrCreate()
```

##### Create dataframe
```py
df = spark.createDataFrame(
  [("Java", 20000), ("Python", 100000), ("Scala", 3000)], #rows
  ["language","users_count"] # columns
)
```

##### Show dataframe
```py
df.show()

+--------+-----------+
|language|users_count|
+--------+-----------+
|    Java|      20000|
|  Python|     100000|
|   Scala|       3000|
+--------+-----------+
```

##### Show with number
```py
df.show(1)

+--------+-----------+
|language|users_count|
+--------+-----------+
|    Java|      20000|
+--------+-----------+
only showing top 1 row
```

##### Show with turncate
```py
df.show(n=3, truncate=False)

+--------+-----------+
|language|users_count|
+--------+-----------+
|    Java|      20000|
|  Python|     100000|
|   Scala|       3000|
+--------+-----------+
```

##### Print schema
```py
df.printSchema()

root
 |-- language: string (nullable = true)
 |-- users_count: long (nullable = true)
```

##### Convert pandas dataframe
> Spark dataframe is a distributed
{: .prompt-tip }

> Pandas dataframe is not a distributed
{: .prompt-tip }

> Warning!!!! You have to use limit() before using toPandas() oherwise all data would rush to driver.
{: .prompt-danger }

```py
df_pd = df.limit(5).toPandas()
df_pd
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>language</th>
      <th>users_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Java</td>
      <td>20000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Python</td>
      <td>100000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Scala</td>
      <td>3000</td>
    </tr>
  </tbody>
</table>
</div>

##### Pandas dataframe length
```py
len(df_pd)

    3
```

##### Pandas dataframe type (class)
```py
type(df_pd)

    pandas.core.frame.DataFrame
```

##### Spark dataframe type (class)
```py
type(df)

    pyspark.sql.dataframe.DataFrame
```

##### Spark session stop
```py
spark.stop()
```