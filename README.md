# Spark Tutorial ReadMe

This ReadMe is a guide to the installation and use of Apache Spark on Linux Ubuntu, with insight into the utility of Spark and examples of using Spark for big data applications

### What is this repository for?

* The amount of data generated on a daily basis is monumental, and the rate at which this data is being produced is monumental. Through this tutorial I hope to give insight into the big impact that the Apache Spark software platform can have on the analysis of big data.
* Software Versions: Spark Version 2.2.0, Java 6 or 7 (compatible with Scala 2.9.2) OR Java 8 (compatible with Scala 2.10.3+)

### Who do I talk to? ###

* For questions, comments and concerns, please contact Luke Pretzie at lpretz2@uic.edu

## Code Used in Tutorial ##

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
