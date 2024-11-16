

This branch tests the Red Hat version of Quarkus wih Hibernate 6 and jakarta-data.
Currently, the build is not working with the Red Hat version of Quarkus undr certain circumtances.

| OS-Vs              | JDK-Vs.     | Hwd               | Result  |
|--------------------|-------------|-------------------|---------|
| Ubuntu 24.04.1 LTS | 21.0.5-tem  | Intel64           | Failure |
| Ubuntu 24.04.1 LTS | 17.0.13-tem | Intel64           | Failure |
| Ubuntu 24.04.1 LTS | 21.0.5-tem  | Intel VM (Vmware) | Failure |
| Ubuntu 24.04.1 LTS | 17.0.13-tem | Intel VM (Vmware) | Failure |
| Ubuntu 24.04.1 LTS | 21.0.5-tem  | Intel VM (WSL 2)  | Success |
| Ubuntu 24.04.1 LTS | 17.0.13-tem | Intel VM (WSL 2)  | Success |
| Ubuntu 22.04.5 LTS | 17.0.13-tem | Intel64           | Success |
| Ubuntu 22.04.5 LTS | 21.0.5-tem  | Intel64           | Success |
| Ubuntu 23.04       | 17.0.13-tem | Intel (WSL 2)     | Success |
| Ubuntu 23.04       | 21.0.5-tem  | Intel (WSL 2)     | Sucess  |
| Win 11 10.0.22631  | 17.0.13-tem | Intel64           | Success |
| Win 11 10.0.22631  | 21.0.5-tem  | Intel64           | Success |
| Sonoma 14.6.1      | 17.0.13-tem | Apple Silicon     | Success |
| Sonoma 14.6.1      | 21.0.5-tem  | Apple Silicon     | Success |
| Fedora 39          | 17.0.13-tem | ARM (Fusion)      | Success |
| Fedora 39          | 21.0.5-tem  | ARM (Fusion)      | Success |

How to reproduce the error:

* Run `mvn clean verify`

I get the following error on Ubuntu 24.04.1 on native hardware and also when running as a VM under VMware.

```text
[error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
```

Details
```text
[ERROR] org.example.LibraryTest.bookFindable -- Time elapsed: 0.013 s <<< ERROR!
java.lang.RuntimeException: 
java.lang.RuntimeException: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
        [error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:732)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:840)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2268)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:409)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:346)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:864)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:238)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:380)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:432)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3288)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

        at io.quarkus.test.junit.QuarkusTestExtension.throwBootFailureException(QuarkusTestExtension.java:634)
        at io.quarkus.test.junit.QuarkusTestExtension.interceptTestClassConstructor(QuarkusTestExtension.java:718)
        at java.base/java.util.Optional.orElseGet(Optional.java:364)
        at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
        at java.base/java.util.ArrayList.forEach(ArrayList.java:1511)
Caused by: java.lang.RuntimeException: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
        [error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:732)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:840)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2268)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:409)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:346)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:864)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:238)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:380)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:432)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3288)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

        at io.quarkus.runner.bootstrap.AugmentActionImpl.runAugment(AugmentActionImpl.java:354)
        at io.quarkus.runner.bootstrap.AugmentActionImpl.createInitialRuntimeApplication(AugmentActionImpl.java:272)
        at io.quarkus.runner.bootstrap.AugmentActionImpl.createInitialRuntimeApplication(AugmentActionImpl.java:62)
        at io.quarkus.test.junit.QuarkusTestExtension.doJavaStart(QuarkusTestExtension.java:219)
        at io.quarkus.test.junit.QuarkusTestExtension.ensureStarted(QuarkusTestExtension.java:601)
        at io.quarkus.test.junit.QuarkusTestExtension.beforeAll(QuarkusTestExtension.java:651)
        ... 1 more
Caused by: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
        [error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:732)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:840)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2268)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:409)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:346)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:864)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:238)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:380)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:432)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3288)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

        at io.quarkus.builder.Execution.run(Execution.java:123)
        at io.quarkus.builder.BuildExecutionBuilder.execute(BuildExecutionBuilder.java:79)
        at io.quarkus.deployment.QuarkusAugmentor.run(QuarkusAugmentor.java:161)
        at io.quarkus.runner.bootstrap.AugmentActionImpl.runAugment(AugmentActionImpl.java:350)
        ... 6 more
Caused by: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:732)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:840)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2268)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:409)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:346)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:864)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:238)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:380)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:432)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3288)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

[INFO] 
[INFO] Results:
[INFO] 
[ERROR] Errors: 
[ERROR]   LibraryTest.bookFindable » Runtime java.lang.RuntimeException: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
        [error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:732)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:840)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2268)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:158)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:409)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:346)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:864)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:238)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:380)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:432)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3288)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

```

Alternatively,
* Run `./mvnw clean quarkus:dev

I get 

```text
2024-11-15 05:33:50,620 ERROR [io.qua.boo.cla.PathTreeClassPathElement] (build-34) Failed to read io/vertx/mutiny/ext/web/Router.class attempting to re-open the zip file. It is likely a jar file changed on disk, you should shutdown your application: java.io.EOFException: Unexpected end of ZLIB input stream
Press [eat jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2275)r more options>
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:175)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:412)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:349)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:870)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:247)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:379)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:431)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3268)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:291)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:733)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:1583)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)


2024-11-15 05:33:50,730 INFO  [io.qua.dep.dev.IsolatedDevModeMain] (main) Attempting to start live reload endpoint to recover from previous Quarkus startup failure
2024-11-15 05:33:51,193 ERROR [io.qua.dep.dev.IsolatedDevModeMain] (main) Failed to start quarkus: java.lang.RuntimeException: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
Press [e[error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:733)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:1583)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2275)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:175)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:412)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:349)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:870)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:247)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:379)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:431)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3268)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

        at io.quarkus.runner.bootstrap.AugmentActionImpl.runAugment(AugmentActionImpl.java:354)
        at io.quarkus.runner.bootstrap.AugmentActionImpl.createInitialRuntimeApplication(AugmentActionImpl.java:272)
        at io.quarkus.runner.bootstrap.AugmentActionImpl.createInitialRuntimeApplication(AugmentActionImpl.java:62)
        at io.quarkus.deployment.dev.IsolatedDevModeMain.firstStart(IsolatedDevModeMain.java:91)
        at io.quarkus.deployment.dev.IsolatedDevModeMain.accept(IsolatedDevModeMain.java:430)
        at io.quarkus.deployment.dev.IsolatedDevModeMain.accept(IsolatedDevModeMain.java:57)
        at io.quarkus.bootstrap.app.CuratedApplication.runInCl(CuratedApplication.java:138)
        at io.quarkus.bootstrap.app.CuratedApplication.runInAugmentClassLoader(CuratedApplication.java:93)
        at io.quarkus.deployment.dev.DevModeMain.start(DevModeMain.java:131)
        at io.quarkus.deployment.dev.DevModeMain.main(DevModeMain.java:62)
Caused by: io.quarkus.builder.BuildException: Build failure: Build failed due to errors
        [error]: Build step io.quarkus.vertx.http.deployment.VertxHttpProcessor#preinitializeRouter threw an exception: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:733)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:1583)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input stream
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2275)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:175)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:412)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:349)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:870)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:247)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:379)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:431)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3268)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more

        at io.quarkus.builder.Execution.run(Execution.java:123)
        at io.quarkus.builder.BuildExecutionBuilder.execute(BuildExecutionBuilder.java:79)
        at io.quarkus.deployment.QuarkusAugmentor.run(QuarkusAugmentor.java:161)
        at io.quarkus.runner.bootstrap.AugmentActionImpl.runAugment(AugmentActionImpl.java:350)
        ... 9 more
Caused by: java.lang.RuntimeException: Unable to read io/vertx/mutiny/ext/web/Router.class
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:343)
        at io.quarkus.paths.PathTreeVisit.process(PathTreeVisit.java:39)
        at io.quarkus.paths.ArchivePathTree.apply(ArchivePathTree.java:141)
        at io.quarkus.paths.PathTreeWithManifest.apply(PathTreeWithManifest.java:75)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.getData(PathTreeClassPathElement.java:336)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:521)
        at io.quarkus.bootstrap.classloading.QuarkusClassLoader.loadClass(QuarkusClassLoader.java:481)
        at io.quarkus.vertx.http.deployment.VertxHttpProcessor.preinitializeRouter(VertxHttpProcessor.java:247)
        at java.base/java.lang.invoke.MethodHandle.invokeWithArguments(MethodHandle.java:733)
        at io.quarkus.deployment.ExtensionLoader$3.execute(ExtensionLoader.java:856)
        at io.quarkus.builder.BuildContext.run(BuildContext.java:256)
        at org.jboss.threads.ContextHandler$1.runWith(ContextHandler.java:18)
        at org.jboss.threads.EnhancedQueueExecutor$Task.doRunWith(EnhancedQueueExecutor.java:2516)
        at org.jboss.threads.EnhancedQueueExecutor$Task.run(EnhancedQueueExecutor.java:2495)
        at org.hreads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1521)
        at java.base/java.lang.Thread.run(Thread.java:1583)
        at org.jboss.threads.JBossThread.run(JBossThread.java:483)
Caused by: java.io.EOFException: Unexpected end of ZLIB input streaminal, [h] for more options>
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem$2.fill(ZipFileSystem.java:2275)
        at java.base/java.util.zip.InflaterInputStream.read(InflaterInputStream.java:175)
        at java.base/java.io.InputStream.readNBytes(InputStream.java:412)
        at java.base/java.io.InputStream.readAllBytes(InputStream.java:349)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystem.newByteChannel(ZipFileSystem.java:977)
        at jdk.zipfs/jdk.nio.zipfs.ZipPath.newByteChannel(ZipPath.java:870)
        at jdk.zipfs/jdk.nio.zipfs.ZipFileSystemProvider.newByteChannel(ZipFileSystemProvider.java:247)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:379)
        at java.base/java.nio.file.Files.newByteChannel(Files.java:431)
        at java.base/java.nio.file.Files.readAllBytes(Files.java:3268)
        at io.quarkus.bootstrap.classloading.PathTreeClassPathElement$Resource.lambda$getData$2(PathTreeClassPathElement.java:341)
        ... 16 more


2024-11-15 05:33:51,202 ERROR [io.qua.dep.dev.IsolatedDevModeMain] (main) Failed to recover after failed start: java.lang.RuntimeException: java.lang.RuntimeException: Unable to start HTTP server
        at io.quarkus.vertx.http.runtime.VertxHttpRecorder.startServerAfterFailedStart(VertxHttpRecorder.java:307)
Press [eat io.quarkus.vertx.http.runtime.devmode.VertxHttpHotReplacementSetup.handleFailedInitialStart(VertxHttpHotReplacementSetup.java:73)
        at io.quarkus.deployment.dev.RuntimeUpdatesProcessor.startupFailed(RuntimeUpdatesProcessor.java:1299)
        at io.quarkus.deployment.dev.IsolatedDevModeMain.firstStart(IsolatedDevModeMain.java:128)
        at io.quarkus.deployment.dev.IsolatedDevModeMain.accept(IsolatedDevModeMain.java:430)
        at io.quarkus.deployment.dev.IsolatedDevModeMain.accept(IsolatedDevModeMain.java:57)
        at io.quarkus.bootstrap.app.CuratedApplication.runInCl(CuratedApplication.java:138)
        at io.quarkus.bootstrap.app.CuratedApplication.runInAugmentClassLoader(CuratedApplication.java:93)
        at io.quarkus.deployment.dev.DevModeMain.start(DevModeMain.java:131)
        at io.quarkus.deployment.dev.DevModeMain.main(DevModeMain.java:62)
Caused by: java.lang.RuntimeException: Unable to start HTTP server
        at io.quarkus.vertx.http.runtime.VertxHttpRecorder.doServerStart(VertxHttpRecorder.java:947)
        at io.quarkus.vertx.http.runtime.VertxHttpRecorder.startServerAfterFailedStart(VertxHttpRecorder.java:299)
        ... 9 more
Caused by: java.util.concurrent.ExecutionException: io.quarkus.runtime.QuarkusBindException: Port(s) already bound: 8080: Address already in use
        at java.base/java.util.concurrent.CompletableFuture.reportGet(CompletableFuture.java:396)
        at java.base/java.util.concurrent.CompletableFuture.get(CompletableFuture.java:2073)
        at io.quarkus.vertx.http.runtime.VertxHttpRecorder.doServerStart(VertxHttpRecorder.java:864)
        ... 10 more
Caused by: io.quarkus.runtime.QuarkusBindException: Port(s) already bound: 8080: Address already in use
        at io.quarkus.vertx.http.runtime.VertxHttpRecorder$11.handle(VertxHttpRecorder.java:831)
        at io.quarkus.vertx.http.runtime.VertxHttpRecorder$11.handle(VertxHttpRecorder.java:812)
        at io.vertx.core.impl.future.FutureImpl$4.onFailure(FutureImpl.java:188)
        at io.vertx.core.impl.future.FutureBase.emitFailure(FutureBase.java:81)
        at io.vertx.core.impl.future.FutureImpl.tryFail(FutureImpl.java:278)
        at io.vertx.core.impl.future.Mapping.onFailure(Mapping.java:45)
        at io.vertx.core.impl.future.FutureBase.emitFailure(FutureBase.java:81)
        at io.vertx.core.impl.future.FutureImpl.tryFail(FutureImpl.java:278)
        at io.vertx.core.impl.future.PromiseImpl.onFailure(PromiseImpl.java:54)
        at io.vertx.core.impl.future.PromiseImpl.handle(PromiseImpl.java:43)
        at io.vertx.core.impl.future.PromiseImpl.handle(PromiseImpl.java:23)
        at io.vertx.core.impl.DeploymentManager.lambda$reportResult$2(DeploymentManager.java:129)
        at io.vertx.core.impl.ContextInternal.dispatch(ContextInternal.java:270)
        at io.vertx.core.impl.ContextInternal.dispatch(ContextInternal.java:252)
        at io.vertx.core.impl.ContextInternal.lambda$runOnContext$0(ContextInternal.java:50)
        at io.netty.util.concurrent.AbstractEventExecutor.runTask(AbstractEventExecutor.java:173)
        at io.netty.util.concurrent.AbstractEventExecutor.safeExecute(AbstractEventExecutor.java:166)
        at io.netty.util.concurrent.SingleThreadEventExecutor.runAllTasks(SingleThreadEventExecutor.java:469)
        at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:569)
        at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:994)
        at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
        at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
        at java.base/java.lang.Thread.run(Thread.java:1583)



--
Press [e] to edit command line args (currently ''), [:] for the terminal, [h] for more options>


```