# Power Fx Data Types

Power Fx works with small, discrete values, much like spreadsheet cells. Each value has a data type that determines how Power Fx formats it, validates it, stores it, and maps it to external data sources.

## Data Type Summary

| Data type | Description | Example |
|---|---|---|
| Boolean | A true or false value. | `true` |
| Choice | A value selected from a defined option set. The displayed label is localizable, while the stored value is numeric. | `ThisItem.OrderStatus` |
| Color | A color value, including alpha transparency. | `Color.Red`, `ColorValue("#102030")`, `RGBA(255, 128, 0, 0.5)` |
| Currency | A number formatted as currency. Internally treated like a numeric value. | `123`, `4.56` |
| Date | A date without a time component, interpreted in the app user’s time zone. | `Date(2019, 5, 16)` |
| DateTime | A date and time value, interpreted in the app user’s time zone. | `DateTimeValue("May 16, 2019 1:23:09 PM")` |
| Decimal | A high-precision base-10 number. Best for business calculations. | `Decimal("1.2345")` |
| Dynamic | A runtime value whose type can vary. Commonly returned from JSON parsing. | `ParseJSON("{ ""Field"" : 1234 }").Field` |
| Float | A base-2 floating-point number with wide range but limited decimal precision. | `8.903e121` |
| GUID | A globally unique identifier. | `GUID()` |
| Hyperlink | A text string containing a URL. | `"https://powerapps.microsoft.com"` |
| Image | A URI string that refers to an image resource. | `"https://northwindtraders.com/logo.jpg"` |
| Media | A URI string that refers to audio or video. | `"https://northwindtraders.com/intro.mp4"` |
| Number | Alias for `Decimal` in most Power Fx hosts, or `Float` in canvas apps. | `123`, `1e4` |
| Record | A structured value made up of named fields. | `{ Company: "Northwind Traders", Staff: 35 }` |
| Record reference | A reference to a row in a table, often used with polymorphic lookups. | `First(Accounts).Owner` |
| Table | A set of records with consistent field names and data types. | `Table({ FirstName: "Sidney" }, { FirstName: "Nancy" })` |
| Text | A Unicode text string. | `"Hello, World"` |
| Time | A time value without a date. | `Time(11, 23, 45)` |
| Void | Used for behavior user-defined functions that do not return a value. | `Hi(): Void = { Notify("Hello!") }` |
| Yes/No | A two-option choice backed by a Boolean value. | `ThisItem.Taxable` |

## Blank Values

All Power Fx data types can contain a blank value.

Use `Blank()` to clear a value:

```powerfx
Set(x, Blank())
```

Use `IsBlank()` to test for blank values, and `Coalesce()` to replace blanks with fallback values.

```powerfx
Coalesce(NickName, FirstName, "Friend")
```

Because blanks are allowed, Boolean and Yes/No values effectively support three states:

- `true`
- `false`
- blank

## Text, Hyperlink, Image, and Media

Text, Hyperlink, Image, and Media values are all based on Unicode text strings.

### Embedded Text

Text strings are enclosed in double quotation marks.

To include a double quote inside a string, use two double quotes:

```powerfx
Notify("Jane said ""Hello, World!""")
```

### String Interpolation

Use `$"..."` strings to embed formulas directly inside text.

```powerfx
$"We have {Apples} apples, {Bananas} bananas, yielding {Apples + Bananas} fruit total."
```

You can use functions inside interpolated strings:

```powerfx
$"Welcome {Coalesce(NickName, FirstName)}, it's great to meet you!"
```

You can also nest interpolated strings:

```powerfx
$"Welcome {Coalesce(Trim($"{First} {Middle} {Last}"), "Friend")}!"
```

### Newlines

Text strings can contain line breaks:

```powerfx
"Line 1
Line 2
Line 3"
```

String interpolation also supports line breaks:

```powerfx
$"Line {1}
Line {1 + 1}
Line {1 + 1 + 1}"
```

### Image and Media Resources

Images, audio, and video can be added as app resources. Power Fx represents these resources as URI text strings.

An image can come from:

- An app resource
- A web URL
- A data URI
- A camera control
- A database-backed media reference

Example image URL:

```powerfx
"https://northwindtraders.com/logo.jpg"
```

Example data URI:

```powerfx
"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAkAAAAFAQMAAACtnVQo..."
```

When an image or media value is saved to a database, the app sends the actual media data, not just the URI reference.

## Numbers

Power Fx supports two numeric types:

| Type | Best for | Notes |
|---|---|---|
| Decimal | Business calculations | Base-10 precision, avoids many rounding issues |
| Float | Scientific or high-range calculations | Base-2 floating point, wider range, approximate precision |

> Note: Power Apps currently supports Float for numbers. Decimal support is planned.

### Decimal

`Decimal` is intended for precise base-10 calculations.

Example:

```powerfx
Decimal("1.2345")
```

Decimal is a better fit for financial and business calculations where precision matters.

### Float

`Float` uses double-precision floating-point behavior. It supports a very large range but can produce small rounding differences.

Example:

```powerfx
Float("1.234")
```

A calculation such as this may not produce exactly zero:

```powerfx
(55 / 100 * 100) - 55
```

That is expected behavior for floating-point arithmetic.

### Numeric Defaults and Conversions

In most Power Fx hosts, literal numbers are Decimal by default:

```powerfx
1.234 * 2
```

Convert text to Decimal:

```powerfx
Value("1.234")
Decimal("1.234")
```

Convert to Float:

```powerfx
Float(1.234)
Float("1.234")
```

Convert back to text:

```powerfx
Text(DecimalValue)
Text(FloatValue)
```

### Mixing Decimal and Float

Decimal and Float values can be mixed, but Decimal values are converted to Float when combined with Float values.

That can lose precision, so avoid mixing them unless there is a good reason.

```powerfx
1.0000000000000000000000000001 * 2
```

Using Float changes the calculation behavior:

```powerfx
1.0000000000000000000000000001 * Float(2)
```

## Date, Time, and DateTime

Power Fx date and time values are affected by time zone behavior.

### Time Zone Categories

| Category | Description |
|---|---|
| User local | Stored in UTC, displayed according to the app user’s time zone |
| Time zone independent | Displayed the same way regardless of the user’s time zone |

Canvas apps use the browser or device time zone. Model-driven apps use the user’s Dataverse time zone setting.

### UTC Conversion

Use Power Fx date/time functions to convert between local time and UTC:

```powerfx
DateAdd(...)
TimeZoneInformation(...)
TimeZoneOffset(...)
```

### Numeric Date/Time Values

Canvas apps store and calculate date/time values in UTC. The `Value()` function returns the number of milliseconds since:

```text
January 1, 1970 00:00:00.000 UTC
```

Example:

```powerfx
Value(Date(1970, 1, 1))
```

This may not return zero depending on the user’s time zone.

## Unix Time Conversion

Unix time is measured in seconds since January 1, 1970 UTC.

Canvas apps use milliseconds, so multiply Unix seconds by `1000`.

```powerfx
Text(1000000000 * 1000, DateTimeFormat.UTC)
```

To convert a Power Fx date/time value to Unix time:

```powerfx
RoundDown(Value(UnixTime) / 1000, 0)
```

To convert Unix time to a Power Fx date:

```powerfx
DateAdd(Date(1970, 1, 1), UnixTime, Seconds)
```

## SQL Server Date/Time Notes

SQL Server date/time types such as `datetime` and `datetime2` do not include time-zone offset information.

Canvas apps treat these values as UTC and user-local unless corrected.

For SQL Server `time` values, canvas apps read and write ISO 8601 duration strings.

Example conversion:

```powerfx
With(
    Match(
        "PT2H1M39S",
        "PT(?:(?<hours>\d+)H)?(?:(?<minutes>\d+)M)?(?:(?<seconds>\d+)S)?"
    ),
    Time(Value(hours), Value(minutes), Value(seconds))
)
```

## Mixing Date and Time Values

`Date`, `Time`, and `DateTime` have different names, but they all hold the same information about dates and times.

A **Date** value can include time information with it, which is usually midnight. A Time value can carry date information, which is usually January 1, 1970. Dataverse also stores time information with a Date Only field but shows only the date information by default. Similarly, canvas apps sometimes distinguish between these data types to determine default formats and controls.

Adding and subtracting date and time values directly isn't recommended because time-zone and other conversions could cause confusing results. Either use the Value function to convert date/time values to milliseconds first and take into account the app user's time zone, or use the [DateAdd]([https://](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-dateadd-datediff)) and [DateDiff]([https://](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-dateadd-datediff)) functions to add or subtract from one of these values.


## Choices and Yes/No

Choice and Yes/No fields display localized labels but store language-independent values.

A Choice value is stored as a number.

A Yes/No value is stored as a Boolean.

Do not compare a choice directly to display text:

```powerfx
// Avoid
If(ThisItem.OrderStatus = "Active", ...)
```

Compare against the choice enumeration instead:

```powerfx
If(ThisItem.OrderStatus = OrderStatus.Active, ...)
```

For a Yes/No field, this is valid:

```powerfx
If(ThisItem.Taxable = TaxStatus.Taxable, ...)
```

Because Yes/No values are Boolean-backed, this is also valid:

```powerfx
If(ThisItem.Taxable, ...)
```

## Key Takeaways

- Every Power Fx value has a data type.
- All data types can be blank.
- Text, Hyperlink, Image, and Media are string-based.
- Decimal is better for business precision.
- Float is better for very large numeric ranges.
- Date/time values require care because of time-zone handling.
- Choices should be compared using their enumeration values, not their display labels.
