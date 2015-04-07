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

##`ember-metal/lib/computed.js`
### Computed Properties
* `.property()` - Turn a function into a property.  Runs the function and caches the result.
* `.meta({key:"value"})` - Pass along a hash of meta data into the computed property function.  The hash is stored on the computed property descriptor under teh key `_meta`.  It can be retrieved using public API call `metaForProperty()`.
* `.cacheFor()` - Return the cached value for a computed property, if it exists.
* `.volatile()` - Set a computed property into non-cached mode.
* `.readOnly()` - Throw an error if there is an attempt to set it.

##`ember-metal/lib/computed_macros.js`
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

##`ember-metal/lib/error.js`
### Ember Errors
Subclasses the JavaScript Error object and is used throughout Ember.  Allows for a stack trace when errors are thrown.

##`ember-metal/lib/events.js`
### Ember Events
* `.addListener(obj, eventName, target, method, once)` - adds an event listener 
* `.removeListener(obj, eventName, target, method)` - removes an event listener
* `.suspendListener(obj, eventName, target, method, callback)` - suspend a listener during a callback
* `.suspendListeners(obj, eventNames, target, method, callback)` - suspend multiple listeners during a callback
* `.on(eventNames..., func)` - execute a function when a specified event or events are triggered

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
