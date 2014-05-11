# propo

This module exposes a single function, a functional helper to define
chained getter / setter on an object.

Eases the process of building a chaining API.

## Install

    npm install propo

## Usage

On a prototype.

```js
var prop = require('propo');

function Stuff() {}
prop(Stuff.prototype)('src');
prop(Stuff.prototype)('dest');
prop(Stuff.prototype)('output');


var stuff = new Stuff();

stuff
  .src('file.json')
  .dest('tmp/output.json')
  .output('log/stdout.log');

console.log(stuff.src());
// => file.json

console.log(stuff._src);
// => file.json
```

Hash / Object.create

```
var obj = {};
prop(obj)('src');
prop(obj)('dest');
prop(obj)('output');

Object.create(obj)
  .src('file.json')
  .dest('tmp/output.json')
  .output('log/stdout.log')
```

Using a forEach iterator

```
['url', 'auth', 'file', 'action'].forEach(prop(Stuff.prototype));
```

### Getter / Setter strategy

A way to get and set props in and out of an object.

- _ - Uses "private-like" properties with a leading `_`. Ex. `obj._src`
- attr - Uses a hash object named `attributes` to hold props. Ex. `obj.attributes.src`

Custom:

```
// Using this.options instead

function opts(name, value) {
  this.options = this.options || {};

  if (!value) return this.options[name];
  this.options[name] = value;
  return this;
}

var obj = {};
prop(obj, { strategy: opts })('file');

// Set
obj.file('file.json');

console.log(obj.options.file);
// => file.json
```

### Validation

Used before setting a property, a simple function returning true on
proper input.

```js
function validate(name, value) {
  if (!value) return;
  if (!value.name) return;
  if (!value.pass) return;
  return true;
}

var obj = {};
prop(obj, { validate: validate })('auth');

// Wrong input are noop
obj.auth({ foo: 'bar' });

console.log(obj.auth());
// => undefined

// Ok input
obj.auth({
  name: 'foo',
  pass: 'bar'
});

console.log(obj.auth());
// => { name: 'foo', pass: 'bar' });
```

---

...
