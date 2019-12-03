Primer
===

## Table Of Contents
1. [Useful Links](#useful-links)
2. [Helpful Tools](#helpful-tools)
3. [Ember Objects](#ember-objects)
4. [General](#general)
5. [Ember Data](#ember-data)
6. [Controllers](#controllers)
7. [Routing](#routing)
8. [Templating](#templating)
9. [Components](#components)
10. [Mixins, Services & Helpers](#mixins--services--helpers)
11. [Locales](#locales)
12. [Testing](#testing)
13. [DockYard Reference](#dockyard-reference)

## Useful Links
Documentation
* [EmberJs Guides](https://guides.emberjs.com/release/)

Linter
* [ESLint](https://eslint.org/)


## Helpful Tools
* [Ember Inspector](https://guides.emberjs.com/release/ember-inspector/)

## Ember Objects
(!Important)
* Ember Objects =/= Javascript Objects, Ember Objects extend plain JavaScript Objects
and adds some functions to create core framework functionality

```js
let emberjs = EmberObject.create({ flag: false });
let js = { flag: false  };

emberjs.toggleProperty('flag'); // Toggles property
js.toggleProperty('flag'); // TypeError: js.toggleProperty is not a function

console.log('em', get(emberjs, 'flag')); // True
console.log('js', js.flag); // False

```

## General
### Import what you use, do not use globals

Use destructuring for desired modules

```js
// Good
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { computed } from '@ember/object';
import { alias } from '@ember/object/computed';

export default Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),

  surname: alias('lastName'),

  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

// Bad
import Ember from 'ember';
import DS from 'ember-data';

export default DS.Model.extend({
  firstName: DS.attr('string'),
  lastName: DS.attr('string'),

  surname: Ember.computed.alias('lastName'),

  fullName: Ember.computed('firstName', 'lastName', {
    get() {
      // Code
    },

    set() {
      // Code
    }
  })
});
```

### Use `get` and `set`

Although when defining a method in a controller, component, etc. you
can be fairly sure `this` is an Ember Object, for consistency, we still use `get`/`set`.

```js
// Good
import { get, set } from '@ember/object';

set(this, 'isSelected', true);
get(this, 'isSelected');

// Bad

this.set('isSelected', true);
this.get('isSelected');

// Even worse
this.isSelected.set('flag', true);
this.isSelected.get('flag');
```

### Use brace expansion

This allows much less redundancy and is easier to read.

Note that **the dependent keys must be together (without space)** for the brace expansion to work.

```js
// Good
fullName: computed('user.{firstName,lastName}', {
  // Code
})

// Bad
fullName: computed('user.firstName', 'user.lastName', {
  // Code
})
```

### Override init

Rather than using the object's `init` hook via `on`, override init and
call `_super` with `...arguments`. This allows you to control execution
order. [Don't Don't Override Init](https://dockyard.com/blog/2015/10/19/2015-dont-dont-override-init)

## Ember Data
### Be explicit with Ember Data attribute types
```js
// Good

export default Model.extend({
  firstName: attr('string'),
  age: attr('number')
});

// Bad

export default Model.extend({
  firstName: attr(),
  age: attr()
});
```

## Controllers
### Alias your model

It provides a cleaner code to name your model `user` if it is a user. It
is more maintainable, and will fall in line with future routable
components

```javascript
export default Controller.extend({
  user: alias('model')
});
```

## Routing
### Route naming
```js
// good

this.route('consultation', { path: ':consultation_id' });

// bad

this.route('consultation', { path: ':consultationId' });
```


## Templating
### Don't yield `this`

Use the hash helper to yield what you need instead.

```hbs
{{! Good }}
{{yield (hash account=account action=(action "setAccount"))}}

{{! Bad }}
{{yield this}}
```

### Use components in `{{#each}}` blocks

Contents of your each blocks should be a single line, use components
when more than one line is needed.

```hbs
{{! Good }}
{{#each consultations as |consultation|}}
  {{consultation-summary consultation=consultation}}
{{/each}}

{{#each consultations as |consultations|}}
  <h1>{{consultation.title}}</h1>
{{/each}}

{{! Bad }}
{{#each consultations as |consultation|}}
  <summary>
    <img src={{consultation.image}} />
    <h1>{{consultation.title}}</h2>
    <p>{{consultation.summar}}</p>
  </summary>
{{/each}}
```
### Always use the `action` keyword to pass actions

Make it clear you are passing an action ^.^

```hbs
{{! Good }}
{{edit-item item=item deleteItem=(action deleteItem)}}

{{! Bad }}
{{edit-item item=item deleteItem=deleteItem}}
```

## Components

## Mixins, Services & Helpers
Cuando usar services, mixins & helpers (Differences)
Mixins: https://dockyard.com/blog/2015/11/09/best-practices-extend-or-mixin
Services:

## Locales
Cómo manejar locales (?)

## Testing

## DockYard Reference
* [Ember.md](https://github.com/DockYard/styleguides/blob/master/engineering/ember.md)
