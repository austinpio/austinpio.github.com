---
layout: post
title: "Storm Setup"
description: ""
category: 
tags: []
---
{% include JB/setup %}

<div dir="ltr" style="text-align: left;" trbidi="on">
i am running ubuntu 14.04 (Trusty) on which i am going to install storm-0.9.3 along with zookeeper-3.4.5 using CDH 5<br />
<br />
Adding the CDH5 repository:<br />
<br />
there are many ways to do this as described in the&nbsp;<a href="http://www.cloudera.com/content/cloudera/en/documentation/core/latest/topics/cdh_ig_cdh5_install.html" target="_blank">&nbsp;cloudera installation manual</a><br />
<br />
one way is to use the deb file.<br />
download the repository deb file or use wget for the Trusty version of Ubuntu (14.04)<br />
<a href="http://archive.cloudera.com/cdh5/one-click-install/trusty/amd64/cdh5-repository_1.0_all.deb" target="_blank">trusty link</a><br />
or<br />
<blockquote class="tr_bq">
wget&nbsp;http://archive.cloudera.com/cdh5/one-click-install/trusty/amd64/cdh5-repository_1.0_all.deb</blockquote>
<br />
once the deb file is available, install it using the dpkg command<br />
<blockquote class="tr_bq">
dpkg -i&nbsp;<b>cdh5-repository_1.0_all.deb</b></blockquote>
this would add the CDH5 repository.<br />
Additional step for ubuntu trusty:<br />
ensure you have a file in your apt preferences<br />
<blockquote class="tr_bq">
<div class="p1">
<span class="s1">/etc/apt/preferences.d/cloudera-cdh5.pref&nbsp;</span></div>
</blockquote>
this file contents ensures that you would get the zookeeper packages from the CDH repository. More explanation for this is available on the cloudera manual.<br />
<br />
run a update after this<br />
<blockquote class="tr_bq">
apt-get update</blockquote>
<br />
<b><u>Zookeeper installation:</u></b><br />
<br />
once you have CDH5 added, we can use the apt-get to install zookeeper<br />
<blockquote class="tr_bq">
apt-get install zookeeper zookeeper-server</blockquote>
once installed you can start using the scripts<br />
<blockquote class="tr_bq">
/usr/lib/zookeeper/bin/zkServer.sh start</blockquote>
you change config at /usr/lib/zookeeper/conf/zoo.cfg<br />
<br />
once started you can test it using<br />
<blockquote class="tr_bq">
echo ruok | nc localhost 2181</blockquote>
it should respond "i am ok"<br />
<br />
Note:<br />
1.zookeeper is "fail-fast", you need to use a supervisor program when running zookeeper in production.<br />
2. zookeeper requires regular cleanup of the log files.<br />
3. it is recommended to setup atleast 3 zookeeper servers, for failover.<br />
Refer&nbsp;<a href="http://zookeeper.apache.org/doc/r3.3.3/zookeeperAdmin.html#sc_maintenance" target="_blank">zookeeper manual</a>&nbsp;for more details.<br />
<br />
<u><b>Storm installation:</b></u><br />
<br />
Download the apache storm distribution ,<br />
i am using apache-storm-0.9.3<br />
<br />
<u><b>Nimbus setup:</b></u><br />
<br />
copy the storm folder to the nimbus server and modify the configuration files.<br />
<blockquote class="tr_bq">
<div class="p1">
<span class="s1">apache-storm-0.9.3/conf/storm.yaml</span></div>
</blockquote>
uncomment the zookeeper setting and specify the zookeeper server ip<br />
<blockquote class="tr_bq">
<span class="s1">########### These MUST be filled in for a storm configuration</span><span class="s1">storm.zookeeper.servers:</span><span class="s1">- "{zookeeper-ip}"</span><span class="s1"># &nbsp; &nbsp; - "server2"</span><br />
<span class="s1">storm.zookeeper.port: 2181</span>storm.local.dir: "/mnt/storm"&nbsp;</blockquote>
it is good to add the zookeeper port, &nbsp;the default ports can be overridden.<br />
<br />
<a href="https://github.com/apache/storm/blob/master/conf/defaults.yaml" target="_blank">default storm configs</a><br />
<br />
start the nimbus<br />
<blockquote class="tr_bq">
/bin/storm nimbus</blockquote>
<u><b>Supervisor setup:</b></u><br />
<br />
copy the storm folder to the supervisor server and modify the configuration files.<br />
<div class="p2">
<span class="s1"></span></div>
<blockquote class="tr_bq">
apache-storm-0.9.3/conf/storm.yaml&nbsp;</blockquote>
uncomment the zookeeper setting and specify the zookeeper server ip<br />
<blockquote class="tr_bq">
<span class="s1">########### These MUST be filled in for a storm configuration</span><span class="s1">storm.zookeeper.servers:</span><span class="s1">- "{zookeeper-ip}"</span><span class="s1">#&nbsp;&nbsp; &nbsp;&nbsp;- "server2"</span><br />
<span class="s1">storm.zookeeper.port: 2181</span>storm.local.dir: "/mnt/storm"&nbsp;</blockquote>
<blockquote class="tr_bq">
nimbus.host: "{nimbus-ip}"&nbsp;</blockquote>
&nbsp;specify the ip of the nimbus machine.<br />
<br />
start the supervisor<br />
<blockquote class="tr_bq">
/bin/storm supervisor</blockquote>
<br />
<b><u>UI setup:</u></b><br />
<br />
repeat the same steps as done for the supervisor.<br />
start the ui<br />
<blockquote class="tr_bq">
/bin/storm ui</blockquote>
http://ui-server:8080 &nbsp;to view the storm ui<br />
<br />
ensure the ports are open between the nimbus, zookeeper, ui and supervisor servers.<br />
<br />
<b><i><u>References:</u></i></b><br />
<a href="https://storm.apache.org/documentation/Setting-up-a-Storm-cluster.html" target="_blank">storm manual</a><br />
<a href="http://www.michael-noll.com/tutorials/running-multi-node-storm-cluster/" target="_blank">storm blog</a><br />
<a href="http://zookeeper.apache.org/doc/r3.3.3/zookeeperAdmin.html" target="_blank">zookeeper manual</a><br />
<div>
<br /></div>
</div>

