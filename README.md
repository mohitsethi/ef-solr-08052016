
Solr - 08052016

1. Setup solr on Vagrant VM.


  Step#1  Open Git-Bash -> Start menu, type Git bash
  Step#2  Check vagrant is installed
      $ which vagrant

  Step#3 If VirtualBox is installed
      -> Start Menu -> Type Virtualbox

  Step#4 Create Vagrantfile
      $ vagrant init

  Step#5 Add Vagrant base box
      $ vagrant box add ef-ubuntu-1404 <location-of-box-file>


  Step#6 Create vagrant machine
      $ vagrant up

  Step#7 Check if vagrant machine is running
      $ vagrant status

  Step#8 Login to Vagrant machine
      $ vagrant ssh


  Step#9 Apache Solr Home: http://lucene.apache.org/solr/
        - Get tarball 

  Step#10 Install Java8 
        - sudo apt-get install python-software-properties software-properties-common
        - sudo add-apt-repository ppa:webupd8team/java
        - sudo apt-get update -y 
        - sudo apt-get install oracle-java8-installer

  Step# 11 Start Solr
        -> Goto Solr extracted location
        -> bin/solr start 
        -> by default solr runs on 8983


  Step#12 Create core
        $ bin/solr create -c gettingstarted

  Step#13 Upload data to Solr
        $ bin/post -c gettingstarted example/exampledocs/*.xml


  Step#14 Query data source
        -> Open Browser: 
            - http://localhost:8983/solr/gettingstarted/select?q=video
            - http://localhost:8983/solr/gettingstarted/select?q=video&fl=id,name,price
            - http://localhost:8983/solr/gettingstarted/select?q=name:black
            - http://localhost:8983/solr/gettingstarted/select?q=price:[0%20TO%20400]&fl=id,name,price
            - http://localhost:8983/solr/gettingstarted/select?q=price:[0%20TO%20400]&fl=id,name,price&facet=true&facet.field=cat



  Step#15 Create a clone core
        -> Go to Solr Admin UI, acquire location of 

  Step#16 Updated managed-schems in core config
        -> validate the field-type
        -> add new custom field to schema
        -> restart solr $ bin/solr restart
        -> verify if field is added to Solr via Admin UI.


  Step#17 Update solrConfig.xml
        -> Updated Requesthandler config:
            <str name="df">text</str> 
            <str name="wt">xml</str>
            <str name="indent">true</str>
            <str name="facet">on</str>
        -> 


## Import SQL Server data to Solr

- Get SQL Server Connector
  Download Microsoft JDBC Driver 4.0 for SQL Server from: http://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=11774

  copy file 'sqljdbc4.jar' to 'contrib/dataimporthandler/lib'

- Setup a new collection

  create a new folder for a new collection - 'myproducts'. The collection will be located in '/solr/myproducts' folder. Create folders conf and data in the collection folder:

```
  /solr/myproducts/conf
  /solr/myproducts/data
```

- solrconfig.xml
  copy solrconfig.xml from an existing collection. Find my version of solrconfig.xml below in this gist.

  edit solrconfig.xml by adding:

```
    <lib dir="../../contrib/dataimporthandler/lib" regex=".*\.jar" />
    <lib dir="../../dist/" regex="solr-dataimporthandler-.*\.jar" />
```

  Make sure that 'dist' folder contains two files for data import handler:

```
    solr-dataimporthandler-4.10.2.jar
    solr-dataimporthandler-extras-4.10.2.jar
```
  
  add these lines to solrconfig.xml:

```
  <requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
      <lst name="defaults">
      <str name="config">data-config.xml</str>
      </lst>
  </requestHandler>
```

- data-config.xml for SQL Server database

```
    <dataConfig>
      <dataSource type="JdbcDataSource" 
                  driver="com.microsoft.sqlserver.jdbc.SQLServerDriver" 
                  url="jdbc:sqlserver://servername\instancename;databaseName=mydb"   
                  user="sa" 
                  password="mypass"/>
      <document>
        <entity name="product"  
          pk="id"
          query="select id,name from products"
          deltaImportQuery="SELECT id,name from products WHERE id='${dih.delta.id}'"
          deltaQuery="SELECT id FROM products  WHERE updated_at > '${dih.last_index_time}'"
          >
           <field column="id" name="id"/>
           <field column="name" name="name"/>       
        </entity>
      </document>
    </dataConfig>
```

- schema.xml

edit file 'schema.xml' accordingly to fields defined in data-import.xml:

```
<schema name="example" version="1.5">
    <field name="_version_" type="long" indexed="true" stored="true"/>
    <field name="id" type="string" indexed="true" stored="true" required="true" multiValued="false" /> 
    <field name="name" type="text_general" indexed="true" stored="true"/>
    ...
```

- add collection to solr

use admin interface in your browser to add a new collection. Add core:

```
name: myproducts
instanceDir: myproducts
```

- Perform full or incremental import

  After successfully adding a collection to Solr you can select it and run dataimport commands:

  full-import - use URL: http://localhost:8983/solr/myproducts/dataimport?command=full-import
  delta-import - use URL: http://localhost:8983/solr/myproducts/dataimport?command=delta-import
  
  The full import loads all data every time, while incremental import means only adding the data that changed since the last indexing. By default, full import starts with removal the existing index (parameter clean=true).

  Note! Use clean=false while running delta-import command.

  debug=true - The debug mode limits the number of rows to 10 by default and it also forces indexing to be synchronous with the request.

- References

http://wiki.apache.org/solr/DIHQuickStart
http://wiki.apache.org/solr/DataImportHandler





password: Administrator/sbsRC4)fWrH


Install-Package SolrNet -Version 0.4.0.4001
https://www.telerik.com/download/fiddler/fiddler4
