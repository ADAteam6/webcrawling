# Web Crawling

<h1>1)Install Apache Nutch and extract:</h1>
<br>
wget https://archive.apache.org/dist/nutch/1.12/apache-nutch-1.12-bin.tar.gz
<br>
tar -xvf apache-nutch-1.12-bin.tar.gz
<br>
<h1>2)Set up your nutch-site.xml:</h1> Add the following commands to the NUTCH-SITE.XML
<br>
```ruby
<property>
 <name>http.agent.name</name>
 <value>My Nutch Spider</value>
</property>
```
