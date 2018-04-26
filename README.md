# Web Crawling

<h1> Installation and Configurate Apache Nutch </h1>

<h3>1) Install Apache Nutch and extract:</h3>
<br>

```
wget https://archive.apache.org/dist/nutch/1.12/apache-nutch-1.12-bin.tar.gz
tar -xvf apache-nutch-1.12-bin.tar.gz
```
<h1>2) Set up your nutch-site.xml:</h1> Add the following commands to the NUTCH-SITE.XML
<br>

```
<property>
 <name>http.agent.name</name>
 <value>My Nutch Spider</value>
</property>
```
<h1>3) Make a "urls" folder and "seed.txt" file in Apache Nutch</h3>

```
cd apache-nutch-1.12
mkdir urls
touch seed.txt
```
<h1>4) Give the path of your nutch_home:</h1> To overcome the error (Permission denied)

```
chmod +x bin/nutch
```
<h1>5) Export Java</h1>

```
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
```
<h1>6) Inject the seed url:</h1> Inject the seed url by running the below command

```
bin/nutch inject crawl/crawldb urls
```
<h1>7) Make Segments:</h1> Make segments in which data will be extracted...

```
bin/nutch generate crawl/crawldb crawl/segments
```
<h1>8) Give name to your particular segment</h1>This generates a fetch list for all of the pages due to be fetched. The fetch list is placed in a newly created segment directory. The segment directory is named by the time it's created. We save the name of this segment in the shell variable s1:

```
s1=`ls -d crawl/segments/2* | tail -1`
echo $s1
```
<h1>9) Start the fetch phase of the segment</h3>

```
bin/nutch fetch $s1
```
<h1>10) Then parse the data</h3>

```
bin/nutch parse $s1
```
<h1>11) Update the database with the results of the fetch</h3>

```
bin/nutch updatedb crawl/crawldb $s1
```
<h1>12) Then, generate, fetch a new segment containing the top scoring 1.000 pages</h3>

```
bin/nutch generate crawl/crawldb crawl/segments -topN 1000
s2=`ls -d crawl/segments/2* | tail -1`
echo $s2

bin/nutch fetch $s2
bin/nutch parse $s2
bin/nutch updatedb crawl/crawldb $s2
```
<h1>13) Invert all of the links</h3> Before indexing first invert all of the links, so that may index incoming anchor text with the pages.

```
bin/nutch invertlinks crawl/linkdb -dir crawl/segments
```

