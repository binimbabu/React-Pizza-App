
Working with components, props, and JSX


Rendering the Root Component and Strict Mode

Webpack bundler looks for the entry point to the project through 'index.js'

Purpose of Strict Mode
Strict Mode renders all components twice during development to help find certain bugs and checks if outdated parts of the React API are being used. While not groundbreaking, it is a good practice to always activate Strict Mode when developing React applications






Components as Building Blocks

React application are entirely made of components. Components are building blocks of user interfaces. Components are piece of that has its own data, logic and appearance (how it works and looks). To build complex UI's by building multiple components and combining them. Components can be reused , nested inside each other and pass data between them.


function App() {
  return (
    <div>
      <h1>Welcome to React</h1>
      <Pizza />
    </div>
  );
}

function Pizza() {
  return <h2>Pizza</h2>;
}

Pizza function is nested in App function (i.e  <Pizza />)



What is JSX

JSX is a declarative syntax that we use to describe what components look like and how they work based on their data and logic. So, it's all about the component's appearance. In practice, this means that each component must return one block of JSX, which React will then use to render the component on the user interface.
JSX is an extension of JavaScript that allows to Embed Javascript, CSS and React components into HTML.
JSX converted to JavaScript using Babel. This conversion is necessary because the browser isn't aware of JSX. JSX is declarative it describes what Ui should look like using JSX based on current data. React is an abstraction away from DOM and never touches DOM. UI as a reflection of current data. 





Styling in components

Inline style

function Header() {
  return (
    <h1
      style={{
        color: "red",
        fontSize: "32px",
        textAlign: "center",
        textTransform: "uppercase",
      }}
    >
      React Pizza App
    </h1>
  );
}




Using Variables for Styles

function Header() {
  const style = {
    color: "red",
    fontSize: "32px",
    textAlign: "center",
    textTransform: "uppercase",
  };
  return <h1 style={style}>React Pizza App</h1>;
}



Importing CSS Files

Webpack takes the imported css files ad injecting them into the application.


import "./index.css";

function App() {
  return (
    <div className="container">
      <Header />
      <Menu />
      <Footer />
    </div>
  );
}

function Header() {
  return (
    <header className="header">
      <h1>React Pizza App</h1>
    </header>
  );
}

function Menu() {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <Pizza />
      <Pizza />
      <Pizza />
      <Pizza />
    </main>
  );
}
function Footer() {
  const hour = new Date().getHours();
  const openhour = 12;
  const closeHour = 22;
  const isOpen = hour >= openhour && hour <= closeHour;
  // else alert("Sorry we are closed");
  return (
    <footer className="footer">
      We are currently open at {new Date().toLocaleTimeString()}
    </footer>
  );
}

function Pizza() {
  return (
    <div>
      <img src="pizzas/spinaci.jpg" alt="Spinach" />
      <h3>Pizza</h3>
      <p>Tomato, mozarella, ham, aragula, and burrata cheese</p>
    </div>
  );
}






Passing and Receiving Props

Props essentially allow us to pass data between components, particularly from parent components to child components. We can imagine props as a communication channel between a parent and a child component.

To define props, we do it in two steps: first, we pass the props into the component, and second, we receive the props in the component that we pass them into

Full example:-

function Menu() {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <Pizza
        name="Pizza"
        ingredient="Tomato, mozarella, ham, aragula, and burrata cheese"
        photoName="pizzas/spinaci.jpg"
        price={10}
      />
      <Pizza
        name="Pizza funghi"
        photoName="pizzas/funghi.jpg"
        ingredient="Tomato Basil"
        price={20}
      />
    </main>
  );
}

function Pizza(props) {
  return (
    <div className="pizza">
      <img src={props.photoName} alt={props.name} />
      <div>
        <h3>{props.name}</h3>
        <p>{props.ingredient}</p>
        <span>{props.price + 3}</span>
      </div>
    </div>
  );
}

Here we pass props as shown below

 <Pizza
        name="Pizza"
        ingredient="Tomato, mozarella, ham, aragula, and burrata cheese"
        photoName="pizzas/spinaci.jpg" price={10}
      />


We receive props as below

function Pizza(props)

place the props like this (i.e src={props.photoName})

<img src={props.photoName} alt={props.name} />


price={10} is given to denote its a number


Props are used to pass data from parent components to child components. Props is an essential tool to configure and customize components. With props parent components control how child components look and work. In props we can pass single values, arrays, objects, functions, even other components.




Rules of JSX

JSX works like HTML, but we can enter 'JavaScript mode' by using {} for text or attributes.
We can place JavaScript expression inside {}. Examples: reference variables, create arrays or objects, map(), ternary operator.
Statements (like if/else, for, switch) aren't allowed in JSX.
JSX produces a JavaScript expression. But we can write JSX anywhere inside a component ( in if/else, assign to variables, pass it into functions). A piece of JSX can only have one root element. If you need more use <React.Fragment> (or short <>)


Difference between JSX and HTML

className instead of HTML's class.
htmlFor instead of HTML's for
Every tags need to be closed (<img /> )
All eventHandlers and other properties need to be camelCased (example onClick, onMouseOver)
CSS inline styles are written like this {{<style>}}
(to reference a variable and then an object)
CSS property names are also camelCased
Comments need to be {}







Rendering Lists


function Menu() {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <ul className="pizzas">
        {pizzaData.map((pizza) => (
          <Pizza pizzaObj={pizza} key={pizza.name} />
        ))}
      </ul>
    </main>
  );
}

function Pizza(props) {
  return (
    <li className="pizza">
      <img src={props.pizzaObj.photoName} alt={props.pizzaObj.name} />
      <div>
        <h3>{props.pizzaObj.name}</h3>
        <p>{props.pizzaObj.ingredient}</p>
        <span>{props.pizzaObj.price}</span>
      </div>
    </li>
  );
}



Full example

import React from "react";
import ReactDOM from "react-dom/client";
//import ReactDOM from "react-dom"; //react version before 18
import "./index.css";

const pizzaData = [
  {
    name: "Focaccia",
    ingredients: "Bread with italian olive oil and rosemary",
    price: 6,
    photoName: "pizzas/focaccia.jpg",
    soldOut: false,
  },
  {
    name: "Pizza Margherita",
    ingredients: "Tomato and mozarella",
    price: 10,
    photoName: "pizzas/margherita.jpg",
    soldOut: false,
  },
  {
    name: "Pizza Spinaci",
    ingredients: "Tomato, mozarella, spinach, and ricotta cheese",
    price: 12,
    photoName: "pizzas/spinaci.jpg",
    soldOut: false,
  },
  {
    name: "Pizza Funghi",
    ingredients: "Tomato, mozarella, mushrooms, and onion",
    price: 12,
    photoName: "pizzas/funghi.jpg",
    soldOut: false,
  },
  {
    name: "Pizza Salamino",
    ingredients: "Tomato, mozarella, and pepperoni",
    price: 15,
    photoName: "pizzas/salamino.jpg",
    soldOut: true,
  },
  {
    name: "Pizza Prosciutto",
    ingredients: "Tomato, mozarella, ham, aragula, and burrata cheese",
    price: 18,
    photoName: "pizzas/prosciutto.jpg",
    soldOut: false,
  },
];

function App() {
  return (
    <div className="container">
      <Header />
      <Menu />
      <Footer />
    </div>
  );
}

function Header() {
  return (
    <header className="header">
      <h1>React Pizza App</h1>
    </header>
  );
}

function Menu() {
  return (
    <main className="menu">
      <h2>Our Menu</h2>
      <ul className="pizzas">
        {pizzaData.map((pizza) => (
          <Pizza pizzaObj={pizza} key={pizza.name} />
        ))}
      </ul>
    </main>
  );
}

function Pizza(props) {
  return (
    <li className="pizza">
      <img src={props.pizzaObj.photoName} alt={props.pizzaObj.name} />
      <div>
        <h3>{props.pizzaObj.name}</h3>
        <p>{props.pizzaObj.ingredient}</p>
        <span>{props.pizzaObj.price}</span>
      </div>
    </li>
  );
}

function Footer() {
  const hour = new Date().getHours();
  const openhour = 12;
  const closeHour = 22;
  const isOpen = hour >= openhour && hour <= closeHour;
  // else alert("Sorry we are closed");
  return (
    <footer className="footer">
      We are currently open at {new Date().toLocaleTimeString()}
    </footer>
  );
}

//react from version 18 onwards
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

//react version before 18
//React.render(<App />, document.getElementById("root"))

 
Components include data, logic and appearance. Data consists of Props and state. Where state is internal data that can be updated by the components logic and props is data coming from outside and can only be updated by parent component. Props are read only, they are immutable. To mutate props we need state. State is used for data change. Props are immutable because if props changes then parent component also changes. Components have to be pure function in terms of state and props (should not change the outside or parent component). This Allows React to optimize apps, avoid bugs. 
React uses one way data flow ( which means data can flow only from parent to child component by using props but not the opposite way). React is one way data flow because it makes application more predictable and easier to understand for developers, makes application to debug easily, two way data flow is less efficient.
 
