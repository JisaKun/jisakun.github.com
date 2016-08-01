---
layout: post
title: Gradle Tutorial
date: 2016-07-26 15:27
thumbnail:
tags: gradle
categories: gradle
published: true

---
原文链接：[Romin Irani's Blog](https://rominirani.com/announcing-gradle-tutorial-series-5fd134223bf8#.2mi16ux8a)（不知道为什么，需要梯子）

原文共分为10篇，后几篇涉及到Android内容，我用不到，就只记录了前四篇。

[TOC]

### Part 1:安装+设置

#### 环境设置

Gradle目录下 **\bin** 文件夹包含有gradle脚本文件（Unix和Windows都有），你需要使用它们来运行gradle命令行。

你要使用它们，就要执行以下步骤：

1. 创建 **系统** 环境变量 `GRADLE_HOME` ，并将它指向Gradle安装目录。
2. 将`%GRADLE_HOME%\bin`添加至**PATH**环境变量下。这可以让你在任何目录下运行gradle命令行。

#### 验证安装

在命令提示符或终端下运营以下命令验证安装：

``` shell
gradle -v
```

这会显示Gradle版本和其他一些信息，如下：

```

------------------------------------------------------------
Gradle 2.13
------------------------------------------------------------

Build time:   2016-04-25 04:10:10 UTC
Build number: none
Revision:     3b427b1481e46232107303c90be7b05079b05b1c

Groovy:       2.4.4
Ant:          Apache Ant(TM) version 1.9.6 compiled on June 29 2015
JVM:          1.8.0_92 (Oracle Corporation 25.92-b14)
OS:           Windows 7 6.1 amd64

```

如果你没有看到类似以上的信息，检查你的安装步骤确保`PATH`路径包含`%GRADLE_HOME%\bin`目录。

#### 关于GROOVY

任何Gradle的讨论离开了[Groovy](http://www.groovy-lang.org/)都是不完整的。Groovy是一种运行在JVM上的非常流行非常强大的语言。

Gradle最受欢迎的特点有：

* 更精简
* 更灵活
* 按你所需的方式配置事务

Groovy在上述特性中发挥了极大的作用。Ant和Maven中使用的XML，而Groovy是一门更高级的语言，他直观有效的语法可以使文件变得不像XML那么冗长，而且它提供修改或者制定标签的强大的可编程能力，也支持闭包等最新编程语言才具有的高级特性，并且可以实时编译！

**那么问题来了：“我需要因此去学习Groovy吗？”** 我可以试着回答你：不需要！Unless you really want to stray away from convention and do things your own way, then you need to know Groovy because that is where the flexibility of Gradle comes in. 对于大多数情况，你只需要使用各种build.gradle模板，虽然他包含了Groovy DSL，你也不需要关心它。

尽管理解Gradle不需要你学习Groovy，但是如果你想了解Groovy的全部威力，你应该学习它。

**Groovy在你安装Gradle的时候就已经默认安装好了一个内部版本，所以你不需要单独下载安装它。**

#### 基本Gradle命令

目前为止我们还没有写任何源码或者是build文件，但我们马上就会做的。现在先试一下下面的命令：

``` shell
gradle -q help
```

这个命令打印了Gradle基本的帮助信息。 `-q`参数表示控制台输入时采用静默模式（quiet mode）。如果你只想看一些精简的信息，这个参数非常有用。

``` shell
gradle -q tasks
```

这会列出你当前可以运行的任务。试试看吧？

``` shell
gradle properties
```

打印为你预设的一些属性。大部分属性可以在你的build文件中修改。这让你感到Gradle在执行你的任务之前为你的项目配置会做一大堆非常繁重的工作。

我们目前还没有去编译Java项目，那是为下一部分准备的内容。我们先要理解Groovy如何给Gradle提供了一整套强大的编程能力。

开始之前，我们先讨论一下`build.gradle`文件，这个文件被用来作为build文件。它包含了你需要Gradle执行的所有命令。整个教程中我们本质上都是通过`build.gradle`文件来创建或使用各式各样的插件或任务，来完成编译、构建、测试、运行我们Java应用的目的。

现在我们执行下面的步骤：

在你选择的文件夹里，比如 `example 1` ，创建`build.gradle`文件。

在`build.gradle`文件中写入以下内容：

``` groovy
task compileTask << {
  System.out.println "compiling..."
}
```

现在，在CMD中进入`build.gradle`所在的文件夹，执行以下命令：

``` shell
gradle -q tasks
```

你会注意到在标准任务之外有我们创建的 **compileTask**：

```
Other tasks
-----------
compileTask
```

我们的编译文件（build.gradle）就是一些列的任务（task）的组合，这里就带出了第一个的概念**任务（task）** ，任务指定了gradle编译系统要为我们执行的代码。当前我们所指定的项目被称作**compileTask**，就如你所看到的，我们使用了Groovy代码来定义这个任务，这个任务所要做的事情，就是简单的执行一个 `System.out.println`。要记住Groovy是高级的JVM语言。

我们要如何运行 **compileTask** 命令呢？

别急，我们还有几点概念需要理解。当我们执行gradle命令时，会在当前文件夹内寻找`build.gradle`文件。一旦找到就使用这个文件。在上面的例子中，Gradle发现了文件中所列的任务并添加至执行列表。

所以我们只执行`gradle`命令，没有其他参数，也不指定任何任务：

``` shell
gradle
```

会输出以下内容：

``` shell
:help
Welcome to Gradle 2.2.1.
To run a build, run gradle <task> ...
To see a list of available tasks, run gradle tasks
To see a list of command-line options, run gradle --help
BUILD SUCCESSFUL
Total time: 2.39 secs
```

输出信息提示你需要在运行命令时指定任务名，如`gradle <task>`。我们再试一次下面的命令：

```shell
gradle -q compileTask
```

输出如下：

``` shell
compiling
```

接下来为 build.gradle 再添加一个任务：

``` groovy
task compileTask << {
 System.out.println "compiling..." 
}
task buildTask << {
 System.out.println "building..."
}
```

现在执行`gradle -q tasks`，你会发现两个任务都出现在**Other Task**列表中：

``` shell
Other tasks
-----------
buildTask
compileTask
```

现在你可以通过`gradle compileTask`或`gradle buildTask`等命令运行这些任务了。

我们还可以指定一个默认任务，这样当我们不指定任务名的时候，默认任务就会执行：

``` groovy
defaultTasks 'buildTask'

task compileTask << {
  System.out.println "compiling..." 
}
task buildTask << {
  System.out.println "building..."
}
```

现在执行 `gradle -q` 就会打印 **building...** 字符串。

最后我们需要知道任务之间存在依赖关系。如果我们使 **buildTask** 依赖于 **compileTask** ，那么每次 **buildTask** 执行前，都会运行 **compileTask**。

``` groovy
defaultTasks 'buildTask'
task compileTask << {
  System.out.println "compiling..." 
}
task buildTask (dependsOn:compileTask) << {
  System.out.println "building..."
}
```

现在再次运行 `gradle -q`，就会一次执行两个任务。



### Part 2:Java 工程

#### Java 插件

让我们先从所有 Java 开发人员最关心的开始：[**Java plugin**](https://docs.gradle.org/current/userguide/java_plugin.html)。这个插件有如下作用：

* 编译（compilation）
* 测试（testing）
* 打包（bundling）

这些全都作用于 Java 工程，尤其是打包通常意味着打包成一个 Jar 文件。

**如果你要使用插件，你就要将它添加到 build.gradle 中：**

``` groovy
apply plugin: <plugin-name>
```

所以要使用 Java 插件，就要这么写：

``` groovy
apply plugin: "java"
```

让我们试着理解代码背后发生了什么。

先创建一个文件夹，比如 example2。在文件夹中创建一个标准 **build.gradle** 文件，填入 `apply plugin: "java"`。

插件会加入一系列处理 Java 工程的任务。运行 `gradle tasks`来了解我们新增加了什么任务。结果如下（只列出了 Build Tasks）：

```
Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend
on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles classes 'main'.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles classes 'test'.
```

这些任务也会彼此依赖，请参照文档 [dependency graph](http://www.gradle.org/docs/current/userguide/java_plugin.html) 。

#### 用 Gradle Java Plugin 构建基本 Java 工程

参照官方文档，Java 插件会按照下面的约定寻找 Java 代码和 Java 测试类：

| Directory               | Meaning                              |
| ----------------------- | ------------------------------------ |
| src/main/java           | Production Java source               |
| src/main/resources      | Production resources                 |
| src/test/java           | Test Java source                     |
| src/test/resources      | Test resources                       |
| src/sourceSet/java      | Java source for the given source set |
| src/sourceSet/resources | Resources for the given source set   |

![xx](https://cdn-images-1.medium.com/max/800/0*EpiEIJtyGBXx8ojU.png)

以上路径不是强制的。如果你要变更到不同的文件夹，只需要在 Java 插件的文档中查阅一下如何替换 SourceSet 路径。

什么是 SourceSet ？

> It identifies the source i.e. grouping for your source files that need to be compiled and built together, some sort of a logical grouping or component. As per the official documentation, the Java plugin defines two standard source sets, called main and test. The main source set contains your production source code, which is compiled and assembled into a JAR file. The test source set contains your unit test source code, which is compiled and executed using JUnit or TestNG.

按照 Java 插件的约定，我们要把所有的 Java 文件放在 **src/main/java** 文件夹内，把所有的测试文件放到 **src/main/test** 文件夹内。

一个工程文件目录结构的示例：

![x](https://cdn-images-1.medium.com/max/800/0*rDp-L0zd2k99cWDT.png)

在包下的 **quoteapp** 文件夹下有一个 **Quote.java** 文件：

``` java
package com.mindstorm.quoteapp;
public class Quote {
 private Long id;
 private String who;
 private String what;
 
 public void setId(Long id) {
 this.id = id;
 }
 public void setWho(String who) {
 this.who = who;
 }
 public void setWhat(String what) {
 this.what = what;
 }
 public Long getId() {
 return id;
 }
 public String getWho() {
 return who;
 }
 public String getWhat() {
 return what;
 }
}
```

现在我们回到项目的根目录，此时根目录下的 build.gradle 文件只有一行代码：`apply plugin:"java"` 。

然后执行下面的命令，注意我们去掉了`-q`参数以便观察完整的控制台输出：

``` shell
gradle assemble
```

这条命令编译并（compiling）将你的 Java 文件打包（building）成 JAR 文件。多项任务被执行，表明 `assemble` 任务依赖其他多项任务。命令的执行结果如下：

``` shell
E:\gradle-projects\example2>gradle assemble
:compileJava
:processResources UP-TO-DATE
:classes
:jar
:assemble
```

你也可以尝试在代码内引入编译错误，看看 `assemble` 命令会输出啥。

这条命令的结果是啥？如果一切顺利，会在你的工程根目录下生成一个 **build** 文件夹。文件夹内有 **libs** 文件夹，其内又包含 **example2.jar** 。这个文件名取决于你的文件夹名。

如果你想清空 build 目录再次构建，可以调用 `gradle clean` 命令后运行 `gradle assemble` 。

以下命令也可以完成检查（check）和装配（assemble）任务：

``` shell
gradle -q build
```

现在继续完善 build.gradle ，给 JAR 文件提供版本号和不同的名字：

``` groovy
apply plugin: 'java'
archivesBaseName = "quote"
version = '1.0-FINAL'
```

现在执行 `gradle assemble` 后生成的 JAR 文件会命名为 \<name>-\<version>.jar ，比如 quote-1.0-FINAL.jar 。

**注意：** You might be wondering from where do I produce stuff like archivesBaseName, etc. The only answer I can give is that the documentation is vast and you will have to navigate yourself through it. For e.g. go to the official page of the Java plugin, search for archivesBaseName and you will understand what is going on. My only advice is to keep the documentation handy, some property or the other will be there, just think logically and you will find the answer in the documentation.

#### 依赖初探

不依赖第三方库的 Java 代码是乏善可陈的。回顾一下上面的 Quote.java ，只是一个简单的 Java bean ，如果要添加一个 **toString()** 方法来增强功能，就要用到第三方库：Apache Commons JAR 。

``` java
package com.mindstorm.quoteapp;
import org.apache.commons.lang3.builder.ToStringBuilder;

public class Quote {
 private Long id;
 private String who;
 private String what;
 
 public void setId(Long id) {
 this.id = id;
 }
 public void setWho(String who) {
 this.who = who;
 }
 public void setWhat(String what) {
 this.what = what;
 }
 public Long getId() {
 return id;
 }
 public String getWho() {
 return who;
 }
 public String getWhat() {
 return what;
 }
public String toString() {
 return ToStringBuilder.reflectionToString(this);
 }
}
```

这时如果执行`gradle assemble`命令，你就会看到标准编译错误伴随着 BUILD FAILED 信息。

解决方法就是修改 build.gradle 文件，让它知道两件事：

* 编译代码需要哪些 JAR 包？
* 去哪里找这些 JAR 包？

去 Gradle 仓库！仓库就是 Gradle 用来定位 JAR 包的地方。Gradle 支持当下广受欢迎的公共仓库，就像 Maven Central 和 Ivy ，甚至是你的本地仓库。如果你之前用过 Maven ，这会听着很熟悉。

你需要在 build.gradle 里追加以下元素来配置一个远程仓库：

``` groovy
respositories {
  mavenCentral()
}
```

这让 Gradle 在 Maven 中央库寻找你指定的任何依赖（JAR 包）。

要牢牢记住这点：任何被发布到 Maven 中央库的文件都有以下三个坐标：

* Group Id （group）
* Artifact Id （name）
* Version （version）

比如你要在 Maven 中央库定位 Apache Commons 3.3.2 JAR 这个包，需要以下代码：

``` xml
<groupId>org.apache.commons</groupId>
<artifactId>commons-lang3</artifactId>
<version>3.3.2</version>
```

依赖暂时先谈到这里，我们先要理解一个概念：**Gradle 的配置（Configurations ）** 。

#### Java 插件提供的配置

目前为止，我们需要向 build.gradle 提供额外的信息，包括编译所需的 JAR 包和在哪里找到这些包。通过 `mavenCentral()` 库我们解决了“去哪里找包”的问题，还需要在 build.gradle 中指定需要哪些文件，比如上面提到的 Apache Commons Lang library 。

为此，Gradle 提出了 **配置（Configuration）** 的概念。一条配置是一系列依赖的集合。你可以用来定义工程的外部依赖。







To be continued ...

