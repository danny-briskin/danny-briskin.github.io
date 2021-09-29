---
layout: post
title: "Setup AspectJ"
date:   2020-08-01 15:40:30 -0400
categories: java
tags: java lombok AOP AspectJ
---
![](/images/setup_aspectj.jpg)
Danny Briskin, QA Consultants Senior Automation Engineer

# Common part
Download AspectJ installation stable release. Version 1.9.5 is used everywhere in the article. Install it to 
```
{your-path-to-aspectj}
```

# Intellij Idea Ultimate edition
If you have an ultimate version of this popular IDE, the setup is quite simple.

Open settings:
```
File | Settings | Build, Execution, Deployment | Compiler | Java Compiler
```
You need to choose ajc compiler and specify the path to the installed Aspectj library
```
{your-path-to-aspectj}/lib/aspectjtools.jar
```
![Settings](/images/ajc-01.webp)
Then, you can run your project and aspects will be weaved to the code automatically.

Optionally you can install Intellij plugin ([AspectJ weaver](https://plugins.jetbrains.com/plugin/1127-aspectj-weaver/)). It can help you to navigate from your code to corresponding aspects and back. Don’t forget to turn on AspectJ weaving option. Right click on project tree, scroll down popup menu to the end
![Settings](/images/ajc-02.webp)

# Intellij Idea Community edition
## Maven project

The following steps are for Maven project, you can do the same with Gradle if you are familiar with that build system.
AspectjJ setup for a Maven project in Community Idea is a bit more complicated but is still easy to apply.

- Add a dependency to pom.xml 
{% highlight xml %}
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjrt</artifactId>
    <version>1.9.5</version>
</dependency> 
{% endhighlight %}
- Add an aspectj-maven-plugin to your build/plugins section. This plugin will execute ajc compiler prior to java compiler 
{% highlight xml %}
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>aspectj-maven-plugin</artifactId>
    <version>1.11</version>
    <configuration>
        <!--check your Java version! -->
        <complianceLevel>1.9</complianceLevel>
        <source>1.9</source>
        <showWeaveInfo>true</showWeaveInfo>
        <verbose>true</verbose>
        <Xlint>ignore</Xlint>
        <encoding>UTF-8</encoding>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>compile</goal>
                <goal>test-compile</goal>
            </goals>
        </execution>
    </executions>
    <dependencies>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjtools</artifactId>
            <version>1.9.5</version>
        </dependency>
    </dependencies>
</plugin> 
{% endhighlight %}
- Add maven-resources-plugin to your build/plugins section. This plugin is needed to copy resources to target directory 
{% highlight xml %}
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-resources-plugin</artifactId>
    <version>3.1.0</version>
    <executions>
        <execution>
            <id>copy-resources</id>
            <!-- here the phase you need -->
            <phase>validate</phase>
            <goals>
                <goal>copy-resources</goal>
            </goals>
            <configuration>
                <outputDirectory>${basedir}/target/classes</outputDirectory>
                <resources>
                    <resource>
                        <directory>src/main/resources</directory>
                        <filtering>true</filtering>
                    </resource>
                </resources>
            </configuration>
        </execution>
    </executions>
 </plugin>
{% endhighlight %}
- Create a Run Configuration. It may be a copy of your existing configuration.
![Settings](/images/ajc-03.webp)
- Add a Before launch step, choose Run Maven Goal and enter
```
clean validate aspectj:compile
```
clean goal will clean target directory, validate will copy resources from src to target, and aspectj:compile will weave aspects into code
- Run the project using that configuration

## Non-Maven(Gradle) project
This part of setup is for projects that are not using any build system like Maven or Gradle (i.e., build is done by java compiler only)
- Make sure that you have installed AspectJ
- Add AspectJ libraries to the project to be able to use aspects (aspectjrt, aspectjtools, aspectjweaver, all can be found in 
```
{your-path-to-aspectj}/lib directory)
```
![Settings](/images/ajc-09.webp)
- Create a Run Configuration. It may be a copy of your existing configuration
- If you are using Unix-like system, add a Before launch step (you don’t need it in Windows). Then choose “Run External Tool”, click [+] (add new tool)
    - Specify cleaning step (delete output directory)
![Settings](/images/ajc-07.webp)

    - Program
```
/bin/rm
```
    - Arguments
```
-rf $OutputPath$
```
    - Working directory 
```
$ContentRoot$
```
- Add Before launch step (should be after delete output directory step, if you have added it) , choose Run External Tool, click [+] (add new tool)
    - Specify ajc compiler step
![Settings](/images/ajc-08.webp)
    - Program – path to ajc executable (in bin directory of installed AspectJ). It will be *ajc* for Unix-like systems or *ajc.bat* for Windows   
    - Arguments (assuming java version is 8)
```
-cp "$Classpath$" -1.8 -Xlint:ignore -showWeaveInfo -sourceroots $Sourcepath$ -d $OutputPath$
```
    - Working directory 
```
$ContentRoot$
```

- You run configuration in Before launch section will look like (in case of 2 configured steps):
![Settings](/images/ajc-06.webp)

- Save and run the configuration
