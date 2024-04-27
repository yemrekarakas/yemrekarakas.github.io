---
title: Apache Spark 01 - Local Spark Installation
author: yek
date: 2024-04-23 13:16:43 +03:00
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

### 01 - Local Spark Installation
> On Centos VM
{: .prompt-info }

#### Create working directory
```bash
mkdir spark_local
cd spark_local

mkdir datasets
```

#### Install Spark
```bash
curl -o spark-3.4.1-bin-hadoop3.tgz https://archive.apache.org/dist/spark/spark-3.4.1/spark-3.4.1-bin-hadoop3.tgz

tar xzf spark-3.4.1-bin-hadoop3.tgz
mv spark-3.4.1-bin-hadoop3 /opt/spark
rm -rf spark-3.4.1-bin-hadoop3.tgz


vim ~/.bashrc


# Spark Home
export SPARK_HOME=/opt/spark
export PATH=$PATH:$SPARK_HOME/bin
export PATH=$PATH:$SPARK_HOME/sbin


source ~/.bashrc
```

#### Install Java
```bash
sudo yum -y install java-11-openjdk-devel.x86_64
```

#### Create virtual environment
```bash
conda create --name sparkenv python=3.8
conda activate sparkenv
```

#### Create requirements.txt
```text
jupyterlab
findspark
pandas>=1.0.5
```

#### Install python packages
```bash
python -m pip install -r requirements.txt
```

#### Start Jupyter lab
```bash
jupyter lab --ip 0.0.0.0 --port 8888
```