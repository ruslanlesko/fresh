---
title: "Java 17 LTS. Changes compared to Java 11"
date: 2021-10-21T23:20:30+03:00
description: "My review of the new Java LTS release with examples"
tags: [Java]
---
Oracle has released the new long-term support (LTS) version 17 of Java in September 2021. I've been working with it for more than a month and I'm now ready to outline all the major changes compared to Java 11, its previous LTS release. I deliberately do not compare it to 16 because most Java users work on LTS versions. That's why it makes sense to have a list of all new features that were introduced in 12, 13, 14, 15, 16, and 17 versions. I haven't tried Java 17 in production yet, so this review is based only on my experience of using Java 17 in personal projects.

## Stronger encapsulation

Java is well-known for its backward compatibility. But this particular release breaks some of it. Now it is not possible to access some internal APIs even via reflection access. While ordinary users don't do this on a regular basis, some of the frameworks and libraries do. 

By internal API I mean all packages that don't start with `java.*` or `javax.*`. The most common one is `sun.misc.*`. This was done to protect internals on a VM level and allow Java to evolve more flexibly without locking on the fact that there are many consumers who depend on these internal packages.

You may notice this issue when you try to run your existing prior-Java-17-project on a Java 17 for the first time. This problem can be solved by upgrading your dependencies to the latest versions. In my case, I got an issue with the Mockito library, which was solved by upgrading it from version 
3.1.0 to 4.0.0.

A deeper explanation is available in [this article by Ben Evans](https://blogs.oracle.com/javamagazine/post/a-peek-into-java-17-continuing-the-drive-to-encapsulate-the-java-runtime-internals) or [JEP 403](https://openjdk.java.net/jeps/403)

## Records

The most visible language change is an introduction of value types or records. They were added to the scope of the Java 16 release. Similar to the data class concept in Kotlin language, records provide a simple syntax for declaring classes as only data holders, without behavior. But records are more strict since they are immutable.

I have applied records to my entities [in one of my projects](https://github.com/ruslanlesko/palermopg/commit/0df849c9cb366d4efb83eca819450f91bf02f35f#diff-4753c90ea21bf3965954140c33635f0187d8cb0a7b564fd2a0141065fd02aae8L5). Here is how it looked before:

```java
public class StorageConsumption {
    private final long userId;
    private final long size;
    private final long limit;

    public StorageConsumption(long userId, long size, long limit) {
        this.userId = userId;
        this.size = size;
        this.limit = limit;
    }

    public long getSize() {
        return size;
    }

    public long getUserId() {
        return userId;
    }

    public long getLimit() {
        return limit;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        StorageConsumption that = (StorageConsumption) o;
        return userId == that.userId &&
                size == that.size &&
                limit == that.limit;
    }

    @Override
    public int hashCode() {
        return Objects.hash(userId, size, limit);
    }
} 
```

To the following form:

```java
public record StorageConsumption(long userId, long size, long limit) { }
```

However, this is not completely interchangeable. Records generate the following accessor methods: 

```java
long userId() { return userId; }
long size() { return size; }
long limit() { return limit; }
```

They do not have `get` prefix. Records are not Java beans, so they are not suitable for JPA. But this is a very handy improvement for any case when immutable data structure with `hashCode`, `equals` and `toString` methods are needed. 

[JEP 395](https://openjdk.java.net/jeps/395)

## `Stream.toList()` method

This change arrived in Java 16. But the original Stream API was introduced a long time ago in Java 8. Since then, the most common line in code which used streams a lot, was `.collect(Collectors.toList())`. In Java 17, it is possible to just write `.toList()` on a stream object and it will be collected to list.

This is not a huge change, but it is important to highlight it because it will make stream operations more convenient. 

## Pattern matching for `instanceof`

Another syntax improvement is pattern matching for `instanceof` (also arrived in Java 16). It allowed me to simplify the following code

```java
if (cause instanceof StorageLimitException) {
	cors(response.setStatusCode(400)).end(((StorageLimitException) cause).json());
}
```

into

```java
if (cause instanceof StorageLimitException sle) {
	cors(response.setStatusCode(400)).end(sle.json());
}
```

This improvement addresses the common idiom (class casting after checking the type) by incorporating it into the language itself.

[JEP 394](https://openjdk.java.net/jeps/394)

## Switch expressions

Since Java 8, the language started incorporating some of the functional programming paradigms. This LTS version brings us to switch expressions. Initially added in Java 14, they allow returning a value.

Looking at my project, I found [one place to apply such refactoring](https://github.com/ruslanlesko/palermopg/commit/a9aa9cdeade0e9b42aa3ac5da69b51a92d199ac8). The method 

```java
public int getRotation() {
    try {
        var exifDirectory = metadata.getFirstDirectoryOfType(ExifIFD0Directory.class);
        if (!exifDirectory.containsTag(TAG_ORIENTATION)) {
            return -1;
        }
        int orientation = exifDirectory.getInt(TAG_ORIENTATION);
        int degrees;
        switch (orientation) {
            case 3: {
                degrees = 180;
                break;
            }
            case 6: {
                degrees = 90;
                break;
            }
            case 8: {
                degrees = 270;
                break;
            }
            default: {
                degrees = 0;
                break;
            }
        }
        return degrees;
    } catch (MetadataException e) {
        logger.error(e.getMessage());
        return -1;
    }
}
```

can now be written as the following

```java
public int getRotation() {
    try {
        var exifDirectory = metadata.getFirstDirectoryOfType(ExifIFD0Directory.class);
        if (!exifDirectory.containsTag(TAG_ORIENTATION)) {
            return -1;
        }
        int orientation = exifDirectory.getInt(TAG_ORIENTATION);
        return switch (orientation) {
            case 3 -> 180;
            case 6 -> 90;
            case 8 -> 270;
            default -> 0;
        };
    } catch (MetadataException e) {
        logger.error(e.getMessage());
        return -1;
    }
}
```

By using arrows instead of a colon, we can specify that the switch is an expression, so it can return values. Also, the break statement is no longer needed in this case. Refactored code looks much cleaner.

[JEP 361](https://openjdk.java.net/jeps/361)

## Sealed classes

For this one I don't have an example I actually use in my project, so will use the one used in the official documentation. Sealed classes or interfaces allow limiting which other classes or interfaces can extend or implement them.

```java
public sealed class Shape
    permits Circle, Square, Rectangle {
}
```

This feature is useful to represent predefined hierarchies of classes with do not allow arbitrary child classes by design (such as graphics primitives or arithmetical operations).

[JEP 409](https://openjdk.java.net/jeps/409)

## Text blocks

Introduced back in Java 15, text blocks are a bit more readable way of writing long string literals. Instead of a typical way of writing SQL query such as

```java
String query = "SELECT \"EMP_ID\", \"LAST_NAME\" FROM \"EMPLOYEE_TB\"\n" +
               "WHERE \"CITY\" = 'INDIANAPOLIS'\n" +
               "ORDER BY \"EMP_ID\", \"LAST_NAME\";\n";
```

we can now write

```
String query = """
               SELECT "EMP_ID", "LAST_NAME" FROM "EMPLOYEE_TB"
               WHERE "CITY" = 'INDIANAPOLIS'
               ORDER BY "EMP_ID", "LAST_NAME";
               """;
```

The latter variant is much more convenient to write and read.

[JEP 378](https://openjdk.java.net/jeps/378)

## Minor improvements

In addition to the following major changes, there are a few minor improvements that can be highlighted as well. 

* Null-pointer exceptions (NPEs) are now user friendly and contain more details regarding the source of the error ([JEP 358](https://openjdk.java.net/jeps/358))

* [Oracle made their JDK and JRE free again](https://www.oracle.com/java/technologies/javase/jdk-faqs.html) (mostly after Amazon and Microsoft started publishing their free builds)

* It is possible to download Oracle JDK without having to login into their portal [by script-friendly commands](https://www.oracle.com/java/technologies/jdk-script-friendly-urls/)

* Garbage collectors continue to evolve by [adding ZGC to the list of provided GCs](https://malloc.se/blog/zgc-jdk17) and [improving the default one (G1)](https://tschatzl.github.io/2021/09/16/jdk17-g1-parallel-gc-changes.html)

## What's next

The next LTS release will be in 2023. It will bring us pattern matching for switch expressions and hopefully many more useful features. It is clear that Java now evolves much faster thanks to the 6 months release schedule and LTS releases once in 2 years. The language becomes more modern and developer-oriented. And this is good news for all Java community, that can be confident in the fact that Java will hold its positions as one of the leading platforms and languages for backend development.