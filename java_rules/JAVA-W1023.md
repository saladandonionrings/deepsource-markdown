# Zip entries should not be empty
**ID:** `JAVA-W1023` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-W1023)

![Major](https://img.shields.io/badge/severity-major-orange) ![Anti-pattern](https://img.shields.io/badge/type-anti_pattern-purple)

Zip file entries should not be ended after writing 0 bytes of data.

Java's [ `ZipOutputStream` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/ZipOutputStream.html) class allows one to build a zip archive file by file. To do so, a new [ `ZipEntry` ](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/zip/ZipEntry.html) structure must be added to the archive through `ZipOutputStream.putNextEntry()` , followed by the bytes of the file itself, then a call to `ZipOutputStream.closeEntry()` .

The `ZipEntry` class only holds data regarding a file, not the file itself. There is always a possibility that the developer will forget to write bytes to the archive after creating a new entry. This issue will be raised if there is a call to `putNextEntry()` followed by a call to `closeEntry()` , without a call to `write()` in between.


## Bad Practice

```java
ZipOutputStream zos = ...;

// ...

ZipEntry entry = new ZipEntry(...);

zos.putNextEntry(entry);
// No call to write!
zos.closeEntry();
```

## Recommended
Make sure to actually write bytes for each created zip entry.


```java
zos.putNextEntry(entry);
zos.write(...);
zos.closeEntry();
```

## Exceptions
If your intent is to explicitly create an empty file in the zip archive, you can safely disregard this issue. Just make sure there is no mistake in your logic.

