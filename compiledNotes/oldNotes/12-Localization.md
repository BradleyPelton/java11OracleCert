# Localization
- Defn i18n = Internalization
- Same application world wide, but adapting to regions

- Defn i10n - Localization
- Translating user interface elements
- changing numbers, dates, currency, etc.

### Locale Class
- each class represents a specific region
- Syntax and semantics of language tags
- fields of Locale class:
    * language - ISO 639 alpha 2 or 3 better langauge code (e.g. "en)
    * script - ISO 15924 alpha 4 letter script code (e..g "Latn" latin)
    * Country - ISO 3166 alpha 2 country code (e.g. "US")
    * Variant (e.g. "WIN" - Windows)
    * Extensions - key value pairs

### Locale has three constructors
- new Locale(String language)
- new Locale(String language, String country)
- new Locale(String language, String country, String variant)
- `Locale aLocale = new Locale("fr", "FR");`
- Theres a builder() class as well

### Locale Class Methods
- getters for country, language, variant, script, ext, default
- getDisplayXxx() - returns the names instead of codes for each fields

### ResourceBundle Class
- contains locale-specific resource as part of the current user's locale
- user for static locale data
- dynamic data is handled elsewhere (such as date)
- ResourceBundles contains key-value pairs
- a key represents a locale-specific object in the bundle
- key:String value:Object

### Properties File for Resource Bundle
- text file
- key-value combinations
- default file
- also Locale specific property files
- same key names across all property files
```
public class TextBumdle {
    psvm {
        Locale loc = new Locale("en", "US");
        ResourceBundle msgBundle = ResourceBundle.getBundle("TextBundle", loc);
        System.out.print(msgBundle.getString("msg1"))
        System.out.print(msgBundle.getString("msg2"))
    }
}
```

###
- oracle ADF Faces
- ADF faces is an implementation of JSF web components
- ADF faces support localization richy

### Bi-direction Text
- Two directionalities
    * LTR - left-to-right
    * RTL - Right-to-left
- reading left to RTL instead of LTR
- back button, play button, are reversed
- Default right alignment instead of left alignment for text
- TextLayout class richly supports bidirectional text, carets, etc.

### Currency Formatting
- Currency Class
- ISO 4217 code
- getInstance
- getSymbol
```
Double amount = 54628.7
Locale currLocale = new Locale("en","US")
Currency currCurrency = Currency.getInstance(currLocale)
NumberFormat currForm = NumberFormat.getCurrencyInstance(currLocale);
System.out.println(currForm.format(amount))
```

### Dates and Time
- only formatting of Dates and Times is on the exam
- import java.time.*
- LocalDate
- LocalTime
- LocalDateTime
- ZonedDateTime
```
LocalDate lDate = LocalDate.of(2022, 10, 31) // 2022-10-31
LocalTime lTime = LocalTime.of(9, 45, 00, 00) // 09:45
LocalDateTime ldt = LocalDateTime.of(2022, 10, 31, 9, 45) // 2022-10-31T09:45
ZonedDateTime zdt = ZonedDateTime.of(2022,10,31,9,45,00,00, ZoneId.of("America/Chicago"));
// 2022-10-31T09:45-85:00[America/Chicago]
String a = ldt.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME)
var pattern = DateTimeFormatter.ofPattern("YYYY-MM-DD");
String formattedStr = ldt.format(pattern)
```

### Date Localization
- 
### LocalDateTime class
- doesn't store timezone
- java.time.LocalDateTime
- methods
    * of()
    * getDayOfXxxx(month,week,year)
    * getXxx(hour,minute,month,etc.)
    * how()
    * format

# LocaleTime
- hour, min, sec
- nanosecon precision
- ISO-8601
- methods
    * of()
    * format
    * getXxx()
    * isAfter()
    * isBefore()
    * plusXxx() (e.g. plusHours)
    * minusXxx() (e.g. minusMinutes)

### DateTimeFormatter Class
- used to print and parse date-time objects
- predefined constants (ISO_LOCALE_DATE)
- Patterns (yyyy-MMM-dd)
- Localized styles (e.g. long medium)
- DateTimeFormatBuilder for more complex formatting
- methods
    * ofLocalizedDate, ofLocalizedDateTime, ofLocalizedTime
    * ofPattern
    * getXxxx()     getZone, getLocale, etc.
    * withZone()
    * withLocale()
    * localizedBy()
```
public class DateTimeDemo {
    psvm {
        LocalDateTime ldt = LocaleDateTime.now();
        String pattern = "dd-MMMM-yyyy HH:mm:ss.SSS";
        DateTimeFormatter form = DateTimeFormatter.ofPattern(pattern);
        String formattedDT= ldt.format(form)
        System.out.println(formattedDT)
    }
}
```

### TimeZone Class
- represents a timezone offset
- figures in daylight savings
- getDefault() for current timezone
- getTimeZone("America/Los_Angeles")
- methods
    * getDisplayName()
    * setDefault()
    * toZoneId()
    * useDaylightTime()
    * setId()

### ZonedDateTime Class
- represents a date-time with time-zone
- 20231-09-03T12:44:30+01:00 Europe/Paris
- methods
    * format()
    * getDayOfMonth, getDayOfWeek, ...
    * getSecond, getMinute, ...
    * of()
    - minusXxx()
    - plusXxx()

```
public class TimeZoneTest {
    psvm {
        Locale.setDefault(Locale.Germany);
        TimeZone ltz = TimeZone.getDefault();
        sout(ltz.getDisplayName(Locale.getDefault));
    }
}
public class ZonedDateTimeDemo {
    psvm {
        ZonedDateTime zdt = ZonedDateTime.now(ZoneId.of("Asia/Tokyo"));
        System.out.println(zdt.toString());
        System.out.println(zdt.toLocalDateTime().getDayOfMonth());
    }
}
```

### TimeStamp Class
- wrapper around java.util.Date
- yyyy-mm-dd hh:mm:ss
- methods
    * before()
    * after()
    * toLocalDateTime()
    * valueOf()
    * compareTo()
    * equals()

```
public class TimeStamp {
    psvm {
        LocaleDateTime ldt = LocalDateTime.now();
        TimeStamp timeStamp = TimeStamp.valueOf(PDT);
        System.out.println(timestamp.toString());
    }
}
```

### Number Localization
- NumberFormat class
- Abstract class for all number formats
- Interface for formatting and parsing Numbers
- Locale specific number formatting
- Locale independent code
- methods
    * format()
    * getCurrencyInstance()
    * setMinimumFractionDigits()

```
public class FormatNumberDemo {
    psvm {
        double num = 32360.856974
        NumberFormat nf = NumberFormat.getInstance(new Locale("en", "US"));
        String val = nf.format(num);
        sout(val); // 32,360.857
    }
}
```

### DecimalFormat Class
- format()
- applyPattern()
- toLocalizedPattern()








