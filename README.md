# jscs-jsdoc
[![Build Status](https://secure.travis-ci.org/jscs-dev/jscs-jsdoc.svg?branch=master)](http://travis-ci.org/jscs-dev/jscs-jsdoc)
[![NPM version](https://badge.fury.io/js/jscs-jsdoc.png)](http://badge.fury.io/js/jscs-jsdoc)
[![Dependency Status](https://david-dm.org/jscs-dev/jscs-jsdoc.png)](https://david-dm.org/jscs-dev/jscs-jsdoc)

`jsdoc` plugin for [jscs](https://github.com/jscs-dev/node-jscs/). [Twitter](https://twitter.com/jscs_dev) | [Mailing List](https://groups.google.com/group/jscs-dev)

## Table of Contents

- [Installation](#plugin-installation)
- [Versioning & Semver](#versioning--semver)
- [Usage](#usage)
- [Rules](#rules)
- [Browser Usage](#browser-usage)

## Plugin installation

`jscs-jsdoc` can be installed using NPM and requires [jscs](https://github.com/jscs-dev/node-jscs/#installation).

Install it globally if you are using globally installed `jscs`

    npm -g install jscs-jsdoc

But better install it into your project

    npm install jscs-jsdoc --save-dev

## Versioning & Semver

We recommend installing `jscs-jsdoc` via NPM using `^`, or `~` if you want more stable releases.

Semver (http://semver.org/) dictates that breaking changes be major version bumps. In the context of a linting tool, a bug fix that causes more errors to be reported can be interpreted as a breaking change. However, that would require major version bumps to occur more often than can be desirable. Therefore, as a compromise, we will only release bug fixes that cause more errors to be reported in minor versions.

Below you fill find our versioning strategy, and what you can expect to come out of a new `jscs-jsdoc` release.

 * Patch release:
   * A bug fix in a rule that causes `jscs-jsdoc` to report less errors;
   * Docs, refactoring and other "invisible" changes for user;
 * Minor release:
   * Any preset changes;
   * A bug fix in a rule that causes `jscs-jsdoc` to report more errors;
   * New rules or new options for existing rules that don't change existing behavior;
   * Modifying rules so they report less errors, and don't cause build failures;
 * Major release:
   * Purposefully modifying existing rules so that they report more errors or change the meaning of a rule;
   * Any architectural changes that could cause builds to fail.

## Usage

To use plugin you should add these lines to configuration file `.jscsrc`:

```json
{
    "additionalRules": [
        "node_modules/jscs-jsdoc/lib/rules/*.js"
    ],
    "jsDoc": {
        "enforceExistence": true
    }
}
```

## Rules

### checkParamNames

Ensures param names in jsdoc and in function declaration are equal

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `param`, `arg`, `argument`

#### Example

```js
"checkParamNames": true
```

##### Valid

```js
/**
 * @param {String} message
 * @param {Number|Object} [line]
 */
function method(message, line) {}
```

##### Invalid

```js
/**
 * @param {String} msg
 * @param {Number|Object} [line]
 */
function method(message) {}
```

### requireParamTypes

Ensures params in jsdoc contains type

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `param`, `arg`, `argument`

#### Example

```js
"requireParamTypes": true
```

##### Valid

```js
/**
 * @param {String} message
 */
function method() {}
```

##### Invalid

```js
/**
 * @param message
 */
function method() {}
```

### checkRedundantParams

Reports redundant params in jsdoc

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `param`, `arg`, `argument`

#### Example

```js
"checkRedundantParams": true
```

##### Valid

```js
/**
 * @param {String} message
 */
function method(message) {}
```

##### Invalid

```js
/**
 * @param {String} message
 */
function method() {}
```

### checkReturnTypes

Reports discrepancies between the claimed in jsdoc and actual type if both exist (code scan)

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `return`, `returns`

#### Example

```js
"checkReturnTypes": true
```

##### Valid

```js
/**
 * @returns {String}
 */
function method() {
    return 'foo';
}
```

##### Invalid

```js
/**
 * @returns {String}
 */
function method(f) {
    if (f) {
        return true;
    }
    return 1;
}
```

### checkRedundantReturns

Report statements for functions with no return

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `return`, `returns`

#### Example

```js
"checkRedundantReturns": true
```

##### Valid

```js
/**
 * @returns {string}
 */
function f() {
    return 'yes';
}
```

##### Invalid

```js
/**
 * @returns {string}
 */
function f() {
    // no return here
}
```

### requireReturnTypes

Ensures returns in jsdoc contains type

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `return`, `returns`

#### Example

```js
"requireReturnTypes": true
```

##### Valid

```js
/**
 * @returns {String}
 */
function method() {}

/**
 * no @return
 */
function method() {}
```

##### Invalid

```js
/**
 * @returns
 */
function method() {}
```

### checkTypes

Reports invalid types for bunch of tags

Type: `Boolean`

Values: `true`

Context: `*`

Tags: `typedef`, `type`, `param`, `return`, `returns`, `enum`, `var`, `prop`, `property`, `arg`, `argument`, `cfg`, `lends`, `extends`, `implements`, `define`

#### Example

```js
"checkTypes": true
```

##### Valid

```js
/**
 * @typedef {Object} ObjectLike
 * @property {boolean} hasFlag
 * @property {string} name
 */

/** @type {number} */
var bar = 1;

/** @const {number} */
var FOO = 2;

/**
 * @const
 * @type {number}
 */
var BAZ = 3;

/**
 * @param {SomeX} x
 * @returns {string}
 */
function method(x) {}
```

##### Invalid

```js
/** @type {some~number} */
var x = 1;
```

### checkRedundantAccess

Reports redundant access declarations

Type: `Boolean`

Values: `true`

Context: `functions`

Tags: `access`, `private`, `protected`, `public`

#### Example

```js
"checkRedundantAccess": true
```

##### Valid

```js
/**
 * @access private
 */
function _f() {}
```

##### Invalid

```js
/**
 * @private
 * @access private
 */
function _f() {}
```

### leadingUnderscoreAccess

Ensures access declaration is set for `_underscored` function names

Type: `Boolean` or `String`

Values: `true` (means not public), `"private"`, `"protected"`

Context: `functions`

Tags: `access`, `private`, `protected`, `public`

#### Example

```js
"checkRedundantAccess": "protected"
```

##### Valid

```js
/**
 * @protected
 */
function _f() {}
```

##### Invalid

```js
function _g() {}

/**
 * @private
 */
function _e() {}
```

### enforceExistence

Ensures jsdoc block exist

Type: `Boolean`

Values: `true`

Context: `functions`

#### Example

```js
"enforceExistence": true
```

##### Valid

```js
/**
 * @protected
 */
function _f() {}
```

##### Invalid

```js
function _g() {}
```

## Browser Usage

NOT SUPPORTED ATM. SORRY.

File [jscs-jsdoc-browser.js](jscs-jsdoc-browser.js) contains browser-compatible version of `jscs-jsdoc`.

Download and include `jscs-jsdoc-browser.js` into your page just after `jscs-browser.js`.

```html
<script src="jscs-browser.js"></script>
<script src="jscs-jsdoc-browser.js"></script>
<script>
    var checker = new JscsStringChecker();
    checker.registerDefaultRules();
    checker.configure({'jsDoc': {/* ... */}});
    var errors = checker.checkString('var x, y = 1;');
    errors.getErrorList().forEach(function (error) {
        console.log(errors.explainError(error));
    });
</script>
```
