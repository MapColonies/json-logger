# json-logger

## Installation
Installation with dependencies
```
pip3 install python-json-logger MapColoniesJSONLogger
```

## Usage Example
```py
import os
from jsonlogger.logger import JSONLogger

APP_NAME = 'my-service'

# there should be a formatter that uses these fields in configuration yaml (e.g. format: '%(timestamp)s %(service)s')
additional_constant_fields = {'service': APP_NAME}

# NOTICE: there should be a matching logger name in configuration yaml (e.g. main-info)
log = JSONLogger('main-info', 
  config={'handlers': {'file': {'filename': 'Meow.log'}}}, # Optional, override values
  additional_fields=additional_constant_fields)

log.critical('system crashed')
log.error('incorrect input')
log.warn('not ready')
log.info('some info', extra={'action_id': 3}) # extra fields
log.debug('some info', extra={'action_id': 4}) # will not be logged because logger level set to INFO ('main-info')
```

## Example Configuration YAML used by the package
```py
{
  'version': 1,
  'formatters': {
    'brief': {
      'format': '%(message)s'
    },
    'json': {
      'format': '%(timestamp)s %(service)s %(loglevel)s %(message)s',
      'class': 'jsonlogger.logger.CustomJsonFormatter'
    }
  },
  'handlers': {
    'console': {
      'class': 'logging.StreamHandler',
      'formatter': 'json',
      'stream': 'ext://sys.stdout'
    },
    'file': {
      'class': 'logging.handlers.RotatingFileHandler',
      'formatter': 'json',
      'filename': '/var/log/osm-seed/logfile.log',
      'maxBytes': 5242880,
      'backupCount': 10
    }
  },
  'loggers': {
    'main-debug': {
      'handlers': [
        'console',
        'file'
      ],
      'level': 'DEBUG'
    },
    'main-info': {
      'handlers': [
        'console',
        'file'
      ],
      'level': 'INFO'
    },
    'main-warning': {
      'handlers': [
        'console',
        'file'
      ],
      'level': 'WARNING'
    },
    'main-error': {
      'handlers': [
        'console',
        'file'
      ],
      'level': 'ERROR'
    },
    'main-critical': {
      'handlers': [
        'console',
        'file'
      ],
      'level': 'CRITICAL'
    }
  }
}

```
