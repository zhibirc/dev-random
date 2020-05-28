## Native browser Array.prototype.sort vs Counting sort implementation

### [2020-05-07]

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

let _vector;
let start;

console.log('Program starts.');

for ( let index = 0; index < COUNT; index += 1 ) vector.push(emitRandom());
_vector = [...vector];

console.log(`Array of ${COUNT} random integers is generated.`);

/**** native Array.prototype.sort ****/

console.log('Test native browser realization of Array.prototype.sort.');

start = Date.now();

_vector.sort((a, b) => a - b);

console.log('Time to sort with Array.prototype.sort: ', Date.now() - start);

_vector = [...vector];

/**** Counting sort ****/

// counting sort (use element "properties")
// each element is in range 0..1000 inclusively

start = Date.now();

const tmpVector = Array(COUNT).fill(0);

for ( let index = 0; index < COUNT; index += 1 ) tmpVector[_vector[index]] += 1;

let _index = 0;

for ( let index = 0; index < COUNT; index += 1 ) {
    for ( let subIndex = 0; subIndex < tmpVector[index]; subIndex += 1 ) {
        _vector[_index] = index;
        _index += 1;
    }
}

console.log('Time to sort with Counting sort: ', Date.now() - start);

console.log('Program ends.');
```

#### Results

| BROWSER                 | TIME      |
|-------------------------|-----------|
| Chromium (83.0.4103.61) |           |
| Firefox (76.0.1 64-bit) |           |
