![Logo](https://image.ibb.co/mEc2zH/Webp_net_resizeimage_2.png)


# Web Crawling using Apache Nutch storing data on HDFS

<h1> Installation and Configurate Apache Nutch </h1>

<h3>1) Install Apache Nutch and extract:</h3>
<br>

```
wget https://archive.apache.org/dist/nutch/1.12/apache-nutch-1.12-bin.tar.gz
tar -xvf apache-nutch-1.12-bin.tar.gz
```
<h3>2) Set up your nutch-site.xml:</h3> Add the following commands to the NUTCH-SITE.XML
<br>

```
<property>
 <name>http.agent.name</name>
 <value>My Nutch Spider</value>
</property>
```
<h3>3) Make a "urls" folder and "seed.txt" file in Apache Nutch</h3>

```
cd apache-nutch-1.12
mkdir urls
touch seed.txt
```
<h3>4) Give the path of your nutch_home:</h3> To overcome the error (Permission denied)

```
chmod +x bin/nutch
```
<h3>5) Export Java</h3>

```
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
```
<h3>6) Inject the seed url:</h3> Inject the seed url by running the below command

```
bin/nutch inject crawl/crawldb urls
```
<h3>7) Make Segments:</h3> Make segments in which data will be extracted...

```
bin/nutch generate crawl/crawldb crawl/segments
```
<h3>8) Give name to your particular segment</h3>This generates a fetch list for all of the pages due to be fetched. The fetch list is placed in a newly created segment directory. The segment directory is named by the time it's created. We save the name of this segment in the shell variable s1:

```
s1=`ls -d crawl/segments/2* | tail -1`
echo $s1
```
<h3>9) Start the fetch phase of the segment</h3>

```
bin/nutch fetch $s1
```
<h3>10) Then parse the data</h3>

```
bin/nutch parse $s1
```
<h3>11) Update the database with the results of the fetch</h3>

```
bin/nutch updatedb crawl/crawldb $s1
```
<h3>12) Then, generate, fetch a new segment containing the top scoring 1.000 pages</h3>

```
bin/nutch generate crawl/crawldb crawl/segments -topN 1000
s2=`ls -d crawl/segments/2* | tail -1`
echo $s2

bin/nutch fetch $s2
bin/nutch parse $s2
bin/nutch updatedb crawl/crawldb $s2
```
<h3>13) Invert all of the links</h3> Before indexing first invert all of the links, so that may index incoming anchor text with the pages.

```
bin/nutch invertlinks crawl/linkdb -dir crawl/segments
```
<h1> Installition and Starting Apache Solr </h1>
<h3>1) Install and Extract Apache Solr</h3>

```
cd
wget https://archive.apache.org/dist/lucene/solr/4.10.1/solr-4.10.1.tgz
tar -xvf solr-4.10.1.tgz
```
<h3>2) Starting Solr</h3>

```
cd solr-4.10.1/example
java -jar start.jar

http://YOUR_IP_ADDRESS:8983/solr
```
<h3>3) Backup the original Solr schema.xml</h3>

```
mv ${APACHE_SOLR_HOME}/example/solr/collection1/conf/schema.xml ${APACHE_SOLR_HOME}/example/solr/collection1/conf/schema.xml.org
```
<h3>4) Copy the Nutch specific schema.xml to replace it</h3>

```
cp ${NUTCH_RUNTIME_HOME}/conf/schema.xml ${APACHE_SOLR_HOME}/example/solr/collection1/conf/
```
<h3>5)run the Solr Index command from  ```${NUTCH_RUNTIME_HOME}```</h3>

```
cd $NUTCH_RUNTIME_HOME
bin/nutch solrindex http://127.0.0.1:8983/solr/ crawl/crawldb -linkdb crawl/linkdb crawl/segments/*
```
<h4> Finished... </h4>


<h3> Thanks for Attention </h3>
<h2> cd ADA/it/bigdata/team6 </h2>
