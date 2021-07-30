[&#8678; get back to the start...](../README.md)
# ES6. Import / Export
## Export
```js
export function functionName(){...}
export class ClassName {...}

export default function functionName(){...}
export default class ClassName {...}
```
## Import
```js
// import all
import * as myModule from '/modules/my-module';

import { functionName, ClassName } from '/modules/my-module';

import functionName from '/modules/module';
import ClassName from '/modules/module';
```