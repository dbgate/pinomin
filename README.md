# pinomin

Minimalistic JSON logger compatible with [pino](https://www.npmjs.com/package/pino) logger. Pino uses lot of optimalizations to be super fast. Sometimes, these optimalizations could cause problems and you need only to create few simple logs. pinomin is naive logger implemented in one file, thanks to compatibility, you could upgrade to pino, if you need better performance.

## Install

Using NPM:

```
$ npm install pinomin
```

Using YARN:

```
$ yarn add pinomin
```

## Usage

```js
const { createLogger } = require('pinomin');

const logger = createLogger();

logger.info('hello world');

const child = logger.child({ a: 'property' });
child.info('hello child!');
```

This produces:

```
{"level":30,"time":1531171074631,"msg":"hello world"}
{"level":30,"time":1531171082399,"msg":"hello child!","a":"property"}
```

## Configure logger
Configuration is different from pino, you could define multiple targets by default.

```js
const logger = createLogger({
  base: { pid: process.pid },
  targets: [
    {
      type: 'console',
      level: process.env.CONSOLE_LOG_LEVEL || process.env.LOG_LEVEL || 'info',
    },
    {
      type: 'stream',
      level: process.env.FILE_LOG_LEVEL || process.env.LOG_LEVEL || 'info',
      stream: fs.createWriteStream('/etc/logs/mylogs.txt', { flags: 'a' }),
    },
  ],
});
```

- There are two types of target available
  - `console` outputs JSON log messages to console with console.log
  - `stream` writes JSON log messages to `stream` property
- `base` property defines base object to be merged into produced JSON log messages


### Development Formatting

pinomin uses the same format as pino logger, so [`pino-pretty`](https://github.com/pinojs/pino-pretty) module could be used for producing nice log outpus during development.

![pretty demo](https://raw.githubusercontent.com/pinojs/pino/master/pretty-demo.png)