# React Components Basics

## Learning Goals

- Understand what a React component is and what it can be used for
- Write React components and identify the DOM elements they create

## Introduction

In this lesson, we'll introduce the heart of React: components. This will
include explaining why they're important and examining a few examples. If the
idea and application of components doesn't click immediately, _do not worry!_
The different moving parts required to understand how to use them will fall into
place as we move forward.

Let's examine a high level overview of what a React component is before we
implement one. The official [React documentation on components][react component]
says it best:

> Components let you split the UI into independent, reusable pieces, and think
> about each piece in isolation.

Components modularize both _functionality_ and _presentation_ in our code. In
order to understand how powerful this is, consider just how intricate web
applications can become. The difficulty in logically arranging, architecting,
and programming these web applications increases with their size. Components are
like little packages: they help us keep everything organized and predictable
while abstracting the ['boilerplate'][boilerplate] code.

Components can do many things, but their end goal is always the same: they all
must contain a snippet of code that describes what they should render to the
DOM.

## React Application Idea

Enough of a description — let's see some examples! While the possibilities of
what we can do with components are endless, the first thing we need to
understand about them is the ways in which they act as **code templates**. Let's
start simply and build up from there using the following as an example:

Let's imagine we want a blog article describing the fact (note: not opinion) of
why Bjarne Stroustrup has the [perfect lecture oration.][bjarne-stroustrup]
We also want our blog article to display comments made by readers.

Fork and clone the repo for this lesson if you'd like to follow along.

After forking the repo, run the `npm install` command to install all the
dependencies and the `npm start` command to run the app. You should see a blank page
in the browser.

### Step 1: Write the Components

When we create components, typically we want each one to be in separate files. 
This type of modularity is part of what makes components reusable and more 
readable as projects get larger. 

These component files should be created in the `src` folder and when using 
TypeScript, they should have the `.tsx` extension. Let's create the file 
for our articles called `Article.tsx`. 

Then, let's write the component for our article:

```jsx
// src/Article.tsx
function Article() {
  return (
    <div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
  );
}
```

Take a moment to read that code line by line:

- We declare a function, `Article`
- The function has a return value of **JSX**, which is our way of telling React
  "Hey, when you want to put this component on the DOM, here is what it should
  become!"
    - Note how we do not explicitly type the function's return value to be 
    `JSX.Element`, it is inferred by TypeScript.

When React creates this element and adds it to the DOM, the resulting element
will look just as you would expect:

```html
<div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
```

> Let's see what it would look like if we were to only render this one component
> in the DOM:

> ![component article example](https://curriculum-content.s3.amazonaws.com/react/component-article-example.png)

> We will learn how to render the component in the next section.

That takes care of our `Article` part of our application. Now let's make a
component to display a single user's comment. 

Create a separate file for the comment named `Comment.tsx`. Inside that file,
write the component:

```jsx
function Comment() {
  return <div>Naturally, I agree with this article.</div>;
}
```

Take the time to read that component line by line. Here is the element that this
would create when added to the DOM:

```html
<div>Naturally, I agree with this article.</div>
```

In both of our examples, React is taking JavaScript code, interpreting that
special JSX syntax within the `return()` statement, and spitting out plain old
HTML that browsers will know how to represent to the user.

Once we have our components in hand, it's time to actually use them.

### Step 2: Use the Components

Now that we have these components written, how do we actually display them on 
the browser? We need to make sure some _other_ component is making use of them 
in its **return statement**. Every React application has some top level component(s). 
Very often, this top level component is simply called `App`. 

For our example, we are following the `App` convention. We've already provided the 
`App.tsx` file for you. Right now, it only has an empty `div`. We need to render the 
Article and Comment components we just created. 

Before we can do so, don't forget we need to `import` the components we want to use 
at the top of the `App.tsx` file: 

```js
// src/App.tsx
import Article from "./Article";
import Comment from "./Comment";
```

Now that `App` has access to our components, we can render them:

```jsx
// src/App.tsx
import Article from "./Article";
import Comment from "./Comment";

function App() {
  return (
    <div>
      <Article />
      <Comment />
    </div>
  );
}
```

Here we can see JSX coming into play a bit more. The code inside the `return()`
still looks a lot like regular HTML, but in addition to rendering a regular old
HTML `<div>` element, we're also rendering our two components. Components are 
rendered in other components by using the same element syntax (`<>`) as HTML -
where instead of the name of a native HTML element, we use the name of the component. 

We've created code that is not only well structured and modular, but also a 
straightforward description of what we want the `App` component to do: render 
the article first, followed by the comment. Here is what the resulting elements 
will look like in the DOM:

```html
<div>
  <div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
  <div>Naturally, I agree with this article.</div>
</div>
```

![component article comment example](https://curriculum-content.s3.amazonaws.com/react/component-article-comment-example.png)

This unpacks logically. The `App` component (being our top level component)
wraps around both `Article` and `Comment`, and we already know what they look
like when they are turned into HTML.

As you may expect, we refer to the `App` component as both the `Comment` and
`Article` component's _parent_ component. Inversely, we refer to `Comment` and
`Article` as _children_ components of `App`.

> How does the App component get rendered, then? Recall from the sample app we 
> looked at in a previous lesson, the `index.tsx` takes the top level component,
> usually `App` and renders it to the DOM.

## Naming Components

You'll notice that both of the custom components we created, `Article` and
`Comment`, are declared as functions whose names start with a **capital
letter**:

```jsx
function Article() {
  return (
    <div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
  );
}

function Comment() {
  return <div>Naturally, I agree with this article.</div>;
}
```

This naming convention is important for a couple very good reasons:

- It helps React developers to easily differentiate between regular functions and
  React components
- More importantly, it's a [rule that we must follow][component capitalization]
  in order for React to render our components correctly.

[component capitalization]:
  https://reactjs.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized

For instance, if we defined our `Article` component using a lower-case letter,
like this:

```jsx
function article() {
  return (
    <div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
  );
}
```

When it came time to use that component within another component, React would
treat it as a regular `<article>` HTML element rather than one of our custom
components:

```jsx
function App() {
  return (
    <div>
      <article />
    </div>
  );
}

// returns these DOM elements:
// <div>
//  <article />
// </div>
```

By naming it with a capital letter instead, we get the desired DOM
elements returned:

```jsx
function App() {
  return (
    <div>
      <Article />
    </div>
  );
}

// returns these DOM elements:
// <div>
//  <div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
// </div>
```

## A Note on Classes

As you're exploring the React documentation, and finding other resources on
React on the internet, you'll probably notice there are multiple syntaxes you
can use for creating components: **function** components and **class**
components.

A **function** component looks like this:

```jsx
function Comment() {
  return <div>Naturally, I agree with this article.</div>;
}
```

Or using the arrow function syntax:

```jsx
const Comment = () => <div>Naturally, I agree with this article.</div>;
```

A **class** component looks like this:

```jsx
class Comment extends React.Component {
  render() {
    return <div>Naturally, I agree with this article.</div>;
  }
}
```

For many years, the only way to work with certain key features of React —
_state_ and _lifecycle_ — was to use **class** components. Since the
introduction of [**hooks**][hooks] in React 16.8, this is no longer true, and
function components can be used for (almost) everything that class components
can.

React's recommendation is that components should be written as function
components moving forward, but class components will continue to be supported as
well.

It's important to learn more about class components later on, so that when you
encounter them in legacy code, you'll still be able to work with them. However,
for the time being, we'll just be focusing on function components.

## Conclusion

We just introduced simplified, bare bones, React components. They are used to
house modularized front end code. In our example — as is often the case — they
contain information on how a portion of our application should be turned into
HTML.

**The minimum requirement for a React component is that it must be a function
that starts with a capital letter and returns JSX.**

Going forward, we will continue with this example and show how components can be
re-used and how they can be written as templates in which content is populated
dynamically.

## Resources

- [React Top-Level API](https://reactjs.org/docs/react-api.html)
- [Introducing JSX](https://reactjs.org/docs/introducing-jsx.html)
- [React Docs](https://reactjs.org/docs/getting-started.html)
- [React Docs (beta)][beta docs]

[react component]: https://reactjs.org/docs/components-and-props.html
[boilerplate]: https://en.wikipedia.org/wiki/Boilerplate_code
[bjarne-stroustrup]: https://www.youtube.com/watch?v=JBjjnqG0BP8
[react docs rewrite]: https://github.com/reactjs/reactjs.org/issues/3308
[react docs hooks]: https://reactwithhooks.netlify.app/
[hooks]: https://reactjs.org/docs/hooks-intro.html
[beta docs]: https://beta.reactjs.org
