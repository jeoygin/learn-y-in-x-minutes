# Learn Polymer in x minutes

## What's Polymer

Web Components usher in a new era of web development based on encapsulated and interoperable custom elements that extend HTML itself. Built atop these new standards, Polymer makes it easier and faster to create anything from a button to a complete application across desktop, mobile, and beyond.

## Install Polymer with Bower

Create `bower.json` file for project with the following command from root of the project:

```sh
$ bower init
```

Install Polymer:

```sh
$ bower install --save Polymer/polymer
```

## Build a Polymer element

Polymer provides ability to create declarative custom elements. We call these "Polymer elements". They looks like any other DOM element, but actually they are filled with handy features. 

### my-element

**1. Load the [Polymer core](https://www.polymer-project.org/docs/polymer/polymer.html)**

```html
<link rel="import" href="../bower_components/polymer/polymer.html">
```
**2. Declare custom element using `<polymer-element>`**

```html
<polymer-element name="my-element" noscript>
  <template>
    <span>I'm <b>my-element</b>. This is my Shadow DOM.</span>
  </template>
</polymer-element>
```

Save the element `<my-element>` to a file `elements/my-element.html`

Notice:

* The `name` attribute is required and must contain a "-".
* The noscript attribute indicates that this is a simple element that doesn’t include any script. An element declared with noscript is registered automatically.

### Reusing other elements

First, install the element:

```sh
$ bower install Polymer/core-ajax
```

And import the element in my-element.html:

```html
<link rel="import" href="../bower_components/polymer/polymer.html">
<link rel="import" href="../bower_components/core-ajax/core-ajax.html">

<polymer-element name="my-element" noscript>
  <template>
    <span>I'm <b>my-element</b>. This is my Shadow DOM.</span>
    <core-ajax url="http://example.com/json" auto response="{{resp}}"></core-ajax>
    <textarea value="{{resp}}"></textarea>
  </template>
</polymer-element>
```

## Create A Simple App

Create an index.html that imports new element `<my-element>`. Remember to include `webcomponents.min.js`.

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- 1. Load platform support before any code that touches the DOM. -->
    <script src="bower_components/webcomponentsjs/webcomponents.min.js"></script>
    <!-- 2. Load the component using an HTML Import -->
    <link rel="import" href="elements/my-element.html">
  </head>
  <body>
    <!-- 3. Declare the element by its tag. -->
    <my-element></my-element>
  </body>
</html>
```

Run the app with Python 2.x:

```sh
$ python -m SimpleHTTPServer 8080
```

Open [http://127.0.0.1:8080/](http://127.0.0.1:8080/) you will enbrace the magic.

## Polymer Features

### Add properties and methods

To define a public API, include a `<script>` tag that calls the `Polymer(...)` constructor.

```html
<polymer-element name="xxx-element">
  <template>
  </template>
  <script>
    Polymer({
      ready: function() {
        //...
      }
    });
  </script>
</polymer-element>
```

### Add lifecycle methods

Lifecycle callbacks are special methods you can define on your element to handle something when the element goes through important transitions:

* created(): when a custom element has been registered
* ready(): when Polymer finishes its initialization

```html
<polymer-element name="ready-element">
  <template>
    This element has a ready() method.
    <span id="el">Not ready...</span>
  </template>
  <script>  
    Polymer({
      owner: "Daniel",
      ready: function() {
        this.$.el.textContent = this.owner +
                                " is ready!";
      }
    });
  </script>
</polymer-element>
```

### Declarative data binding

Data binding is a great way to quickly propagate changes in your element. You can bind properties in your component using the `{{}}`.

```html
<polymer-element name="name-tag">
  <template>
    This is <b>{{owner}}</b>'s name-tag element.
  </template>
  <script>
    Polymer({
      owner: 'Daniel'
    });
  </script>
</polymer-element>
```

### Publish properties

Published properties can be used to define an element's "public API". Publish a property by listing it in the attributes attribute in `<polymer-element>`.

```html
<polymer-element name="color-picker"
         attributes="owner color">
  <template>
    This is a <strong>{{owner}}</strong>'s color-picker. 
    He likes the color <b style="color: {{color}}">{{color}}</b>.
  </template>
  <script>
    Polymer({
      // These default values are overridden
      // by the user's attribute values.
      color: "red",
      owner: "Daniel"
    });
  </script>
</polymer-element>
```

### Automatic node finding

Shadow DOM, on the other hand, is a self-contained document-like subtree; IDs in that subtree do not interact with IDs in other trees. Each Polymer element generates a map of IDs to node references in the element’s template. This map is accessible as `$` on the element and can be used to quickly select the node you wish to work with.

## Reference

1. [Getting the code](https://www.polymer-project.org/docs/start/getting-the-code.html)
2. [Polymer in 10 minutes](https://www.polymer-project.org/docs/start/creatingelements.html)
3. [API reference](https://www.polymer-project.org/docs/polymer/polymer.html)
