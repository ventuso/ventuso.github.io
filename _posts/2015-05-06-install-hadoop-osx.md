---
layout: post
title:  "Install Hadoop stack on OSX"
date:   2015-05-06 16:00:00
categories: guide
published: true
tags: [hadoop,bigdata]
noToc: true
---

### SSH Setup and Key Generation
SSH setup is required to do different operations on a cluster such as starting, stopping, distributed daemon shell operations. To authenticate different users of Hadoop, it is required to provide public/private key pair for a Hadoop user and share it with different users.

The following commands are used for generating a key value pair using SSH. Copy the public keys form id_rsa.pub to authorized_keys, and provide the owner with read and write permissions to authorized_keys file respectively.

{% highlight sh linen %}
ssh-keygen -t rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
chmod 0600 ~/.ssh/authorized_keys 
{% endhighlight %}

### Install HomeBrew
Paste the following command at the terminal:

{% highlight sh linen %}
	ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Perform update all HomeBrew recipes

{% highlight sh linen %}
brew update
{% endhighlight %}

### Install Hadoop
	
{% highlight sh linen %}
brew install hadoop
{% endhighlight %}

Hadoop will be install in the following directory ( x.x.x is the Hadoop version)

	/usr/local/Cellar/hadoop/x.x.x

### Configuring Hadoop in Pseudo Distributed Mode
#### Edit core-site.xml
The **core-site.xml** file contains information such as the port number used for Hadoop instance, memory allocated for the file system, memory limit for storing the data, and size of Read/Write buffers.

The file can be located at */usr/local/Cellar/hadoop/x.x.x/libexec/etc/hadoop/core-site.xml*. Open the core-site.xml and add the following properties in between  *\<configuration\>* and  *\</configuration\>* tags.

{% highlight xml linen %}
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
        <description>A base for other temporary directories.</description>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration> 
{% endhighlight %}

#### Edit hdfs-site.xml
The **hdfs-site.xml** file contains information such as the value of replication data, namenode path, and datanode paths of your local file systems. It means the place where you want to store the Hadoop infrastructure.

The file can be located at */usr/local/Cellar/hadoop/x.x.x/libexec/etc/hadoop/hdfs-site.xml*. Open the core-site.xml and add the following properties in between  *\<configuration\>* and  *\</configuration\>* tags.

{% highlight xml linen %}
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>file:///usr/local/Cellar/hadoop/hdfs/namenode </value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>file:///usr/local/Cellar/hadoop/hdfs/datanode </value>
    </property>
</configuration>
{% endhighlight %}

#### Edit yarn-site.xml
This file is used to configure yarn into Hadoop. It can be located at */usr/local/Cellar/hadoop/x.x.x/libexec/etc/hadoop/yarn-site.xml* , open  and add the following properties in between the *\<configuration\>* and  *\</configuration\>* tags in this file.

{% highlight xml linen %}
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
{% endhighlight %}

#### Edit mapred-site.xml
This file is used to specify which MapReduce framework we are using. By default, Hadoop contains a template of yarn-site.xml. First of all, it is required to copy the file from **mapred-site.xml.template** to **mapred-site.xml** file using the following command.

{% highlight sh linen %}
cp mapred-site.xml.template mapred-site.xml 
{% endhighlight %}

It can be located at */usr/local/Cellar/hadoop/x.x.x/libexec/etc/hadoop/mapred-site.xml.template*. After copied, Open **mapred-site.xml** file and add the following properties in between the  *\<configuration\>* and  *\</configuration\>* tags in this file.

{% highlight xml linen %}
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
{% endhighlight %}

#### Verifying Hadoop Installation
The following steps are used to verify the Hadoop installation.
Go to:

{% highlight sh linen %}
/usr/local/Cellar/hadoop/x.x.x/bin
{% endhighlight %}

##### Step 1: Name Node Setup
Set up the namenode using the command “hdfs namenode -format” as follows.

{% highlight sh linen %}
hdfs namenode -format 
{% endhighlight %}

##### Step 2: Verifying Hadoop dfs
The following command is used to start dfs. Executing this command will start your Hadoop file system.

{% highlight sh linen %}
start-dfs.sh 
{% endhighlight %}

##### Step 3: Verifying Yarn Script
The following command is used to start the yarn script. Executing this command will start your yarn daemons.

{% highlight sh linen %}
start-yarn.sh
{% endhighlight %}

##### Step 4: Accessing Hadoop on Browser
The default port number to access Hadoop is 50070. Use the following url to get Hadoop services on browser.

	http://localhost:50070/

##### Step 5: Verify All Applications for Cluster
The default port number to access all applications of cluster is 8088. Use the following url to visit this service

	http://localhost:8088/

#### Alias
To simplify life edit your ~/.profile using vim or your favorite editor and add the following two commands

{% highlight sh linen %}
alias hstart="/usr/local/Cellar/hadoop/2.6.0/sbin/start-dfs.sh;/usr/local/Cellar/hadoop/2.6.0/sbin/start-yarn.sh"
alias hstop="/usr/local/Cellar/hadoop/2.6.0/sbin/stop-yarn.sh;/usr/local/Cellar/hadoop/2.6.0/sbin/stop-dfs.sh"
{% endhighlight %}

and execute

{% highlight sh linen %}
source ~/.profile
{% endhighlight %}

Next time, we can run Hadoop just by typing 
    
    hstart

and stop using

    hstop