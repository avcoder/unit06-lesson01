---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Intro to React
Frontend Development: Unit 06 - Lesson 01

- [ ] History of React and Build Tools
- [ ] Creating a React App using Vite
- [ ] Building (Transpiling) React app to normal JS
- [ ] Intro to JSX

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# History of React

- In 2010s, Facebook developers had to overcome a UI problem; their chat icon would indicate new messages when in reality there were none
- At the time Facebook was using a PHP variant and adding new features at a fast pace, and edge cases like this would pop up; Fixing bugs opened other bugs.
- Solution: they began creating an experimental, internal tool called React
- "Let's talk about some of the problems behind MVC that make it not scale". -- Facebook Software Engineer (Watch until  7:33 [video](https://www.youtube.com/watch?v=8pDqJVdNa44#t=3m31s))
- Today: check out how React is doing according to the [State of JS Survey 2024](https://2024.stateofjs.com/en-US/libraries/#all_tools_experience)
- As of Dec 2024, React v.19 became stable (introduced React Server Components, and "Actions")

---
transition: slide-left
---

# Review: Creating HTML elements using vanilla JS

- Q: How might you create an anchor tag, with an `href="#"`, with the text `Hello world` in vanilla JS?
- Q: Now how might you create a function that accepts an HTML tag type (ex: `h1` or `a` etc), the tag's attributes, and text content or embedded tag?
```js
const a = createElement('a', { href: '#' }, 'Click me')
const p = createElement('p', { class: 'highlight'}, a)
```

`console.log(p)` should then output:

```html
<p class="highlight">
  <a href="#">Click me</a>
</p>
```

- Would you prefer to write JS code like the first example, OR JS-like code that looked like the the HTML code above but still functioned like the JS code?

<!--
`const a = document.createElement('a')`
`a.setAttribute('href', '#')`
`a.textContent = 'Hello world'`
```
function createElement(tag, props, children) {
   const a = document.createElement(tag);
   const propsList = Object.entries(props)
   propsList.forEach(([key, val]) => a.setAttribute(key, val));

   if (typeof children === 'string') { 
      a.textContent = children;
   } else {
      a.appendChild(children);
   }
    return a
}
```
-->

---
transition: slide-left
---

# React createElement
How does React render elements?

- see [codepen](https://codepen.io/codevilla/pen/WbvqpZg?editors=1010)
```js
const element = React.createElement( // Q: How might you implement createElement() using vanilla JS?
  'p',
  { id: 'hello' },
  'Hello World!'
);

// OR alternatively use JSX instead of above.  Imagine creating nested elements using above!?
const Element = () => <h1 id='hello'>Hello World!</h1>; // syntax error? How can the browser understand "HTML"?
```

- Q: How does JSX compare to vanilla JS when creating elements?
- Q: What is a virtual DOM tree and why does React use it over regular DOM?
- Q: Why do we import 'react' and 'react-dom'?  What happens if we use JSX without importing React?
- Q: What does `console.log(element)` return?
- Q: What does `createRoot(element)` do?
- Q: What does `root.render(element)` do?

---
transition: slide-left
---

# Vite
How does the browser render React code?

- React is used as part of a build system (historically used not `vite`, but `create-react-app`)
- All of React code will get processed eventually into regular JS files
- Problems Vite (and other bundlers like Webpack) solves:
   - In modern web apps (React, Vue etc.) we often write code using languages the browser doesn't understand:
      - ES6+ JS, Typescript, JSX/TSX, SCSS/Less, Modules (import/export), asset bundling (images/fonts)
   - Since browsers don't understand this, we need tools to:
      - transpile your code to vanilla JS/CSS that browsers understand
      - bundle multiple modules into fewer ones for performance
      - optimize output for prod (minify, tree-shaking/code splitting, Hot module replacement (HMR), etc...)

---
transition: slide-left
---

# Historical JS build tools

| Tool        | Era       | Main Use                    | Key Innovation                      |
| ----------- | --------- | --------------------------- | ----------------------------------- |
| Grunt       | 2011      | Task automation             | First major task runner             |
| Gulp        | 2013      | Stream-based task runner    | Code over config                    |
| Webpack     | 2014      | Bundler                     | All-in-one, plugins, HMR            |
| Rollup      | 2015      | Library bundling            | Tree shaking + ES modules           |
| Parcel      | 2017      | App bundling (zero config)  | Fast, simple setup                  |
| Vite        | 2020      | Modern bundler + dev server | ESM in dev, Rollup in prod          |

- see [State of JS Survey for Build Tools](https://2024.stateofjs.com/en-US/libraries/build_tools/)
---
transition: slide-left
---

# Simple React project using Vite

- Run `npm create vite@latest`
- Explore folder structure and React code
- Trace flow from `index.html` to end
- Try building via `npm run build`
   - Q: What folder gets created and what's in it?
   - Q: Where'd the React code go?
   - Q: Try running Live Server in that new folder
   - Try uploading project to Netlify

---
transition: slide-left
---

# Exercise: Create simple React button

- we'll deal with JSX in more next class, but for now we'll use JSX to start thinking in components
- Goal: Create a React component called `<Button>` that has one prop called `label` that produces:
  ```jsx
  <Button label="Submit" />
  ```
   <img src="/assets/button.png" ><br/>

1. Create a new folder `/src/components`
1. Within that folder, create new component file `Button.jsx` or `Button.tsx` 
1. In that file create a simple function that returns JSX
    ```jsx
    function Button(props) {
      return <button style={{ backgroundColor: "#3b82f6" }}>{props.label}</button>;
    }
    export default Button;
    ```
1. import `Button.jsx` inside `App.tsx` and show it (for now use style for bg, and `props.label`)

---
transition: slide-left
---

# Exercise: Create simple React button (pg.2)
Continuing with the last exercise

1. add a prop called `badge` that allows you to display a number beside the label
  ```jsx
  <Button label="Submit" badge="2" />
  ```
  <img src="/assets/button2.png" >

---
transition: slide-left
---

# Event Handlers: onClick

- let's make it so that if our `<Button>` is clicked, it will console.log('hello world')
- ex: <button onClick={ function-defn-goes-here } ... >
  ```jsx
  <Button
    label="Submit"
    clickHandler={() => console.log("hello world")}
  />

  <button style={{ backgroundColor: "#3b82f6" }} onClick={props.clickHandler}>
  ```

---
transition: slide-left
---

# Exercise: Replace button with Button

- in `App.tsx` it currently uses `<button>` to display `count is 1`
- Delete that and use our `<Button>` component instead 

<img src="/assets/button3.png" >


---
layout: image-right
transition: slide-left
image: /assets/dodds.png
backgroundSize: 400px 400px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 📖 [Do you build FE or BE first?](https://www.codemzy.com/blog/build-front-end-or-back-end-first)
- 🧢 [React Crash Course](https://www.youtube.com/watch?v=LDB4uaJ87e0)
- 🗾 [Beginner's Guide to React](https://egghead.io/courses/the-beginner-s-guide-to-react)
- ⛰️ [Epic React](https://www.epicreact.dev/tutorials/get-started-with-react)
- 🎬 [Scrimba - Learn React](https://scrimba.com/learn-react-c0e)r

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

---
transition: slide-left
---

# React DevTools + Common Errors

- Install React DevTools for your browser and explore (should be similar to Vue DevTools)

Try the following common errors to see what errors look like in React:
- Forgetting to `return` JSX
- Confusing camelCase for HTML attributes `tabindex vs tabIndex`
- Using class instead of className
- if using inline styles, using hyphenated names instead of camelCase `background-color vs backgroundColor`
- Multiple top-level elements without a wrapper/fragment
- Using if statements directly inside JSX like `return ( if (show) { <p>Visible</p> });`
- Not using `key` in lists
- JSX requires properly closed tags, even for void elements. ex: `<input /> <br />`
- any red squiggly lines? React related? TS related?

---
transition: slide-left
---

# Exercise: Use React DevTools to find component

- Just like you right-click inspect DOM element to find that particular HTML element in the DOM, you can do similar with React components
- Tip: try clicking on a React component, then in console enter `$r`
- Goto: instagram.com
   - Try to find the React component for the Login button

---
transition: slide-left
---

# Separation of Concerns

<img src="/assets/sepconcerns.png" style="height: 450px">

---
transition: slide-left
---

# Exercise: Draw components
Use your Zoom pencil to draw all the potential components on the following 


<img src="/assets/weather.png">

---
transition: slide-left
---

# Exercise: Draw components (pg.2)
Use your Zoom pencil to draw all the potential components on the following images

1. [Payment page](https://ebizcharge.com/wp-content/uploads/2022/06/Tarlet-Payment-Page.png)
1. [LinkedIn Learning Courses page](https://spotlight.lehigh.edu/sites/spotlight.lehigh.edu/files/Screen%20Shot%202020-08-14%20at%2010.32.20%20AM_0.png)
1. [Dashboard](https://assets.justinmind.com/wp-content/uploads/2020/02/dashboard-design-example-hcare.png)

---
transition: slide-left
---

# Exercise: Create simple React websites

- Create a component called `<FollowPerson>`
- see https://tachyons.io/components/lists/follower-notifications/index.html

   <img src="/assets/inbox.png" />

- What props do you think we'll need for this component?
- What other potential components could be developed/imported inside `<FollowPerson>` component?

---
transition: slide-left
---

# Exercise: Create simple React websites

- Reverse engineer: convert this entire [Email template layout](https://pure-css.github.io/layouts/email/) to JSX; [Github code here](https://github.com/pure-css/pure/tree/main/site/static/layouts/email)


---
transition: slide-left
---

# Homework

- Start working on your "Weather Forecasting App" assignment due Aug 17 midnight EST