# PageRank-Using-Hadoop-MR

**DriverClass.java** 

**Description:-**
	This file is used to run the MR jobs for creating Create Link Graph, Process PageRank and Cleanup and Sorting.
	This driver class launches thee LinkGraph MR jobs which compute the pagerank. Then it runs the PageRank.java and then finally PageRankSorting.java MR jobs.

**Commands to launch :-** 
1. Create a  jar file for the following java files using eclipse. File -> export as JAR file

2. Create a input directory & Push the input files into the hadoop directory

hadoop fs -mkdir /user/cloudera/pagerank/ /user/cloudera/pagerank/input
hadoop fs -put <fileName> /user/cloudera/pagerank/input

**3. Run the driver class**
hadoop jar pagerank.jar DriverClasss /user/cloudera/pagerank/input /user/cloudera/intialpagerank/output /user/cloudera/pagerank/output /user/cloudera/pageranksorting/output

The final output is in the following path /user/cloudera/pageranksorting/output. I am deleting the intermediate files as well & Sequence file format for intermediate files.

**LinkGraph.java :**  
 
**First MR :-**
This MR job is used to compute the number of lines in the files.
     Mapper description:
	This is used to emit ("1","1");
     Reducer description:
	This is used to compute the total number of links in the page and then update the counter.

**Second MR:-** 
This map job is used to compute the initial page rank value which equals inverse of total number of documents in corpus & out links for all the links.
  


**Pagerank.java:**

This is used to compute the pagerank and run 10 iterations. 

**Mapper description:-** 
The map job emits two types of records. One record with the link node and all the outlinks with the initial page rank value.(flag value- true)
Another record will be emitted with the inlinks and the pagerank value divided by the number of outlinks.I have flag value to appended to differentiate the type of record in the reducer(flag value - false). This values will used to sum up to form the page rank in the reducer.

**Reducer description:-**  	
This reduce job sums up the page rank values if the flag value(true). And finally appends it to the record containing the flag to true. If the record containing flag value to true is not available its ignored.


**PageRankSorting.java**

This is used to sort the page rank based on the values.

**Mapper description:-**
The just extracts the link and then pagerank value. And then sends the pagerank value as key. And then sends the node name as value. 
I have implemented a comparator to sort it in descending order before it sends to the reducer.   

**Reducer Description:-** 
This justs writes the output the file. Here I interchange the key and value before writing it. I have set the reducer count to 1.

**InvertedIndex.java**
This is used to generate the inverted index for the words in the text tag.

**Map:-** 
This is used split the words and write the words as key and their ids as value.

**Reduce:-**
This is used write the page rank values in descending order in the file.

**Commands to run InvertedIndex:-** 
hadoop jar pagerank.jar InvertedIndex /user/cloudera/pagerank/input /user/cloudera/invertedindex/output
