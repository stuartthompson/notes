# Reduce

The reduce function is used to iterate values in an array and reduce the
contents into a single output. This is useful for summarizing data or
transforming a collection into a different format.

The following example shows a reducer operating on an array containing three
values and returning their sum.

```javascript
let ct = 0;
function myReducer(acc, val, ix, arr) {
  console.log(`[${ct++}] acc: ${acc} val: ${val} ix: ${ix} arr: ${arr}`);
  return acc + val;
}

let output = [3,2,1].reduce(myReducer, 1);

console.log('output: ' + output);
```

## Parameters

The reducer function takes four parameters:
* Accumulator
* Value
* Index
* Array

### Accumulator

The first parameter received by the reducer is the accumulated value. This
will become the final output that is returned once the reducer has iterated all
values in the array.

### Value

The current value being processed. The reducer will be called once for each
value in the array. On each call, the current value that is being operated on
is passed as this parameter.

### Index

The index in the array of the current value that is being processed.

### Array

The array that is being processed.

## Call Count based upon Initial Value

The number of times the reducer is called depends upon whether an initial
value is provided.

This code is invoked twice:

```javascript
[1,2,3].reduce(myReducer)
```

This code is invoked three times:

```javascript
[1,2,3].reduce(myReducer, 1);
```

The reason for this is due to how initial values are handled. If no initial
value is passed to reduce(...) then the first value in the array is used as
the initial value. In this case, the reducer function is called one fewer
times. There is no reason to call the reducer for the first value in the array
since there is no initial value to modify.

This becomes important if a reducer function contains side effects. Reducers
should not contain side effects because the number of times the reducer is
called varies based upon the length of the array and upon whether an initial
value is provided.

