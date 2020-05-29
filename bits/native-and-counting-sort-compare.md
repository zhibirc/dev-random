## Native browser Array.prototype.sort vs Counting sort implementation

#### [2020-05-07]

History begins when I was interest which algorithm the native browser `sort` function uses. My short research shown that standard sorting algorithm is quite efficient, but is different among different browsers: some implement _Merge sort_, some implement
_Quicksort_ ("Tony Hoare's sort"), it's often to analyze input sequences before create a decision what algorithm should be appropriate in particular case (adaptive approach).

After this I had one question -- is it possible to sort more quickly? Even if we are talking about a subset of all possible input sequences.

Then I made the assumption that _Counting sort_ could be such an algorithm. And, it seems, it's true, at least at current time. See implementation and evaluation statistics:

```js
// preset
const COUNT       = 1e6;
const BOUND_UPPER = 1000;
const BOUND_LOWER = 0;

const emitRandom = () => (BOUND_LOWER + Math.random() * (BOUND_UPPER - BOUND_LOWER + 1)) | 0;

const vector = [];

let vector1;
let vector2;
let start;

console.log('Program starts.');

// fill array with pseudo-random numbers, it's enough for current purpose
for ( let index = 0; index < COUNT; index += 1 ) vector.push(emitRandom());

// create two clones from original array
vector1 = [...vector];
vector2 = [...vector];

console.log(`Array of ${COUNT} random integers is generated.`);

/**** native Array.prototype.sort ****/

console.log('Test native browser realization of Array.prototype.sort.');

start = Date.now();

vector1.sort((a, b) => a - b);

console.log('Time to sort with Array.prototype.sort: ', Date.now() - start);

/**** Counting sort ****/

start = Date.now();

const tmpVector = Array(BOUND_UPPER + 1).fill(0);

for ( let index = 0; index < COUNT; index += 1 ) tmpVector[vector2[index]] += 1;

let _index = 0;

for ( let index = 0; index <= BOUND_UPPER; index += 1 ) {
    for ( let subIndex = 0; subIndex < tmpVector[index]; subIndex += 1 ) {
        vector2[_index] = index;
        _index += 1;
    }
}

console.log('Time to sort with Counting sort: ', Date.now() - start);

// ensure vector1 and vector2 are identical
console.assert(JSON.stringify(vector1) === JSON.stringify(vector2), 'Two sorted arrays has different elements order!');
//console.log(vector1.length === vector2.length && vector1.every((item, index) => item === vector2[index]));

console.log('Program ends.');
```

### Results

| BROWSER                 | TIME      |
|-------------------------|-----------|
| Chromium (83.0.4103.61) |           |
| Firefox (76.0.1 64-bit) |           |
| *Node.js (12.17.0)      |           |

* use `1e9` instead of `1e6` of array elements, run as `node --max-old-space-size=4096 [file]` to prevent "out of memory" error

### Theory

_Counting sort_ is the special sorting algorithm with linear time complexity **O(N)**. It means that there are some requirements for sequence that is being sorted.
In other words the algorithm is not indifferent to the input sequence. The core idea is to _count_ of repetitions of each element in a sequence and use this information
to get the final sorted array.
