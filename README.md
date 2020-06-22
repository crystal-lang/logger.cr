# logger

Provides the legacy `Logger` functionality.

## Installation

1. Add the dependency to your `shard.yml`:

   ```yaml
   dependencies:
     logger:
       github: crystal-lang/logger.cr
   ```

2. Run `shards install`

## Usage

The `Logger` class provides a simple but sophisticated logging utility that you can use to output messages.

The messages have associated levels, such as `INFO` or `ERROR` that indicate their importance.
You can then give the `Logger` a level, and only messages at that level or higher will be printed.

For instance, in a production system, you may have your `Logger` set to `INFO` or even `WARN`.
When you are developing the system, however, you probably want to know about the program’s internal state,
and would set the `Logger` to `DEBUG`.

### Example

```crystal
require "logger"

log = Logger.new(STDOUT)
log.level = Logger::WARN

# or
log = Logger.new(STDOUT, level: Logger::WARN)

log.debug("Created logger")
log.info("Program started")
log.warn("Nothing to do!")

begin
  File.each_line("/foo/bar.log") do |line|
    unless line =~ /^(\w+) = (.*)$/
      log.error("Line in wrong format: #{line}")
    end
  end
rescue err
  log.fatal("Caught exception; exiting")
  log.fatal(err)
end
```

If logging to multiple locations is required, an `IO::MultiWriter` can be
used.

```crystal
file = File.new("production.log", "a")
writer = IO::MultiWriter.new(file, STDOUT)

log = Logger.new(writer)
log.level = Logger::DEBUG
log.debug("Created logger")
```

## Contributing

1. Fork it (<https://github.com/crystal-lang/logger.cr/fork>)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
