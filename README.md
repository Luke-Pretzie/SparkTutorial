# Spark Tutorial ReadMe

This ReadMe is a guide to the installation and use of Apache Spark on Linux Ubuntu, with insight into the utility of Spark and examples of using Spark for big data applications

### What is the purpose of this repository?

* The amount of data generated on a daily basis is monumental, and the rate at which this data is being produced is exponential. Through this tutorial I hope to give insight into the big impact that the Apache Spark software platform can have on the analysis of big data.

### Necessary Software
* Software Versions: Spark Version 2.2.0, Java 6 or 7 with with Scala 2.9.2 OR Java 8 with Scala 2.10.3+
* Linux OS (Written in Ubuntu Distribution, exercise caution if using different distributions)

### Who do I talk to?

* For questions, comments and concerns, please contact Luke Pretzie at lpretz2@uic.edu

## The Impact of Big Data
* 90% of the data in the world today has been created in the last two years alone.
* US healthcare system alone could create $300 billion in value annually with proper applications of big data (Manyika et al.)

![Alt text](https://media.nationalpriorities.org/uploads/total_spending_pie%2C__2015_enacted.png)
 
### Big Data Software Tools: Their Importance & Current Applications
 
* 2 big data methods: Hadoop and Spark
    * Hadoop - Multiple computer nodes create the Hadoop Distributed File Systems (HDFS), sharing the load of computation between multiple computers, and the MapReduce algorithm, for for data distribution & processing   
    * Spark - Borrows the HDFS concept and improves it; moving data into/out of files is faster, and info is saved over the whole network as opposed to individual computers. This technology is known as Resilient Distributed Datasets (RDDs)

### Hadoop's MapReduce Algorithm

* Hadoop uses the MapReduce algorithm
    * Map converts one set of data into another set of data, breaking down individual elements into tuples
    * Reduce takes the output from the map and combines the data tuples into smaller sets of tuples
        * Tuples are lists which are difficult to change (Ceri et al., 1993)
    * This data is distributed over the cluster and processed
    * Takes a long time to write and read information to and from a disk
    * Reading and writing is to assure that backups are made in case of failure

![Alt text](https://cs.calvin.edu/courses/cs/374/exercises/12/lab/MapReduceWordCount.png)

#### HDFS in 3 Sentences
"This system is where the data sets you use in Spark are stored. HDFS has the ability to split a data set into partitions known as blocks. Copies of these blocks are stored on other servers in the Hadoop cluster. That is, an individual file is actually stored as smaller blocks that are replicated across multiple servers in the entire cluster”.
                                    -IBM Analytics

### What is an RDD?

An RDD is a resilient and distributed collection of records spread over one or many partitions. This allows programmers to spread the load of the computation across many computers and solve problems faster (https://jaceklaskowski.gitbooks.io/mastering-apache-spark-2/spark-rdd.html) 

* Resilient: Fault-tolerant, can recompute damaged or missing partitions (blocks) due to node failures
* Distributed: Data resilient on multiple nodes in a cluster (HDFS)
* Dataset: The collection of partitioned data itself

## Code Used in Tutorial

Determine which version of Java is currently installed:
```
~~~
java -version    
~~~
```
Request updates for entire system:
```
~~~
sudo apt-get update  
~~~
```
Install JDK (Java Development Kit)
```
~~~
sudo apt-get install default-jdk    
~~~
```
Install Scala:
```
~~~
sudo apt-get install scala      
~~~
```
Open Scala REPL:
```
~~~
scala    
~~~
```
Test basic Scala commands to ensure program is working properly:
```
~~~
println("Hello World")                  // Prints text within quotation marks to screen
~~~
```

Determine which version of Scala you have to check compatibility with JDK:
```
~~~
scala -version
~~~
```

Git Installation Terminal Command (Useful for communicating with other Spark users, highly recommended):
```
~~~
sudo apt-get install git       
~~~
```

Run Spark Shell:
```
~~~
cd spark-2.0.2-bin-hadoop2.7            // Change directory to Spark folder

cd bin                                  // Change directory to bin folder

./spark-shell                           // Run the Spark shell (Should result in a popup)

println("Spark shell is running")       // Prints text within quotation marks to screen
~~~
```

Basic Spark Exercise:
```
~~~
val data = 1 to 10000                   // Creates a collection of integers from 1 to 10000 using the Scala language

val distData = sc.parallelize(data)     // Creates the RDD for the list of the integers from 1 to 10000 and saves it as a variable

distData.filter( _ < 10 ).collect()     // Filters integers less than ten and saves them to an array
~~~
```
An error message example which may be removed in the final draft:
```
~~~
// ----------------------------Note: May erase this section in final draft--------------------------------------

    Val lines = sc.textFile("hdfs://�")                                     // Base RDD                                        

    Val errors = lines.filter(_.startswith("ERROR"))                        // Transformed RDDs

    Val messages = errors.map(_.split("\t")).map(r => r(1)); messages.cache 

    messages.filter(_.contains("mysql")).count()                            // Count number of mysql errors

    messages.filter(_.contains("php")).count()                              // Count number of php errors

                                                                            // Continue filtering error messages as necessary

//---------------------------------------------------------------------------------------------------------------
~~~
```
Example 2: Word Count
```
~~~
val f = sc.textFile("README.md")                                                    // Saves the Read Me file as a variable

val wc = f.flatMap( l => l.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)    // Splits words after every space and lists their frequency

wc.saveAsTextFile("wc_out")                                                         // Saves word count as text file in directory

cd ~/Downloads/spark-2.2.0-bin-hadoop2.7/bin                                        // Takes us to bin directory

cd wc_out.txt                                                                       // Changes directory to wc_out.txt folder

vim part-00000                                                                      // Opens the first data partition file
~~~
```
