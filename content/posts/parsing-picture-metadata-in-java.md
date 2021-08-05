---
title: "Parsing picture metadata in Java"
date: 2021-08-05T16:20:00+02:00
description: "Java has a good library to parse date, orientation, and much more metadata from pictures."
tags: [Java]
---

If your system supports images, then most likely you need to extract some metadata from them on the back-end. Luckily for you, [Drew Noakes](https://github.com/drewnoakes) has just the right library for that: [metadata-extractor](https://github.com/drewnoakes/metadata-extractor).

## Adding to dependencies

Maven

```xml
<dependency>
    <groupId>com.drewnoakes</groupId>
    <artifactId>metadata-extractor</artifactId>
    <version>2.16.0</version>
</dependency>
```

Gradle

```groovy
dependencies {
    implementation 'com.drewnoakes:metadata-extractor:2.16.0'
}
```

## Example of fetching picture capture date and orientation

```java
public Date parseDateCaptured(byte[] pictureData)
    throws ImageProcessingException, IOException {
    var metadata = ImageMetadataReader.readMetadata(new ByteArrayInputStream(data));
    var exifDirectory = metadata.getFirstDirectoryOfType(ExifSubIFDDirectory.class);
    return exifDirectory.getDate(ExifSubIFDDirectory.TAG_DATETIME_ORIGINAL);
}

public int parseOrientationDegrees(byte[] pictureData)
    throws ImageProcessingException, IOException {
    var metadata = ImageMetadataReader.readMetadata(new ByteArrayInputStream(data));
    var exifDirectory = metadata.getFirstDirectoryOfType(ExifSubIFDDirectory.class);
    return exifDirectory.getInt(ExifIFD0Directory.TAG_ORIENTATION);
}
```

The examples above are very straightforward and do not perform any data validation. In the production code, you definitely should check the presence of required tags before extracting these attributes.

You may check out my usage of this library in [one of the PalermoPG classes](https://github.com/ruslanlesko/palermopg/blob/master/src/main/java/com/leskor/palermopg/meta/MetaParser.java).

## Formats

I showed the example on one of the most popular metadata formats: Exif. However, this library support much more, even video and audio metadata. Checkout [the GitHub repo](https://github.com/drewnoakes/metadata-extractor) for more information.