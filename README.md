## How to Integrate Rails and React Using Shakapacker
Shakapacker is an open-source gem that makes it easy to get Rails and React together without messing with NPM or Webpack directly. If you’re building your first app in Rails and React, this is the easiest way to get started, but if you have experience with one or both technologies, there are some great benefits to using shakapacker. In this article, we’ll walk through what Shakapacker does, why you should use it, and how you can get started using it in your projects today!

## Prerequisites
Before you can use Shakapacker, you need to have a few things set up: 
- Ruby
- SQLite3( Run sqlite3 --version to confirm)
- Node.js and Yarn (Rails uses Yarn as the package manager)
- Rails 6 (which you can install with gem install rails)
-  Shakapacker gem installed.

## Shakapacker
This is the official, successor to [rails/webpacker](https://github.com/rails/webpacker).ShakaCode currently maintains it. Internal naming for shakapacker still uses webpacker where possible for v6. 

It is a tool that helps you integrate Rails and React. It takes care of all the configuration for you, so all you need to do is add a few lines of code to your project. Shakapacker is easy to use and it will make your life easier when working with both Rails and React.

## Webpack
Webpack is a  module builder built on top of Node. js. It handles the frontend of an app using HTML, JavaScript, and CSS files and other assets such as image files through plugins.

## In which situations it can be used?
Shakapacker can be used when you want to use React components in your Rails application, or when you want to use Rails views in your React application. It's also helpful when you need to share data between your Rails and React applications. You can even use Shakapacker to mix and match Ruby and JavaScript code in your project.

## Creating a hello world starter app
set up React and Rails at the same time using the following command:
```bash
rails new react-rails-webpacker --webpack=react
```
This will generate a react rails app that uses webpack with the webpacker gem as the rails wrapper
Create a controller that will be responsible for serving the index page with React. To do this, run the command below:
```bash
rails g controller pages index
```
This creates a `pages_controller.rb` file in `app/controllers` with an index action.
set the root in `/config/routes.rb` to the newly generated index page:
```rb
Rails.application.routes.draw do
  root 'pages#index'
end
```
In the `app/javascript/packs` folder, `javascript.js` and `hello_react` file will be generated. 
Clear the content of `app/views/pages/index.html.erb`  .Then add the snippet below in `app/views/layouts/application.html.erb` just before the closing head tag in `<%= javascript_pack_tag 'hello_react' %>`

This  renders `<div>Hello React</div>` at the bottom of the page.

In the hello_react.js file, the following code will be generated:
```js
import React from 'react'
import ReactDOM from 'react-dom'
import PropTypes from 'prop-types'

const Hello = props => (
  <div>Hello {props.name}!</div>
)

Hello.defaultProps = {
  name: 'David'
}

Hello.propTypes = {
  name: PropTypes.string
}

document.addEventListener('DOMContentLoaded', () => {
  ReactDOM.render(
    <Hello name="React" />,
    document.body.appendChild(document.createElement('div')),
  )
})
```
You can change  hello name from react to world or a word of your preference then run `rails s` to start the development server.

## Create react- rails app
Change the `hello_react` file to `counter.jsx` ,then change  the `pack_tag` snippet to `<%= javascript_pack_tag 'counter' %>` . In the `javascript/packs` folder add the following files, `application .css` for styling and `counter.jsx`. Create components folder in javascript folder then add a `Counter. jsx` file.

In `components/Counter.jsx` add state hook which adds React state to function components.
```jsx
import React, { useState} from 'react'
``` 
Initialize the count state to 0 by calling the useState Hook directly inside the component since we are using const to declare our function.
```jsx
const Counter = () => {
 const [count, setCount] = useState(0);
 const increase = () => setCount(count+1);
 const decrease = () => setCount(count-1);
```
useState Hook returns a pair of values, to which we give names. We’re calling our variable count because it holds the number of button clicks. 

Now re-render the Counter component by passing the new count value to it.
```jsx
  render(
<div>   
<button style={{backgroundColor: "lightblue"}}onClick={decrease}>-</button>
     <span>{count}</span>
     <button onClick={increase}>+</button>
 </div>
 )
}
export default Counter;
```
`style={{backgroudColor: “lightblue” }}` is the inline styling of the react file.

In `application/packs/counter.jsx` import React and ReactDOM  as shown below:
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
Then add the following react code:
document.addEventListener('DOMContentLoaded', () => {
 ReactDOM.render(
   <Counter/>,
   document.body.appendChild(document.createElement('div')),
 )
})
```
In the above code, the DOMContentLoaded event fires when the document is loaded and the DOM tree is entirely constructed.`ReactDOM.render` then renders the element to the DOM. Then we use the `appendChild()` to move an existing child node to a new position within the document.

Now create the application.css  file in `javascript/packs`  folder and style your app using the code below:
```css
button { background-color: pink;
   height: 100px;
   width: 100px;
   margin-right: 50px;
  margin-left: 50px;
  }
 ```
After adding the CSS file add the snippet below to `app/views/layouts/application.html.erb` file inside the head tag:
```html
<%= stylesheet_pack_tag 'application' %>
```
Don’t forget to import the external files in `javascript/packs/counter.js` file as shown below:

```js
import Counter from '../components/Counter';
import './application.css'
```
Open your app/layouts/pages/index.html.erb and add the following code:
```html
<div class=”counter-wrapper”>
<h1 style="color:blue;">Counter</h1>
<p>This is a simple demonstration of using
webpacker with react and rails hit + to add and - to subtract </p>
</div>
```
Below is your expected output :

![image](https://user-images.githubusercontent.com/61587290/186114788-5872516e-538f-474d-89d6-42fcd3be943f.png)

[Source Code](https://github.com/gatwirival/React-Rails-Webpacker)

## Add shakapacker to existing rails app

> NB: When creating a new rails app using rails 6+ we run the following command `rails new myapp --skip-javascript` 

Add the newest version of webpacker to our rails app(shakapacker). By adding gem ‘shakapacker’ to your gemfile, then run bundle add `shakapacker --strict`  
Keep your npm packages up-to-date by running `yarn` or `npm`

## Understanding the shakapacker react generated structure

1. Pack

webpack has a notion of entry points which are the files that it looks for first when it starts compiling your JavaScript code. Webpacker gem creates the application pack in the form of this application.js file under app/javascript/packs. 

2. Pack_tag

They create a link tag that references the named pack file, 

3. The webpacker.yml file. 

Configures how webpacker works and you can turn on and off different functions and extensions. You may not need to modify the file at all in most cases

### Webpacker.yml file layout

> config
 ...
>  webpack folder
- - development.js
- - environment.js
- - production.js
- - test.js

- webpacker.yml

## What are its advantages?
Shakapacker is a tool that can help you quickly and easily integrate Rails and React. It has several advantages, including - You don't need to install any dependencies other than Rails and React itself
- It's very easy to use, as all you have to do is add shakapacker in your Gemfile and run it in the terminal
- The integration process only takes about 10 minutes
- You can preview changes without restarting the server or closing the browser tab
- It uses Hot Reloading which is much faster than refreshing the page or opening a new tab each time there are updates - All files are kept under one folder so that everything is neat and tidy - If you want to switch back to plain rails for some reason, just remove shakapacker from your Gemfile
- There's also a plugin for VSCode

## Things you should consider while integrating two technologies.
There are a few things you should consider while integrating two technologies: 
- compatibility
- security 
- performance 
- scalability
- documentation
Compatibility is important because you want to make sure the two technologies can work together. Security is important because you want to make sure your data is safe. Performance is important because you want your website or application to run smoothly. Scalability is important because you want your website or application to be able to handle more traffic as it grows.

## Conclusion
If you're looking for a way to integrate Rails and React, Shakapacker is a great option. It's easy to use and can streamline your development process. Plus, it's open source, so you can contribute to the project if you find bugs or have suggestions for improvements.


