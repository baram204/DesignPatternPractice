# Zepplein on Windows 10

For instantly use apache zeppelin on windows 10.

Because Github transfer restriction, currently can't download this repository.

**Tested version**
- Spark version 2.1.0
- Python version 3.6.1
- Zeppelin version zeppelin-0.7.2-bin-all

**Tested notebook**
- Zeppelin Tutorial/Basic Features (Spark)

> currently support only 64bit windows. if you want to use it on 32bit, download files taged 32bit and install it instead of 64bit.

**Table of Contents**

1. Use windows file
    1. Use this repository file
    2. Make your own install
    3. Common task
2. Use doker image
    1. Use this repository file
    2. Make your own image
    3. Common task
3. Reference

## Use windows file

Run zeppelin directly on windows 10

### Use this repository file

How to use instantly run zeppelin with this repositoryfile.

clone this repository. and unzip all.

so, this repository has three directory.

```
ZeppleinWin10
  |- dependency
     - spark
     - hadoop
  |- prerequisite
     - jdk8.exe
     ......
  |- zeppelin
     - bin
     - config
     ....
  README.md
```

then jump to `Common task` section.

>  If you use docker-toolbox. recommand you to place zepplein inside `c:\Users`.

> Don't clone files inside `C:\Program Files` zepplein can't recognize empty space between folder name.

### Make your own install

1. install Prequsite

    - jdk8 - use ./prequsite/jdk-8u171-windows-x64.exe or [download](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

2. download Prequsite
    - winutils - use ./prequsite/winutils-master folder or [here](https://github.com/steveloughran/winutils)
    - zeppelin 0.7.2 [here](https://zeppelin.apache.org/releases/zeppelin-release-0.7.2.html)
    - hadoop 2.7.1 [here](https://archive.apache.org/dist/hadoop/core/hadoop-2.7.1/)

3. extract all downloads to place you want

    >  If you use docker-toolbox. recommand you to place zepplein inside `c:\Users`.

    > Don't clone files inside `C:\Program Files` zepplein can't recognize empty space between folder name.


4. copy all spark jars to zepplein spark interpreter folder

    from `[SPARK folder]\jars\*.jar`
    to `[ZEPPELINfolder]\interpreter\spark`

5. copy all pyspark zips to pyspark interpreter folder

    from `[SPARK folder]\python\lib\*.zip`
    to `[ZEPPELINfolder]\interpreter\spark`

7. inside spark interpreter folder, remove some jars has name start with `datanucleus`

    like

    `[ZEPPELINfolder]\interpreter\spark\datanucleus*.jar`

    specific

    `[ZEPPELINfolder]\interpreter\spark\datanucleus-api-jdo-3.2.6.jar`
    `[ZEPPELINfolder]\interpreter\spark\datanucleus-core-3.2.10.jar`
    `[ZEPPELINfolder]\interpreter\spark\datanucleus-rdbms-3.2.9.jar`

7. rename two template file inside `[ZEPPELINfolder]\conf`

    from zeppelin-env.cmd.template
    to  zeppelin-env.cmd

    from zeppelin-site.template
    to zeppelin-site

8. give some info to zepplein with mod zepplein-env.cmd for proper spark(scala) & pyspark working

    ```cmd
    set JAVA_HOME=%JAVA_HOME%
    set SPARK_HOME=
    set HADOOP_CONF_DIR=%HADOOP_HoME%\etc\hadoop
    set PYTHONPATH=%SPARK_HOME%\python;%SPARK_HOME%\python\lib\py4j-0.9.2-src.zip;%SPARK_HOME%\python\lib\pyspark.zip
    ```

    > SPARK_HOME should be empty. if not, there will be error on running spark inside zeppelin. error has nothing todo with spark well working outside zeppelin.

7. goto Next Sectin.

### Common task

1. set System Environment like below:

    variable name : JAVA_HOME
    variavle value : your java folder
    or
    variavle value : `somewhere\ZeppleinWin10\prerequisite\Java\jdk1.8.0_171`

    variable name : SPARK_HOME
    variavle value : your hadoop folder
    or
    variavle value : `somewhere\ZeppleinWin10\dependency\spark`

    variable name : HADOOP_HOME
    variavle value : your hadoop folder
    or
    variavle value : `somewhere\ZeppleinWin10\dependency\hadoop`

    variable name : ZEPPLEIN_HOME
    variavle value : your zepplein folder
    or
    variavle value : `somewhere\ZeppleinWin10\zeppelin`

2. run zeppelin.cmd inside `ZeppleinWin10/bin`

3. upper-right icon click -> interpreter -> spark -> edit -> zeppelin.spark.useHiveContext to false

## Use doker image

Run zeppelin on Docker-machine

### (not yet)Use this repository file

Buid or pull image toooooooo long & hard to wait. so I save it here.

`docker save apache/zeppelin:0.7.3 > zep.tar`

`docker load < zep.tar`

### Make your own image : Build or  Pull Docker Image & run

1. Build your own image with Dockerfile

    read & take Dockerfile at [Launching Zeppelin with Docker in 1 minute](https://www.zepl.com/viewer/notebooks/bm90ZTovLzFhbWJkYS85MjcyZjk5ZTk1NTI0YTdhYmU1M2Q1YTA0ZWZlZmUxNS9ub3RlLmpzb24).

    `docker build . -t apache/zeppelin:0.7.3`

2. Pull image from server

`docker pull apache/zeppelin:0.7.3`


### Common task

1. Make & Run your container with image

    without volume mount

    `docker run -p 8080:8080 --rm --name zeppelin apache/zeppelin:0.7.3`

    with volume mount

    `docker run -p 8080:8080 --rm -v /c/Users/somewhere/zeppelin/logs:/logs -v /c/Users/somewhere/zeppelin/notebook:/notebook -e     ZEPPELIN_LOG_DIR='/logs' -e ZEPPELIN_NOTEBOOK_DIR='/notebook' --name zeppelin apache/zeppelin:0.7.3`

    > make sure you're docker shared folder properly configure. if youre using docker-toolbox you could only mount folder inside `c:\Users`

2. start & stop container

    `docker stop zepplein`
    `docker start zepplein`


3. after all works fine. upper-right icon click -> interpreter -> spark -> edit -> zeppelin.spark.useHiveContext to false

## Reference

- empty SPARK_HOME it different from official install document :[Zeppelin on windows does throws error after installation and trying to run a cell](https://issues.apache.org/jira/browse/ZEPPELIN-2677)
- set HADOOP_HOME : [Error while configuring Apache Zeppelin on Windows 10](https://stackoverflow.com/questions/48656537/error-while-configuring-apache-zeppelin-on-windows-10)
- set HADOOP_CONF_DIR : [How to add the hadoop and yarn configuration file to the Spark application class path?](https://community.hortonworks.com/questions/85757/how-to-add-the-hadoop-and-yarn-configuration-file.html)
- copy interpreter files : [Zeppelin, Spark, PySpark Setup on Windows (10)](https://gist.github.com/codspire/7b0955b9e67fe73f6118dad9539cbaa2)
- HADOOP winutils.exe : [Export SPARK_HOME part of Spark Interpreter for Apache Zeppelin](https://zeppelin.apache.org/docs/0.7.2/interpreter/spark.html#1-export-spark_home)
- zeppelin.spark.useHiveContext to false [Start Zeppelin part of Apache Zeppelin installation on Windows 10](https://hernandezpaul.wordpress.com/2016/11/14/apache-zeppelin-installation-on-windows-10/)
