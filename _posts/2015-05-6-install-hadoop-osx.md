---
layout: post
title:  "Install Hadoop stack on OSX"
date:   2015-05-06 16:00:00
categories: guide
published: true
tags: [hadoop,bigdata]
noToc: false
---

# SSH Setup and Key Generation
SSH setup is required to do different operations on a cluster such as starting, stopping, distributed daemon shell operations. To authenticate different users of Hadoop, it is required to provide public/private key pair for a Hadoop user and share it with different users.

The following commands are used for generating a key value pair using SSH. Copy the public keys form id_rsa.pub to authorized_keys, and provide the owner with read and write permissions to authorized_keys file respectively.

{% highlight sh linen %}
ssh-keygen -t rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
chmod 0600 ~/.ssh/authorized_keys 
{% endhighlight %}

# Install HomeBrew
Paste the following command at the terminal:

{% highlight sh linen %}
	ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

Perform update all HomeBrew recipes

{% highlight sh linen %}
brew update
{% endhighlight %}

# Install Hadoop
	
{% highlight sh linen %}
brew install hadoop
{% endhighlight %}

Hadoop will be install in the following directory ( x.x.x is the Hadoop version)

	/usr/local/Cellar/hadoop/x.x.x

# Configuring Hadoop in Pseudo Distributed Mode
## Edit core-site.xml
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

## Edit hdfs-site.xml
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

## Edit yarn-site.xml
This file is used to configure yarn into Hadoop. It can be located at */usr/local/Cellar/hadoop/x.x.x/libexec/etc/hadoop/yarn-site.xml* , open  and add the following properties in between the *\<configuration\>* and  *\</configuration\>* tags in this file.

{% highlight xml linen %}
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
{% endhighlight %}

## Edit mapred-site.xml
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

## Verifying Hadoop Installation
The following steps are used to verify the Hadoop installation.
Go to:

{% highlight sh linen %}
/usr/local/Cellar/hadoop/x.x.x/bin
{% endhighlight %}

### Step 1: Name Node Setup
