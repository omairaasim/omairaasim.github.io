---
layout: post
title: Word count example doing using Apache PIG
description: Word count using PIG - Step by step tutorial on how to do word count using Apache PIG.
categories: [hadoop-tutorials,apache-pig]
tags: [pig]
fullview: false
comments: true

---

In this tutorial, we are going to do the word count example using Apache PIG.

If you have not installed PIG - please follow the steps in this tutorial.

  - [Installing Apache PIG on MAC OS](/hadoop-tutorials/apache-pig/2016/06/15/install-apache-pig.html)

&nbsp;    
Lets get started !!!

&nbsp;

**Step 1: Create a sample file to do word count**

First lets create a sample file to do the word count. I am going to use the vi editor to create a file called wordcount.txt. As sugggested in earlier tutorials - I am using MAC Terminal to connect to the virtual machine.

```
vi /home/hadoop/word_count.txt
```
&nbsp;  

- Then press "i" to enable insert mode
- Copy/paste the following lines  
&nbsp;  


```
apache pig was invented by yahoo
pig is a data flow language
using pig language we can process any data
```
&nbsp;  

- Then press "esc" to exit insert mode
- Finally press ":wq" to write and quit  
&nbsp;  

**Step 2: Copy the file to HDFS**

Use the following command to copy the file into HDFS. I'm creating a new directory /data/pig to store the pig example files.

```
hadoop fs -copyFromLocal /home/hadoop/word_count.txt /data/pig/word_count.txt
```
&nbsp;  

You can verify if the file has been copied to HDFS by running the command

```
hadoop fs -ls /data/pig
```

&nbsp;  

**Step 3: Invoke PIG**

This will take you to the grunt prompt.

```
pig
```

&nbsp;

**Step 4: Load the data**

First step is to load the data from the file. We use the load command for that purpose.

```
all_lines = load '/data/pig/word_count.txt' as (lines:chararray);
```
&nbsp;  
Lets describe this relation

```
describe all_lines;
```
&nbsp;  
The out put will be

```
all_lines: {lines: chararray}
```

This is interpreted as all_lines is name of relation. It contains 1 field *lines* of type chararray.

&nbsp;  
Since this is our first example - lets look at how the data looks. Run the dump command which will run a map reduce program to show the output.

```
dump all_lines;
```
&nbsp;  
You will see 3 tuples

```
(apache pig was invented by yahoo)
(pig is a data flow language)
(using pig language we can process any data)
```
&nbsp;  

**Step 5: Tokenize the data**  


- **FOREACH...GENERATE**  

  This is like our standard looping. It is used to act on every row in a relation. Since we want to take each line and tokenize it, we will be using FOREACH...GENERATE to loop through each of the lines in the file.

- **TOKENIZE**  

  We need to tokenize each line into separate word. We can use the built in TOKENIZE function.
  This function will split on whitespace and each result is placed in its own tuple and all tuples are placed in a bag.

- **FLATTEN**  

  If we want to remove nesting and elevate each field to a top level field, we can use the FLATTEN modifier.

&nbsp;  

To understand this - lets run these two examples to see the difference in output.
&nbsp;  

```
words_without_flatten = FOREACH all_lines GENERATE TOKENIZE(lines) as word;
dump words_without_flatten;
```
&nbsp;  

The output will be as shown

```
({(apache),(pig),(was),(invented),(by),(yahoo)})
({(pig),(is),(a),(data),(flow),(language)})
({(using),(pig),(language),(we),(can),(process),(any),(data)})
```
&nbsp;  

As you can see, the TOKENIZE function has split the lines into single words and placed each word into its own tuple and all tuples are placed in a bag for a single line.

&nbsp;  

Lets see how the data looks if we run the same command with FLATTEN modifier.
&nbsp;  

```
words_with_flatten = FOREACH all_lines GENERATE FLATTEN(TOKENIZE(lines)) as word;
dump words_with_flatten;
```
&nbsp;  

Here you can see - each word has been elevated to a top level tuple. For doing word count - data in this format is more useful to use so we can group it as shown in next step.

```
(apache)
(pig)
(was)
(invented)
(by)
(yahoo)
(pig)
(is)
(a)
(data)
(flow)
(language)
(using)
(pig)
(language)
(we)
(can)
(process)
(any)
(data)
```
&nbsp;  

Lets describe this relation

```
describe words_with_flatten;
```
&nbsp;  
This is the output. The relation *words_with_flatten* has 1 field word of type chararray.

```
words_with_flatten: {word: chararray}
```
&nbsp;  

**Step 6: Apply GROUP...BY**

As the name suggest, the GROUP...BY statement is used to group the data in a single relation.

```
grouped_words = GROUP words_with_flatten BY word;
dump grouped_words;
```

&nbsp;  

In the output for each word, it will show a bag containing tuples corresponding to the number of times the word occurred. For example, the word pig occurred 3 times - so you can see a bag containing 3 tuples of word pig.

```
(a,{(a)})
(by,{(by)})
(is,{(is)})
(we,{(we)})
(any,{(any)})
(can,{(can)})
(pig,{(pig),(pig),(pig)})
(was,{(was)})
(data,{(data),(data)})
(flow,{(flow)})
(using,{(using)})
(yahoo,{(yahoo)})
(apache,{(apache)})
(process,{(process)})
(invented,{(invented)})
(language,{(language),(language)})
```

&nbsp;  

Lets describe this relation.

```
describe grouped_words;
```

&nbsp;  
This is the output. This means the relation grouped_words has 2 fields, *group* (the name PIG assigns by default to the group by field) and a bag *words_with_flatten*

```
grouped_words: {group: chararray,words_with_flatten: {(word: chararray)}}
```

If you notice the first field is the grouping field and by default PIG gives it an alias *group*.
The second field is a bag containing the grouped fields which has the same schema as the original relation.

&nbsp;  

**Step 7: Count the words**

The last step is pretty easy. We have to iterate through the loop and for each word, count the number of tuples.

```
word_count = FOREACH grouped_words GENERATE group as Word, COUNT(words_with_flatten) AS WordCount;
dump word_count;
```

The output is

```
(a,1)
(by,1)
(is,1)
(we,1)
(any,1)
(can,1)
(pig,3)
(was,1)
(data,2)
(flow,1)
(using,1)
(yahoo,1)
(apache,1)
(process,1)
(invented,1)
(language,2)
```

Thats the result of our word count program.

To exit grunt , type quit.

&nbsp;  

**Step 8: Lets put all together**

Rather than executing one line at a time in grunt prompt - we can save the entire script in a file and run it. We can also save the output in a file.

- Create wordcount.pig file

```
vi /home/hadoop/wordcount.pig
```
&nbsp;  

- Copy paste the code below and save it

```
all_lines = LOAD '/data/pig/word_count.txt' AS (lines:chararray);
words_with_flatten = FOREACH all_lines GENERATE FLATTEN(TOKENIZE(lines)) AS word;
grouped_words = GROUP words_with_flatten BY word;
word_count = FOREACH grouped_words GENERATE group AS Word, COUNT(words_with_flatten) AS WordCount;
STORE word_count INTO '/output/pig/wordcount';
```
&nbsp;  

- Run the script as follows

```
pig /home/hadoop/wordcount.pig
```
&nbsp;  

- Check the output

```
hadoop fs -cat /output/pig/wordcount/part-r-00000
```
&nbsp;  

Hope you enjoyed this tutorial. Word count is like the Hello World equivalent of PIG :)
