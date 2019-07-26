# @odjs/classes

[![CircleCI](https://circleci.com/gh/odjs/classes.svg?style=svg)](https://circleci.com/gh/odjs/classes) [![Greenkeeper badge](https://badges.greenkeeper.io/odjs/classes.svg)](https://greenkeeper.io/) [![npm](https://img.shields.io/npm/v/@odjs/classes.svg)](https://www.npmjs.com/package/@odjs/classes) ![npm type definitions](https://img.shields.io/npm/types/@odjs/classes.svg) [![codecov](https://codecov.io/gh/odjs/classes/branch/master/graph/badge.svg)](https://codecov.io/gh/odjs/classes) [![Known Vulnerabilities](https://snyk.io//test/github/odjs/classes/badge.svg?targetFile=package.json)](https://snyk.io//test/github/odjs/classes?targetFile=package.json) [![NPM LICENSE](https://img.shields.io/npm/l/@odjs/classes.svg)](LICENSE)

Classname management for [@odjs/dom](https://github.com/odjs/dom)

*While this project has been created to be used internally in [@odjs/dom](https://github.com/odjs/dom), it can be used as standalone both in Node.js and in the browser.*

## Install

```bash
npm i @odjs/classes
```

*For the browser you can use one of our [cdn](#cdn) scripts, or you can use a tool like Webpack, Browserify or Parcel.*

[*see usage section...*](#usage)

## API

### classes

***syntax***

```typescript
classes(...names: ClassName[]): string;
```

*Accepts any number of [ClassName](#classname) arguments and returns [normalized classname](#object-normalization) string.*

***example***

```javascript
classes(
  "btn",
  ["btn-small", "btn-red"],
  { "is-rounded has-border": true, "is-enable": false },
  { "has-border": false },
);
```

```console
> "btn btn-small btn-red is-rounded"
```

*note the* `"has-border"` *is not present in the resulting string, see [object normalization feature](#object-normalization) for more information.*

### fromArray

*This method has been removed in* `v0.1.0` *as the same result can be achieved with the* [`classes`](#classes) *method. It is still being exposed for compatibility but it points to the* [`classes`](#classes) *method.*

### fromObj

***syntax***

```typescript
fromObj(object: ClassObject, mormalize?: boolean): string;
```

*This method expects a [ClassObject](#classobject) as first and required argument, and an optional* `normalize` *argument, which sets whether or not to normalize the input object. See see [object normalization feature](#object-normalization) for more information.*

***example***

```javascript
fromObj({
  "btn btn-small": true,
  "is-enabled": false,
});
```

```console
> "btn btn-small"
```

## Types

### ClassName

```typescript
type ClassName = string | ClassObject | ClassArray;
```

### ClassObject

```typescript
interface ClassObject {
  [key: string]: any;
}
```

### ClassArray

```typescript
interface ClassArray {
  [index: number]: string | ClassObject | ClassArray;
}
```

## CDN

### jsDelivr

```html
<script src="https://cdn.jsdelivr.net/npm/@odjs/classes@latest/dist/map.umd.js"></script>
```

*or for production...*

```html
<script src="https://cdn.jsdelivr.net/npm/@odjs/classes@latest/dist/map.umd.min.js"></script>
```

*[more options...](https://www.jsdelivr.com/package/npm/@odjs/classes?version=latest)*

### unpkg

```html
<script src="https://unpkg.com/@odjs/classes@latest/dist/map.umd.js"></script>
```

*for production...*

```html
<script src="https://unpkg.com/@odjs/classes@latest/dist/map.umd.min.js"></script>
```

*[more options...](https://unpkg.com/@odjs/classes@latest/)*

## Usage

### Node.js

```javascript
const { classes } = require("@odjs/classes");
element.className = classes({ btn: true, red: true });
```

### Browser

*After including the* `script` *tag in your html file,* `classes` *will be available globally.*

```javascript
element.className = classes.classes({ btn: true, red: true });
```

## Features

### Object Normalization

*Objects with "multi-class" keys (keys that contain spaces) will be normalized, which allows to extend a "single-class" after it's been set from a "multi-class". This behavior is enabled in* [`classes`](#classes) *method and optional in* [`fromObj`](#fromobj) *method.*

***example***

```javascript
const classObj1 = {
  "btn btn-small is-rounded is-enabled": true,
};

const classObj2 = {
  "is-rounded": false,
};

console.log(
  classes(classObj1, classObj2),
);
```

```console
> "btn btn-small is-enabled"
```

*Using this feature within a single object should work most of the times, however since key-value-pair iteration order is implementation dependent, it may lead to unpredictable results and therefore it is not recommended. Pass an argument that evaluates to* `true` *to* `fromObj` *to enable optimization.*

```javascript
const classObj = {
  "btn btn-small is-rounded is-enabled": true,
  "is-rounded": false,
};

console.log( fromObj(classObj, true) );
```

```console
> "btn btn-small is-enabled" > most of the times!
```

## License

[MIT](LICENSE) &copy; [Manuel Fernández](https://github.com/manferlo81)
