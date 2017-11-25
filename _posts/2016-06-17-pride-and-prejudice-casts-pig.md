---
layout: post
title: Pride and Prejudice - Find which character is famous using Apache PIG
description: A simple PIG program to count the number of times each characters name has appeared in the book pride and prejudice.
categories: [hadoop-tutorials,apache-pig]
tags: [pig]
fullview: false
comments: true

---

In this tutorial, we are going to take the simple word count example and spice it up. We are going to use Apache PIG to determine the number of times each characters name has appeared in the book pride and prejudice.

If you have not installed PIG - please follow the steps in this tutorial.

  - [Installing Apache PIG on MAC OS](/hadoop-tutorials/apache-pig/2016/06/15/install-apache-pig.html)

&nbsp;    
Lets get started !!!

&nbsp;

Just for reference, these are the main characters we are going to look for. I'm going by their first names.

- Catherine
- Elizabeth
- Collins
- George
- Jane
- Lydia
- Caroline
- Kitty
- Charles  
&nbsp;  


**Step 1: Download the book**

```
wget https://s3.amazonaws.com/omairaasim.github.io/data/pride_and_prejudice.txt
```
&nbsp;  

**Step 2: Copy the book into HDFS**

```
hadoop fs -copyFromLocal /home/hadoop/pride_and_prejudice.txt /data/pig/pride_and_prejudice.txt
```
&nbsp;  

**Step 3: Load the data**

Start pig and load the data

```
pnp_book = LOAD '/data/pig/pride_and_prejudice.txt' AS (lines:chararray);
```
&nbsp;  

**Step 4: Tokenize the data**

Lets tokenize and flatten the data so we get one word in each line. Also in this step - I'm replacing all punctuation marks and ("'s'"). If we don't do that, then when we tokenize - these 3 would be considered as separate tokens

- Elizabeth
- Elizabeth.
- Elizabeth's

```
tokens = FOREACH pnp_book GENERATE FLATTEN (TOKENIZE(REPLACE(lines, '\'[s]|[\\.;,!:]',''))) AS word;
```
&nbsp;  

**Step 5: Filter the data**

```
character_names = FILTER tokens BY
                                (
                                    LOWER(word) matches LOWER('Elizabeth') OR  
                                    LOWER(word) matches LOWER('Collins') OR
                                    LOWER(word) matches LOWER('George') OR  
                                    LOWER(word) matches LOWER('Jane') OR
                                    LOWER(word) matches LOWER('Lydia') OR  
                                    LOWER(word) matches LOWER('Caroline') OR
                                    LOWER(word) matches LOWER('Kitty') OR  
                                    LOWER(word) matches LOWER('Charles')
                                );
```
&nbsp;

**Step 6: Group the words together**

Next we will group the words

```
group_by_name = GROUP character_names BY LOWER(word);
```
&nbsp;  

**Step 7: Count the occurrences**

Next we will count the number of times each character name has occurred

```
count_names = FOREACH group_by_name GENERATE group, COUNT(character_names) as total_count;
```
&nbsp;  

**Step 8: Order by max count**

```
order_count = ORDER count_names BY total_count DESC;
```
&nbsp;  

**Step 9: Save the script**

Lets save it as a pride_and_prejudice.pig file

```
STORE order_count INTO '/output/pig/pride_and_prejudice';
```
&nbsp;  

**Step 10: Output**

The output is saved in the hdfs folder we specified in step 9. To view the results - you can use this command.

```
hadoop fs -cat /output/pig/pride_and_prejudice/part-r-00000
```

```
elizabeth 633
jane      293
collins	  180
lydia	  170
kitty	   70
caroline   17
george      8
charles	    7
```
&nbsp;  

**Here is the complete script**

```
pnp_book = LOAD '/data/pig/pride_and_prejudice.txt' AS (lines:chararray);
tokens = FOREACH pnp_book GENERATE FLATTEN (TOKENIZE(REPLACE(lines, '\'[s]|[\\.;,!:]',''))) AS word;
character_names = FILTER tokens BY
                                (
                                    LOWER(word) matches LOWER('Elizabeth') OR  
                                    LOWER(word) matches LOWER('Collins') OR
                                    LOWER(word) matches LOWER('George') OR  
                                    LOWER(word) matches LOWER('Jane') OR
                                    LOWER(word) matches LOWER('Lydia') OR  
                                    LOWER(word) matches LOWER('Caroline') OR
                                    LOWER(word) matches LOWER('Kitty') OR  
                                    LOWER(word) matches LOWER('Charles')
                                );
group_by_name = GROUP character_names BY LOWER(word);
count_names = FOREACH group_by_name GENERATE group, COUNT(character_names) as total_count;
order_count = ORDER count_names BY total_count DESC;
STORE order_count INTO '/output/pig/pride_and_prejudice';
```
&nbsp;  

As you can see - this is nothing but an extension of word count program but we've used it to do a simple analysis.
