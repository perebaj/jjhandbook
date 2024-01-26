# Frontend

Most of my ideas and personal projects don't have a fancy frontend. Since I started to code, I've been more interested in AI, Big Data and backend stuff. But, as the same way that I'm focused to recap some [DS&A concepts](../dsa/), I think that is a good opportunity to learn more about frontend development.

So here I will try to keep a list of topics that I want to learn and some resources that I'm using to do that.

![frontend](./frontend.png)

## How I'm planning to learn

- Instead of starting to learn from a public resource, I will try to use to payed course that I have access, after have a good base, then I think that would be a good idea to start to learn from free resources, reading articles and documentations.
To do that, I will use the [Rocketseat Platform](https://www.rocketseat.com.br/)

# React Course

## What is React?

React is a library that enable the rendering of components, that in other words, are pieces of code, such as HTML, CSS and JavaScript, that can be reused in the application.

## Rendering Patterns

### SSR(Server Side Rendering) and SPA(Single Page application)

Both are rendering pattern that we can use to build our frontend application. The SSR pattern was the first one to be used, but with the evolution of the web, the SPA pattern started to be more used.

Basically, the SSR pattern is used to render the application on the server side, and then send the HTML to the client. The SPA pattern is used to render the application on the client side, using JavaScript.

In the end, the SPA, splits the application in two parts, the client and the server. The server is responsible to provide the data to the client, and the client is responsible to render the application. This was caused by the evolution of the web, to create more interactive applications, that need to be updated frequently, consuming some backend API to get the data.

### Bundler & Compilers

**Compilers:** 

Tools that are responsible to convert a code from a language that my ecosystem understand. The most common compiler that we use in the frontend is the Babel;

**Bundlers**:

It's a tool that bundle all the code(all files that we need to run our application) in a single file. The most common bundler that we use in the frontend is the Webpack, Vite and Snowpack.

### Basic tools to get started 

- Node
- NPM
- N (Node version manager)
- Vite (Bundler)

### Components

Components are pieces of code that can be reused in the application. In other words, a functions and return HTML, CSS and JavaScript.

**Example**

```jsx
function App() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
```

OBS: JSX file type extension, that allow us to write HTML inside the JavaScript code.

#### Named and Default exports

**Named exports**

```jsx
// Post.jsx

export function Post() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

// index.jsx
import { Post } from './Post';
```

**Default exports**

```jsx
// Post.jsx
function Post() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default Post;

// index.jsx

import App from './App';
```

### Props

Properties are used to pass data through components.

**Example**

```jsx
//Post.jsx

export function Post(props) {
  return (
    <div>
      <h1>{props.title}</h1>
    </div>
  );
}

// index.jsx

import { Post } from './Post';

function App() {
  return (
    <div>
      <Post title="Hello World" />
    </div>
  );
}
```
