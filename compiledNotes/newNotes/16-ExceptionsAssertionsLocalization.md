# Chapter 16 - Exceptions, Assertions, and Localization

# EXAM UPDATE
- Asserts removed from the exam

### Reviewing Exceptions
- var is allowed in try-with-resource
- Throwable is the parent of all Exceptions
- NumberFormatException    < IllegalArgumentException
- FileNotFoundException    < IOException
- NotSerializableException < IOException

# Creating Custom Exceptions
- `class CannotSwimException extends Exception {}`
- "Checked" vs "unchecked" is inherited since being a subclass is transitive. 
- Three most common Exception constructors
    * No argument constructor created manually
    * One arg constructor (Exception) that wraps the "cause" exception
    * One arg constructor (String)    that passes a message

### Stack Traces
- defn: shows the exception along with the method calls it took to get there
- e.printStackTrace()

# try-with-resource
- aka automatic resource management 
- resources must implement AutoCloseable interface (.close() method)
- implicit finally
- semicolon separated
- NEW: possible to use resources declared prior to the try-with-resource if final/effectively_final
- NEW: just use the resource name instead of declaration
```
final var bookReader = new FileReader("A");
try(bookReader) {
    //
}
```
- `e.getSuppressed()`
- Suppressed exceptions - when multiple exceptions are thrown, all but the first are suppressed
- exceptions in implicit finally can be suppressed if the try statement throws a fatal exception

---------------------------------------------------------------------------------------------------

# Assertions
- removed from exam

---------------------------------------------------------------------------------------------------

# Working with Dates & Times
- The new Java 11 Exam reduced the amount of date/time classes that we need to know
- Still need to know how to format dates, just not the classes themselves. 

### Creating Dates & Times
- Before Java 8: `Date`          and `SimpleDateFormat`
- New    Java 8: `LocalDate`     and `DateTimeFormatter`
- LocalDate     - day,month,year        - `2020-10-14`
- LocalTime     - time of day           -            `12:45:20.000`
- LocalDateTime - day and time w/o TZ   - `2020-10-14T12:45:20.000`
- ZonedDateTime - Date and time with TZ - `2020-10-14T12:45:20.000-04:00[America/New_York]`
- All four of the classes have static factory methods
    * .now()
    * .of()
        - `LocalDate.of(2020, 10, 20);`
        - You can omit params and defaults will be provided 
        - `LocalTime.of(6, 15)` // no seconds
        - `LocalDateTime.of(LocalDate.now(), LocalTime.now())` // passing LocalDate and LocalTime as params to LDT
- All constructors are private for LocalDate. Factory methods are the only way. 
- Months are 1-indexed (october = 10). Alternatively. `Month.OCTOBER` enum was created

### Formatting Dates and Times
- .getDayOfWeek() // TUESDAY
- .getMonth       // OCTOBER
- .getYear        // 2020
- .getDayOfYear() // 294
- DateTimeFormatter class.
- Prebuilt formats:
    * `ld.format(DateTimeFormatter.ISO_LOCAL_DATE)`
- custom formats:
    * `DateTimeFormatter.ofPattern("MMMM dd, yyyy 'at' hh:mm");`
- DateTimeFormatter cant be used with the wrong type (using time for date or date for time)
- Format symbols: `y,d,M,h,m,s` all obvious. `a` = "am/pm", `z` = Time Zone name, `Z` = Time Zone offset (-0400)
- `formatterObject.format(ldtObject) vs ldtObject.format(formatterObject)` equivalent. Same thing, different caller.
- You can escape the formatter syntax using `''` single quotes. `'at'`
    * Using single quotes `"MMMM dd 'Party''s at'"` // double single quotes next to each other. using single quote to escape single quote

---------------------------------------------------------------------------------------------------

# i18n and l10n
- defn: i18n - internationalization (i18n) is the process of designing and developing software or products that can be adapted to different languages and cultures
- defn: l10n -  localization (l10n) is the process of adapting a product or content for a specific locale or market
- l10n includes (country, language) pair and formatting dates and numbers in the correct locale
- Builder pattern = creational design pattern where object is first created, then properties are set with methods
- Factory pattern = creational design pattern where a factory class is used to provide instances of an object instead of relying on constructors. 

### Locale class
- java.util
- `Locale locale  = Locale.getDefault()` //en_US
- Locale identifier:
    - en_US
    - lowercase language code
        * en
    - Uppercase country code:
        * US
- Constants
    * Locale.GERMAN  // de
    * Locale.GERMANY // de_DE
- Constructors
    * new Locale("fr")       // fr
    * new Locale("hi", "IN") // hi_IN
- Builder Pattern (not factory!)
    `Locale usLoc = new Locale.Builder().setLanguage("en").setRegion("US").build();`
- Set locale
    * `Locale.setDefault(otherLocale)`

### Localizing Numbers
- java.text
- formatting turns (Object/primitives) into (Strings)
- NumberFormat
- NumberFormat factory methods
    * .getInstance()           - general-purpose formatter
    * .getNumberInstance()     - same as .getInstance()
    * .getCurrencyInstance()   - For formatting monetary amounts
    * .getPercentageInstance() - For formatting percentages
    * .getIntegerInstance()    - Rounds decimal values before displaying
    * all above methods are overloaded for (Locale otherLocale) param.
- .format()
    * `nformatter.format(3_200_000)` // 3,200,200
    * `nformatter.getCurrencyInstance.format(48d)` // $48.00
- .parse()
    * returns Number object
    * throws checked exception `ParseException`
    * `nformatter.parse("40,45")` // 40.45 in en_US
    * `(Double) nformatter.getCurrencyInstance.parse("$92,807.99")` // $92807.99 cast Number -> Double
- Custom Number Formatter
    * new DecimalFormat("###,###,###.0") 
    * `#` = Omit the position if no digit
    * `0` = Put 0 in position if no digit exists

### Localizing Dates
- 
    * DateTimeFormatter.ofLocalizedDate(dateStyle)
    * DateTimeFormatter.ofLocalizedTime(timeStyle)
    * DateTimeFormatter.ofLocalizedDateTime(dateTimeStyle)
    * DateTimeFormatter.ofLocalizedDateTime(dateStyle, timeStyle)
- .withLocale()
    - `dtf.format(dateTimeObject).withLocale(otherLocale)`
- Locale.Category enum specifies
- You can set Locale to  display and format in different formats. 
- Locale.DISPLAY in one locale but Locale.FORMAT in another

---------------------------------------------------------------------------------------------------

### Resource Bundles
- defn: resource bundle - contains the locale-specific objects to be used by a program
- Similar to a map with keys and values
- commonly stored in a .properties file
- .properties files are in key=value format
- Zoo_en
- Zoo_fr
    - var rb = ResourceBundle.getBundle("Zoo", locale)
    - rb.getString("ZOO_TITLE_CASE")
    - rb.keySet()
- .getBundle("name")
- .getBundle("name", locale)
- Bundles are a hierarchy that override the values above if present
- Bundle resolution
    1. Exact _fr_FR
    2. Just request language _fr
    3. default _en_US
    4. default just language _en
    5. no locale (Zoo.properties)
- If resource not in any bundle, then `MissingResourceException` thrown at runtime

### Formatting Messages
- `MessageFormat.format("Hello, {0} and {1}", "Tammy", "Henry")`
- Different than `String.format()` ??? Research

### Properties Class
- Properties class functions like a `HashMap<String, String>`
- new Properties()
- props.setProperty("name", "San Diego Zoo")
- .getProperty("camel") // null
- props.get() is map.get(). Shouldn't be used. use .getProperty()
- props.getProperty() is map.getOrDefault()
- props.getProperty("missingKey", "defaultValue")








---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
