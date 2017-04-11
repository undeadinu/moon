---
title: Getting Started
---

This introduction guide will get you started building basic applications with Moon in no time, to get started, create an HTML file and put this into it:

```html
<body>
  <script src="https://unpkg.com/moonjs"></script>
  <script>
    // Our Code Goes Here
  </script>
</body>
```

Now just follow along!

#### Initialization

First, let's create a new **Moon Instance**, this is where all our options for Moon go, and where we initialize it.

```js
var app1 = new Moon({
  el: "#app1",
  data: {
    msg: "Hello Moon!"
  }
});
```

The `el` option is recognized by Moon, and Moon mounts itself onto this element.

The `data` option is also recognized by Moon, this is an object where we can store all of our data, this data can be updated at any time, and Moon makes the necessary changes to the DOM in real-time!

Let's show the user this message, by adding this to the HTML:

```html
<div id="app1">
  <p>{{msg}}</p>
</div>
```

Notice the `{{mustache}}` syntax? This is used to interpolate properties in the `data` your provide. Moon analyzes these and will update this element every time you change the `msg` property.

We should now have something that looks like this:

<div id="app1" class="example">
  <p>{{msg}}</p>
</div>

<script>
  var app1 = new Moon({
    el: "#app1",
    data: {
      msg: "Hello Moon!"
    }
  });
</script>

#### Changing Data

Moon can update the DOM as a result of you changing the data. To change data, you use an **instance method** called `set`. You can now do something like:

```html
<div id="app2">
  <p>{{msg}}</p>
</div>
```

```js
var app2 = new Moon({
  el: "#app2",
  data: {
    msg: "Hello Moon!"
  }
});

app2.set('msg', "Changed Message!");
```

<div id="app2" class="example">
  <p>{{msg}}</p>
</div>

<script>
  var app2 = new Moon({
    el: "#app2",
    data: {
      msg: "Hello Moon!"
    }
  });
  app2.set('msg', "Changed Message!");
</script>

Go ahead, try entering `app2.set('msg', 'New Message!')` in the console!

#### Methods

Methods allow you to reuse code throughout your Moon applications. You can put them in the `methods` option when initializing the Moon instance. To call these methods, we use the `callMethod` function, this takes the method to call as the first parameter, and any arguments as an array in the second parameter.

You have to use the `callMethod` function every time you call a method, or else `this` won't be available as a reference to the instance.

```html
<div id="app3">
  <p>{{msg}}</p>
</div>
```

```js
var app3 = new Moon({
  el: "#app3",
  data: {
    msg: "Hello Moon!"
  },
  methods: {
    changeMessage: function(msg) {
      this.set('msg', msg);
    }
  }
});

app3.callMethod('changeMessage', ['New Message!']);
```

<div id="app3" class="example">
  <p>{{msg}}</p>
</div>

<script>
var app3 = new Moon({
  el: "#app3",
  data: {
    msg: "Hello Moon!"
  },
  methods: {
    changeMessage: function(msg) {
      this.set('msg', msg);
    }
  }
});
app3.callMethod('changeMessage', ['New Message!']);
</script>

Go ahead, try entering:
```js
app3.callMethod('changeMessage', ['Calling a Method!']);
```
 in the console!

#### Conditional Rendering

Let's get started with our first _directive!_ Directives are ways of adding special behavior to elements. Right now, we are going to use `m-if`. This lets us put in any data, including `{{mustache}}` templates into the directive as an attribute. If it is truthy, it will be rendered, if it is falsy, it won't be rendered (the element won't exist).

You can put any valid javascript expression for the value, such as `true === false`, but remember to always use templates for data.

```html
<div id="app4">
  <p m-if="{{condition}}">The Condition is True!</p>
</div>
```

```js
var app4 = new Moon({
  el: "#app4",
  data: {
    condition: true
  }
});
```

<div id="app4" class="example">
  <p m-if="{{condition}}">The Condition is True!</p>
</div>

<script>
var app4 = new Moon({
  el: "#app4",
  data: {
    condition: true
  }
});
</script>

You can also use `m-show`, and this will toggle the `display` property of the element.

Go ahead, try entering `app4.set('condition', false)` in the console!

#### Loops

Another directive (`m-for`) allows you to iterate through arrays and display them! If you change any elements of the array, the DOM will be updated to match.

```html
<div id="app5">
  <ul>
    <li m-for="item in {{list}}">{{item}}</li>
  </ul>
</div>
```

The `item` will now be available to us as an **alias** for each item in the list.

```js
var app5 = new Moon({
  el: "#app5",
  data: {
    list: ['Item - 1', 'Item - 2', 'Item - 3', 'Item - 4']
  }
});
```

<div id="app5" class="example">
  <ul>
    <li m-for="item in {{list}}">{{item}}</li>
  </ul>
</div>

<script>
var app5 = new Moon({
  el: "#app5",
  data: {
    list: ['Item - 1', 'Item - 2', 'Item - 3', 'Item - 4']
  }
});
</script>

Go ahead, try entering `app5.set('list', ['New Item', 'Another Item'])` in the console!

#### Event Listeners

Great! We've been able to conditionally render elements, and render lists, but what if we need to get data from the user? For this, and adding other events, we use the `m-on` directive.

The syntax for this directive is like: `event:method`. The event is passed as an argument, like `m-on:click`. If you need custom parameters, you can use `event:method('custom parameter')`.

```html
<div id="app6">
  <p>{{count}}</p>
  <button m-on:click="increment">Increment</button>
</div>
```

```js
var app6 = new Moon({
  el: "#app6",
  data: {
    count: 0
  },
  methods: {
    increment: function() {
      // Increment the count by one
      var count = this.get('count');
      count++;
      this.set('count', count);
    }
  }
});
```

<div id="app6" class="example">
  <p>{{count}}</p>
  <button m-on:click="increment">Increment</button>
</div>

<script>
var app6 = new Moon({
  el: "#app6",
  data: {
    count: 0
  },
  methods: {
    increment: function() {
      // Increment the count by one
      var count = this.get('count');
      count++;
      this.set('count', count);
    }
  }
});

</script>

Go ahead, try clicking the button to increment the count in real-time!

#### Components

Like React/Vue/Angular/Mithril, Moon provides a component system. There are two main types of components, we'll be talking about normal components.

##### Registering

To register a component, use the global `component` method, with the component name as the first argument, and all options as the second argument. A component can take (most) arguments a regular instance can take.

```js
Moon.component("name", {
  // options
});
```

Once this component is registered, you can use it in your HTML like:

```html
<component-name></component-name>
```

For example:

```html
<div id="app7">
  <my-component></my-component>
</div>
```

```js
Moon.component('my-component', {
  template: "<p>This is a Component!</p>"
});

var app7 = new Moon({
  el: "#app7"
});
```

This will render:

<div id="app7" class="example">
  <my-component></my-component>
</div>

<script>
Moon.component('my-component', {
  template: "<p>This is a Component!</p>"
});

var app7 = new Moon({
  el: "#app7"
});
</script>

Components can be nested within each other, and each have their own scope. Updating one component's data **will not** update any other components other than itself.

##### Props

Components do not have access to data from their parent. To pass data down from the parent, you can use `props`. Define them in your component options, and you will have access to them via `{{mustache}}` templates. You can pass them by putting them as attributes.

```html
<div id="app8">
  <my-component content="{{parentMsg}}"></my-component>
</div>
```

```js
Moon.component('my-component', {
  props: ['content'],
  template: "<p>Data from Parent: {{content}}</p>"
});

var app8 = new Moon({
  el: "#app8",
  data: {
    parentMsg: "Parent Data"
  }
});
```

<div id="app8" class="example">
  <my-component content="{{parentMsg}}"></my-component>
</div>

<script>
Moon.component('my-component', {
  props: ['content'],
  template: "<p>Data from Parent: {{content}}</p>"
});

var app8 = new Moon({
  el: "#app8",
  data: {
    parentMsg: "Parent Data"
  }
});
</script>

Go ahead, try entering `app8.set('parentMsg', 'New Parent Data')` and watch the component being updated!

#### Next Steps

Congrats! You just learned the core features of the Moon API! If you'd like to see some advanced features, check out the API docs, or the other guides.