Extensibility - A simple extensibility layer for your JavaScript application
==========================================
[![Build Status](https://travis-ci.org/BorisKozo/extensibility.js.png?branch=master)](https://travis-ci.org/BorisKozo/extensibility.js)

This library is a collection of simple patterns that can help you manage the modularization and decoupling of your
 application components. It is conceptually based on the SharpDevelop addin tree and is built with the
 "turtles all the way down" methodology (i.e. each concept is using the previous concepts of the library).
 This library is **not** a replacement for Backbone/Ember/Angular/Knockout/<your framework> as it provides complementary
 features that can integrate with these frameworks and in general it is designed to play well with others.

 If you have any suggestions, improvements or ideas for this library I would be more than happy to hear them and integrate them.

## Documentation

This readme file contains basic usage examples and
details on the full API, including methods,
attributes and helper functions.
To use the library, include  ````dist/extensibility.js```` or ````dist/extensibility.min.js```` in your
index.html.

The library has two dependencies - lodash and Promise polyfill for browsers that do not support ES6 Promise function.
It should be possible to load this library using AMD or Common.js loaders and in Node.js as long as the dependencies are provided.

Some objects contain private function denoted with prefixed ```$``` sign. You should not normally call these functions.
Changes made to the signature or the return value of these function does not constitute a breaking API change.

## The Registry

The center of this library is the registry. The registry is basically a tree. Each node in the tree has a string name
and it contains an array of items. We refer to these items as _addins_ (SharpDevelop: codons), more on addins in
a following section. Since every node in the tree may contain addins, each node has a _path_ within the tree.
The path contains the names of the nodes in the order they must be traversed to reach the desired node. For example,
to reach a node with the name ```foo``` which is a direct child of the node ```boo``` which is a direct child
of the tree root, we need to use the path ```boo/foo```. The root node is denoted by the empty string so the path
```/boo/foo``` is also correct. The path delimiter is ```/``` and it is (currently) hardcoded. The registry is a global
singleton.

 The Registry API is encapsulated within the ```EJS.registry``` object and is as follows:

 #### EJS.registry.$getNode(axes, createIfNotExists) -> node
 Tries to get a tree node from the registry tree based on the given _axes_. _axes_ can be a valid path string or
 an array of node names. If the requested node or any of the nodes in the path to the requested node do not exist,
 creates the node based on the _createIfNotExists_ argument. If the requested node is not found ```null``` is returned.

 ```js
 var newNode = EJS.registry.$getNode(['foo','boo'],true); //creates the node foo under the root node and the node boo
                                                          //under the foo node. The last node (i.e. boo) is returned
 console.log(newNode === EJS.registry.$getNode('foo/boo', false)) // true
 ```

#### EJS.registry.$clear()
Forcefully removes all the nodes and their content from the registry. You should never call this function unless you
are absolutley sure you know what you are doing.

#### EJS.registry.verifyAxis(axis) -> boolean
Determines if the given axis is a valid axis name for the registry (i.e. can be used as a node name). The axis should be
a non empty string which does not contain the delimiter.

```js
  EJS.registry.verifyAxis(null); //false
  EJS.registry.verifyAxis(undefined); //false
  EJS.registry.verifyAxis(10); //false
  EJS.registry.verifyAxis(''); //false
  EJS.registry.verifyAxis('foo'); //true
  EJS.registry.verifyAxis('foo/boo'); //false
  EJS.registry.verifyAxis('You can put almost any string here'); //true
```

#### EJS.registry.joinPath(<axes|paths>) -> path
Creates a single path out of the given _axes_ or _paths_. The axes or paths can be any number of separate strings
or an array containing strings up to any depth. The proper delimiter is automatically added between the
axes and paths as the path is built.

```js
  EJS.registry.joinPath('a', 'bcd', 'ef'); // 'a/bcd/ef'
  EJS.registry.joinPath('a/bcd', 'ef/g');  //'a/bcd/ef/g');
  EJS.registry.joinPath(['a', ['bcd'], ['ef', 'g']]); //'a/bcd/ef/g';
```


#### EJS.registry.splitPath(path) -> [axes]
Breaks the given path to its individual axes. Returns an array containing all the axes as strings.

```js
  EJS.registry.breakPath('abv/efg/aaa'); //['abv', 'efg', 'aaa']
```js

#### EJS.registry.pathExists(path) -> boolean
Returns true if the tree node for the given path exists in the tree. Note that when you create a node at some path,
all the nodes along the path are also created. For example if you insert into the path a/b/c then a, a/b, and a/b/c
will be created.

```js
 EJS.registry.$getNode(['foo','boo'],true);
 EJS.registry.pathExists('foo'); //true
 EJS.registry.pathExists('foo/boo'); //true
 EJS.registry.pathExists('boo'); //false
```


## Unit Tests

1. Be sure you have NodeJS and NPM installed on your system

2. Run `npm install` to install Karma and Mocha

3. From the project folder, run `npm run-script test1` to execute the unit tests

## Building

1. Run `grunt build`

If all went well, the appropriate files should be generated in the dist directory

##License
(MIT License)
Copyright (c) 2014 Boris Kozorovitzky, 

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


