# maven

Maven 翻译为"专家"、"内行"，是 Apache 下的一个纯 Java 开发的开源项目。基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。

Maven 是一个项目管理工具，可以对 Java 项目进行构建、依赖管理。

Maven 也可被用于构建和管理各种项目，例如 C#，Ruby，Scala 和其他语言编写的项目。

## 特点

* 项目设置遵循统一的规则。
* 任意工程中共享。
* 依赖管理包括自动更新。
* 一个庞大且不断增长的库。
* 可扩展，能够轻松编写 Java 或脚本语言的插件。
* 只需很少或不需要额外配置即可即时访问新功能。
* 基于模型的构建 − Maven能够将任意数量的项目构建到预定义的输出类型中，如 JAR，WAR 或基于项目元数据的分发，而不需要在大多数情况下执行任何脚本。
* 项目信息的一致性站点 − 使用与构建过程相同的元数据，Maven 能够生成一个网站或PDF，包括您要添加的任何文档，并添加到关于项目开发状态的标准报告中。
* 发布管理和发布单独的输出 − Maven 将不需要额外的配置，就可以与源代码管理系统（如 Subversion 或 Git）集成，并可以基于某个标签管理项目的发布。它也可以将其发布到分发位置供其他项目使用。Maven 能够发布单独的输出，如 JAR，包含其他依赖和文档的归档，或者作为源代码发布。
* 向后兼容性 − 您可以很轻松的从旧版本 Maven 的多个模块移植到 Maven 3 中。
* 子项目使用父项目依赖时，正常情况子项目应该继承父项目依赖，无需使用版本号，
* 并行构建 − 编译的速度能普遍提高20 - 50 %。
* 更好的错误报告 − Maven 改进了错误报告，它为您提供了 Maven wiki 页面的链接，您可以点击链接查看错误的完整描述。

## 约定目录和作用
目录|目的
-|-
${basedir}|存放pom.xml和所有的子目录
${basedir}/src/main/java|项目的java源代码
${basedir}/src/main/resources|项目的资源，比如说property文件，springmvc.xml
${basedir}/src/test/java|项目的测试类，比如说Junit代码
${basedir}/src/test/resources|测试用的资源
${basedir}/src/main/webapp/WEB-INF |web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面
${basedir}/target|打包输出目录
${basedir}/target/classes|编译输出目录
${basedir}/target/test-classes|测试编译输出目录
Test.java|Maven只会自动运行符合该命名规则的测试类
~/.m2/repository|Maven默认的本地仓库目录位置

# POM
POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。

执行任务或目标时，Maven 会在当前目录中查找 POM。它读取 POM，获取所需的配置信息，然后执行目标。

POM 中可以指定以下配置：
* 项目依赖
* 插件
* 执行目标
* 项目构建 profile
* 项目版本
* 项目开发者列表
* 相关邮件列表信息

在创建 POM 之前，我们首先需要描述项目组 (groupId), 项目的唯一ID。
```
<project xmlns = "http://maven.apache.org/POM/4.0.0"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0
    http://maven.apache.org/xsd/maven-4.0.0.xsd">
 
    <!-- 模型版本 -->
    <modelVersion>4.0.0</modelVersion>
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group -->
    <groupId>com.companyname.project-group</groupId>
 
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project</artifactId>
 
    <!-- 版本号 -->
    <version>1.0</version>
</project>
```
所有 POM 文件都需要 project 元素和三个必需字段：groupId，artifactId，version。

节点|描述
-|-
project|工程的根标签。
modelVersion|模型版本需要设置为 4.0。
groupId|这是工程组的标识。它在一个组织或者项目中通常是唯一的。例如，一个银行组织 com.companyname.project-group 拥有所有的和银行相关的项目。
artifactId|这是工程的标识。它通常是工程的名称。例如，消费者银行。groupId 和 artifactId 一起定义了 artifact 在仓库中的位置。
version|这是工程的版本号。在 artifact 的仓库中，它用来区分不同的版本。


# Maven 构建生命周期

阶段|处理|描述
:-:|:-:|-
validate|验证项目|验证项目是否正确且所有必须信息是可用的
compile|执行编译|源代码编译在此阶段完成
Test|测试|使用适当的单元测试框架（例如JUnit）运行测试。
package|打包|创建JAR/WAR包如在 pom.xml 中定义提及的包
verify|检查|对集成测试的结果进行检查，以保证质量达标
install|安装|安装打包的项目到本地仓库，以供其他项目使用
deploy|部署|拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程

为了完成 default 生命周期，这些阶段（包括其他未在上面罗列的生命周期阶段）将被按顺序地执行。

Maven 有以下三个标准的生命周期：

* clean：项目清理的处理
* default(或 build)：项目部署的处理
* site：项目站点文档创建的处理

例如下面的命令： clean 和 pakage 是构建阶段， dependency:copy-dependencies 是目标

` mvn clean dependency:copy-dependencies package `

这里的 clean 阶段将会被首先执行，然后 dependency:copy-dependencies 目标会被执行，最终 package 阶段被执行。

# Maven 仓库

在 Maven 的术语中，仓库是一个位置（place）。Maven 仓库是项目中依赖的第三方库，这个库所在的位置叫做仓库。在 Maven 中，任何一个依赖、插件或者项目构建的输出，都可以称之为构件。

Maven 仓库能帮助我们管理构件（主要是JAR），它就是放置所有JAR文件（WAR，ZIP，POM等等）的地方。

Maven 仓库有三种类型：
* 本地（local）
* 中央（central）
* 远程（remote）

## 本地仓库
Maven 的本地仓库，在安装 Maven 后并不会创建，它是在第一次执行 maven 命令的时候才被创建。

运行 Maven 的时候，Maven 所需要的任何构件都是直接从本地仓库获取的。如果本地仓库没有，它会首先尝试从远程仓库下载构件至本地仓库，然后再使用本地仓库的构件。

默认情况下，不管Linux还是 Windows，每个用户在自己的用户目录下都有一个路径名为 .m2/respository/ 的仓库目录。

Maven 本地仓库默认被创建在 %USER_HOME% 目录下。要修改默认位置，在 %M2_HOME%\conf 目录中的 Maven 的 settings.xml 文件中定义另一个路径。

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 
   http://maven.apache.org/xsd/settings-1.0.0.xsd">
      <localRepository>C:/MyLocalRepository</localRepository>
</settings>
```
当你运行 Maven 命令，Maven 将下载依赖的文件到你指定的路径中。

## 中央仓库
Maven 中央仓库是由 Maven 社区提供的仓库，其中包含了大量常用的库。

中央仓库包含了绝大多数流行的开源Java构件，以及源码、作者信息、SCM、信息、许可证信息等。一般来说，简单的Java项目依赖的构件都可以在这里下载到。

* 这个仓库由 Maven 社区管理。
* 不需要配置。
* 需要通过网络才能访问。

要浏览中央仓库的内容，maven 社区提供了一个 URL：http://search.maven.org/#browse。使用这个仓库，开发人员可以搜索所有可以获取的代码库。

## 远程仓库
如果 Maven 在中央仓库中也找不到依赖的文件，它会停止构建过程并输出错误信息到控制台。为避免这种情况，Maven 提供了远程仓库的概念，它是开发人员自己定制仓库，包含了所需要的代码库或者其他工程中用到的 jar 文件。

## Maven 依赖搜索顺序
执行 Maven 构建命令时，Maven 开始按照以下顺序查找依赖的库：

1. 在本地仓库中搜索，如果找不到，执行步骤 2，如果找到了则执行其他操作。
2. 在中央仓库中搜索，如果找不到，并且有一个或多个远程仓库已经设置，则执行步骤 4，如果找到了则下载到本地仓库中以备将来引用。
3. 如果远程仓库没有被设置，Maven 将简单的停滞处理并抛出错误（无法找到依赖的文件）。
4. 在一个或多个远程仓库中搜索依赖的文件，如果找到则下载到本地仓库以备将来引用，否则 Maven 将停止处理并抛出错误（无法找到依赖的文件）。

## Maven 阿里云(Aliyun)仓库
Maven 仓库默认在国外， 国内使用难免很慢，我们可以更换为阿里云的仓库。

第一步:修改 maven 根目录下的 conf 文件夹中的 setting.xml 文件，在 mirrors 节点上，添加内容如下：
```
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
</mirrors>
```

第二步: pom.xml文件里添加：
```
<repositories>  
        <repository>  
            <id>alimaven</id>  
            <name>aliyun maven</name>  
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
            <releases>  
                <enabled>true</enabled>  
            </releases>  
            <snapshots>  
                <enabled>false</enabled>  
            </snapshots>  
        </repository>  
</repositories>
```

# Maven 插件

Maven 生命周期的每一个阶段的具体实现都是由 Maven 插件实现的。

Maven 实际上是一个依赖插件执行的框架，每个任务实际上是由插件完成。Maven 插件通常被用来：

* 创建 jar 文件
* 创建 war 文件
* 编译代码文件
* 代码单元测试
* 创建工程文档
* 创建工程报告

插件通常提供了一个目标的集合，并且可以使用下面的语法执行：

`<code>mvn [plugin-name]:[goal-name]</code>`

例如，一个 Java 工程可以使用 maven-compiler-plugin 的 compile-goal 编译，使用以下命令：

`<code>mvn compiler:compile</code>`

下面是一些常用插件的列表：

插件|描述
-|-
clean|构建之后清理目标文件。删除目标目录。
compiler|编译 Java 源文件。
surefile|运行 JUnit 单元测试。创建测试报告。
jar|从当前工程中构建 JAR 文件。
war|从当前工程中构建 WAR 文件。
javadoc|为工程生成 Javadoc。
antrun|从构建过程的任意一个阶段中运行一个 ant 任务的集合。

示例
```
<build>
<plugins>
   <plugin>
   <groupId>org.apache.maven.plugins</groupId>
   <artifactId>maven-antrun-plugin</artifactId>
   <version>1.1</version>
   <executions>
      <execution>
         <id>id.clean</id>
         <phase>clean</phase>
         <goals>
            <goal>run</goal>
         </goals>
         <configuration>
            <tasks>
               <echo>clean phase</echo>
            </tasks>
         </configuration>
      </execution>     
   </executions>
   </plugin>
</plugins>
</build>
```
* 插件是在 pom.xml 中使用 plugins 元素定义的。
* 每个插件可以有多个目标。
* 你可以定义阶段，插件会使用它的 phase 元素开始处理。我们已经使用了 clean 阶段。
* 你可以通过绑定到插件的目标的方式来配置要执行的任务。我们已经绑定了 echo 任务到 maven-antrun-plugin 的 run 目标。

# Maven 引入外部依赖

要添加依赖项，我们一般是先在 src 文件夹下添加 lib 文件夹，然后将你工程需要的 jar 文件复制到 lib 文件夹下。

```
<dependencies>
    <!-- 在这里添加你的依赖 -->
    <dependency>
        <groupId>ldapjdk</groupId>  <!-- 库名称，也可以自定义 -->
        <artifactId>ldapjdk</artifactId>    <!--库名称，也可以自定义-->
        <version>1.0</version> <!--版本号-->
        <scope>system</scope> <!--作用域-->
        <systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath> <!--项目根目录下的lib文件夹下-->
    </dependency> 
</dependencies>
```

# Maven archetype
Maven 使用 archetype(原型) 来创建自定义的项目结构，形成 Maven 项目模板。archetype 也就是原型，是一个 Maven 插件，准确说是一个项目模板，它的任务是根据模板创建一个项目结构。我们将使用 quickstart 原型插件创建一个简单的 java 应用程序。

# Maven 项目文档
```
<project>
  ...
<build>
<pluginManagement>
    <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.3</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>2.7</version>
        </plugin>
    </plugins>
    </pluginManagement>
</build>
 ...
</project>
```

# Maven SNAPSHOT
一个大型的软件应用通常包含多个模块，并且通常的场景是多个团队开发同一应用的不同模块。举个例子，设想一个团队开发应用的前端，项目为 app-ui(app-ui.jar:1.0)，而另一个团队开发应用的后台，使用的项目是 data-service(data-service.jar:1.0)。

现在可能出现的情况是开发 data-service 的团队正在进行快节奏的 bug 修复或者项目改进，并且他们几乎每隔一天就要发布库到远程仓库。 现在如果 data-service 团队每隔一天上传一个新版本，那么将会出现下面的问题：

data-service 团队每次发布更新的代码时都要告知 app-ui 团队。
app-ui 团队需要经常地更新他们 pom.xml 文件到最新版本。

为了解决这种情况，快照的概念派上了用场。快照是一种特殊的版本，指定了某个当前的开发进度的副本。不同于常规的版本，Maven 每次构建都会在远程仓库中检查新的快照。 现在 data-service 团队会每次发布更新代码的快照到仓库中，比如说 data-service:1.0-SNAPSHOT 来替代旧的快照 jar 包。

项目快照 vs 版本
对于版本，如果 Maven 以前下载过指定的版本文件，比如说 data-service:1.0，Maven 将不会再从仓库下载新的可用的 1.0 文件。若要下载更新的代码，data-service 的版本需要升到1.1。

快照的情况下，每次 app-ui 团队构建他们的项目时，Maven 将自动获取最新的快照(data-service:1.0-SNAPSHOT)。

示例
app-ui 项目的 pom.xml 文件
app-ui 项目使用的是 data-service 项目的 1.0 快照。
```
<project xmlns="http://maven.apache.org/POM/4.0.0" 
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>app-ui</groupId>
   <artifactId>app-ui</artifactId>
   <version>1.0</version>
   <packaging>jar</packaging>
   <name>health</name>
   <url>http://maven.apache.org</url>
   <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
   </properties>
   <dependencies>
      <dependency>
      <groupId>data-service</groupId>
         <artifactId>data-service</artifactId>
         <version>1.0-SNAPSHOT</version>
         <scope>test</scope>
      </dependency>
   </dependencies>
</project>
```
data-service 项目的 pom.xml 文件
data-service 项目为每次小的改动发布 1.0 快照。
```
<project xmlns="http://maven.apache.org/POM/4.0.0" 
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>data-service</groupId>
   <artifactId>data-service</artifactId>
   <version>1.0-SNAPSHOT</version>
   <packaging>jar</packaging>
   <name>health</name>
   <url>http://maven.apache.org</url>
   <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
   </properties>
</project>
```

# Maven Web 应用

文件夹结构

文件夹结构 | 描述
-|-
trucks | 包含 src 文件夹和 pom.xml 文件
src/main/webapp | 包含 index.jsp 文件和 WEB-INF 文件夹
src/main/webapp/WEB-INF | 包含 web.xml 文件
src/main/resources | 包含图片、properties资源文件

## 构建 Web 应用
进入项目目录执行 mvn 命令：
` mvn clean package `
执行后在 target 文件夹，找到 xxx.war 文件，并复制到 web 服务器的 web 应用目录，然后重启 web 服务器即可。
