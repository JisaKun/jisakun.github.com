---
layout: post
title: Gradle Tutorial
date: 2016-07-26 15:27
thumbnail:
tags: gradle
categories: gradle
published: false

---
原文链接：[Romin Irani's Blog](https://rominirani.com/announcing-gradle-tutorial-series-5fd134223bf8#.2mi16ux8a)

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

别急，我们还有几点概念需要理解。当我们执行gradle命令时，会在当前文件夹内寻找`build.gradle`文件。一旦找到就使用这个文件。在上面的例子中，

