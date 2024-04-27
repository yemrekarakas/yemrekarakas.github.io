---
title: Apache Spark 03 - Read Data From File
author: yek
date: 2024-04-23 13:56:43 +03:00
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

### 03 - Read Data From File
> Jupyter Notebook
{: .prompt-info }


```python
import findspark
findspark.init("/opt/spark")
from pyspark.sql import SparkSession
```

```python
spark = SparkSession.builder \
.appName("Read Data From File") \
.master("local[2]") \
.getOrCreate()
```

```python
! curl -o datasets/Mall_Customers.csv \
https://raw.githubusercontent.com/erkansirin78/datasets/master/Mall_Customers.csv

      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  4365  100  4365    0     0   5321      0 --:--:-- --:--:-- --:--:--  5336
```

```python
! ls -l datasets | grep Mall

    -rw-rw-r--. 1 yek yek 4365 Apr 23 14:04 Mall_Customers.csv
```

```python
df = spark.read.csv("file:///home/yek/spark_local/datasets/Mall_Customers.csv")

# other option
# df = spark.read.format("csv").load(path="/home/yek/spark_local/datasets/Mall_Customers.csv")

# another option
# df = spark.read.load(path="/home/yek/spark_local/datasets/Mall_Customers.csv", format="csv", header=True, inferShema=True)
```

```python
df.show(5)

    +----------+------+---+------------+-------------+
    |       _c0|   _c1|_c2|         _c3|          _c4|
    +----------+------+---+------------+-------------+
    |CustomerID|Gender|Age|AnnualIncome|SpendingScore|
    |         1|  Male| 19|       15000|           39|
    |         2|  Male| 21|       15000|           81|
    |         3|Female| 20|       16000|            6|
    |         4|Female| 23|       16000|           77|
    +----------+------+---+------------+-------------+
    only showing top 5 rows

```

```python
df.count()

    201
```


```python
df.limit(5).toPandas()
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
      <th>_c0</th>
      <th>_c1</th>
      <th>_c2</th>
      <th>_c3</th>
      <th>_c4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>CustomerID</td>
      <td>Gender</td>
      <td>Age</td>
      <td>AnnualIncome</td>
      <td>SpendingScore</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Male</td>
      <td>19</td>
      <td>15000</td>
      <td>39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Male</td>
      <td>21</td>
      <td>15000</td>
      <td>81</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Female</td>
      <td>20</td>
      <td>16000</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Female</td>
      <td>23</td>
      <td>16000</td>
      <td>77</td>
    </tr>
  </tbody>
</table>
</div>


#### Header Option

```python
df2 = spark.read \
.option("header", "True") \
.csv("file:///home/yek/spark_local/datasets/Mall_Customers.csv")
```


```python
df2.show(5)

    +----------+------+---+------------+-------------+
    |CustomerID|Gender|Age|AnnualIncome|SpendingScore|
    +----------+------+---+------------+-------------+
    |         1|  Male| 19|       15000|           39|
    |         2|  Male| 21|       15000|           81|
    |         3|Female| 20|       16000|            6|
    |         4|Female| 23|       16000|           77|
    |         5|Female| 31|       17000|           40|
    +----------+------+---+------------+-------------+
    only showing top 5 rows

```

```python
df2.limit(5).toPandas()
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
      <th>CustomerID</th>
      <th>Gender</th>
      <th>Age</th>
      <th>AnnualIncome</th>
      <th>SpendingScore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Male</td>
      <td>19</td>
      <td>15000</td>
      <td>39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Male</td>
      <td>21</td>
      <td>15000</td>
      <td>81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Female</td>
      <td>20</td>
      <td>16000</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Female</td>
      <td>23</td>
      <td>16000</td>
      <td>77</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Female</td>
      <td>31</td>
      <td>17000</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
</div>


```python
df2.printSchema()

    root
     |-- CustomerID: string (nullable = true)
     |-- Gender: string (nullable = true)
     |-- Age: string (nullable = true)
     |-- AnnualIncome: string (nullable = true)
     |-- SpendingScore: string (nullable = true)

```

#### InferSchema Option

```python
df3 = spark.read \
.option("header", True) \
.option("inferSchema", True) \
.csv("file:///home/yek/spark_local/datasets/Mall_Customers.csv")
```

```python
df3.printSchema()

    root
     |-- CustomerID: integer (nullable = true)
     |-- Gender: string (nullable = true)
     |-- Age: integer (nullable = true)
     |-- AnnualIncome: integer (nullable = true)
     |-- SpendingScore: integer (nullable = true)

```

#### Seperator Option - default comma(,)

```python
df4 = spark.read \
.option("header", "True") \
.option("inferSchema", "True") \
.option("sep", ",") \
.csv("file:///home/yek/spark_local/datasets/Mall_Customers.csv")
```

```python
df4.show(5)

    +----------+------+---+------------+-------------+
    |CustomerID|Gender|Age|AnnualIncome|SpendingScore|
    +----------+------+---+------------+-------------+
    |         1|  Male| 19|       15000|           39|
    |         2|  Male| 21|       15000|           81|
    |         3|Female| 20|       16000|            6|
    |         4|Female| 23|       16000|           77|
    |         5|Female| 31|       17000|           40|
    +----------+------+---+------------+-------------+
    only showing top 5 rows

```

```python
df4.limit(5).toPandas()
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
      <th>CustomerID</th>
      <th>Gender</th>
      <th>Age</th>
      <th>AnnualIncome</th>
      <th>SpendingScore</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Male</td>
      <td>19</td>
      <td>15000</td>
      <td>39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Male</td>
      <td>21</td>
      <td>15000</td>
      <td>81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Female</td>
      <td>20</td>
      <td>16000</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Female</td>
      <td>23</td>
      <td>16000</td>
      <td>77</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Female</td>
      <td>31</td>
      <td>17000</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
</div>


```python
spark.stop()
```
