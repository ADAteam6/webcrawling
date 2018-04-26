# Web Crawling

<h1>1) Install Apache Nutch and extract:</h1>
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
export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
```
<h1>5) Inject the seed url:</h1> Inject the seed url by running the below command

```
bin/nutch inject crawl/crawldb urls
```
