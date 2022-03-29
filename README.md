# DisTA
A distributed dynamic taint tracking tool for java-based systems.

========
DisTA can perform system-agnostic dynamic taint tracking (DTA) for Java-based distributed systems. It is developed based on an intra-node taint analysis tool [Phosphor](https://github.com/gmu-swe/phosphor).

Running
-------
DisTA's configuration follows Phosphor's style.

The instrumenter takes two primary arguments: first a path containing the classes to instrument, and then a destination for the instrumented classes. 

```
usage: java -jar DisTA.jar [input] [output]
```

Then, to instrument the JRE we'll run:
`java -jar DisTA.jar /Library/Java/JavaVirtualMachines/jdk1.8.0_222.jdk/Contents/Home jre-inst`

After you do this, make sure to chmod +x the binaries in the new folder, e.g. `chmod +x jre-inst/bin/*`

The next step is to instrument the code which you would like to track. This time when you run the instrumenter, pass your entire (compiled) code base to DisTA for instrumentation, and specify an output folder for that.

We can now run the instrumented code using our instrumented JRE, as such:
`JAVA_HOME=jre-inst/ $JAVA_HOME/bin/java  -Xbootclasspath/a:DisTA.jar -javaagent:DisTA.jar -cp path-to-instrumented-code your.main.class`

Note: It is not 100% necessary to instrument your application/library code in advance - the javaagent will detect any uninstrumented class files as they are being loaded into the JVM and instrument them as necessary. If you want to do this, then you may want to add the flag `-javaagent:DisTA.jar=cacheDir=someCacheFolder` and DisTA will cache the generated files in `someCacheFolder` so they aren't regenerated every run. 

<!-- Interacting with DisTA
-----
-->
Building
------
DisTA is a maven project. You can generate the jar with a simple `mvn package`.
