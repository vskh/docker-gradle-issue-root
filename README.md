# docker-gradle-issue-root
Sample code to illustrate the https://github.com/bmuschko/gradle-docker-plugin/issues/144

1. Root project (docker-gradle-issue-root) has definition of spring platform bom to be used in subprojects.
2. Subproject (docker-gradle-issue) has definition of sample spring dependency without version showing that BOM from step 1 works (one can run `gradlew build` and see that dependencies are packaged to application artifact).
3. With this setup, running `gradlew dockerBuildImage` is failing:
```
% ./gradlew dockerBuildImage                            [11:18] 
:docker-gradle-issue:compileJava
warning: [options] bootstrap class path not set in conjunction with -source 1.7
1 warning
:docker-gradle-issue:compileGroovy UP-TO-DATE
:docker-gradle-issue:processResources UP-TO-DATE
:docker-gradle-issue:classes
:docker-gradle-issue:jar
:docker-gradle-issue:startScripts
:docker-gradle-issue:distTar
:docker-gradle-issue:dockerCopyDistResources
:docker-gradle-issue:dockerDistTar
:docker-gradle-issue:dockerBuildImage FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':docker-gradle-issue:dockerBuildImage'.
> org/bouncycastle/openssl/PEMParser

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

Total time: 12.097 secs
```
4. Commenting out import of BOM using spring dependency management plugin (and explicitly specifying any spring dependency) makes image build work again.
