Some other rules
=====================

Here are some other rules, not specified in the PSR.

- Use the latest PHP version if possible.
- When declaring an array, elements of the array can be split into multiple lines. When doing so, the following rules should be observed:
  - The first part of the array must be placed on a new line.
  - Each line is allowed only one element, the element is indent once.
  - Must have a comma at the end of the last element.
  - The array end of the array declaration (`)` for `[]` or `)` for `array (`) must be placed on a new line.

```php
// Bad
$a = [$foo,
    $bar
];

// Bad
$a = [
    $foo, $bar,
    $baz,
];

// Bad
$a = [
    $foo,
    $bar
];

// Bad
$a = [
    $foo,
    $bar,];

// Bad
$a = [$foo, $bar,];

// Good
$a = [
    $foo,
    $bar,
];

$a = [$foo, $bar];
```

- For PHP> = 5.4, use `[]` to declare the array, instead of using `array ()`

```php
// Good
$a = [];
$b = [
    $key => $value,
    $key2 => $value2,
];

// Bad
$a = array();
$b = array(
    $key => $value,
    $key2 => $value2,
);
```

- Variable name is written as `camelCase`. For the property name inside the model, it can be written as `snake_case` to match the names of the columns in the database tables.
- Use `'` for a normal string. Only use `"`when inside the PHP variable.

```php
$normalString = 'A String';
$specialString = "This is {$normalString}";
```
- There should be a space before and after operators such as `+`, `-`, `*`, `/`, `.`, `>`, `<`, `==` ...
