# React createElement()

## Overview

In this lesson, we'll call `React.createElement()` and also describe how we can build elements out of other React elements. 

## Objectives
1. Call `React.createElement()` and describe its arguments
2. Use `ReactDOM.render()` to render an element to a page 
3. Describe how we can build elements out of other React elements
4. Add child elements and nested child elements
5. Pass properties to an element


## Baby steps
![Baby steps](https://media.giphy.com/media/2D4tYGhHKFYre/giphy.gif)

Let's write some code! Before we start though, we need to set some things up. We're going to use the React (and
ReactDOM) libraries, so we should include those in our code. If you haven't done so yet, run `npm install` to install
our dependencies. Now let's add the libraries by loading the right scripts in our `index.html` file:

```html
<script src="node_modules/react/dist/react.js"></script>
<script src="node_modules/react-dom/dist/react-dom.js"></script>
```

These should go in the body tag, after any content, but _before_ the `index.js` script. That file will contain our own
code, so it expects React to already be loaded by that point!

To kick things off, let's create a really basic page title in React using `React.createElement()`. Open up your `index.js`
file and add the following:

```js
const title = React.createElement('h1', {}, 'My First React Code');
```

Let's briefly talk about the arguments of `React.createElement()`.

The first one is type of element we're creating, in this case an `<h1>` tag. This could also be another React component.
If we're creating an HTML element, we pass in the name as a string, just like we did above. If we're creating a React
component, we pass in the variable that the component is assigned to.

The second argument is an object containing properties ('props' in React terms) that get passed to the component. Since
we're just getting started with React, we won't use these just yet — but be aware that the second options serves this
purpose.

Finally, the last argument is the children of that component. This can be a quoted string like shown above, in which
case the content will be interpreted as text. However, we can also pass in a reference to another component, allowing us
to nest elements and components within each other (we'll get to that in a bit).

Now that we have our element, it's time to render it to the page. We do this using `ReactDOM.render()`. This takes two
arguments: the first one being the thing we want to render (our `title` element), and the second one is a target DOM
node to render things into.

```js
ReactDOM.render(
  title,
  document.getElementById('main')
);
```

Even though all of this could easily fit on one line, it's generally a good idea to add line breaks to your arguments
for `ReactDOM.render()`. Sometimes, the element you're rendering to the page can also contain children itself. The
formatting using line breaks allows us to keep our code clean.

Make sure to refresh the page, and voila, our title appears!

## Becoming a parent
Now that we know how to render stuff, let's make our app a little more complex by introducing child elements. We'll
create an element to render our title in (let's call it a container). Remember how we added a text child in our `title`
component? Now we're going to add an element as a child, so we pass it by reference instead of using a string:

```js
const title = React.createElement('h1', {}, 'My First React Code');
const container = React.createElement('div', {}, title);

ReactDOM.render(
  container,
  document.getElementById('main')
);
```

Our `title` seems a little lonely though... Let's add a sibling! We'll create a `paragraph` element and then pass it as
another child to our `container` children.

```js
const title = React.createElement('h1', {}, 'My First React Code');
const paragraph = React.createElement('p', {}, 'Writing some more HTML. Cool stuff!');
const container = React.createElement('div', {}, [title, paragraph]);

ReactDOM.render(
  container,
  document.getElementById('main')
);
```

Note how if we want to add multiple children, we use an _array_!

## Your children's children
We can nest children as much as we want. We also don't need to store our elements in variables before using them, we can
declare them inline as well (though the downside of this is less readable code):

```js
const list =
  React.createElement('div', {},
    React.createElement('h1', {}, 'My favorite ice cream flavors'),
    React.createElement('ul', {},
      [
        React.createElement('li', {}, 'Chocolate'),
        React.createElement('li', {}, 'Vanilla'),
        React.createElement('li', {}, 'Banana')
      ]
    ));

ReactDOM.render(list, document.getElementById('main'));
```

## Adding attributes
As mentioned before, we pass properties to an element using the second argument (an object, which we've left empty for
now). Suppose we wanted to add some classes to make our ice cream flavors stand out. What would we need to add? Let's
try this...

```js
const list =
  React.createElement('div', {},
    React.createElement('h1', {}, 'My favorite ice cream flavors'),
    React.createElement('ul', {},
      [
        React.createElement('li', { class: 'brown' }, 'Chocolate'),
        React.createElement('li', { class: 'white' }, 'Vanilla'),
        React.createElement('li', { class: 'yellow' }, 'Banana')
      ]
    ));

ReactDOM.render(list, document.getElementById('main'));
```

Oops! That doesn't seem to work. If we take a look at our console, we'll see a helpful error message (ignore the error
message about a `key` property, we'll get to that in another lesson):

```
Warning: Unknown DOM property class. Did you mean className?
    in li
    in ul
    in div
```

Using `className` instead will do the trick. Awesome! The prop is called `className` because `class` is a _reserved
keyword_ in JavaScript. Using reserved keywords as keys in an object is something that you should never do, since this
can result in unexpected behavior. Instead, React expects the `className` prop instead, if we want to add a class to
our element.

We can also add any other HTML attributes here, like `disabled`, `id`, and so on. These props are also used to pass in
custom data to our components, but we'll get to that later!

## Resources
- [Raw React](http://jamesknelson.com/learn-raw-react-no-jsx-flux-es6-webpack/)
