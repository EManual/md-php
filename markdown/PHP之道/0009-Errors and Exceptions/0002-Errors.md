## Errors 
In many "exception-heavy" programming languages, whenever anything goes wrong an exception will be thrown. This is
certainly a viable way to do things, but PHP is an "exception-light" programming language. While it does have
exceptions and more of the core is starting to use them when working with objects, most of PHP itself will try to keep
processing regardless of what happens, unless a fatal error occurs.

For example:

```console
$ php -a
php > echo $foo;
Notice: Undefined variable: foo in php shell code on line 1
```

This is only a notice error, and PHP will happily carry on. This can be confusing for those coming from
"exception-heavy" languages, because referencing a missing variable in Python for example will throw an exception:

```console
$ python
>>> print foo
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'foo' is not defined
```

The only real difference is that Python will freak out over any small thing, so that developers can be super sure any
potential issue or edge-case is caught, whereas PHP will keep on processing unless something extreme happens, at which
point it will throw an error and report it.

### Error Severity

PHP has several levels of error severity. The three most common types of messages are errors, notices and warnings.
These have different levels of severity; `E_ERROR`, `E_NOTICE`, and `E_WARNING`. Errors are fatal run-time errors and
are usually caused by faults in your code and need to be fixed as they'll cause PHP to stop executing. Notices are
advisory messages caused by code that may or may not cause problems during the execution of the script, execution is
not halted. Warnings are non-fatal errors, execution of the script will not be halted.

Another type of error message reported at compile time are `E_STRICT` messages. These messages are used to suggest
changes to your code to help ensure best interoperability and forward compatibility with upcoming versions of PHP.

### Changing PHP's Error Reporting Behaviour

Error Reporting can be changed by using PHP settings and/or PHP function calls. Using the built in PHP function
`error_reporting()` you can set the level of errors for the duration of the script execution by passing one of the
predefined error level constants, meaning if you only want to see Warnings and Errors - but not Notices - then you can
configure that:

```php
<?php
error_reporting(E_ERROR | E_WARNING);
```

You can also control whether or not errors are displayed to the screen (good for development) or hidden, and logged
(good for production). For more information on this check out the [Error Reporting][errorreport] section.

### Inline Error Suppression

You can also tell PHP to suppress specific errors with the Error Control Operator `@`. You put this operator at the
beginning of an expression, and any error that's a direct result of the expression is silenced.

```php
<?php
echo @$foo['bar'];
```

This will output `$foo['bar']` if it exists, but will simply return a null and print nothing if the variable `$foo` or
`'bar'` key does not exist. Without the error control operator, this expression could create a `PHP Notice: Undefined
variable: foo` or `PHP Notice: Undefined index: bar` error.

This might seem like a good idea, but there are a few undesirable tradeoffs. PHP handles expressions using an `@` in a
less performant way than expressions without an `@`. Premature optimization may be the root of all programming
arguments, but if performance is particularly important for your application/library it's important to understand the
error control operator's performance implications.

Secondly, the error control operator **completely** swallows the error. The error is not displayed, and the error is
not sent to the error log. Also, stock/production PHP systems have no way to turn off the error control operator. While
you may be correct that the error you're seeing is harmless, a different, less harmless error will be just as silent.

If there's a way to avoid the error suppression operator, you should consider it. For example, our code above could be
rewritten like this:

```php
<?php
echo isset($foo['bar']) ? $foo['bar'] : '';
```

One instance where error suppression might make sense is where `fopen()` fails to find a file to load. You could check
for the existence of the file before you try to load it, but if the file is deleted after the check and before the
`fopen()` (which might sound impossible, but it can happen) then `fopen()` will return false _and_ throw an error. This
is potentially something PHP should resolve, but is one case where error suppression might seem like the only valid
solution.

Earlier we mentioned there's no way in a stock PHP system to turn off the error control operator. However, [Xdebug] has
an `xdebug.scream` ini setting which will disable the error control operator. You can set this via your `php.ini` file
with the following.

```ini
xdebug.scream = On
```

You can also set this value at runtime with the `ini_set` function

```php
<?php
ini_set('xdebug.scream', '1')
```

The "[Scream]" PHP extension offers similar functionality to Xdebug's, although Scream's ini setting is named
`scream.enabled`.

This is most useful when you're debugging code and suspect an informative error is suppressed. Use scream with care,
and as a temporary debugging tool. There's lots of PHP library code that may not work with the error control operator
disabled.


* [Error Control Operators]
* [SitePoint]
* [Xdebug]
* [Scream]


### ErrorException

PHP is perfectly capable of being an "exception-heavy" programming language, and only requires a few lines of code to
make the switch. Basically you can throw your "errors" as "exceptions" using the `ErrorException` class, which extends
the `Exception` class.

This is a common practice implemented by a large number of modern frameworks such as Symfony and Laravel. By default
Laravel will display all errors as exceptions using the [Whoops!] package if the `app.debug` switch is turned on, then
hide them if the switch is turned off.

By throwing errors as exceptions in development you can handle them better than the usual result, and if you see an
exception during development you can wrap it in a catch statement with specific instructions on how to handle the
situation. Each exception you catch instantly makes your application that little bit more robust.

More information on this and details on how to use `ErrorException` with error handling can be found at
[ErrorException Class][errorexception].

* [Error Control Operators]
* [Predefined Constants for Error Handling]
* [`error_reporting()`][error_reporting]
* [Reporting][errorreport]


[errorreport]: /#error_reporting
[Xdebug]: http://xdebug.org/docs/basic
[Scream]: http://php.net/book.scream
[Error Control Operators]: http://php.net/language.operators.errorcontrol
[SitePoint]: http://www.sitepoint.com/
[Whoops!]: http://filp.github.io/whoops/
[errorexception]: http://php.net/class.errorexception
[Predefined Constants for Error Handling]: http://php.net/errorfunc.constants
[error_reporting]: http://php.net/function.error-reporting
