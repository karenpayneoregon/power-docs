# Microsoft Power Fx overview

:bulb: [Web page](https://learn.microsoft.com/en-us/power-platform/power-fx/overview)


# Use variables in a formula

To use a variable in a Power Fx formula, you must suggest a prefix to its name to indicate the variable's scope:

- For [system](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-variables-about#system-variables) variables, use `System`.
- For [global](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-variables-bot) variables, use `Global`.
- For [topic](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-variables-about) variables, use `Topic`.
- For [environment](https://learn.microsoft.com/en-us/microsoft-copilot-studio/authoring-variables-about) variables, use `Environment`.

# To get the current date and time in a specific time zone <!-- markdownlint-disable-line MD025 -->

To get the current date and time in a specific time zone using Power Automate, use the `convertFromUtc()` expression. Power Automate evaluates everything in UTC by default, so you must explicitly convert it to your desired destination time zone.

## Using an Expression

```powerautomate
convertFromUtc(utcNow(), 'Eastern Standard Time', 'yyyy-MM-dd')
```

*Note: Replace 'Eastern Standard Time' with your target time zone identifier and 'yyyy-MM-dd'*

### Common Time Zone String Identifiers

- US Eastern: Eastern Standard Time
- US Mountain: Mountain Standard Time
- US Pacific: Pacific Standard Time
- United Kingdom: GMT Standard Time

### Custom Formatting Reference

- yyyy-MM-dd :arrow_right: 2026-06-24
- MM/dd/yyyy :arrow_right: 06/24/2026
- dd-MM-yyyy HH:mm :arrow_right: 24-06-2026 14:30 (24-hour time)
- hh:mm tt :arrow_right: 02:30 PM (12-hour time with AM/PM)


## Accommodate time zones

The Date and time entity captures a date and time in Coordinated Universal Time (UTC). However, you might want to display the date and time based on the user's location instead.

:bulb: [Web page](https://learn.microsoft.com/en-us/microsoft-copilot-studio/manage-date-and-time)

### Get the user's time zone

Use these system variables to get information about the user's time zone:

- `Conversation.LocalTimeZone` (read-write): Stores the user's time zone as a string. You can optionally set this variable to any time zone listed on the Noda Time website.
- `Conversation.LocalTimeZoneOffset` (read-only): Gets the UTC offset from Conversation.LocalTimeZone and stores it as a time value.


### Display the date and time in the local time zone

Copilot Studio stores the date and time in UTC. Before displaying a date and time to customers, add the time zone offset to convert the value to the user's local time zone.

In this example, we get the current date and time using the Power Fx `Now()` function, and then add the time zone offset. It isn't possible to use the `Conversation.LocalTimeZoneOffset` system variable directly in a Power Fx formula. Instead, we use a Set variable value node to create a variable and then assign it the value of `Conversation.LocalTimeZoneOffset`.

:bulb: [Web page](https://learn.microsoft.com/en-us/microsoft-copilot-studio/manage-date-and-time#display-the-date-and-time-in-the-local-time-zone)

## Now, Today, IsToday, UTCNow, UTCToday, IsUTCToday functions

| Functions | Applies to                                                                                                                                     |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Now       | Canvas apps, Copilot Studio, Desktop flows, Dataverse formula columns, Model-driven apps, Power Platform CLI, Dataverse functions, Power Pages |
| Today     | Canvas apps, Copilot Studio, Desktop flows, Dataverse formula columns, Model-driven apps, Power Platform CLI, Dataverse functions, Power Pages |
| IsToday   | Canvas apps, Copilot Studio, Desktop flows, Model-driven apps, Power Platform CLI, Dataverse functions, Power Pages                            |
| UTCNow    | Dataverse formula columns, Power Pages                                                                                                         |
| UTCToday  | Dataverse formula columns, Power Pages                                                                                                         |

:bulb: [Web page](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-now-today-istoday)

## Common Date Functions

- **Date**: The Date function converts individual Year, Month, and Day values to a Date/Time value. [:green_book:](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-date-time)
  - Date(year, month, day): Converts individual numerical components into a standard Date/Time value.
- **DateAdd**: Adds or subtracts units to a date/time (e.g., DateAdd(Today(), 5, TimeUnit.Days)).
- **DateDiff**: Subtracts two dates to find the difference (e.g., DateDiff(StartDate, EndDate, TimeUnit.Days)).
- **DateValue, TimeValue, and DateTimeValue** functions [:green_book:](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-datevalue-timevalue)
  - **DateValue** function converts a date string (like "10/01/2014") to a date/time value.
  - **TimeValue** function converts a time string (like "12:15 PM") to a date/time value.
  - **DateTimeValue** function converts a date and time string (like "January 10, 2013 12:13 AM") to a date/time value.

1. **Calculate a Future or Past Date** To calculate a date 14 days in the future, set a variable using the DateAdd function:DateAdd(Today(), 14, TimeUnit.Days)
1. **Find the Number of Days Between Two Dates** To find the duration between two date/time variables (e.g., Topic.StartDate and Topic.EndDate), use DateDiff:DateDiff(Topic.StartDate, Topic.EndDate, TimeUnit.Days)
1. **Extract Date Components** If you only need the specific day, month, or year from a variable, use the respective extraction functions:
    - Day(Topic.MyDate) or Day(datetime)  Returns the day of the month as a number from 1 to 31.
    - Month(Topic.MyDate) or Month(datetime)  Returns the month as a number from 1 to 12.
    - Year(Topic.MyDate) or Year(datetime)  Returns the year (e.g., 2026)
    - Weekday(datetime): Returns the day of the week, ranging from 1 (Sunday) to 7 (Saturday)

### Conversion

- DateValue(string) / DateTimeValue(string): Converts a recognized text string (like "May 10, 2026") into an actual date/time variable

:bulb: [Web page](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-datetime-parts) Day, Month, Year, Hour, Minute, Second, and Weekday functions

## Conditional

[If and Switch](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-if) functions

The **If** function tests one or more conditions until a true result is found. If such a result is found, a corresponding value is returned. If no such result is found, a default value is returned. In either case, the returned value might be a string to show, a formula to evaluate, or another form of result.

**Syntax**

If( Condition, ThenResult [, DefaultResult ] )

If( Condition1, ThenResult1 [, Condition2, ThenResult2, ... [ , DefaultResult ] ] )

- Condition(s) - Required. Formula(s) to test for true. Such formulas commonly contain comparison [operators](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/operators) (such as <, >, and =) and test functions such as [IsBlank](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-isblank-isempty) and [IsEmpty](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-isblank-isempty).
- ThenResult(s) - Required. The corresponding value to return for a condition that evaluates to true. 
- DefaultResult - Optional. The value to return if no condition evaluates to true. If you don't specify this argument, blank is returned.

The **Switch** function evaluates a formula and determines whether the result matches any value in a sequence that you specify. If a match is found, a corresponding value is returned. If no match is found, a default value is returned. In either case, the returned value might be a string to show, a formula to evaluate, or another form of result.

## Text function

The **Text** function formats a number or a date/time value based on one of these types of arguments:

- A predefined date/time format, which you specify by using the `DateTimeFormat` enumeration. For dates and times, this approach is preferred as it automatically adjusts to each user's language and region.
- A custom format, which comprises a string of placeholders that define, for example, whether numbers show a decimal separator and dates show the full name of the month, the month as an abbreviation, or the month as a number. Power Apps supports a subset of the placeholders that Microsoft Excel does. In this string, the language placeholder specifies the language in which to interpret the other placeholders. If the custom format includes a period, for example, the language-format placeholder specifies whether the period is a decimal separator (ja-JP) or a thousands separator (es-ES).

### Core Text Functions Available

- `Text()`: Converts numbers, choices, or dates into text. Useful for formatting, e.g., Text(1234.567, "$#,##0.00")
- `Concatenate()` / &: Joins multiple text strings together. e.g., "Hello " & Topic.UserName.
- `Substitute()`: Replaces a part of a string with another string.
- `Left()` / `Right()` / `Mid()`: Extracts a specified number of characters from a text string. [:green_book:](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-left-mid-right)
  - [Single string](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-left-mid-right#single-string) examples
    - Left("Hello World", 5) returns "Hello"
    - Right("Hello World", 5) returns "World"
    - Mid("Hello World", 7, 5) returns "World"
  - [Single-column table](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-left-mid-right#single-column-table) examples
- `Upper()` / `Lower()` / `Proper()`: Converts the case of a string. [:green_book:](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-lower-upper-proper)
- `Trim()` / `TrimEnds()`: Removes unnecessary spaces from strings
- `StartsWith` / `EndsWith`: Checks if a string starts or ends with a specific substring. [:green_book:](https://learn.microsoft.com/en-us/power-platform/power-fx/reference/function-startswith)


### Common Use Cases

1. **Choice to Text Conversion**: Use **Text**(`Topic.ChoiceVariable`) in a variable assignment node to convert an entity choice into a standard string before sending it to tools like SharePoint or Dataverse.
1. **Number Formatting**: Format numbers or currencies before outputting them as a message to the user.
1. **Data Extraction**: Parse out specific details from a user's raw input string.

## Create expressions using Power Fx

Power Fx is a low-code language that uses Excel-like formulas. Use Power Fx to create complex logic that allows your agents to manipulate data.

:bulb: [Web page](https://learn.microsoft.com/en-us/microsoft-copilot-studio/advanced-power-fx)

## Youtube

- Using [Power FX](https://www.youtube.com/watch?v=wOEKVS7AQZY) with Microsoft Copilot Studio | EP26