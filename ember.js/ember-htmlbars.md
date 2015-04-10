#ember-htmlbars

## Compatibility
## `ember-htmlbars/lib/compat/helper.js`
Wraps a Handlebars helper with an HTMLBars helper for backwards compatibility.

## `ember-htmlbars/lib/compat/register-bound-helper.js`
`Ember.Handlebars.registerBoundHelper(name, func)` - registers a bound Handlebars helper.  These bound helpers behave similarly to regular Handlebars helpers, with the added ability to re-render when the underlying data changes.

```javascript
Ember.Handlebars.registerBoundHelper('capitalize', function(value) {
  return Ember.String.capitalize(value);
});
```

Like normal Handlebars helpers, bound helpers have access to the options passed into the helper call.

```javascript
Ember.Handlebars.registerBoundHelper('repeat', function(value, options) {
  var count = options.hash.count;
  var a = [];
  while(a.length < count) {
      a.push(value);
  }
  return a.join('');
});
```

### Helpers
## `ember-htmlbars/lib/helpers/component.js`
The `{{component}}` helper lets you add instances of `Ember.Component` to a template.  It's primary usage is for cases where you need to dynamically change the type of component that is rendered.

It can be used within a template like so:  `{{component dynamicValue}}`

Sticking the use case outlined above, `dynamicValue` would be a computed property that determines has some kind of logic determining the name of which component needs to be rendered.

```javascript
dynamicValue: Ember.computed('value', function() {
	if (this.get('value')) {
		return 'truthy-component';
	} else {
		return 'falsy-component';
	}
})
```

## `ember-htmlbars/lib/helpers/debugger.js`
* Execute the `debugger` statement in the current template's context.
* When using the debugger, you'll have access to a `get` function that can be used like so: `> get('foo')`
* Debugger is aware of keywords, so you used `{{debugger}}` within an `{{#each items as |item|}}`, you could access values like so: `> get('item.name')`
* Check the context of the view: `> context`

## `ember-htmlbars/lib/helpers/each.js`
* This is an extension of the Handlebars `{{#each}}` helper.
* Yield the inner block once for each item in an array.
* Can use `{{else}}` for outputting a block if there are no items in the array.
* Can specify an `itemViewClass` that'll be used for each item in the array.
* Can specify an `emptyViewClass` which will be used if there are no items in the array (similar to `{{else}}`)
  
```
{{#each person in developers}}
  {{person.name}}
{{else}}
  <p>Sorry, nobody is available for this task.</p>
{{/each}}
```

* Can specify an `itemController` to "decorate" the data in a controller.  Example from source:

```javascript
App.RecruitController = Ember.ObjectController.extend({
  isAvailableForHire: function() {
    return !this.get('isEmployed') && this.get('isSeekingWork');
  }.property('isEmployed', 'isSeekingWork')
})
```

```handlebars
{{#each person in developers itemController="recruit"}}
  {{person.name}} {{#if person.isAvailableForHire}}Hire me!{{/if}}
{{/each}}
```

## `ember-htmlbars/lib/helpers/input.js`
* `{{input}}` helper is used to create an HTML `<input>` tag into a template.  If a `type` isn't specified, it'll default to `text`.  Can also use `checkbox`.
* Can pass hardcoded values for attributes by putting them in quotes: `{{input type="text" value="text"}}`
* Can pass bound values for attributes by not using quotes: `{{input type="text" value=computedValue}}`

Attributes for `{{input type="text"}}`:
 <table>
  <tr><td>`readonly`</td><td>`required`</td><td>`autofocus`</td></tr>
  <tr><td>`value`</td><td>`placeholder`</td><td>`disabled`</td></tr>
  <tr><td>`size`</td><td>`tabindex`</td><td>`maxlength`</td></tr>
  <tr><td>`name`</td><td>`min`</td><td>`max`</td></tr>
  <tr><td>`pattern`</td><td>`accept`</td><td>`autocomplete`</td></tr>
  <tr><td>`autosave`</td><td>`formaction`</td><td>`formenctype`</td></tr>
  <tr><td>`formmethod`</td><td>`formnovalidate`</td><td>`formtarget`</td></tr>
  <tr><td>`height`</td><td>`inputmode`</td><td>`multiple`</td></tr>
  <tr><td>`step`</td><td>`width`</td><td>`form`</td></tr>
  <tr><td>`selectionDirection`</td><td>`spellcheck`</td><td>&nbsp;</td></tr>
 </table>

Can also pass in actions based on user events `{{input type="text" key-press="someAction"}}`:
* `enter`
* `insert-newline`
* `escape-press`
* `focus-in`
* `focus-out`
* `key-press`
* `key-up`

Attributes for `{{input type="checkbox"}}`:
* `checked`
* `disabled`
* `tabindex`
* `indeterminate`
* `name`
* `autofocus`
* `form`

## `ember-htmlbars/lib/helpers/loc.js`
Convenient way to localize text within a template.

```javascript
Ember.STRINGS = {
  '_welcome_': 'Bonjour'
};
```

```handlebars
<div class='message'>
  {{loc '_welcome_'}}
</div>
```

```html
<div class='message'>
  Bonjour
</div>
```

## `ember-htmlbars/lib/helpers/log.js`
Allows for output of the value of variables in the current rendering context.  `log` also accepts primitive types such as strings or numbers.

```handlebars
{{log "myVariable:" myVariable }}
```