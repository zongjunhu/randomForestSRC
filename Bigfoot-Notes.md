# Bigfoot overview

CCS `hadoop` cluster - `bigfoot` runs `Cloudera 5.7.1` on `CentOS-7.2`. It has the following built-in utils.

1. Spark: spark-1.6.0 (standard default) and 2.2.0 (custom installation. downloaded binary from Apache Spark site)
2. Gcc: 4.8.5
3. Java: Sun Java 1.8.0_60

# Compile and build Spark packages

Compiling spark package is successfuly following the procedures below,

1. download and install apache-ant-1.10.1 in ~/tools/
2. download and install apach-maven-3.5.2 in ~/tools/
3. set `JAVA_HOME`
    ```
    export JAVA_HOME=/usr/java/jdk1.8.0_60
    ```
4. add ant and maven to executable search `PATH`
    ```
    export PATH=$PATH:~/tools/apache-ant-1.10.1/bin:~/tools/apache-maven-3.5.2/bin
    ```
5. check out project from github clone
    ```
    git clone https://github.com/zongjunhu/randomForestSRC.git
    ```
6. Update `maven` configuration file `src/main/resources/spark/pom.xml` to the following.    
    ```xml
            <compilerExecutable>gcc</compilerExecutable>
            <linkerExecutable>gcc</linkerExecutable>

            ...
            <compilerStartOption>-std=gnu99</compilerStartOption>
            <compilerStartOption>-I${env.JAVA_HOME}/include</compilerStartOption>
            <compilerStartOption>-I${env.JAVA_HOME}/include/darwin</compilerStartOption>
            <compilerStartOption>-I${env.JAVA_HOME}/include/linux</compilerStartOption>
    ``` 
    The updated version use older `gcc` compiler and include java Linux specific header files.
7. compile and build spark package
    ```
    cd randomForestSRC
    ant build-spark
    ```
# Run test case on Spark

Stay in top directory in source code clone. Run the followings.

1. upload input data to `hdfs`. This location is hard coded in java source code
    ```
    hadoop fs -mkdir test-classes
    hadoop fs -mkdir test-classes/data
    hadoop fs -put target/spark/src/test/resources/data/mtcars.csv test-classes/data/
    ```
2. submit job to spark custom installation under `/opt/`
    ```
    /opt/spark-2.2.0-bin-hadoop2.6/bin/spark-submit --class HelloRandomForestSRC --master yarn --jars target/spark/target/randomForestSRC-tests.jar target/spark/target/randomForestSRC.jar
    ```
    Job failed because c library file is not available on cluster compute node.
    ```
    Can't load library: /home/zhu/tests/randomForestSRC-bigfoot/librandomForestSRC.so
    ```
