# Code Splitting
This example illustrates a very simple case of [Code Splitting](https://webpack.github.io/docs/code-splitting.html) with `require.ensure`.

## example.js
``` javascript
{{example.js}}
```

## explanation
* Scripts `a.js` and `b.js` are imported normally with `require(module)` via [CommonJS](https://webpack.github.io/docs/commonjs.html):
```javascript
{{example.js-snippet}}
```
* `c.js` is imported with `require.ensure(dependencies, callback)`:
```javascript
{{example.js-snippet}}
```
* What this does: 
  * makes the module available, but doesn't execute it
  * webpack will load it on demand
* `b` and `d` are imported via CommonJS in the `require.ensure` callback:
```javascript
{{example.js-snippet}}
```
* How webpack handles this:
  * webpack detects that these are in the "on-demand" callback and loads them on-demand
  * webpack's optimizer can optimize `b` away as it is already available through the parent chunks

You can see that webpack outputs two files/chunks:

* `output.js` is the entry chunk and contains
  * the module system
  * chunk loading logic
  * the entry point `example.js`
  * module `a`
  * module `b`
* `1.js` is an additional chunk (on demand loaded) and contains
  * module `c`
  * module `d`

You can see that chunks are loaded via JSONP. The additional chunks are pretty small and minimize well.

## js/output.js
``` javascript
{{js/output.js}}
```

## js/1.js
``` javascript
{{js/1.js}}
```

Minimized

``` javascript
{{min:js/1.js}}
```

## Info
### Uncompressed
```
{{stdout}}
```

### Minimized (uglify-js, no zip)
```
{{min:stdout}}
```