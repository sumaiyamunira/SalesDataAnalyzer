# SalesDataAnalyzer: Big Data Sales Analytics

This repository contains a Hadoop MapReduce application for performing big data analytics on sales data. The application calculates the total sales for each item based on the provided input file, showcasing the power of big data processing.

## Table of Contents

- [Introduction](#introduction)
- [Usage](#usage)
- [Hadoop Commands](#hadoop-commands)
- [Output](#output)
- [SalesDataAnalyzer.java](#salesdataanalyzerjava)

## Introduction

The Hadoop MapReduce application processes large-scale sales data from a file, applying a summation operation to calculate the total sales for each item. It leverages the scalability and parallel processing capabilities of Hadoop to handle big data analytics efficiently.

## Usage

To harness the potential of big data analytics, follow the steps below:

1. **Compile Java Code:**
   ```bash
   export HADOOP_CLASSPATH=$($HADOOP_HOME/bin/hadoop classpath)
   javac -cp $HADOOP_CLASSPATH SalesDataAnalyzer.java
