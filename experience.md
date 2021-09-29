---
typora-root-url: ./
typora-copy-images-to: experience_img
---

# 1.未成年校验（jdk7切换jdk8的踩坑之旅）

1.服务器依然使用的是tomcat7，首先需要设置tomcat的bin目录下的2个文件，catalina.sh和setclasspath.sh

catalina.sh最开头加入以下代码

```shell
export JAVA_HOME=/opt/java88
export JRE_HOME=/opt/java88/jre
export CATALINA_HOME=/usr/local/tomcat/andriod/passport #tomcat下项目根目录
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=${JAVA_HOME}/bin:$PATH
```

setclasspath.sh最开头加入以下代码

```shell
export JAVA_HOME=/opt/java88
export JRE_HOME=/opt/java88/jre
```

还有一处需要修改一下

```shell
if [ -z "$JAVA_HOME" -a -z "$JRE_HOME" ]; then
  # Bugzilla 37284 (reviewed).
  if $darwin; then
    if [ -d "/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home" ]; then
      #export JAVA_HOME="/System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home"
      export JAVA_HOME="/opt/java88" # 这里需要修改一下
    fi
```

指定好以后，使用source /etc/profile生效一下（也不知道有没有用，网上有人说需要这样操作一下）

2.由于加密校验代码使用jackson的三个jar包，需要添加，但是添加后tomcat7启动后会报类似这样的错

```java
SEVERE: Unable to process Jar entry [com/sun/glass/ui/Accessible.class] from Jar [jar:file:/opt/java88/jre/lib/ext/jfxrt.jar!/] for annotations
org.apache.tomcat.util.bcel.classfile.ClassFormatException: Invalid byte tag in constant pool: 18
```

此时，需要在tomcat/conf/catalina.properties文件里加上忽略jar包

```properties
tomcat.util.scan.DefaultJarScanner.jarsToSkip=\
bootstrap.jar,commons-daemon.jar,tomcat-juli.jar,\
annotations-api.jar,el-api.jar,jsp-api.jar,servlet-api.jar,\
catalina.jar,catalina-ant.jar,catalina-ha.jar,catalina-tribes.jar,\
jasper.jar,jasper-el.jar,ecj-*.jar,\
tomcat-api.jar,tomcat-util.jar,tomcat-coyote.jar,tomcat-dbcp.jar,\
tomcat-jni.jar,tomcat-spdy.jar,\
tomcat-i18n-en.jar,tomcat-i18n-es.jar,tomcat-i18n-fr.jar,tomcat-i18n-ja.jar,\
tomcat-juli-adapters.jar,catalina-jmx-remote.jar,catalina-ws.jar,\
tomcat-jdbc.jar,\
commons-beanutils*.jar,commons-codec*.jar,commons-collections*.jar,\
commons-dbcp*.jar,commons-digester*.jar,commons-fileupload*.jar,\
commons-httpclient*.jar,commons-io*.jar,commons-lang*.jar,commons-logging*.jar,\
commons-math*.jar,commons-pool*.jar,\
jstl.jar,\
geronimo-spec-jaxrpc*.jar,wsdl4j*.jar,\
ant.jar,ant-junit*.jar,aspectj*.jar,jmx.jar,h2*.jar,hibernate*.jar,httpclient*.jar,\
jmx-tools.jar,jta*.jar,log4j*.jar,mail*.jar,slf4j*.jar,\
xercesImpl.jar,xmlParserAPIs.jar,xml-apis.jar,\
dnsns.jar,ldapsec.jar,localedata.jar,sunjce_provider.jar,sunmscapi.jar,\
sunpkcs11.jar,jhall.jar,tools.jar,\
sunec.jar,zipfs.jar,\
apple_provider.jar,AppleScriptEngine.jar,CoreAudio.jar,dns_sd.jar,\
j3daudio.jar,j3dcore.jar,j3dutils.jar,jai_core.jar,jai_codec.jar,\
mlibwrapper_jai.jar,MRJToolkit.jar,vecmath.jar,\
junit.jar,junit-*.jar,ant-launcher.jar,jackson-annotations-2.12.1.jar,jackson-core-2.12.1.jar,jackson-databind-2.12.1.jar,\
nashorn.jar
```

3.tomcat7启动居然会报一个这种莫名奇妙的异常

```java
org.eclipse.jdt.internal.compiler.classfmt.ClassFormatException
    at org.eclipse.jdt.internal.compiler.classfmt.ClassFileReader.<init>(ClassFileReader.java:372)
```

经过网上查找资源后，从网上下载ecj-4.2.2.jar，重命名后，然后替换tomcat的lib里的ecj-3.7.2.jar包。

4.如果需要tomcat一启动就加载某个类，执行static代码块，需要在项目的web.xml里配置成监听器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">
<web-app>
	<display-name>passport</display-name>
    <listener>
        <listener-class>cn.stickgame.thread.IdCheckStatusSchedule</listener-class>
    </listener>
	<servlet>
		<servlet-name>action</servlet-name>
		<servlet-class>cn.stickgame.net.CommServlet</servlet-class>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>action</servlet-name>
		<url-pattern>/GameComm</url-pattern>
	</servlet-mapping>
</web-app>
```

5.最后就是加密问题了，未成年校验的加密很坑，AES-128/GCM + BASE64，jdk7居然不支持，折腾了几天，头大，最后只能换成jdk8了，前前后后接这个SDK差不多花了快一周了，记录以下，供后来的人参考。





