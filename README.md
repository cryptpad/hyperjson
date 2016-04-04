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

### `fromDOM(domElement[, predicate, filter])`

Accepts any DOM element and returns JSON (HyperJSON) representing all the relevant attributes of that DOM element and its children.

`fromDOM` accepts two optional arguments, both of which are functions:

#### predicate(DOMElement)

If you provide this function, `fromDOM` will call it for every element which it will attempt to serialize.
If the predicate returns a truthy value, it will proceed to serialize it and its children.
If the predicate returns a falsey value, it will terminate.

```
var hj = Hyperjson.fromDOM(document.body, function (el) {
    // suppose you don't want to serialize h4 elements or their children
    return el.tagName !== 'H4';
});
```

#### filter(hyperjson)

After having serialized the tree, but before returning the results, `fromDOM` will run the contents through `filter`.
At this point, you are free to modify the hyperjson in any way, adding/removing/modifying elements as necessary for your purposes.

Anything that you don't want serialized should be filtered here.

```
var hj = Hyperjson.fromDOM(document.body, null, function (h) {
    // suppose you want to serialize 'STRIKE' as 'S' elements
    if (h[0] === 'STRIKE') {
        // this will replace <STRIKE> recursively
        h[0] === 'S';
    }
    return h;
});
```

### `callOn(hyperJSON, callback)`

Accepts valid hyperjson and a callback which adheres to the Hyperscript API:

```
function myCallback (tagType, attributes, children) {
    /* your code here */
};
```

The most prominent examples of callbacks are [hyperscript](https://github.com/dominictarr/hyperscript) and [virtual-hyperscript](https://github.com/Matt-Esch/virtual-dom), which produce DOM elements or virtual-DOM elements, respectively.

You can create custom plugins for filtering or modifying elements, or for producing alternative data structures that adhere to a similar structure.
