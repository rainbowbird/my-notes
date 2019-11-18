> Written with [StackEdit](https://stackedit.io/).

The logging library takes a ==modular approach== and offers several categories of components: loggers, handlers, filters, and formatters.
-   `Loggers` expose the interface that application code directly uses.
-   `Handlers` send the log records (created by loggers) to the appropriate destination.
-   `Filters` provide a finer grained facility for determining which log records to output.
-   `Formatters` specify the layout of log records in the final output.

Log event information is passed between loggers, handlers, filters and formatters in a [`LogRecord`](https://docs.python.org/3/library/logging.html#logging.LogRecord "logging.LogRecord") instance.

==Logging is performed by calling methods on instances of the [`Logger`](https://docs.python.org/3/library/logging.html#logging.Logger "logging.Logger") class== (hereafter called _loggers_). Each instance has a `name`, and they are conceptually arranged in a namespace hierarchy using dots (periods) as separators. For example, a logger named ‘scan’ is the parent of loggers ‘scan.text’, ‘scan.html’ and ‘scan.pdf’. Logger names can be anything you want, and indicate the area of an application in which a logged message originates.

A good convention to use when naming loggers is to use a module-level logger, in each module which uses logging, named as follows:
```python
logger = logging.getLogger(__name__)
```
This means that logger names track the package/module hierarchy, and it’s intuitively obvious where events are logged just from the logger name.

The root of the hierarchy of loggers is called ==the root logger==. That’s the logger used by the functions [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug"), [`info()`](https://docs.python.org/3/library/logging.html#logging.info "logging.info"), [`warning()`](https://docs.python.org/3/library/logging.html#logging.warning "logging.warning"), [`error()`](https://docs.python.org/3/library/logging.html#logging.error "logging.error") and [`critical()`](https://docs.python.org/3/library/logging.html#logging.critical "logging.critical"), which just call the same-named method of the root logger. The functions and the methods have the same signatures. ==The root logger’s name is printed as ‘root’ in the logged output==.

It is, of course, possible to log messages to different destinations.
Destinations are served by _handler_ classes. You can create your own log destination class if you have special requirements not met by any of the built-in handler classes.

By default, no destination is set for any logging messages.
If you call the functions [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug "logging.debug"), [`info()`](https://docs.python.org/3/library/logging.html#logging.info "logging.info"), [`warning()`](https://docs.python.org/3/library/logging.html#logging.warning "logging.warning"), [`error()`](https://docs.python.org/3/library/logging.html#logging.error "logging.error") and [`critical()`](https://docs.python.org/3/library/logging.html#logging.critical "logging.critical"), they will check to see if no destination is set; and if one is not set, they will set a destination of the console (`sys.stderr`) and a default format for the displayed message before delegating to the root logger to do the actual message output.

The default format set by [`basicConfig()`](https://docs.python.org/3/library/logging.html#logging.basicConfig "logging.basicConfig") for messages is:
```python
severity:logger name:message
```
You can change this by passing a format string to [`basicConfig()`](https://docs.python.org/3/library/logging.html#logging.basicConfig "logging.basicConfig") with the _format_ keyword argument.

## Configuring Logging
Programmers can configure logging in three ways:
1.  Creating loggers, handlers, and formatters explicitly using Python code that calls the configuration methods listed above.
2.  Creating a logging config file and reading it using the  [`fileConfig()`](https://docs.python.org/3/library/logging.config.html#logging.config.fileConfig "logging.config.fileConfig")  function.
3.  Creating a dictionary of configuration information and passing it to the  [`dictConfig()`](https://docs.python.org/3/library/logging.config.html#logging.config.dictConfig "logging.config.dictConfig")  function.
```python
import logging

# create logger
logger = logging.getLogger('simple_example')
logger.setLevel(logging.DEBUG)

# create console handler and set level to debug
ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)

# create formatter
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# add formatter to ch
ch.setFormatter(formatter)

# add ch to logger
logger.addHandler(ch)

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warning('warn message')
logger.error('error message')
logger.critical('critical message')
```
Running this module from the command line produces the following output:
```
$ python simple_logging_module.py
2005-03-19 15:10:26,618 - simple_example - DEBUG - debug message
2005-03-19 15:10:26,620 - simple_example - INFO - info message
2005-03-19 15:10:26,695 - simple_example - WARNING - warn message
2005-03-19 15:10:26,697 - simple_example - ERROR - error message
2005-03-19 15:10:26,773 - simple_example - CRITICAL - critical message
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNzE2NDA2NjMwXX0=
-->