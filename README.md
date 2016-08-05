
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

