---
title: "Setting up Checkstyle linter for Java"
date: 2023-05-28T12:20:00+01:00
description: "Enforcing coding standards and performing static code analysis for Java with Checkstyle"
tags: [Java, Tools]
---
Modern software development requires automated code checks to ensure that common code style standards are met. This is usually done by linters, which are available for many programming languages and usually built in a CI pipeline.

## What is a linter?

A linter is a tool that analyzes your source code for potential errors, stylistic inconsistencies, and other issues that can affect code quality and maintainability. The name "linter" comes from the [Unix utility lint](https://www.ibm.com/docs/en/aix/7.2?topic=l-lint-command), which was first released in the 1970s.

Linters are commonly used in software development to help identify and prevent common programming errors and to enforce consistent coding styles and best practices. Some common tasks that linters perform include:
* Checking for syntax errors and common mistakes, such as missing semicolons or undefined variables.
* Identifying potential issues with code logic, such as unreachable code or unused variables.
* Enforcing coding standards and best practices, such as indentation, variable naming, and commenting.
* Highlighting potential security vulnerabilities, such as SQL injection or cross-site scripting attacks.

By using a linter, you can catch errors and potential issues early in the development process, before they become more difficult and expensive to fix. Linters can also help enforce consistent coding standards across a project or team, making it easier to read and maintain the code over time.

## What are the options for Java?

Some modern languages have linters built-in as a part of the language toolchain. Unfortunately, Java doesn't ship with one, so we have to use an external one. 

There are many options available, such as [SpotBugs](https://spotbugs.github.io), [Checkstyle](https://checkstyle.org), [PMD](https://pmd.github.io), and [SonarQube](https://docs.sonarqube.org/latest/). In this article, we will focus on Checktyle for its simplicity. However, I recommend checking out other options as well.

## Configuring Checkstyle for Java project

### Add it as a plugin to Gradle (or Maven)

```groovy
plugins {
    id 'checkstyle'
}
```

### Specifcy version and maximum warnings allowed before failing the build.

```groovy
checkstyle {
 toolVersion '10.2'
 maxWarnings 0
}
```

### Add your coding style configuration as xml file

Codestyle configuration should be stored under `config/checkstyle/checkstyle.xml` from the root of your project.

You can use some of the reference code styles as a starting point. As for me, I prefer Google Code Style which can be copied from here: https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml. Now you can edit it to suit your preferences.

### Add a plugin for IntelliJ IDEA

Install Checkstyle plugin from the JetBrains plugin marketplace: https://plugins.jetbrains.com/plugin/1065-checkstyle-idea

This will allow you to set the formatting options of IDE to your specific code style:
1. Go to Settings -> Editor -> Code Style -> Java
2. Click Manage -> Import -> choose "CheckStyle Configuration" and select a corresponding CheckStyle configuration XML file

Now you can format each code file according to your code style rules. And they are enforced by the Gradle plugin: for every build which contains code style rules violations, you will have a failure with details on which rule was violated and what file needs to be fixed, as shown in the example output below.

```bash
[WARN] /Users/ruslan/Projects/demo/app/src/main/java/com/leskor/demo/DemoController.java:107: Line is longer than 100 characters (found 111). [LineLength]
```
Congratulations! Now you have linter as a part of your CI pipeline and development workflow, as well as uniform coding standards across your project enforced. 