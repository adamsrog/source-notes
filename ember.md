# Ember Namespace (ember-metal)

`ember-metal/lib/core.js`
## Canary Features
Can enable Canary features by using `Ember.ENV.FEATURES = {}` in `config/environment.js`.  This allows for features that aren't being used to be left out of the build. Possible configurations options:
* `EmberENV.ENABLE_ALL_FEATURES` - force all features to be enabled.
* `EmberENV.ENABLE_OPTIONAL_FEATURES` - enable any features that have not been explicitly enabled/disabled.

## Prototype Extensions
`Function`, `String` and `Array` have prototype extensions enabled by default.  They can be disabled by setting `Ember.ENV.EXTEND_PROTOTYPES = false`  in `config/environment.js`.

`ember-metal/lib/array.js`
## Array Prototype Extension
This adds various extensions:
* `.map()`
* `.forEach()`
* `.indexOf()`
* `.lastIndexOf()`
* `.filter()`

`ember-metal/lib/computed.js`
## Computed Properties
* `.property()` - Turn a function into a property.  Runs the function and caches the result.
* `.meta({key:"value"})` - Pass along a hash of meta data into the computed property function.  The hash is stored on the computed property descriptor under teh key `_meta`.  It can be retrieved using public API call `metaForProperty()`.
* `.cacheFor()` - Return the cached value for a computed property, if it exists.
* `.volatile()` - Set a computed property into non-cached mode.
* `.readOnly()` - Throw an error if there is an attempt to set it.



