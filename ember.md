# ember-metal

##`ember-metal/lib/core.js`
### Canary Features
Can enable Canary features by using `Ember.ENV.FEATURES = {}` in `config/environment.js`.  This allows for features that aren't being used to be left out of the build. Possible configurations options:
* `EmberENV.ENABLE_ALL_FEATURES` - force all features to be enabled.
* `EmberENV.ENABLE_OPTIONAL_FEATURES` - enable any features that have not been explicitly enabled/disabled.

### Prototype Extensions
`Function`, `String` and `Array` have prototype extensions enabled by default.  They can be disabled by setting `Ember.ENV.EXTEND_PROTOTYPES = false`  in `config/environment.js`.

##`ember-metal/lib/array.js`
### Array Prototype Extension
This adds various extensions:
* `.map()`
* `.forEach()`
* `.indexOf()`
* `.lastIndexOf()`
* `.filter()`

### Function Prototype Extension
(from `ember-metal/lib/mixin.js`)
These extensions are available on a Function:
* `.observes(propertyNames..., function)` - `Ember.observer()` - Executes `function` anytime any `propertyName` changes.  This may become asynchronous.
* `.observesImmediately(propertyNames..., function)` - `Ember.immediateObserver()` - Executes `function` anytime any `propertyName` changes.  Maintains synchronous behavior.
* `.observesBefore(propertyNames..., function(obj, keyName))` - `Ember.beforeObserver()` - Fires before a property changes


##`ember-metal/lib/computed.js`
### Computed Properties
* `.property()` - Turn a function into a property.  Runs the function and caches the result.
* `.meta({key:"value"})` - Pass along a hash of meta data into the computed property function.  The hash is stored on the computed property descriptor under teh key `_meta`.  It can be retrieved using public API call `metaForProperty()`.
* `.cacheFor()` - Return the cached value for a computed property, if it exists.
* `.volatile()` - Set a computed property into non-cached mode.
* `.readOnly()` - Throw an error if there is an attempt to set it.

##`ember-metal/lib/computed_macros.js`
(includes `ember-metal/lib/is_empty.js`, `ember-metal/lib/is_none.js`, `ember-metal/lib/alias.js`)
### Computed Property Macros
* `.empty(dependentKey)` - returns true if `dependentKey` is null, empty string, empty array, or empty function
* `.notEmpty(dependentKey)` - returns true if `dependentKey` is NOT null, empty string, empty array or empty function
* `.none(dependentKey)` - returns true if `dependentKey` is null or undefined
* `.not(dependentKey)` - returns the inverse boolean value of the `dependentKey` value
* `.bool(dependentKey)` - returns the `dependentKey` property value into a boolean value
* `.match(dependentKey, RegExp)` - returns boolean based on matching value of supplied `RegExp` (uses [`.test()`](https://developer.mozilla.org/en-US/docs/Web/EXSLT/regexp/test))
* `.equal(dependentKey, value)` - returns true if `dependentKey` is equal to `value`
* `.gt(dependentKey, value)` - returns true if `dependentKey` is greater than `value` 
* `.gte(dependentKey, value)` - returns true if `dependentKey` is greater than or equal to `value`
* `.lt(dependentKey, value)` - returns true if `dependentKey` is less than `value`
* `.lte(dependentKey, value)` - returns true if `dependentKey` is less than or equal to `value`
* `.and(dependentKey, ...)` - returns boolean after performing logical `and` on supplied `dependentKeys`
* `.or(dependentKey, ...)` - returns boolean after performing logical `or` on supplied `dependentKeys`
* `.any(dependentKey, ...)` - returns first truthy value from the provided `dependentKeys`
* `.collect(dependentKey, ...)` - returns array of values comprised of provided `dependentKeys`
* `.alias(dependentKey)` - returns an alias for the `dependentKey` that can also use `set` and `get` as if it were the original property
* `.oneWay(dependentKey)` - returns an alias for the `dependentKey`, but using `set` will not change the aliased property (i.e. not bidirectional)
* `.readOnly(dependentKey)` - returns an alias for the `dependentKey`, but will throw an error if `set` is used

##`ember-metal/lib/error.js & logger.js`
### Ember Errors & Logging
Subclasses the JavaScript Error object and is used throughout Ember.  Allows for a stack trace when errors are thrown.  Multiple arguments can be passed to the `Ember.Logger` methods and they'll be joined together with a space.  The `Ember.Logger` methods include:
* `Ember.Logger.log(arguments)` - normal log
* `Ember.Logger.info(arguments)` - print to console in blue text
* `Ember.Logger.debug(arguments)` - print to console in blue text
* `Ember.Logger.warn(arguments)` - print to console with warning icon
* `Ember.Logger.error(arguments)` - print to console with red text, error icon and stack trace
* `Ember.Logger.assert(arguments)` - if value passed in isn't truthy, throw an error and stack trace

##`ember-metal/lib/events.js`
### Ember Events
* `.addListener(obj, eventName, target, method, once)` - adds an event listener 
* `.removeListener(obj, eventName, target, method)` - removes an event listener
* `.suspendListener(obj, eventName, target, method, callback)` - suspend a listener during a callback
* `.suspendListeners(obj, eventNames, target, method, callback)` - suspend multiple listeners during a callback
* `.on(eventNames..., func)` - execute a function when a specified event or events are triggered

From `ember-metal/lib/main.js`:
A function may be assigned to `Ember.onerror` to be called when Ember internals encounter an error.  Useful for specialized error handling and reporting code.
```javascript
Ember.onerror = function(error) {
  Em.$.ajax('/report-error', 'POST', {
    stack: error.stack,
    otherInformation: 'whatever app state you want to provide'
  });
};
```

##`ember-metal/lib/get_properties.js`
### Getting multiple properties
`.getProperties(objects, strings.../Array of strings)` allows for getting multiple properties at once.
```javascript
Ember.getProperties(record, 'firstName', 'lastName', 'zipCode');
// { firstName: 'John', lastName: 'Doe', zipCode: '10011' }
```
... is the same as ...
```javascript
Ember.getProperties(record, ['firstName', 'lastName', 'zipCode']);
// { firstName: 'John', lastName: 'Doe', zipCode: '10011' }
```

##`ember-metal/lib/merge.js`
### Merging two objects
`Ember.merge(firstObject, secondObject)` - merges contents of `secondObject` into `firstObject`.
```javascript
Ember.merge({first: 'Tom'}, {last: 'Dale'}); // {first: 'Tom', last: 'Dale'}
var a = {first: 'Yehuda'};
var b = {last: 'Katz'};
Ember.merge(a, b); // a == {first: 'Yehuda', last: 'Katz'}, b == {last: 'Katz'}
```

##`ember-metal/lib/mixin.js`
### Ember Mixins
Believe first section contains logic for enabling mixins.  Lots of merging of objects and setup of listeners, observers, etc.  TODO: revisit this part

##`ember-metal/lib/run_loop.js`
### Ember Run Loop
The run loop appears to just wrap `backburner` methods.  Allows for executing methods in a `RunLoop` which ensures any deferred actions including bindings and views updates are flushed at the end.  TODO: learn more about `backburner`.

* `run.queues` - array of named queues that determines flush order at end of `RunLoop`.  defaults to `['sync', 'actions', 'destroy']`
* `sync` - binding synchronization
* `actions` - runs after synchronization completes
* `destroy` - 

* `run()` - run in a `RunLoop`.  Alternatively, lower level way to use `RunLoop` is to wrap code in `run.begin()` ...code ... `run.end()`.
* `run.join()` - create a run loop if needed, and queue itself to run on the existing run-loops action queue
* `run.bind()` - allows for specification of which context to call the specified function in.  From source: This ability makes this method a great way to asynchronously integrate third-party libraries into your Ember application.
* `run.schedule()`- schedule a function to run on one of the aforementioned queues
* `run.sync()` - flush any events scheduled in the `sync` queue.
* `run.later(context, function, timeout)` - invoke a method after a specified period of time (milliseconds).  This should be used instead of `setTimeout()` to ensure synchronization of script execution cycles.
* `run.once(context, function, arguments)` - schedule a function to run one time during current `RunLoop`.  This is the equivalent of calling `scheduleOnce()` in the `actions` queue.  Returns time information used to cancel (if necessary).
* `run.scheduleOnce(queue, context, function, arguments)` - schedule a function to run in a specified queue

