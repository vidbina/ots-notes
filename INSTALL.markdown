# OSX Installation

## SVN
Because setting up JavaHL may be problematic on a OSX one may use the SVNKit
interface instead.

 - navigate to Preferences > Team > SVN
 - change SVN interface to "SVNKit"

## Maven
On OSX, the `/usr/libexec/java_home` tool may be used to determine the path of
the JDK for a given version. Generally the `$JAVA_HOME` env var will contain 
the path returned by `/usr/libexec/java_home`. Sometimes a discrepancy may 
exist between the `$JAVA_HOME` version (observable through `java -version`) and
the java version assumed by Maven (observable through `mvn -version`).

This issue may be resolved by updating the `.mavenrc` file to contain the proper
path to the intended JDK

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 1.7)
```

## Java3D
Some components of OpenTrafficSim require [Java3D][java3d]. Especially the 
rendering of infrastructures may demand the presence of these libraries.

[java3d]: http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-java-client-419417.html#java3d-1.5.1-oth-JPR
[svn-1_6-osx]: http://jason.pureconcepts.net/2012/10/updating-svn-mac-os-x/
[javahl]: http://subclipse.tigris.org/wiki/JavaHL
[javahl-osx-prob]: http://stackoverflow.com/questions/9303293/subclipse-and-javahl-installation-headache
