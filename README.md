# Redis Appender for log4js-node

Stores log events in a [Redis](https://redis.io) database. Plugin for log4js > 2.x
```bash
npm install @log4js-node/redis
```

## Configuration

* `type` - `redis`
* `host` - `string` (optional, defaults to `127.0.0.1`) - the location of the redis server
* `port` - `integer` (optional, defaults to `6379`) - the port the redis server is listening on
* `pass` - `string` (optional) - password to use when authenticating connection to redis
* `layout` - `object` (optional, defaults to `messagePassThroughLayout`) - the layout to use for log events (see [layouts](layouts.md)).
* `channel` - `string` - (required if no `list` specified) the redis channel that log events will be published to
* `list` - `string` - (required if no `channel` specified) the redis list that log events will be inserted to

The appender will use the Redis PUBLISH command to send the log event messages to the channel. Otherwise the appender will use the Redis RPUSH command to send the log event messages to the list.

## Example

- Pub

```javascript
log4js.configure({
  appenders: {
    redis: { type: '@log4js-node/redis', channel: 'logs' }
  },
  categories: { default: { appenders: ['redis'], level: 'info' } }
});
```

This configuration will publish log messages to the `logs` channel on `127.0.0.1:6379`.

- List

```javascript
log4js.configure({
  appenders: {
    redis: { type: '@log4js-node/redis', list: 'someLogList' }
  },
  categories: { default: { appenders: ['redis'], level: 'info' } }
});
```

This configuration will insert log messages to the `someLogList` list on `127.0.0.1:6379`.
