# hyperjson

An abstract datatype compatible with the [hyperscript](https://github.com/dominictarr/hyperscript "Hyperscript on github") API.

## What's _hyperscript_?

Hyperscript is everything you need to know about a dom element to be able to recreate it.
Instead of writing `<h1>Hello!</h1>` you write `h('h1', 'Hello!')`.

Hyperscript works clientside and serverside, and there are now several other projects adhering to the same API.

## What's _hyperjson_?

Hyperjson is an abstraction of the data component (the arguments passed to the `h` function) from the function itself.
It is intended to be used clientside.

It provides two functions:

### `fromDOM(domElement)`

Accepts any DOM element and returns JSON (HyperJSON) representing all the relevant attributes of that DOM element and its children.

### `callOn(hyperJSON, callback)`

Accepts valid hyperjson and a callback which adheres to the Hyperscript API:

```
function myCallback (tagType, attributes, children) {
    /* your code here */
};
```

The most prominent examples of callbacks are [hyperscript](https://github.com/dominictarr/hyperscript) and [virtual-hyperscript](https://github.com/Matt-Esch/virtual-dom), which produce DOM elements or virtual-DOM elements, respectively.

You can create custom plugins for filtering or modifying elements, or for producing alternative data structures that adhere to a similar structure.
