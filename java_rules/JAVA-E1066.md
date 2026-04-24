# Private fields which are only set to null should be removed
**ID:** `JAVA-E1066` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1066)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

A private field has been found which is only set to `null` . Despite this, the field is still accessed (either as a method call target, or as a method argument). This could cause a `NullPointerException` to occur.

It may be that logic surrounding this null field is incomplete, hence the lack of an assignment to the field. It may also be that potential assignments have been commented out / removed for debugging purposes.


## Bad Practice
In the example below, `getResponse()` will always throw an exception due to `url` always being null.


```java
class Example {
    // url is not set to anything but null.
    private String url = null;

    String getResponse() throws MalformedURLException, IOException {
        // Will cause a MalformedURLExeption if url is not set!
        URL address = new URL(url);

        // ...
    }
}
```

## Recommended
Make sure to actually set variables to something other than `null` before use. If there is no need to set the value to anything else, remove the null variable entirely and just use `null` when required.

If the variable should be set by API consumers or by other internal code, add a setter that is appropriately accessible.


```java
void setURL(String url) {
    this.url = url;
}
```

## Exceptions
If the concerned field will be assigned through reflection, this issue may be ignored. Make sure to properly document such cases.

