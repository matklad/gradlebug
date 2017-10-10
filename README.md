Reproduction for https://github.com/gradle/gradle/issues/3119.

Expected behavior (Gradle 4.1)

```
λ sed -i 's/gradle-.\..-all/gradle-4.1-all/g' gradle/wrapper/gradle-wrapper.properties

λ tail -n 1 gradle/wrapper/gradle-wrapper.properties
distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip

λ ./gradlew build

BUILD SUCCESSFUL in 20s
6 actionable tasks: 6 executed
```


Current behavior

```
λ rm -fr build subst/build

~/projects/gradlebug master
λ sed -i 's/gradle-.\..-all/gradle-4.2-all/g' gradle/wrapper/gradle-wrapper.properties

~/projects/gradlebug master
λ tail -n 1 gradle/wrapper/gradle-wrapper.properties
distributionUrl=https\://services.gradle.org/distributions/gradle-4.2-all.zip

~/projects/gradlebug master
λ ./gradlew build

FAILURE: Build failed with an exception.

* What went wrong:
A problem occurred configuring root project 'gradlebug'.
> Cannot expand ZIP '/home/jetbrains/projects/gradlebug/subst/build/distributions/subst.zip' as it does not exist.

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

* Get more help at https://help.gradle.org

BUILD FAILED in 0s
```

That is, dependency substitution occured, but the build of the substituted project was not 
executed. One can execute the build manually though:


```
λ cd subst

λ ../gradlew build

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed

λ cd ../

λ ./gradlew build

BUILD SUCCESSFUL in 0s
6 actionable tasks: 6 executed
```