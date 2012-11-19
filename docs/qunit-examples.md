
## Usage examples

### Wildcards

In this example, `grunt qunit` will test all `.html` files in the test directory. First, the wildcard is expanded to match each individual file. Then, each matched filename is converted to the appropriate `file://` URI. Finally, [QUnit][qunit] is run for each URI.

```javascript
// Project configuration.
grunt.initConfig({
  qunit: {
    all: ['test/*.html']
  }
});
```

With a slight modification, `grunt qunit` will test all `.html` files in the test directory _and all subdirectories_. See the [minimatch](https://github.com/isaacs/minimatch) module's documentation for more details on wildcard patterns.

```javascript
// Project configuration.
grunt.initConfig({
  qunit: {
    all: ['test/**/*.html']
  }
});
```

### Testing via http:// or https://

In circumstances where running unit tests from `file://` URIs is inadequate, you can specify `http://` or `https://` URIs instead. If `http://` or `https://` URIs have been specified, those URIs will be passed directly into [QUnit][qunit] as-specified.

In this example, `grunt qunit` will test two files, served from the server running at `localhost:8000`.

```javascript
// Project configuration.
grunt.initConfig({
  qunit: {
    all: ['http://localhost:8000/test/foo.html', 'http://localhost:8000/test/bar.html']
  }
});
```

_Note: grunt does NOT start a server at `localhost:8000` automatically. While grunt DOES have a [server task](task_server.md) that can be run before the qunit task to serve files statically, it must be started manually..._

### Using the built-in static webserver

If a web server isn't running at `localhost:8000`, running `grunt qunit` with `http://localhost:8000/` URIs will fail because grunt won't be able to load those URIs. This can be easily rectified by starting the built-in static web server via the [server task](task_server.md).

In this example, running `grunt server qunit` will first start a static web server on `localhost:8000`, with its base path set to the Gruntfile's directory. Then, the `qunit` task will be run, requesting the specified URIs from that server.

```javascript
// Project configuration.
grunt.initConfig({
  qunit: {
    all: ['http://localhost:8000/test/foo.html', 'http://localhost:8000/test/bar.html']
  },
  server: {
    port: 8000,
    base: '.'
  }
});

// A convenient task alias.
grunt.registerTask('test', ['server', 'qunit']);
```

_Note: in the above example, an [alias task](types_of_tasks.md) called `test` was created that runs both the `server` and `qunit` tasks._

### Specifying a custom PhantomJS timeout

If you have long-running asynchronous tests, you can specify an optional timeout value. In the following example, the default value of `5000` is overridden with the value `10000` (timeout values are in milliseconds):

```javascript
// Project configuration.
grunt.initConfig({
  qunit: {
    options: {
      timeout: 10000
    },
    all: ['test/**/*.html']
  }
});
```

As in other multi tasks, this value can be further overridden on a per-target basis.

## Debugging

Running grunt with the `--debug` flag will output a lot of PhantomJS-specific debugging information. This can be very helpful in seeing what actual URIs are being requested and received by PhantomJS.

See the [qunit task source](../tasks/qunit.js) for more information.