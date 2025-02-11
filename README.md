
# WordCount-Using-MapReduce-Hadoop

This repository is designed to test MapReduce jobs using a simple word count dataset.

## Objectives

By completing this activity, students will:

1. **Understand Hadoop's Architecture:** Learn how Hadoop's distributed file system (HDFS) and MapReduce framework work together to process large datasets.
2. **Build and Deploy a MapReduce Job:** Gain experience in compiling a Java MapReduce program, deploying it to a Hadoop cluster, and running it using Docker.
3. **Interact with Hadoop Ecosystem:** Practice using Hadoop commands to manage HDFS and execute MapReduce jobs.
4. **Work with Docker Containers:** Understand how to use Docker to run and manage Hadoop components and transfer files between the host and container environments.
5. **Analyze MapReduce Job Outputs:** Learn how to retrieve and interpret the results of a MapReduce job.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn install
```

### 3. **Move JAR File to Shared Folder**

Move the generated JAR file to a shared folder for easy access:

```bash
mv target/*.jar shared-folder/input/code/
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/code/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/dataset
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/dataset
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command:

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/dataset/input.txt /output
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output

### ** Project Overview **

This project demonstrates a **Word Count** implementation using Hadoop **MapReduce**. It processes a large dataset, counts the frequency of words, and outputs the results. The project is executed inside a **Dockerized Hadoop cluster** to simulate a distributed computing environment.

### **1️⃣ Mapper (WordMapper.java)**  

- Reads input text line by line.
- Splits each line into words.
- Emits **(word, 1)** pairs as output.

### **2️⃣ Reducer (WordReducer.java)**  

- Receives grouped key-value pairs **(word, [1,1,1,...])**.
- Aggregates the counts for each word.
- Outputs **(word, total_count)**.

### **3️⃣ Driver (Controller.java)** 

- Configures and initiates the MapReduce job.
- Specifies the **input path, output path, and Mapper/Reducer classes**.
- Handles job execution and monitoring.


### ** Input And Output **

 ### sample input
```bash
Hello world
Hello Hadoop
Hadoop is powerful
Hadoop is used for big data
```
### Sample output

```bash 
Hadoop  3
MapReduce       1
Rather  1
a       2
across  1
allows  1
and     2
application     1
at      1
clusters        1
component       1
computation     1
computers.      1
core    1
data    1
deliver 1
designed        2
detect  1
distributed     1
each    1
ecosystems.     1
failures        1
for     1
framework       1
from    1
handle  1
hardware        1
high-availability,      1
is      4
itself  1
large   1
layer.  1
library 1
local   1
machines,       1
of      4
offering        1
on      1
processing      1
rely    1
scale   1
servers 1
sets    1
single  1
storage.        1
than    1
that    1
the     4
thousands       1
to      4
up      1
```