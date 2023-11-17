# SalesDataAnalyzer: Big Data Sales Analytics

This repository contains a Hadoop MapReduce application for performing big data analytics on sales data. The application calculates the total sales for each item based on the provided input file, showcasing the power of big data processing.

## Table of Contents

- [Introduction](#introduction)
- [Hadoop Commands](#hadoop-commands)
- [Output](#output)
- [SalesDataAnalyzer.java](#salesdataanalyzerjava)

## Introduction

The Hadoop MapReduce application processes large-scale sales data from a file, applying a summation operation to calculate the total sales for each item. It leverages the scalability and parallel processing capabilities of Hadoop to handle big data analytics efficiently.

## Hadoop Commands

To harness the potential of big data analytics, follow the steps below:

1. **Create Directory in HDFS:**
    ```bash
      $HADOOP_HOME/bin/hadoop fs -mkdir sales_data
    $HADOOP_HOME/bin/hadoop fs -mkdir sales_output

3. **Upload a File Containing Sales Data in HDFS:**
     ```bash
      $HADOOP_HOME/bin/hadoop fs -put /home/bigdata/Desktop/sales.txt  sales_data

4. **Check File List in HDFS:**
     ```bash
      $HADOOP_HOME/bin/hadoop fs -ls sales_data

5. **Compile Java Code and Create Jar File:**
     ```bash
      export HADOOP_CLASSPATH=$($HADOOP_HOME/bin/hadoop classpath)
      javac -cp $HADOOP_CLASSPATH SalesDataAnalyzer.java jar SalesDataAnalyzer.jar SalesDataAnalyzer*.class

6. **Run MapReduce Job:**
$HADOOP_HOME/bin/hadoop jar SalesDataAnalyzer.jar SalesDataAnalyzer sales_data/sales.txt sales_output


## Output
The aggregated results will be available in the specified output_path on the Hadoop File System, showcasing the efficiency of big data processing in handling extensive datasets.


## SalesDataAnalyzer.java
The SalesDataAnalyzer.java file contains the source code for the Hadoop MapReduce application. It consists of a Mapper and Reducer class for calculating total sales by item.

  ```bash
   import java.io.IOException;
   import java.util.StringTokenizer;
   
   import org.apache.hadoop.conf.Configuration;
   import org.apache.hadoop.fs.Path;
   import org.apache.hadoop.io.IntWritable;
   import org.apache.hadoop.io.Text;
   import org.apache.hadoop.mapreduce.Job;
   import org.apache.hadoop.mapreduce.Mapper;
   import org.apache.hadoop.mapreduce.Reducer;
   import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
   import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
   
   public class SalesDataAnalyzer {
      public static void main(String[] args) throws Exception {
           Configuration conf = new Configuration();
           Job job = Job.getInstance(conf, "SumByItem");
           job.setJarByClass(Solution3.class);
           job.setMapperClass(SumMapper.class);
           job.setCombinerClass(SumReducer.class);
   	job.setReducerClass(SumReducer.class);
           job.setOutputKeyClass(Text.class);
           job.setOutputValueClass(IntWritable.class);
           FileInputFormat.addInputPath(job, new Path(args[0]));
           FileOutputFormat.setOutputPath(job, new Path(args[1]));
           System.exit(job.waitForCompletion(true) ? 0 : 1);
       }
   
   
   public static class SumMapper
               extends Mapper<Object, Text, Text, IntWritable> {
   
   	private Text itemName = new Text();
           private IntWritable calculatedValue = new IntWritable();
   
        	public void map(Object key, Text value, Context context)
                   throws IOException, InterruptedException {
               StringTokenizer itr = new StringTokenizer(value.toString());
   
               if (itr.hasMoreTokens()) {
                   String itemNameValue = itr.nextToken();
                   int pricePerUnit = Integer.parseInt(itr.nextToken());
                   int totalUnits = Integer.parseInt(itr.nextToken());
   
                   int calculated = pricePerUnit * totalUnits;
                   itemName.set(itemNameValue);
                   calculatedValue.set(calculated);
                   context.write(itemName, calculatedValue);
               }
           }
       }
   
   public static class SumReducer
               extends Reducer<Text, IntWritable, Text, IntWritable> {
    	private IntWritable result = new IntWritable();
   
           public void reduce(Text key, Iterable<IntWritable> values, Context context)
                   throws IOException, InterruptedException {
               int sum = 0;
   	    for (IntWritable val : values) {
                   sum += val.get();
               }
   
               result.set(sum);
               context.write(key, result);
           }
       }
   }
   
   
   
   
   
   
   
   
   
