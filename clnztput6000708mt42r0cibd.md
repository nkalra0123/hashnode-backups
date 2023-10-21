---
title: "mvn - A short guide to Maven"
datePublished: Sat Oct 21 2023 09:14:31 GMT+0000 (Coordinated Universal Time)
cuid: clnztput6000708mt42r0cibd
slug: mvn-a-short-guide-to-maven
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/8Gg2Ne_uTcM/upload/d1cbedb6b07e797803a5df2bdeeaeed5.jpeg
tags: java, mvn

---

### **Maven Basics:**

#### What is Maven?

Maven is a build tool that manages the project lifecycle, handling tasks such as compilation, testing, packaging, and dependency management. It uses a Project Object Model (POM) file, typically named `pom.xml`, to configure the project.

It simplifies the build process, project management, and dependency resolution.

The Maven build lifecycle consists of a series of phases that define the order in which goals are executed. Here are the standard phases in Maven:

1. **validate:** Validates the project is correct and all necessary information is available.
    
2. **compile:** Compiles the source code of the project.
    
3. **test:** Runs tests on the compiled source code using a suitable unit testing framework.
    
4. **package:** Takes the compiled code and packages it in its distributable format, such as a JAR or WAR.
    
5. **integration-test:** Performs integration tests on the package.
    
6. **verify:** Runs checks on the results of integration tests to ensure quality criteria are met.
    
7. **install:** Installs the package into the local repository, for use as a dependency in other projects.
    
8. **deploy:** Copies the final package to the remote repository for sharing with other developers and projects.
    

These phases are executed sequentially, and each phase is made up of goals. Goals are specific tasks that run in each phase. For example, the `compile` phase has a goal of compiling the source code, and the `test` phase has a goal of running tests.

### **mvn clean install Explained:**

The `mvn clean install` command is a common and powerful Maven command used in the build process.

* **clean:** This phase removes the `target` directory, which contains compiled classes, JARs, and other generated files from previous builds. It ensures a clean slate for the new build.
    
* **install:** This phase executes the default phases up to the `install` phase. It compiles the source code, runs tests, packages the application, and installs it in the local Maven repository (`~/.m2/repository`).
    

In summary, `mvn clean install` is often used to perform a full build, ensuring that the project is compiled, tested, and packaged, and the resulting artifact is installed locally.

### **Other Useful Maven Commands:**

1. **mvn clean:** Cleans the project by removing the `target` directory.
    
2. **mvn test:** Runs the tests of the project.
    
3. **mvn package:** Packages the compiled code into a distributable format, like a JAR or WAR file.
    
4. **mvn dependency:tree:** Displays the project's dependency tree.
    
5. **mvn help:effective-pom:** Shows the effective POM, including inheritance and active profiles.
    
6. **mvn versions:display-dependency-updates:** Displays the dependencies that have newer versions available.
    

### **The mvnw Wrapper:**

The `mvnw` script (or `mvnw.cmd` on Windows) is a Maven wrapper script designed to ensure a consistent build environment across different machines. It allows you to use Maven without a globally installed version.

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.