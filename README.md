# Vdt.js

> vdt is short for virtual-dom template

`Vdt.js` is a template engine based on virtual-dom technology.
It is inspired by [React](https://github.com/facebook/react)/[virtual-dom](https://github.com/Matt-Esch/virtual-dom),
and uses [JSX](https://facebook.github.io/react/docs/jsx-in-depth.html) as template syntax.

See [TodoMVC](http://javey.github.io/vdt-todomvc/) implemented by `Vdt.js`

# Features

* Just the ui. Just the template further more. But more powerful than common template.
* Virtual-dom. Diff update unlike [Handlebars](https://github.com/daaain/Handlebars)/[mustache.js](https://github.com/janl/mustache.js).
* Lightweight. Rewrite a compiler instead of [jstransform](https://github.com/facebook/jstransform). Discard ES6 syntax sugar and JS analysis, so it's faster.
* Easy to use. You can use it with any other js library, such as jQuery. See [vdt-todomvc](https://github.com/Javey/vdt-todomvc)

# Install

```shell
npm install vdt.js --save
```

# Example

[demo](http://javey.github.io/vdt/demo.html)

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>vdt template engine demo</title>
</head>
<body>
<script type="text/vdt" id="template">
    var a = 1;
    <div className="users">
        <input id="add" value={input} /> input: {input}
        <ul>
            {users.map(function(user, index) {
                return <li className="user" id={user.id}>{index}: {user.name}</li>
            })}
        </ul>
        <div>this is a({a})</div>
    </div>
</script>
<script type="text/javascript" src="../dist/vdt.js"></script>
<script type="text/javascript">
    var data = {
        users: [
            {name: 'John', id: '1'},
            {name: 'Javey', id: '2'},
            {name: 'Tom', id: '3'},
            {name: 'Sarah', id: '4'}
        ],
        input: ''
    };

    var str = document.getElementById('template').innerHTML,
        vdt = Vdt(str),
        dom = vdt.render(data);

    document.body.appendChild(dom);

    var input = document.getElementById('add');
    input.addEventListener('input', function(e) {
        data.input = e.target.value;
        vdt.update(data);
    });
    input.addEventListener('change', function(e) {
        data.users.push({name: e.target.value, id: data.users.length});
        data.input = '';
        vdt.update(data);
    });
</script>
</body>
</html>
```

# Api

## Vdt(source)

Compile `source` then return a vdt object.

* @param `source` {String|Function} JSX template source or a template function returned by `Vdt.compile`
* @return {Object} a vdt object

## Vdt.compile(source)

Compile JSX template source then return a template function which should pass to `Vdt`.

The returned function has a property named source. You can use it to pre-process JSX.

* @param `source` {String} JSX template source
* @return {Function} a template function should pass to `Vdt`.

### template.source

The source code of template function.

## The `vdt` object

The object returned by `Vdt`.

### vdt.render(data, [thisArg])

Handle data and return a dom.

* @param `data` {Object} data passed to template
* @param `thisArg` {Object} the binding of this in template
* @return {Dom} html dom

### vdt.update(data)

Update the dom using the new data.

* @param `data` {Object} the whole data passed to template
* @return {Dom} html dom which has updated

## Vdt.parse(source)

Parse JSX template to an ast object

* @param `source` {String} JSX template source
* @return {Object} abstract syntax tree object

## Vdt.stringify(ast)

Stringify the ast object to hscript string.

* @param `ast` {Object} abstract syntax tree object
* @return {String} hscript string with a return expression at end

## Vdt.virtualDom

The object exported by [virtual-dom](https://github.com/Matt-Esch/virtual-dom) module.

# Benchmark

See [Benchmark](http://javey.github.io/vdt/benchmark/benchmark.html)

## Render(compile every time)

* Vdt.js#render x 5,454 ops/sec ±2.40% (89 runs sampled)
* Lodash#render x 2,390 ops/sec ±3.68% (81 runs sampled)
* Underscore#render x 6,035 ops/sec ±5.86% (81 runs sampled)
* Handlebars#render x 959 ops/sec ±6.16% (77 runs sampled)
* Mustache#render x 4,899 ops/sec ±6.09% (84 runs sampled)

__Fastest is Underscore#render__

## Update(cache compiled result)

* Vdt.js#update x 14,724 ops/sec ±3.61% (87 runs sampled)
* Lodash#update x 7,734 ops/sec ±2.70% (84 runs sampled)
* Underscore#update x 7,989 ops/sec ±4.52% (89 runs sampled)
* Handlebars#update x 7,200 ops/sec ±2.63% (86 runs sampled)
* Mustache#update x 7,747 ops/sec ±2.40% (96 runs sampled)

__Fastest is Vdt.js#update__