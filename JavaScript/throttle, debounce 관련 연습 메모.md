[Throttle 와 Debounce 개념 정리하기](https://pks2974.medium.com/throttle-%EC%99%80-debounce-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-2335a9c426ff)

[Array.prototype.at() - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/at)

[Lodash](https://lodash.com/)

```jsx
console.log(Infinity)
VM278:1 Infinity
undefined
console.log(-Infinity)
VM301:1 -Infinity
undefined
console.log(0)
VM333:1 0
undefined
console.log(+0)
VM351:1 0
undefined
console.log(-0)
VM365:1 -0
undefined
console.log(0 === -0)
VM439:1 true
undefined
console.log(0 === +0)
VM461:1 true
undefined
NaN
NaN
NaN === NaN
false
NaN === NaN
false
typeof 1
'number'
typeof {}
'object'
typeof undefined
'undefined'
typeof null
'object'
0.1 + 0.2
0.30000000000000004
Math.max()
-Infinity
Math.min()
Infinity
Math.min(0)
0
Math.min(23123231313)
23123231313
Math.max(undefined)
NaN
JSON.stringify(undefined)
undefined
JSON.stringify(null)
'null'
JSON.stringify(-0)
'0'
[] === []
false
{} === {}
false
var a = []
undefined
a === a
true
console.log([])
VM1410:1 []length: 0[[Prototype]]: Array(0)at: ƒ at()concat: ƒ concat()constructor: ƒ Array()copyWithin: ƒ copyWithin()entries: ƒ entries()every: ƒ every()fill: ƒ fill()filter: ƒ filter()find: ƒ find()findIndex: ƒ findIndex()findLast: ƒ findLast()findLastIndex: ƒ findLastIndex()flat: ƒ flat()flatMap: ƒ flatMap()forEach: ƒ forEach()includes: ƒ includes()indexOf: ƒ indexOf()join: ƒ join()keys: ƒ keys()lastIndexOf: ƒ lastIndexOf()length: 0map: ƒ map()pop: ƒ pop()push: ƒ push()reduce: ƒ reduce()reduceRight: ƒ reduceRight()reverse: ƒ reverse()shift: ƒ shift()slice: ƒ slice()some: ƒ some()sort: ƒ sort()splice: ƒ splice()toLocaleString: ƒ toLocaleString()toString: ƒ toString()unshift: ƒ unshift()values: ƒ values()Symbol(Symbol.iterator): ƒ values()length: 0name: "values"arguments: [Exception: TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
    at Function.invokeGetter (<anonymous>:3:28)]caller: [Exception: TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
    at Function.invokeGetter (<anonymous>:3:28)][[Prototype]]: ƒ ()[[Scopes]]: Scopes[0]Symbol(Symbol.unscopables): at: truecopyWithin: trueentries: truefill: truefind: truefindIndex: truefindLast: truefindLastIndex: trueflat: trueflatMap: trueincludes: truekeys: truevalues: true[[Prototype]]: Objectconstructor: ƒ Object()hasOwnProperty: ƒ hasOwnProperty()isPrototypeOf: ƒ isPrototypeOf()propertyIsEnumerable: ƒ propertyIsEnumerable()toLocaleString: ƒ toLocaleString()toString: ƒ toString()valueOf: ƒ valueOf()__defineGetter__: ƒ __defineGetter__()__defineSetter__: ƒ __defineSetter__()__lookupGetter__: ƒ __lookupGetter__()__lookupSetter__: ƒ __lookupSetter__()__proto__: Array(0)at: ƒ at()length: 1name: "at"arguments: (...)caller: (...)[[Prototype]]: ƒ ()[[Scopes]]: Scopes[0]concat: ƒ concat()constructor: ƒ Array()copyWithin: ƒ copyWithin()entries: ƒ entries()every: ƒ every()fill: ƒ fill()filter: ƒ filter()find: ƒ find()findIndex: ƒ findIndex()findLast: ƒ findLast()findLastIndex: ƒ findLastIndex()flat: ƒ flat()flatMap: ƒ flatMap()forEach: ƒ forEach()includes: ƒ includes()indexOf: ƒ indexOf()join: ƒ join()keys: ƒ keys()lastIndexOf: ƒ lastIndexOf()length: 0map: ƒ map()pop: ƒ pop()push: ƒ push()reduce: ƒ reduce()reduceRight: ƒ reduceRight()reverse: ƒ reverse()shift: ƒ shift()slice: ƒ slice()some: ƒ some()sort: ƒ sort()splice: ƒ splice()toLocaleString: ƒ toLocaleString()toString: ƒ toString()unshift: ƒ unshift()values: ƒ values()Symbol(Symbol.iterator): ƒ values()Symbol(Symbol.unscopables): {at: true, copyWithin: true, entries: true, fill: true, find: true, …}[[Prototype]]: Objectget __proto__: ƒ __proto__()set __proto__: ƒ __proto__()
undefined
[].at(0)
undefined
[1,2,3].at(2)
3
[1,2,3].at(3)
undefined
''
''
New String()
VM1641:1 Uncaught SyntaxError: Unexpected identifier
new String()
String {''}
```