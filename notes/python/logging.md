Good resources to study Python Logging and the `logging` module in Python:
- Python docs:
	- https://docs.python.org/3/howto/logging.html#logging-howto
	- https://docs.python.org/3/howto/logging-cookbook.html

A ChatGPT-created poem that highlights the peculiarity of logging code:
> In the realm of code, logging's a fickle dance,
> Too many logs may leave budgets askance,
> Find the balance, and debug you can,
> But log too much, you'll end up in a jam!

### Pros and Cons of Logging
| Pros                                                  | Cons                                         |
| ----------------------------------------------------- | -------------------------------------------- |
| Lets you see what happened in a request               | Can make your code less readable             |
| Gain historical knowledge into your application       | Need to pay for log storage                  |
| Once set up, logging is easy                          | A bit confusing the first time you set it up |
| Set up alerts and dashboards when certain logs happen |                                              |

A *logger* schedules log information for output
A *handler* sends the log information to a destination
A *formatter* defines how the log will be displayed

![quick logging primer](../assets/Pasted%20image%2020250412173518.png)

This is an example:

![example of logging primer](../assets/Pasted%20image%2020250412173902.png)

### Logging Levels

![logging levels](../assets/Pasted%20image%2020250412174026.png)

### Code Examples of Logging

```python
import loggin

logging.basicConfig(level=logging.DEBUG) # sets the logging level for the codebase
logger - logging.getLogger(__name__) # sets the import path or __name__ of file or module

logger.debug('This message is a debug message')
logger.info('This message is an informational message')
logger.warning('This message is a warning')
logger.error('This message is an error message')
logger.critical('This message is a critical message')
```

```python
import logging

console = logging.StreamHandler() # default handler that prints to stdout
file_handler = logging.FileHandler("file.log") # handler to logs to disk file
logging.basicConfig(level=logging.DEBUG, handlers=[console, file_handler])

logger = logging.getLogger(__name__)

logger.debug('This message is a debug message')
logger.info('This message is an informational message')
```

Adding log formatting to `baseConfig`:

```python
import logging

console = logging.StreamHandler() # default handler that prints to stdout
file_handler = logging.FileHandler("file.log") # handler to logs to disk file
logging.basicConfig(
	level=logging.DEBUG,
	format="%(asctime)s %(levelname)s %(name)s:%(lineno)d %(message)s",
	handlers=[console, file_handler],
)

logger = logging.getLogger(__name__)

logger.debug('This message is a debug message')
logger.info('This message is an informational message')
```

### `logging.baseConfig`
A blessing or a curse?
- `logging.baseConfig` gives us some nice defaults
- But using it means we don't have as much control
- Instead, it's better to setup our logger manually
- We'll create our logger, handlers, and formatters
- Then we'll set each handler's formatter and add the handlers to the logger.

```python
import logging

# Get the logger to set its level
logger = logging.getLogger(__name__)
logger.setLevel(logging.DEBUG)

# Create the handlers
console = logging.StreamHandler()
file_handler = logging.FileHandler("file.log")

# Create the formatter
formatter = logging.Formatter(
	"%(asctime)s %(levelname)s %(name)s:%(lineno)d %(message)s"
)

# Add the formatter to the handlers
console.setFormatter(formatter)
file_handler.setFormatter(formatter)

# Add the handlers to the logger
logger.addHandler(console)
logger.addHandler(file_handler)

# Actually use the logger
logger.debug('This message is a debug message')
logger.info('This message is an informational message')
```

### Logger Inheritance
- if we define a logger called `storeapi` (with handlers and formatters)
- then other loggers with specific names will use the same handlers and formatters
- which means?
	- `storeapi.routers.post`
	- `storeapi.security`
	- `storeapi.main`
- the `.`  (dot) separates inheritance levels
	- anything under `storeapi.*` will use the `storeapi` logger configuration

![logger inheritance](../assets/Pasted%20image%2020250412180533.png)

