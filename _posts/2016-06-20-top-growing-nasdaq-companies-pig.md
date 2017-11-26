---
layout: post
title: Find the top growing companies in Nasdaq using Apache PIG
description: A simple PIG program to calculate the growth rate of companies in a given month.
categories: [hadoop-tutorials,apache-pig]
tags: [pig]
fullview: false
comments: true

---

In this tutorial, we are going to find out the stock of which companies grew the most in a given month.

If you have not installed PIG - please follow the steps in this tutorial.

  - [Installing Apache PIG on MAC OS](/hadoop-tutorials/apache-pig/2016/06/15/install-apache-pig.html)

&nbsp;    
Lets get started !!!

&nbsp;

Time to move on to some real world cases. This script is using just 1 month of data. We are going to download the Nasdaq data for May 2016 and find out the top 10 companies whose stock grew the most that month.

**Step 1: Download the Nasdaq data**

There are 2 files here. One is a zip file which contains the daily data and the other file contains the stock symbol and company name.

```
wget https://s3.amazonaws.com/omairaasim.github.io/data/nasdaq_may_2016.zip
wget https://s3.amazonaws.com/omairaasim.github.io/data/nasdaq_company_list.csv
```
&nbsp;  

**Step 2: Copy the data into HDFS**

First lets unzip the file and copy it into HDFS.

```
mkdir nasdaq
unzip nasdaq_may_2016.zip -d /home/hadoop/nasdaq/
```
&nbsp;  
If you dont have unzip - install it using

```
sudo apt-get install unzip
```

Now lets copy the files to HDFS

```
hadoop fs -copyFromLocal /home/hadoop/nasdaq /data/pig/nasdaq/stocks
hadoop fs -copyFromLocal /home/hadoop/nasdaq_company_list.csv /data/pig/nasdaq
```

You can verify if the files are copied using

```
hadoop fs -ls /data/pig/nasdaq/stocks
hadoop fs -ls /data/pig/nasdaq/
```

**Step 3: Load the data into PIG**

Start pig.

In Pig, while loading data - if you just specify the folder name - Pig will upload all the files in that folder. Since we have many daily stock files - we will just specify the folder name to upload data.

Secondly this data contains header row. To skip header row while uploading data, we can use CSVExcelStorage from piggybank.jar

```
nasdaq_data = LOAD '/data/pig/nasdaq/stocks' USING org.apache.pig.piggybank.storage.CSVExcelStorage(',','NO_MULTILINE','UNIX','SKIP_INPUT_HEADER') AS (symbol:chararray, date:chararray, open:float, high:float,low:float,close:float, volume:long);

nasdaq_company_list = LOAD '/data/pig/nasdaq/nasdaq_company_list.csv' USING PigStorage(',') AS (symbol:chararray, name:chararray);
```
&nbsp;  
The parameters CSVExcelStorage takes are

- Delimiter
- 'NO_MULTILINE': used to specify whether fields contain multiple lines.
- 'UNIX': used to specify operating system.
- 'SKIP_INPUT_HEADER': used to specify if you want to skip the first row.

If we do a dump, we will see the data is in this format

```
dump nasdaq_data;

(AAAP,17-May-16,28.13,28.76,27.75,27.86,74500)
(AAL,17-May-16,32.25,33.29,32.02,32.64,13303400)
(AAME,17-May-16,3.45,3.53,3.31,3.35,5900)
(AAOI,17-May-16,9.29,9.56,9.17,9.23,379400)
(AAON,17-May-16,28.27,28.27,27.0,27.24,214400)

```

**Step 4: Filter Data by date**

Since we want to calculate the growth - we only need to stock prices of the beginning of the month and end of the month. The beginning of May month starts on May 2nd and ends on 31st May. So we are interested in only these 2 dates.

So lets filter out May 2nd and May 31st data using the FILTER operator. Since we are comparing dates, I'm using the **ToDate** function to convert the chararray to date.

```
may2data = FILTER nasdaq_data BY ToDate(date,'dd-MMM-yy') == ToDate('02-May-16','dd-MMM-yy');
may31data = FILTER nasdaq_data BY ToDate(date,'dd-MMM-yy') == ToDate('31-May-16','dd-MMM-yy');
```
&nbsp;  

Lets dump may2data and may31data data to see some if filtering is done correctly

```
dump may2data;

(ZNGA,2-May-16,2.39,2.4,2.34,2.37,9780600)
(ZSAN,2-May-16,2.17,2.17,2.01,2.02,3100)
(ZUMZ,2-May-16,16.89,16.99,16.39,16.66,390000)
(ZYNE,2-May-16,8.13,8.13,7.6,7.77,112800)
```

That looks right. Lets confirm may31data

```
dump may31data;

(ZN,31-May-16,1.63,1.63,1.56,1.58,21700)
(ZNGA,31-May-16,2.58,2.6,2.55,2.57,7696500)
(ZUMZ,31-May-16,14.54,14.98,14.49,14.88,700800)
(ZYNE,31-May-16,7.75,9.29,7.75,8.74,456200)
```

So the filtered data looks good.

**Step 5: Perform a Join**

Since we need the closing price of both 2nd May and 31st May - lets join these 2 relations.

```
join_data = JOIN may2data BY symbol, may31data BY symbol;
```

Lets look at the resulting relation.

```
dump join_data;

(WPPGY,2-May-16,116.8,118.33,116.71,118.21,108700,WPPGY,31-May-16,116.89,117.3,115.26,115.93,117800)
(WTFCM,2-May-16,27.62,27.85,27.47,27.83,12900,WTFCM,31-May-16,28.26,28.3,27.72,28.3,7600)
(XGTIW,2-May-16,0.11,0.13,0.08,0.13,15200,XGTIW,31-May-16,0.13,0.15,0.11,0.14,37100)
(ZIONW,2-May-16,2.7,2.7,2.7,2.7,15200,ZIONW,31-May-16,3.0,3.01,2.91,2.91,9100)

```
&nbsp;
As you can see for each symbol - we have both the 2nd May and 31st data in one tuple.

**Step 6: Calculate Growth rate**

Next we will calculate the growth rate.

The formula for calculating growth rate is very simple

```
May31st closing price - May 2nd closing price
--------------------------------------------- * 100
          May 2nd closing price
```

So lets apply that in PIG. Since the column names are same in both relations may2data and may31data - the way we access them is using the disambiguate operator double colon ::

I'm also using the ROUND_TO to round of the result to 2 decimal places.

```
growth_rate = FOREACH join_data GENERATE may2data::symbol as symbol,
ROUND_TO(( ((may31data::close-may2data::close)/may2data::close) * 100),2) AS growth;

```

Lets Dump this to see the output

```
dump growth_rate;

(WHLRP,1.17)
(WHLRW,-20.0)
(WLRHU,0.76)
(WLRHW,21.31)
(WPPGY,-1.93)
(WTFCM,1.69)
(XGTIW,7.69)
(ZIONW,7.78)

```

We are almost there.

&nbsp;  

**Step 7: Get the top 10 companies that have grown the most**

So in the above step - it shows only the stock symbols. Lets just go 1 step further to get the company names. If you remember in Step 3 we loaded another file nasdaq_company_list which has the company names.

This is how we get the company names.

```
join_company_name = JOIN growth_rate BY symbol, nasdaq_company_list BY symbol;
growth_rate_company = FOREACH join_company_name GENERATE growth_rate::symbol, nasdaq_company_list::name, growth_rate::growth;

```

Lets dump to see how this looks

```
dump growth_rate_company;

(WHLRW,"Wheeler Real Estate Investment Trust,-20.0)
(WPPGY,WPP plc,-1.93)
(WTFCM,Wintrust Financial Corporation,1.69)
(XGTIW,"XG Technology,7.69)
(ZIONW,Zions Bancorporation,7.78)

```

As you can see - now for each stock symbol - we have the company name.
&nbsp;  


**Step 8: Get the top 10 companies that have grown the most**

Next we will find the top 10 companies whose stock has grown the most. This is simple in PIG.

We first order the data by growth and then limit it to 10.

```
order_data = ORDER growth_rate_company BY growth DESC;
top_10 = LIMIT order_data 10;
```

Lets dump the data to see

```
dump top_10;

(SYNC,"Synacor,130.37)
(CCXI,"ChemoCentryx,126.47)
(HOVNP,Hovnanian Enterprises Inc,126.04)
(NSPH,"Nanosphere,117.95)
(CPXX,Celator Pharmaceuticals Inc.,96.73)
(NERV,"Minerva Neurosciences,94.62)
(CLRB,"Cellectar Biosciences,91.18)
(BOSC,B.O.S. Better Online Solutions,90.19)
(PTI,"Proteostasis Therapeutics,76.5)
(AVXS,"AveXis,76.29)
```

&nbsp;  

