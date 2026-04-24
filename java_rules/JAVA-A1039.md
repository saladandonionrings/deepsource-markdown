# Audit: File is set as world readable or writable
**ID:** `JAVA-A1039` | **Link:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-A1039)

![Critical](https://img.shields.io/badge/severity-critical-red)![Security](https://img.shields.io/badge/type-security-red)

This code appears to open a file using [`Context.openFileOutput(String, int)`](https://developer.android.com/reference/android/content/Context#openFileOutput(java.lang.String,%20int)), but sets the mode (the second argument) to be one of [`Context.MODE_WORLD_READABLE`](https://developer.android.com/reference/android/content/Context#MODE_WORLD_READABLE) or [`Context.MODE_WORLD_WRITABLE`](https://developer.android.com/reference/android/content/Context#MODE_WORLD_WRITEABLE).

This is dangerous; it will always throw a `SecurityException` in android versions above jellybean (API level 17), and in the worst case could be abused by a malicious actor to access or manipulate data and even code.


## Bad Practice

```java
FileOutputStream fos = openFileOutput("somefile.txt", Context.MODE_WORLD_READABLE);
```

## Recommended
Use the `MODE_PRIVATE` or `MODE_APPEND` (if your file already exists) instead to privately create a writable file.


```java
FileOutputStream fos = openFileOutput("somefile.txt", Context.MODE_PRIVATE);
```
If you need to expose this file to other applications/activities, consider using the [content provider API](https://developer.android.com/reference/android/content/ContentProvider) to do so instead.

