# Using week year (YYYY) in place of year (yyyy) may produce incorrect results
**ID:** `JAVA-E1006` | **Lien:** [DeepSource](https://deepsource.com/directory/java/issues/JAVA-E1006)

![Major](https://img.shields.io/badge/severity-major-orange) ![Bug Risk](https://img.shields.io/badge/type-bug_risk-green)

Java's date formatting can be used to represent a date in terms of either the week of the year or in terms of the day and month, as usual.

The format used to represent years in terms of the week is `YYYY` . It is quite easy to accidentally mix up the normal format for years ( `yyyy` ) with the week year format ( `YYYY` ).

Such a mixup will completely throw off any attempt at parsing dates, and will also produce inconsistent output when formatting dates.


```java
SimpleDateFormat sdf1 = new SimpleDateFormat("YYYY-MM-dd"); // Uses the week-year format.

SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd");

sdf1.parse("2021-03-26"); // Sun Dec 27 00:00:00 UTC 2020 !!!
sdf2.parse("2021-03-26"); // Fri Mar 26 00:00:00 UTC 2021
```
The reason for this behavior is that when `YYYY` is not used in conjunction with the `ww` (week-of-year) field, the resulting date-time formatter will note that there is no `ww` field defined, and will set it along with the `u` (day-of-week) field to whatever is the first day of the week (it may be Sunday or Monday, depending on the locale). This fixes the date to be the first day of the first week of whatever year is parsed by the formatter.

Here is another example of weird behavior:


```java
Date d1 = sdf2.parse("2020-12-29");
System.out.println(sdf1.format(d1)); // 2021-12-29 - the year is 2021 now!
```
On the new year, dates work a bit differently when formatted in terms of the week instead of just the day, month and year. The week that January the 1st falls under is considered to be the first week of the week-year. And thus, a new week-year may even begin on the 27th or the 26th of December of the previous year. December 29th, 2020 is Tuesday of the first week of 2021, and so, is considered to be a part of week-year 2021, not 2020.


## Bad Practice
With `SimpleDateFormat` :


```java
Date badDate = new SimpleDateFormat("YYYY-MM-dd").parse("2021-05-02); // This date will be parsed as the "first day of the first week of 2021".

Date goodDate = new SimpleDateFormat("yyyy-MM-dd").parse("2020-12-28");
String badDateStr = new SimpleDateFormat("YYYY-MM-dd).format(goodDate); // dateStr = "2021-12-28"
```
Using `DateTimeFormatter` :


```java
String dateStr = DateTimeFormatter.ofPattern("YYYY-MM-dd").format(goodDate); // Again, dateStr = "2021-12-28"
```

## Recommended
Use `yyyy` to format dates.


```java
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd");

SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");

// ...
```

## Exceptions
If you actually want to format or parse a date based on the week, use `YYYY` in conjunction with `ww` (week of year) and optionally `u` (day of week) to get correct results:


```java
Date date = new SimpleDateFormat("YYYY-ww-u").parse("2021-37-5"); // This date corresponds to 2021-09-16.

DateTimeFormatter.ofPattern("YYYY-ww-u").format(date); // returns 2021-37-5
```
