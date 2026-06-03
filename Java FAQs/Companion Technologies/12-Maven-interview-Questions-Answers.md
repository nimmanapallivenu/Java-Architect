# 12 Maven interview Questions & Answers

## Table of Contents

- [Q1: What is the dif ference between snapshot versions and release versions?](#q1)
- [Q2: Why do you need a SNAPSHOT version?](#q2)
- [Q3: Do you have control over the snapshot versions?](#q3)
- [Q4: What is the dif ference between a reactor pom and a parent pom?](#q4)
- [Q5: How do you handle multi-module projects in Maven?](#q5)
- [Q6: What are the dif ferent aspects managed by Maven in the SDLC?](#q6)
- [Q7: What is the dif ference between “DependencyManagement” and “Dependencies” as sho](#q7)
- [Q8: What format does a maven repository use to uniquely identify an artifact in its ](#q8)
- [Q9: What is an “attached artifact” and why would you need a “classifier”?](#q9)
- [Q10: How do you inherit dependencies or plugins in Maven? How are sub modules aggrega](#q10)
- [Q11: What are the dif ferent dependency scopes have you used?](#q11)
- [Q12: What is the purpose of Maven assemply plugin?](#q12)

---

## Q1: What is the dif ference between snapshot versions and release versions?

**Answer:**

The term “ SNAPSHOT ” means the build is a snapshot of your code at a given time, which means downloading 1.0-SNAPSHOT today might give a
different file than downloading it tomorrow or day after . When you are ready to release your project, you will change 1.0-SNAPSHOT to 1.0 in the pom.xml
file. The 1.0 is the release version. After release, any new development work will stat using 1.1-SNAPSHOT and so on.
“SNAPSHOT” also means unstable version. Unlike regular versions, Maven checks for a new SNAPSHOT version in a remote repository at least once a day by
default or if you use -U flag to every time you build. A team working on a cash-service module will release SNAPSHOT of its updated code every day to
repository – let’ s say cash-service:1.0-SNAPSHOT replacing an older cash-service:1.0-SNAPSHOT jar . In case of released versions, if Maven once downloaded
the version say cash-service:1.0, it will never try to download a newer 1.0 available in repository . In order to download the updated code, the cash-service
version needs to be upgraded to 1.1.
When you release artifacts to nexus, you can have a snapshotRepository and a repository for the releases.

---

## Q2: Why do you need a SNAPSHOT version?

**Answer:**

If a team working on a cash-service jar module keeps uploading a new version every other day , the other teams that depend on this cash-service module
1. Need to be regularly notified so that they can update their pom.xml file to use the newer version.
2. In lar ge projects, it is not easy to keep communicating these version changes. Using the older versions can cause unforeseen build issues<distributionManagement >
 <snapshotRepository >
 <id>mycompany -snapshots </id>
 <name >mycompany Snapshots </name >
 <url>http://myhost:8080/nexus/content/repositories/mycompany-snapshots</url>
 <uniqueV ersion >true</uniqueV ersion >
 </snapshotRepository >
 <repository >
 <id>mycompany -releases </id>
 <name >mycompany Releases </name >
 <url>http://myhost:8080/nexus/content/repositories/mycompany-releases</url>
 </repository >
</distributionManagement >

If you have a “SNAPSHOT” version that regularly gets overridden by the team that is making the regular changes and the team that depends on the cash-service
module will also get their local repositories updated and use the latest code. This will alleviate the above 2 problems.

---

## Q3: Do you have control over the snapshot versions?

**Answer:**

Yes. For example, a cash-service-1.0.jar library is considered as a stable version, and if Maven finds it in the local repository , it will use this one for the
current build.
Now , if you need a cash-service-1.0-SNAPSHOT .jar library , Maven will know that this version is not stable and is subject to changes since it a SNAPSHOT . So,
Maven will try to find a newer version in the remote repositories, even if a version of this library is found on the local repository . However , this check is made
only once per day by default, but you can modify this update policy .
The other possible updatePolicy values are daily (i.e. default), interval:30 (every 30 minutes), and never .
– always : Maven will check for a newer SNAPSHOT version on every build.
– daily : Once a day (this is the default unless you use -U flag with mvn clean package -U)
– interval:XXX : an interval in minutes (XXX)
– never : Maven will never try to retrieve another version from the remote repository . It will do that only if it doesn’ t exist locally . The SNAPSHOT version will
be handled as the stable version.
You can also turn of f this by setting enabled to false. Even though Maven automatically fetches the latest SNAPSHOT on a daily basis by default, you can force
maven to download latest snapshot build using -U switch to any maven command.<repository >
 <id>central </id>
 <url>...</url>
 <snapshots >
 <enabled >true</enabled >
 <updatePolicy >always </updatePolicy >
 </snapshots >
</repository >

---

## Q4: What is the dif ference between a reactor pom and a parent pom?

**Answer:**

The mechanism in Maven that handles multi-module projects is referred to as the reactor . A reactor pom is a pom.xml file you define with , and Maven will
read all of these modules and build then in the correct order based on dependencies.
A parent pom is a pom from which other poms inherit via a section. Poms that inherit from a parent are referred to as “child poms.” There are dif ferent types of
parent poms.
Maven pomsmvn clean package -U

---

## Q5: How do you handle multi-module projects in Maven?

**Answer:**

A multi-module project is defined by a parent POM referencing one or more sub modules. Y ou can have dif ferent structures like
Appr oach 1. parent pom is in the project root
Appr oach 2. separate project for the parent pom
The parent pom.xml will define the modules that needs to be aggregated.myproject /
myproject -client /
myproject -model /
myproject -services /
pom.xml
myproject /
mypoject -parent /
 pom.xml
myproject -client /
myproject -model /
myproject -services /
<project xmlns ="....">
<description >MyApp </description >

<modelV ersion >4.0.0 </modelV ersion >
<groupId >com.myapp </groupId >
<artifactId >myproject </artifactId >
<version >1.0.0 -RC1.0-SNAPSHOT </version >
<packaging >pom</packaging >
<name >MyProject </name >
 
<scm>
 <url>http://sdlc/svn/myapp/myproject/trunk/</url>
 <connection >scm:svn:http://sdlc/svn/myapp/myproject/trunk</connection>
 <developerConnection >scm:svn:http://sdlc/svn/myapp/myproject/trunk</developerConnection>
</scm>
<modules >
 <module >model </module >
 <module >client </module >
 <module >services </module >
</modules >
 
.....
</project >
<?xml version ="1.0" encoding ="UTF-8" ?>
<project xmlns ="ns">
<modelV ersion >4.0.0 </modelV ersion >
<artifactId >myproject -model </artifactId >
<name >MyProject Model </name >
<packaging >jar</packaging >
<parent >
 <groupId >com.myapp </groupId >
 <artifactId >myproject </artifactId >
 <version >1.0.0 -RC1.0-SNAPSHOT <</version >
 <relativePath >../pom.xml</relativePath >
</parent >
 ...
</project >

Q. Which approach do you favor?
A. Approach 1 is the is the default maven convention for multi-module projects, and the intention is to be scalable to a lar ge scale build. If parent pom has its
own life cycle, and if it can be released separately of the other modules, then approach 2 may be an option.

---

## Q6: What are the dif ferent aspects managed by Maven in the SDLC?

**Answer:**

SCMs (Source control), Dependencies (i.e transitive dependencies), Builds, Releases, Documentation, and Distribution.

---

## Q7: What is the dif ference between “DependencyManagement” and “Dependencies” as shown below in poms?
Parent pom with version defined
and child pom with no version.<dependencyManagement >
<dependencies >
 <dependency >
 <groupId >javax .servlet </groupId >
 <artifactId >javax .servlet -api</artifactId >
 <version >3.0.1 </version >
 <scope >provided </scope >
 </dependency >
 //..
</dependencies >
</dependencyManagement >
<dependencies > 
<dependency >
 <groupId >javax .servlet </groupId >
 <artifactId >javax .servlet -api</artifactId >
 <scope >provided </scope >
</dependency >

**Answer:**

Dependency Management allows to consolidate and centralize the management of dependency versions without adding dependencies which are inherited
by all children. It will be a maintenance night-mare to change version numbers of all child poms in a lar ge commercial project. “DependencyManagement”
allows you to define a standard version of an artifact to use across multiple projects.

---

## Q8: What format does a maven repository use to uniquely identify an artifact in its repository?

**Answer:**

[groupId]-[artifactId]-[version]-[classifier].[type]
groupId : e.g. com.myapp
artifactId : e.g. myproject
version : e.g. 1.0.0-RC1.0-SNAPSHOT
classifier : e.g. sources, javadocs, bin and bundle (default: no classifier)
type: pom,jar ,war,ear (default is jar)

---

## Q9: What is an “attached artifact” and why would you need a “classifier”?

**Answer:**

Attached artifacts are additional related artifacts that get installed and deployed along with the “main” (e.g jar , war , or ear) artifact. These are most often,
javadocs, sources, resouce bundles, properties files, etc, but can actually be any file.
Attached artifacts automatically share the same groupId , ArtifactId and version as the main artifact but they are distinguished with an additional Classifier
field.

---

## Q10: How do you inherit dependencies or plugins in Maven? How are sub modules aggregated?

**Answer:**

In Maven Inheritance, you will have a multi-module maven project where a centrally managed POM file contains all the “approved” versions of artifacts.
In this “parent” pom was a “dependencyManagement” section with all the preferred versions of libraries within the or ganization. When you build a child of this
parent, Maven will retrieve this parent to mer ge the parent pom with the child pom. Y ou can have a look to the entire pom.xml by running the command mvn
help:ef fective-pom. This parent pom is also known as the reactor pom. The child modules like myproject-ui, myproject-security , etc will be inheriting the
versions from the parent.
Child pom.xml. The parent tag defines the parent to inherit form.//...
</dependencies >
<?xml version ="1.0" encoding ="UTF-8" ?>
<project xmlns ="http://maven.apache.or g/POM/4.0.0" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"

Like “dependencyManagement” the “pluginManagement” in parent contains plugin elements intended to configure child project builds that inherit from this
one. However , this only configures plugins that are actually referenced within the plugins element in the children. The children have every right to override
pluginManagement definitions.
The child modules are aggregated. The principle is that every command that you run on on the parent (i.e. reactor pom) will also be run on all the sub-modules.
The order of the modules will be defined by the Reactor , which will look at inter -modules dependencies to find which module must be built before the others. If
there are no dependencies, then it will take the list of the modules as they are defined in the parent pom.xml.xsi:schemaLocation ="http://maven.apache.or g/POM/4.0.0 http://maven.apache.or g/xsd/maven-4.0.0.xsd" >
<modelV ersion >4.0.0 </modelV ersion >
<groupId >com.myproject </groupId >
<artifactId >myproject -ui</artifactId >
<version >${myproject .version }</version >
<packaging >war</packaging >
<name >${project .artifactId }</name >
<url>http://www .myor ganization.com</url>
<parent >
 <groupId >com.myproject </groupId >
 <artifactId >myproject -parent -build </artifactId >
 <version >${myproject .version }</version >
</parent >
.. 
</project >
<?xml version ="1.0" encoding ="UTF-8" ?>
<project xmlns ="http://maven.apache.or g/POM/4.0.0" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
xsi:schemaLocation ="http://maven.apache.or g/POM/4.0.0 http://maven.apache.or g/xsd/maven-4.0.0.xsd" >
<modelV ersion >4.0.0 </modelV ersion >
<groupId >com.myproject </groupId >
<artifactId >myproject -parent -build </artifactId >
<packaging >pom</packaging >
<version >${myproject .version }</version >
<name >${project .artifactId }</name >
<url>http://www .myor ganization.com</url>
...
<modules >
 <module >myproject -security </module >

Q. Is this inheritance of dependencies a good idea?
A. It is a good idea when you only have say 5-10 sub modules (i.e. child modules). But, if you have 50+ child modules inheriting from the parent dependency
management or plugin management will cause a re-release of anything inheriting from it. For example, if you have 20+ projects inheriting or g.jboss.seam jar ,
and if that jar goes from 2.2.2.Final to 2.3, then you will have to re-release the 20+ projects inheriting from it. It appears that as of version 2.0.9, there is a new
scope on a dependency called import to aggregate dependencies without inheriting them.
So, like you favor composition or aggr egation over inheritance , in maven pom also favor aggregation over inheritance.

---

## Q11: What are the dif ferent dependency scopes have you used?

**Answer:**

compile, provided, test, and import.
Compile means that you need the JAR for compiling and running the app. For a web application, as an example, the JAR will be placed in the WEB-INF/lib
directory . <module >myproject -content </module >
 <module >myproject -data</module >
 <module >myproject -ui</module >
 <module >myproject -analytics </module >
</modules >
</project >
<dependencyManagement >
 <dependencies >
 <dependency >
 <groupId >com.myproject </groupId >
 <artifactId >myproject -parent -build </artifactId >
 <version >${myproject .version }</version >
 <type>pom</type>
 <scope >import </scope >
 </dependency >
 ....
 </dependencies >

Provided means that you need the JAR for compiling, but at run time there is already a JAR provided by the environment so you don’ t need it packaged with
your app. For a web app, this means that the JAR file will not be placed into the WEB-INF/lib directory .
Test scope indicates that the dependency is not required for normal use of the application, and is only available for the test compilation and execution phases.
import scope is used to aggregate the specified POM with the dependencies in that POM’ s “DependencyManagement ” section. This scope is only used on a
dependency of type pom in the section and available only from Maven version 2.0.9.

---

## Q12: What is the purpose of Maven assemply plugin?

**Answer:**

Maven assembly plugin is primarily intended to allow users to aggregate the project output along with its dependencies, modules, site documentation, and
other files into a single distribution archive like zip, tar , tar.gz, war , etc. Another example where this is handy is to create deployable files for dif ferent
environments like myapp-1.10-SNAPSHOT -dev.zip, myapp-1.10-SNAPSHOT -test.zip, and myapp-1.10-SNAPSHOT -prod.zip where each containing
environment related properties files packaged. There are 3 steps to create an assembly:
1) choose or write the assembly descriptor files to use like dev-assembly .xml, prod-assembly .xml, etc
2) configure the Assembly Plugin in your project’ s pom.xml, and include the above descriptor files
3) run “mvn assembly:single” on your project.
The dev-assembly file:
<assembly
xmlns ="http://maven.apache.or g/plugins/maven-assembly-plugin/assembly/1.1.2"
xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
xsi:schemaLocation ="http://maven.apache.or g/plugins/maven-assembly-plugin/assembly/1.1.2
http://maven.apache.or g/xsd/assembly-1.1.2.xsd" >
<id>dev</id>
<includeBaseDirectory >false </includeBaseDirectory >
<formats >
 <format >zip</format >
</formats >
<fileSets >
 <fileSet >
 <directory >config /dev</directory >
 <outputDirectory >/app-properties </outputDirectory >
 </fileSet >
 <fileSet >

The pom.xml file <directory >jboss -config /dev/deploy </directory >
 <outputDirectory >/ds</outputDirectory >
 </fileSet >
</fileSets >
<files>
 <file>
 <source >jboss -config /common /myapp -container .properties </source >
 <outputDirectory >/myapp -container -properties </outputDirectory >
 <destName >my-app-dev.properties </destName >
 </file>
</files>
</assembly >
<plugins >
 ....
 <plugin >
 <artifactId >maven -assembly -plugin </artifactId >
 <version >2.2.1 </version >
 <executions >
 <execution >
 <id>assemble </id>
 <phase >package </phase >
 <configuration >
 <descriptors >
 <descriptor >assembly /dev-assembly .xml</descriptor >
 <descriptor >assembly /test-assembly .xml</descriptor >
 <descriptor >assembly /prod-assembly .xml</descriptor >
 </descriptors >
 </configuration >
 <goals >
 <goal>single </goal>
 </goals >
 </execution >
 </executions >
 </plugin >
 ...

The main goal in the assembly plugin is the single goal. It is used to create all assemblies.
Now to generate dif ferent packages like myapp-1.10-SNAPSHOT -dev.zip, myapp-1.10-SNAPSHOT -test.zip, and myapp-1.10-SNAPSHOT -prod.zip<plugins >
mvn package

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
