CCS `hadoop` cluster - `bigfoot` runs `Cloudera 5.7.1` on `CentOS-7.2`. It has the following built-in utils.

1. Spark: spark-1.6.0
2. Gcc: 4.8.5
3. Java: Sun Java 1.8.0_60

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
