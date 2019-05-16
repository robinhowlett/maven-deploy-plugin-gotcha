# Finding out why a Maven build failed to deploy

This repository is a companion to the blog post, ["Solved: When the Maven Deploy Plugin silently fails to deploy"](http://www.robinhowlett.com/blog/2019/05/15/solved-when-the-maven-deploy-plugin-silently-fails-to-deploy/), and demonstrates both the issue at hand (in the [`master`](https://github.com/robinhowlett/maven-deploy-plugin-gotcha/tree/master) branch) and the solution (in the [`fix`](https://github.com/robinhowlett/maven-deploy-plugin-gotcha/compare/fix?expand=1#diff-600376dffeb79835ede4a0b285078036) branch).

When running with a `mvn clean deploy`, the `master` branch build results in the root module ("pom-root") and its child ("module-two") silently failing to deploy:

```
...
[INFO] --- maven-deploy-plugin:2.8.2:deploy (default-deploy) @ module-two ---
[INFO] Deploying com.snaplogic:module-two:1.0-SNAPSHOT at end
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] module-one ......................................... SUCCESS [  3.477 s]
[INFO] pom-root ........................................... SUCCESS [  0.228 s]
[INFO] module-two ......................................... SUCCESS [  1.446 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.308 s
[INFO] Finished at: 2019-05-15T12:10:55-06:00
[INFO] Final Memory: 24M/437M
[INFO] -----------------------------------------------------------------
```

Running the same command in the `fix` branch, shows the deploy succeeding:

```
[INFO] 
[INFO] --- maven-deploy-plugin:2.8.2:deploy (default-deploy) @ module-two ---
Downloading from dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/1.0-SNAPSHOT/maven-metadata.xml
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/1.0-SNAPSHOT/pom-root-1.0-20190515.171609-1.pom
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/1.0-SNAPSHOT/pom-root-1.0-20190515.171609-1.pom (1.8 kB at 602 kB/s)
Downloading from dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/maven-metadata.xml
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/1.0-SNAPSHOT/maven-metadata.xml
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/1.0-SNAPSHOT/maven-metadata.xml (594 B at 119 kB/s)
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/maven-metadata.xml
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/pom-root/maven-metadata.xml (279 B at 70 kB/s)
Downloading from dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/maven-metadata.xml
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/module-two-1.0-20190515.171610-1.jar
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/module-two-1.0-20190515.171610-1.jar (2.1 kB at 710 kB/s)
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/module-two-1.0-20190515.171610-1.pom
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/module-two-1.0-20190515.171610-1.pom (533 B at 178 kB/s)
Downloading from dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/maven-metadata.xml
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/maven-metadata.xml
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/1.0-SNAPSHOT/maven-metadata.xml (767 B at 256 kB/s)
Uploading to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/maven-metadata.xml
Uploaded to dev: file:///Users/rhowlett/.m2/temp-repository/com/snaplogic/module-two/maven-metadata.xml (281 B at 94 kB/s)
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] module-one ......................................... SUCCESS [  2.970 s]
[INFO] pom-root ........................................... SUCCESS [  0.131 s]
[INFO] module-two ......................................... SUCCESS [  0.652 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.905 s
[INFO] Finished at: 2019-05-15T11:16:10-06:00
[INFO] Final Memory: 19M/309M
[INFO] ------------------------------------------------------------------------
```

A explanation of why this occurred and what was done to fix it is outlined in the post linked above.
