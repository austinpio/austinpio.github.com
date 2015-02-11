---
layout: post
title: "Storm Setup"
description: ""
category: 
tags: []
---
{% include JB/setup %}

<div dir="ltr" style="text-align: left;" trbidi="on">
i am running ubuntu 14.04 (Trusty) on which i am going to install storm-0.9.3 along with zookeeper-3.4.5 using CDH 5

Adding the CDH5 repository:

there are many ways to do this as described in the&nbsp;[&nbsp;cloudera installation manual](http://www.cloudera.com/content/cloudera/en/documentation/core/latest/topics/cdh_ig_cdh5_install.html)

one way is to use the deb file.

download the repository deb file or use wget for the Trusty version of Ubuntu (14.04)

[trusty link](http://archive.cloudera.com/cdh5/one-click-install/trusty/amd64/cdh5-repository_1.0_all.deb)

or

> wget&nbsp;http://archive.cloudera.com/cdh5/one-click-install/trusty/amd64/cdh5-repository_1.0_all.deb

once the deb file is available, install it using the dpkg command

> dpkg -i&nbsp;**cdh5-repository_1.0_all.deb**
this would add the CDH5 repository.

Additional step for ubuntu trusty:

ensure you have a file in your apt preferences

> <div class="p1">
> <span class="s1">/etc/apt/preferences.d/cloudera-cdh5.pref&nbsp;</span></div>
this file contents ensures that you would get the zookeeper packages from the CDH repository. More explanation for this is available on the cloudera manual.

run a update after this

> apt-get update

**<u>Zookeeper installation:</u>**

once you have CDH5 added, we can use the apt-get to install zookeeper

> apt-get install zookeeper zookeeper-server
once installed you can start using the scripts

> /usr/lib/zookeeper/bin/zkServer.sh start
you change config at /usr/lib/zookeeper/conf/zoo.cfg

once started you can test it using

> echo ruok | nc localhost 2181
it should respond "i am ok"

Note:

1.zookeeper is "fail-fast", you need to use a supervisor program when running zookeeper in production.

2. zookeeper requires regular cleanup of the log files.

3. it is recommended to setup atleast 3 zookeeper servers, for failover.

Refer&nbsp;[zookeeper manual](http://zookeeper.apache.org/doc/r3.3.3/zookeeperAdmin.html#sc_maintenance)&nbsp;for more details.

<u>**Storm installation:**</u>

Download the apache storm distribution ,

i am using apache-storm-0.9.3

<u>**Nimbus setup:**</u>

copy the storm folder to the nimbus server and modify the configuration files.

> <div class="p1">
> <span class="s1">apache-storm-0.9.3/conf/storm.yaml</span></div>
uncomment the zookeeper setting and specify the zookeeper server ip

> <span class="s1">########### These MUST be filled in for a storm configuration</span><span class="s1">storm.zookeeper.servers:</span><span class="s1">- "{zookeeper-ip}"</span><span class="s1"># &nbsp; &nbsp; - "server2"</span>
> 
> <span class="s1">storm.zookeeper.port: 2181</span>storm.local.dir: "/mnt/storm"&nbsp;
it is good to add the zookeeper port, &nbsp;the default ports can be overridden.

[default storm configs](https://github.com/apache/storm/blob/master/conf/defaults.yaml)

start the nimbus

> /bin/storm nimbus
<u>**Supervisor setup:**</u>

copy the storm folder to the supervisor server and modify the configuration files.

<div class="p2">
<span class="s1"></span></div>
> apache-storm-0.9.3/conf/storm.yaml&nbsp;
uncomment the zookeeper setting and specify the zookeeper server ip

> <span class="s1">########### These MUST be filled in for a storm configuration</span><span class="s1">storm.zookeeper.servers:</span><span class="s1">- "{zookeeper-ip}"</span><span class="s1">#&nbsp;&nbsp; &nbsp;&nbsp;- "server2"</span>
> 
> <span class="s1">storm.zookeeper.port: 2181</span>storm.local.dir: "/mnt/storm"&nbsp;
> nimbus.host: "{nimbus-ip}"&nbsp;
&nbsp;specify the ip of the nimbus machine.

start the supervisor

> /bin/storm supervisor

**<u>UI setup:</u>**

repeat the same steps as done for the supervisor.

start the ui

> /bin/storm ui
http://ui-server:8080 &nbsp;to view the storm ui

ensure the ports are open between the nimbus, zookeeper, ui and supervisor servers.

**_<u>References:</u>_**

[storm manual](https://storm.apache.org/documentation/Setting-up-a-Storm-cluster.html)

[storm blog](http://www.michael-noll.com/tutorials/running-multi-node-storm-cluster/)

[zookeeper manual](http://zookeeper.apache.org/doc/r3.3.3/zookeeperAdmin.html)

<div>

</div>
</div>
