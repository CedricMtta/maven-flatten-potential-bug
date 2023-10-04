# README

## Steps to reproduce:

Add the following profile and properties to your ~/.m2/settings.xml

```xml
	<activeProfiles>
		...
		<activeProfile>maven-flatten-plugin-potential-bug</activeProfile>
        ...
	</activeProfiles>
	
	<profiles>
        ...
		<profile>
			<id>maven-flatten-plugin-potential-bug</id>
			<properties>
				<unrelated.module.version>1.0-SNAPSHOT</unrelated.module.version>
			</properties>
		</profile>
        ...
    </profiles>
```


Install the unrelated-module and compile the repo maven-flatten-plugin-potential-bug
```bash
cd unrelated-module
mvn clean install

cd ../maven-flatten-plugin-potential-bug
mvn clean package
```

Expected stacktrace:
```bash
 mvn clean package
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] maven-flatten-plugin-potential-bug                                 [pom]
[INFO] child-module                                                       [jar]
[INFO]
[INFO] ---------< com.cedricmtta:maven-flatten-plugin-potential-bug >----------
[INFO] Building maven-flatten-plugin-potential-bug 1.0-SNAPSHOT           [1/2]
[INFO]   from pom.xml
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] --- clean:3.2.0:clean (default-clean) @ maven-flatten-plugin-potential-bug ---
[INFO] 
[INFO] --- flatten:1.5.0:clean (flatten.clean) @ maven-flatten-plugin-potential-bug ---
[INFO] Deleting C:\DEV\maven-flatten-potential-bug\maven-flatten-plugin-potential-bug\.flattened-pom.xml
[INFO]
[INFO] --- flatten:1.5.0:flatten (flatten) @ maven-flatten-plugin-potential-bug ---
[INFO] Generating flattened POM of project com.cedricmtta:maven-flatten-plugin-potential-bug:pom:1.0-SNAPSHOT...
[INFO] 
[INFO] --------------------< com.cedricmtta:child-module >---------------------
[INFO] Building child-module 1.0-SNAPSHOT                                 [2/2]
[INFO]   from child-module\pom.xml
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- clean:3.2.0:clean (default-clean) @ child-module ---
[INFO] Deleting C:\DEV\maven-flatten-potential-bug\maven-flatten-plugin-potential-bug\child-module\target
[INFO]
[INFO] --- flatten:1.5.0:clean (flatten.clean) @ child-module ---
[INFO]
[INFO] --- resources:3.3.1:resources (default-resources) @ child-module ---
[INFO] Copying 0 resource from src\main\resources to target\classes
[INFO]
[INFO] --- flatten:1.5.0:flatten (flatten) @ child-module ---
[INFO] Generating flattened POM of project com.cedricmtta:child-module:jar:1.0-SNAPSHOT...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for maven-flatten-plugin-potential-bug 1.0-SNAPSHOT:
[INFO]
[INFO] maven-flatten-plugin-potential-bug ................. SUCCESS [  0.535 s]
[INFO] child-module ....................................... FAILURE [  0.272 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.972 s
[INFO] Finished at: 2023-10-04T11:00:15+02:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.codehaus.mojo:flatten-maven-plugin:1.5.0:flatten (flatten) on project child-module: 1 problem was encountered while building the effective model for com.cedricmtta:child-module:1.0-SNAPSHOT
[ERROR] [ERROR] 'dependencies.dependency.version' for com.cedricmtta:unrelated-module:jar must be a valid version but is '${unrelated.module.version}'. @ 
[ERROR]
[ERROR] -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException
[ERROR]
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <args> -rf :child-module
```