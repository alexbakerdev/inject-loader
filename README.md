# This is a fork of [inject-loader](https://github.com/plasticine/inject-loader)

Add configurable options to webpack for inject loader
### `validate`: 
How to handle validation of dependencies. A valid injection is for a module that module depends on. Possible values:
  * `'error'` - throw if invalid injection 
  * `'warning'` - log warning
  * `false` - silently don't inject invalid dependencies
### `exportInjected`: `boolean`
Whether to export module with `__injected__` property that contains injected modules for that module. 

e.g. :
```js
module.exports = {
...
  injectLoader: { 
    validate: false,
    exportInjected: true
  }
...
}
```
### Other changes
* Fixed integration tests, by making them use project karma install rather than global.

# inject-loader

[![build status](https://img.shields.io/travis/plasticine/inject-loader/master.svg?style=flat-square)](https://travis-ci.org/plasticine/inject-loader) [![Gemnasium](https://img.shields.io/gemnasium/plasticine/inject-loader.svg?style=flat-square)](https://gemnasium.com/plasticine/inject-loader) [![npm version](https://img.shields.io/npm/v/inject-loader.svg?style=flat-square)](https://www.npmjs.com/package/inject-loader) [![npm downloads](https://img.shields.io/npm/dm/inject-loader.svg?style=flat-square)](https://www.npmjs.com/package/inject-loader)

**💉👾 A Webpack loader for injecting code into modules via their dependencies**

This is particularly useful for writing tests where mocking things inside your module-under-test is sometimes necessary before execution.

`inject-loader` was inspired by, and builds upon ideas introduced in [jauco/webpack-injectable](https://github.com/jauco/webpack-injectable).

### Usage

[Documentation: Using loaders](http://webpack.github.io/docs/using-loaders.html)

Use the inject loader by adding `inject!` when you use `require`, this will return a function that can be passed things to inject.

By default all `require` statements in an injected module will be altered to be replaced with an injector, though if a replacement it not specified the default will be used.

### Examples

Given some code in a module like this:

```javascript
// MyStore.js

var Dispatcher = require('lib/dispatcher');
var EventEmitter = require('events').EventEmitter;
var handleAction = require('lib/handle_action');

Dispatcher.register(handleAction, 'MyStore');
```

You can manipulate it’s dependencies when you come to write tests as follows:

```javascript
// If no flags are provided when using the loader then
// all require statements will be wrapped in an injector
MyModuleInjector = require('inject!MyStore')
MyModule = MyModuleInjector({
  'lib/dispatcher': DispatcherMock,
  'events': EventsMock,
  'lib/handle_action': HandleActionMock
})
```

There are a few examples of complete test setups for both Webpack 1 & 2 in the [`example`](./example) folder.

## License

MIT (http://www.opensource.org/licenses/mit-license.php)
