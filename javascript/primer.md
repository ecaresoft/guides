Primer
===

### Table Of Contents
1. [Useful Links](#useful-links)
2. [ECMAScript](#ecmascript)
3. [Let & const](#let--const)
4. [Classes](#classes)
5. [Arrow Functions](#arrow-functions)
6. [Promises](#promises)

### Useful Links
Styleguide
* [Javascript styleguide](styleguide.md)

Documentation
* [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

Interesting reads
* [Javascript the Good parts](https://github.com/dwyl/Javascript-the-Good-Parts-notes) (or read [the book](https://www.amazon.com.mx/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742/ref=sr_1_1?__mk_es_MX=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=JavaScript%3A+The+Good+Parts&qid=1574368961&sr=8-1))
* [WTF JS](https://github.com/denysdovhan/wtfjs/blob/master/README.md) (or watch the [talk](https://www.youtube.com/watch?v=et8xNAc2ic8))

### ECMAScript
ECMAScript (henceforth abbreviated as ES) is a specification which Javascript implements.
While this is true, it is most commonly used as the extension of functionality over
vanilla Javascript, which includes features as classes, scope delimitation, promises, etc.
This allows us to build cleaner & more maintalable code over using plain vanilla JS.

### Let & Const
`var` is the old way of declaring a variable, with the arrival of ES5, we were introduced to `let` & `const`. So which
one do we use?
- Don't use `var`
- Use `let`
- Don't use `const` unless declaring a constants

Why don't we use `var` anymore? Because `var` is global or scoped to the function, while `let` & `const` are scoped to the block.
```js
scopes() {
  if (true) {
    var foo = "meep";
    let bar = "meep";
  }

  console.log(foo); // > meep
  console.log(bar); // > ReferenceError
}
```
More on this: [Var, let and const- what's the difference?](https://dev.to/sarah_chima/var-let-and-const--whats-the-difference-69e)

So when do we use `const`?

**Declaring constant values**
```js
export const Heredofamilial = [
  'diabetes',
  'heart_diseases',
  'hypertension',
  'thyroid_diseases',
  'other'
];
```
Source: [Nimbo X](https://github.com/ecaresoft/nimbox/blob/4900ec1f58e8b5bd3fac2f80fe969c00e6b0609b/app/models/medical-history/elements.js#L17-L23)

**Creating test stubs**
Use it to create test stubs outside of the scope of test to be used in the test suite for that particular
piece of code.

*Note:* Preferrably declare it in another file (such as a [test helper](https://guides.emberjs.com/v1.10.0/testing/test-helpers/#toc_custom-test-helpers))
over a stub if you are going to reuse it.

```js
const membershipGoldStub = Service.extend({
  hasLite: false
});

test('render lock when template is locked for basic user', function(assert) {
  this.container.registry.register('service:mockMembership', membershipBasicStub);
  // ...
});
```
Source: [Nimbo X](https://github.com/ecaresoft/nimbox/blob/60b399df20c3a0efbaf2dec25324bd887c4261ba/tests/integration/components/accounts/configuration-prescription/selector-test.js#L12-L14)

### Classes
One of the new features of ES6 is the introduction of classes, this allows the language to
have a method for importing different modules (like with require.js) without pollutting the global scope.
```js
import Controller from '@ember/controller';
import { inject } from '@ember/service';

// Most frameworks add inheritance via the `extend` (or `create`) method.
export default Controller.extend({
  session: inject('session'),

  actions: {
    authenticate(username, password) {
      this.get('session').authenticate('authenticator:oauth2', username, password);
    }
  }
});
```
Source: [Apollo](https://github.com/ecaresoft/apollo/blob/master/app/pods/login/controller.js)

While classes are similar to the other languages, most it's important to understand what the
keywords `import`, `export, and `default` do.

The `import` syntax consist of
```js
import module from 'location';
import { module } from 'location';
```

What's the difference between the two?
* The first one imports the whole module, usually defined by using `default`
* The second one imports only that given part of the module, for example, given a file
  like this

```js
export {
  one,
  two,
  three
};

```
the corresponding import would be
```js
import { one } from 'file';
```

### Arrow Functions
Arrow functions were introduced in ES6, and their main use is to ensure Context is properly maintaned.
```js
let object = {
  init() {
    // this will work
    arrowButton.click(() => {
      this._showPopup();
    });

    // this won't, as `this` is contextually binded to the anonymous function
    anonButton.click(function() {
      this._showPopup();
    });
  },

  _showPopup() {
    alert('test!');
  }
}
```

Arrow functions should only be used when making anonymous functions, for example
using array functions or when resolving promises.
```js
// arrays
products.map((product) => {
  return product.name;
});

// promises
ajax.request(someURL).then((response) => {
  return console.log(response);
});
```

### Promises
The old way of handling aysncronious request is using event callbacks, which is prone to leave messy code.
```js
_loaded(response) {
  // loaded
}

_failed(error) {
  // failed
}

ajax.request('users', _loaded, _failed);
```

In ES6, Promises where introduced as a standard API to handle requests, which makes them more sensible to read and
understand.
```js
ajax.request('users').then((response) => {
  // loaded
}).catch((error) => {
  // failed
});
```

More on this: [Promises](https://developers.google.com/web/fundamentals/primers/promises)

As a bonus, in ES8, `aysnc` and `await` were introduced to further simplify the code. You can declare a function as
`async` and inside then use `await` on all asyncronious requests to treat them like there are syncronious.
```js
try {
  let users = await store.findAll('user');
  // loaded
} catch (error) {
  // failed
}
```
