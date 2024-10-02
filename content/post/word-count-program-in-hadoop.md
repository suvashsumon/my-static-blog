---
title: "Word Count Program in Hadoop"
date: 2024-10-02T12:26:38+06:00
draft: false
tags: ["hadoop", "distributed-system"]
categories: ["hadoop", "distributed-system"]
---

# 0. Overview
This tutorial will guide you through building a Word Count program using the Hadoop MapReduce framework. The basic idea is to store the input file in the HDFS structure of Hadoop and run the program from the local machine. The program will read this input file, count the number of words, and write the result back to HDFS.

### Steps involved:
1. Create an input file and upload it to HDFS.
2. Write a Mapper, Reducer, and Driver in Java.
3. Compile the code and create a jar file.
4. Run the jar file using the Hadoop command.
5. View the output stored in HDFS.

# 1. Create Input File
To create a folder in `hdfs`:
```
hdfs dfs -mkdir /suvash/in
```

This will create a folder `in` inside a folder `suvash`.

Lets, create an input file in local and then upload it to the `/suvash/in` of hdfs. I am assuming that, you are already inside `/home/hadoop` directory of your local pc.

```
vi input.txt
// write some input
```
Now copy this file into HDFS:
```
hdfs dfs -put input.txt /suvash/in
```


# 2. Create Program
Write your program on the local machine.

### Driver.java
```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class Driver {
    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, "word count");
        job.setJarByClass(Driver.class);
        job.setMapperClass(WMapper.class);
        job.setCombinerClass(WReducer.class);
        job.setReducerClass(WReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```
### WMapper.java
```java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

public class WMapper extends Mapper<LongWritable, Text, Text, IntWritable> {
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        String[] words = value.toString().split("\\s+");
        for (String str : words) {
            word.set(str);
            context.write(word, one);
        }
    }
}
```

### WReducer.java
```java
import java.io.IOException;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

public class WReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
    public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
            sum += val.get();
        }
        context.write(key, new IntWritable(sum));
    }
}
```

# 3. Creating Jar
To create the jar file, first compile your Java files:
```
javac -classpath `Hadoop classpath` -d . WMapper.java WReducer.java Driver.java
```
This will create class files for the Java files. Now it's time to generate the jar file:
```
jar cf jar_file_name.jar *.class
```
This will create a jar file named jar_file_name.jar.

# 4. Running Jar File
To run this jar file, use this command:
```
hadoop jar jar_file_name.jar Driver /suvash/in/input.txt /suvash/out
```
Here:
- `Driver` is the base class name of the program.
- `/suvash/in/input.txt` is the input file path in HDFS.
- `/suvash/out` is the output file path in HDFS.


# 5. View The Result
To view the content of the /suvash/out folder, run this command:
```
hdfs dfs -cat /suvash/out/part-r-00000
```
By following these steps, you can run the Word Count program on Hadoop and see the word frequency in the input file.
