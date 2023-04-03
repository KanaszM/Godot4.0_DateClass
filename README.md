# Godot 4.x Date Class
The `Date` class is a powerful tool for precise manipulation of `day`, `month`, `year` values. It can set, clamp, increment, and format these values, and also generate calendars and calculate `ISO 8601` day and week numbers.

## How to use
1. Place the `Date.gd` file anywhere in your project directory.
2. Call the `Date(day: int, month: int, year: int)` constructor to create a new `Date` object. If any of these arguments are omitted, they will default to the current day, month, or year.
```godot
var date_today: Date = Date() # Will create a date with the current date values
var date_first_of_april_2023: Date = Date(1, 4, 2023) # Will create a date representing the first of April 2023
````
* The `day`, `month` or `year` values can be changed by assigning values directly or by calling the `set_day`, `set_month`, `set_year` methods.
The set values will be automatically clamped, meaning that setting `date.day = 50` will be clamped to the last day of the current `date.month`
* The `increment_day`, `increment_month`, `increment_year` methods can be used to increment positively if the `mode` parameter is `True` else negatively by the value of `max_iteration` parameter.
```godot
var date: Date = Date(30, 4, 2023) # A date representing the last of April 2023
date.increment_day(True, 1) # Will increment the day value by one. The date will now change to represent the first of May 2023
````
**Consider the in-engine documentation for more information on the class members and methods.**

## Features
### Overriding Parameters
Methods that include the parameters `d`, `m`, and `y` allow for the overriding of the `day`, `month`, and `year` values within the class. If any of these arguments are set to an integer value, the corresponding member will be replaced with the provided value. If any of these arguments are left null, the method will use the respective member value. This provides a flexible way to modify specific date components within the method while keeping other components intact.

### Builder Pattern
Any method that returns a `Date` value will in fact return the same class reference. This enables the use of the builder pattern, allowing for chained method calls to set different components at once.

```godot
func _ready():
    # Calling the Date constructor without any arguments, will return the current date.
    date: Date = Date.new()
    # Combine the set_day() and set_year() methods to set both the day and year of the date object.
    date.set_day(4).set_year(1991)
```

### Formatting Dates
When using the `format_day()`, `format_month()` or `format_year(`) methods, the `format` parameter can be either a string or an integer, indicating the number of characters in the desired format. For any other format length, the function returns the full name of the day or month.

```godot
func _ready() -> void:
    var date: Date = Date.new(1, 1, 2000)
    # All will print: "Jan"
    print(date.format_month(3))
    print(date.format_month("mmm"))
    print(date.format_month("XyZ"))
```

The `join_formats()` can then be used to join multiple `format_day()`, `format_month()` or `format_year()` methods regardless of order or quantity.

```godot
func _ready() -> void:
    var date: Date = Date.new(10, 11, 2002)
    # Will print: "2002-10-November-Sun"
    print(date.join_formats([
        date.format_year(4),
        date.format_day(2),
        date.format_month(4),
        date.format_day(4),
    ]))
```
