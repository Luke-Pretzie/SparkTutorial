# Spark Tutorial

This ReadMe is a guide to the installation and use of Apache Spark on Linux Ubuntu, with insight into the utility of Spark and examples of using Spark for big data applications

### What is the purpose of this repository?

* The amount of data generated on a daily basis is monumental, and the rate at which this data is being produced is exponential. Through this tutorial I hope to give insight into the big impact that the Apache Spark software platform can have on the analysis of big data.

### Necessary Software
* Software Versions: Spark Version 2.2.0, Java 6 or 7 with with Scala 2.9.2 OR Java 8 with Scala 2.10.3+
* Linux OS (Written in Ubuntu 14.04 Distribution, exercise caution if using different distributions)

### Who do I talk to?

* For questions, comments and concerns, please contact Luke Pretzie at lpretz2@uic.edu

## Background

### The Impact of Big Data
* 90% of the data in the world today has been created in the last two years alone.
* US healthcare system alone could create $300 billion in value annually with proper applications of big data (http://www.mckinsey.com/business-functions/digital-mckinsey/our-insights/big-data-the-next-frontier-for-innovation)

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

### How Spark Works With RDDs

* Spark processes data using a driver and executors
    * Driver: Initializes the work. Could be a desktop, laptop, etc.
    * Executors: The processors which are actually doing the computation, such as a supercomputer cluster or a server cluster
* These executors read in the data as blocks from the HDFS:

***
![Alt text](https://raw.githubusercontent.com/Luke-Pretzie/SparkTutorial/master/Picture1.png)
***

* When the program reaches action/command 1, the executors process all the data, cache the result, and then resubmit it to the driver for eventual evaluation.
* For actions/commands 2 and beyond, however, because most of the information is stored in the caches of the executors, we process straight from the cache without consulting the HDFS:

***
![Alt text](https://raw.githubusercontent.com/Luke-Pretzie/SparkTutorial/master/Picture4.png)
***

## Necessary Software Installation Procedures

### Ubuntu Installation

* Refer to https://linus.nci.nih.gov/bdge/installUbuntu.html for installation procedure of Linux Ubuntu on Oracle VirtualBox virtual machine. Read everything before the "BRB-DGE" link, the last sentence of the page.
    * Ensure that you set the amount of RAM to 4096 MB on the "Memory Size" tab, and set the size of the virtual hard drive to 100 GB on the "File location and size" tab. Follow all other procedures as listed.
* For more information regarding the installation and usage, you can also refer the following link. Read everything before “Basic Shell Commands”: https://github.com/pophipi/WEVOTE/blob/master/LINUXINSTALL.md

NOTE: In order to copy and paste code using the Linux Ubuntu terminal (activated by pressing Ctrl+Alt+T on the Ubuntu Desktop), you need to add shift to your Windows hotkey commands:

* Ctrl+X: Cuts text/photos/information from applications   => Ctrl+Shift+X: Cuts text/photos/information from Terminal Only
* Ctrl+C: Copies text/photos/information from applications => Ctrl+Shift+C: Copies text/photos/information from Terminal Only
* Ctrl+V: Pastes text/photos/information to applications   => Ctrl+Shift+V: Pastes text/photos/information to Terminal Only

#### All code seen in this tutorial will be entered using the Linux Ubuntu terminal.

First, check to see which version of Java is already installed:
```
java -version    // Tells you which version of Java you have
```
A message saying “Java can be found in the following packages…” means you most likely need to install Java.
We will install the Java already packaged with Ubuntu. To do this:
```          
sudo add-apt-repository ppa:webupd8team/java -y    // Get Java 8 repository
sudo apt-get update                                // Request updates for entire system
sudo apt-get install oracle-java8-installer        // Get Java 8 installer
sudo apt-get install oracle-java8-set-default      // Set Java 8 as default after it has been installed
```

Note: Must use Scala 2.10 .3+ with Java 8 or Scala 2.9.2 with Java 6 or 7:
```
scala -version    // Checks which version of Scala you have, if installed
```

Install Scala, if necessary, using this code taken from https://gist.github.com/osipov/c2a34884a647c29765ed:
```
sudo apt-get remove scala-library scala
sudo wget www.scala-lang.org/files/archive/scala-2.10.4.deb
sudo dpkg -i scala-2.10.4.deb
sudo apt-get update
sudo apt-get install scala
```

Open Scala REPL (Read-Evaluate-Print-Loop language shell. Only receives single inputs) :
```
scala                                   // Opens Scala REPL
```

Test basic Scala commands to ensure program is working properly:
```
println("Hello World")                  // Prints text within quotation marks to screen
```

Exit out of REPL:

```
:q                                      // Quits most REPLs that one can find
```

##### If for whatever reason you decide to switch from Scala 2.10.3+ and Java 8 to Scala 2.9.2 and Java 6 or 7, the following code will remove all Java related packages from the computer, allowing you to reinstall whatever software you desire:
```
dpkg-query -W -f='${binary:Package}\n' | grep -E -e '^(ia32-)?(sun|oracle)-java' -e '^openjdk-' -e '^icedtea' -e '^(default|gcj)-j(re|dk)' -e '^gcj-(.*)-j(re|dk)' -e '^java-common' | xargs sudo apt-get -y remove
```
Now download git technology (Allows for easier communication and cooperation between programmers working on the same project or using the same software. Highly recommended to install):
```
sudo apt-get install git                // Install git
```

Now it is time to install Spark:
* Go to https://spark.apache.org/downloads.html & download pre-built for Hadoop[ 2.7 version of Spark (2.0 or later, preferably. This tutorial uses Spark 2.2.0 & therefore recommends it).
* BE SURE TO USE THE MOZILLA FIREFOX BROWSER THAT COMES PRE-BUILT WITH UBUNTU FOR THIS TASK
* Save .tgz file somewhere where it can be easily found (Saved to Downloads folder by default).
* Change directory to where .tgz file is saved and open it:
```
cd                                                      // Goes to Home directory
cd <Directory where you saved the .tgz file on Ubuntu>  // Goes to specified directory; "cd Downloads" should be the default
tar xvf spark-2.2.0-bin-hadoop2.7.tgz                   // Opens the Hadoop “tarball” (Archive File)
```

Run Spark Shell, starting from within the Downloads folder:
```
cd spark-2.2.0-bin-hadoop2.7            // Change directory to Spark folder
cd bin                                  // Change directory to bin folder
```
At any point in time when navigating these directories in Bash (Not Scala), type "ls" to list what files are in the directory you are currently looking at. Typing ls in the bin directory shows a list of the files in there. Make note of the "beeline" and "spark-shell" files:
```
ls                                      // List all files and directories in the current directory
./spark-shell                           // Run the Spark shell file, opening the Spark shell (Should result in a popup)
println("Spark shell is running")       // Prints text within quotation marks to screen
```

Now it is finally time for out first Spark exercise - Creating a list of integers and filtering it:
```
val data = 1 to 10000                   // Creates a collection of integers from 1 to 10000 using the Scala language
val distData = sc.parallelize(data)     // Creates the RDD for the list of the integers and saves it as a variable
distData.filter( _ < 10 ).collect()     // Filters integers less than ten and saves them to an array
```
An error message example which may be removed in the final draft:
```
// ----------------------------Note: May erase this section in final draft--------------------------------------

    Val lines = sc.textFile("hdfs://�")                                     // Base RDD                                        

    Val errors = lines.filter(_.startswith("ERROR"))                        // Transformed RDDs

    Val messages = errors.map(_.split("\t")).map(r => r(1)); messages.cache 

    messages.filter(_.contains("mysql")).count()                            // Count number of mysql errors

    messages.filter(_.contains("php")).count()                              // Count number of php errors

                                                                            // Continue filtering error messages as necessary

//---------------------------------------------------------------------------------------------------------------
```
A slightly more advanced example is where we search through a text file in order to find which words are used and how frequently:
```
val f = sc.textFile("beeline")                                                     // Saves the beeline file as a variable
val wc = f.flatMap( l => l.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)   // Splits words after every space and lists their frequency
wc.saveAsTextFile("beeText")                                                       // Saves word count as text file in directory
:q                                                                                 // Quit Spark REPL
cd beeText                                                                         // Changes directory to wc_out.txt folder
vim part-00000                                                                     // Opens the first data partition file
```

## Unlisted Credits
* https://medium.com/@josemarcialportilla/installing-scala-and-spark-on-ubuntu-5665ee4b62b1
* https://www.ibm.com/analytics/us/en/technology/hadoop/hdfs/ 
* https://www.toptal.com/spark/introduction-to-apache-spark
* https://github.com/ceteri/intro_spark
* Hey, Tony; Pápay, Gyuri (2014). The Computing Universe: A Journey through a Revolution. Cambridge University Press. p. 76. ISBN 978-1-31612322-5
* https://media.nationalpriorities.org/uploads/total_spending_pie%2C__2015_enacted.png
* https://cs.calvin.edu/courses/cs/374/exercises/12/lab/MapReduceWordCount.png
* Ceri, Stefano, Katsumi Tanaka, and Shalom Tsur. Deductive and Object-oriented Databases: Third International Conference, DOOD '93, Phoenix, Arizona, USA, December 6-8, 1993: Proceedings. Berlin: Springer, 1993. Print.
* http://www.mckinsey.com/business-functions/digital-mckinsey/our-insights/big-data-the-next-frontier-for-innovation
