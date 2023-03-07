---
title: opengrok搭建代码阅读平台
date: 2022-04-07 22:31:08
categories:
- 工具
tags:
---



## install
1. install java11
=====>
- yum search java-11
- yum install java-11xxx
- yum install java-11-jrexxx
=====>
2. dowload opengrop.tar.gz
====>
wget https://github.com/OpenGrok/OpenGrok/releases/$xxx
====>
3. install universal-ctags
====>
- mkdir ~/garbage
- cd garbage
- wget https://github.com/universal-ctags/ctags/archive/p5.9.20201108.0.tar.gz
- tar -zxf p5.xxx
- cd ctagsxxx
- follow $ctagsxxx/doc/autotools.rst instructions
====>
4. install tomcat



## configure
1. mkdir $opengrok/{src,data,dist,etc,log}
2. tar -C $opengrok/dist/ --strip-components=1 -xzf opengrok.tar.gz
3. cp $opengrok/dist/doc/logging.properties $opengrok/etc
```
# logging.properties

handlers= java.util.logging.FileHandler, java.util.logging.ConsoleHandler

java.util.logging.FileHandler.pattern = $opengrok/log/opengrok%g.%u.log
java.util.logging.FileHandler.append = false
java.util.logging.FileHandler.limit = 0
java.util.logging.FileHandler.count = 30
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = org.opengrok.indexer.logger.formatter.SimpleFileLogFormatter

java.util.logging.ConsoleHandler.level = WARNING
java.util.logging.ConsoleHandler.formatter = org.opengrok.indexer.logger.formatter.SimpleFileLogFormatter

org.opengrok.level = FINE

```
4. cp $opengrok/dist/lib/source.war $tomcat/webapps
5. $tomcat/bin/startup.sh
6. cd $tomcat/webapps/source/
7. vim $tomcat/webapps/source/WEB-INF/web.xml
```
# web.xml

<param-name>CONFIGURATION</param-name>
<param-value>$opengrok/etc/configuration.xml</param-value>
```
8. vim deploy.sh
```
# deploy.sh

java \
    -Djava.util.logging.config.file=/root/softs/opengrok/etc/logging.properties \
	-jar /root/softs/opengrok/dist/lib/opengrok.jar \
	-c /root/bin/exctags \
	-s /root/softs/opengrok/src -d /root/softs/opengrok/data -H -P -S -G -v \
	-W /root/softs/opengrok/etc/configuration.xml -U http://127.0.0.1:8888/source
```

## how to run
1. put the source code which you need to study into $opengrok/src directory
2. ./deploy.sh(it will genorate the source code index using ctags. !!!need a while time!!!)
3. in the browser typing `http://192.168.1.100:8888/source` url.
4. enjoy the reading


## reference
1. opengrok install-configure-usage
https://zhuanlan.zhihu.com/p/24369747

2. OpenGrok wiki
https://github.com/oracle/opengrok/wiki/How-to-setup-OpenGrok

