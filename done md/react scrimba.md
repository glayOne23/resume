Parts :
1. Structure and Component Concept done
2. Styling in CSS done
3. Props  done
4. Mapping Components 
5. Class based Components
    - 5.1. State
    - 5.2. Lifecyle
    - 5.3. Conditional Render
6. Form
7. React Container & Component Architecture
8. Hook
---
# Part 1 : Structure and Component Concept

## 3 Why use react :
1. Virutal DOM
2. Reusable component
3. Hirable
4. Maintained by Facebook

## 4 ReactDOM and JSX
1. in index.html
    ``` html
    <html>
        <head>
            <link rel="stylesheet" href="style.css">
        </head>
        <body>
            <!-- take a look at this one, id="root", this will be filled with react component from index.js -->
            <div id="root"></div>
            <script src="index.pack.js"></script>
        </body>
    </html>
    ```
2. In index.js
    ```html
    <!-- import react for JSX -->
    import React from "react"
    <!-- import react DOM for manipulating DOM -->
    import ReactDOM from "react-dom"


    <!-- ReactDOM.render(WHAT DO I WANT TO RENDER, WHERE DO I WANT TO RENDER IT) -->
    ReactDOM.render(
        <div>
            <h1>Hello world!</h1>
            <p>This is a paragraph</p>
        </div>, 
        document.getElementById("root")
    )
    <!-- u must wrap rendered stuff into 1 (using <div> or <ReactFragment>), so it  will be 1 element with 2 element inside -->
    ```

## 6. React Functional Component
``` html
import React from "react"
import ReactDOM from "react-dom"

//this is functional component, u can use it repeatedly
function MyInfo() {
  return 
  (
    <!-- <React.Fragment> can be used to wrap elements into 1 instead of using <div>  -->
    <React.Fragment>  
        <h1>Muhammad Hammam Islami</h1>
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
        </ul>
    </React.Fragment>  
  )
}

ReactDOM.render(
  <MyInfo />,
  document.getElementById("root")
)
```

## 8. Move Component into Separate Files
- we will move MyInfo() into components/MyInfo.js, and the import it into index.js
- we put all components inside components folder to easily organize it
1. In MyInfo.js
    ```html
    <!-- u must import it to use jsx -->
    import React from "react"

    function MyInfo() {
        return (
            <!-- <React.Fragment> can be used to wrap elements into 1 instead of using <div>  -->
            <React.Fragment>  
                <h1>Muhammad Hammam Islami</h1>
                <ul>
                    <li>1</li>
                    <li>2</li>
                    <li>3</li>
                </ul>
            </React.Fragment>  
        )
    }

    <!-- to export Mynfo.js u must add this line -->
    export default MyInfo
    ```
2. In index.js
    ``` html
    import React from "react"
    import ReactDOM from "react-dom"

    import MyInfo from "./components/MyInfo"

    ReactDOM.render(<MyInfo />, document.getElementById("root"))
    ```

## 10 React Parent/Child Components Practice
- The idea is making React website which contain of 3 elements; Header, Main, and Footer
- we need :
    1. index.html 
    2. index.js -> to render App.js
    3. App.js -> to include all components
    4. Header.js
    5. MainContent.js
    6. Footer.js
1. in index.html
    ``` html
    <html>
        <head>
        </head>
        <body>
            <div id="root"></div>
            <script src="index.pack.js"></script>
        </body>
    </html>
    ``` 
2. in index.js
    ``` html
    import React from "react"
    import ReactDOM from "react-dom"

    import App from "./App"

    ReactDOM.render(<App />, document.getElementById("root"))
    ```
3. in App.js
    ``` html
    import React from "react"

    import Header from "./components/Header"
    import MainContent from "./components/MainContent"
    import Footer from "./components/Footer"

    function App() {
        return (
            <div>
                <Header />
                <MainContent />
                <Footer />
            </div>
        )
    }

    export default App
    ```
4. in components/Header.js
    ``` html
    import React from "react"

    function Header() {
        return (
            <header>This is the header</header>
        )
    }

    export default Header
    ```
5. in components/Main.js
    ``` html
    import React from "react"

    function MainContent() {
        return (
            <main>This is the main section</main>
        )
    }

    export default MainContent
    ```
6. in components/Footer.js
    ``` html
    import React from "react"

    function Footer() {
        return (
            <footer>This is the footer</footer>
        )
    }

    export default Footer
    ```
- result :<br />
    ![](img_react_scrimba/components.png?raw=true)
---

# Part 2 Styling React with CSS 

## 12 Styling React with CSS Classes
- use `className` instead of `class` to use css classes in jsx,
- Why ? because class is reserved keyword in javascript and jsx uses javascript dom api underneath,
- So actually you must do like this :
    `document.getElementById("something").className += " new-class-name"`
1. in Header.js
    ```html
    <header className="navbar">This is the header</header>
    ```
2. in styles.css
    ```css
    body {
        margin: 0;
    }

    .navbar {
        height: 100px;
        background-color: #333;
        color: whitesmoke;
        margin-bottom: 15px;
        text-align: center;
        font-size: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    ```
- result :<br />
     ![](img_react_scrimba/className.png?raw=true)


## 14 JSX to Javascript and Back
- How JSX and Javascript play together? JSX line start with `<` tag, once in jsx line, everything will be treated as regular html, however as soon as we want go back into javascript, we put curly bracket.
- example :
    ``` js
    import React from "react"
    import ReactDOM from "react-dom"

    function App() {
    const firstName = "Bob"
    const lastName = "Ziroll"
    
    return (
        <h1>Hello {firstName + " " + lastName}!</h1>
    )
    }

    ReactDOM.render(<App />, document.getElementById("root"))
    ```
- same example with es6 syntax :
    ``` js
    import React from "react"
    import ReactDOM from "react-dom"
    import App from "./App"

    function App() {
    const firstName = "Bob"
    const lastName = "Ziroll"
    
    return (
        <h1>Hello {`${firstName} ${lastName}`}!</h1>
    )
    }

    ReactDOM.render(<App />, document.getElementById("root"))
    ```
## 15 Styling React with Inline Styles
1. `style` in jsx has a different way to call it, u must change the value into javascript using {} and the u must make it as object using {} inside {}, so it will be double curly brackets {{}}
- example :
    ```html
    <!-- instead of this -->
    <!-- <h1 style="color:blue;" > My name is John Blue </h1> -->
    <!-- use this -->
    <h1 style= {{ color:"blue" }} > My name is John Blue </h1>
    ```
2. for `-` in inline property, u must use camelCase , example:
    ``` html
    <!-- instead of this -->
    <!-- <h1 style="color:blue;background-color:red;" > My name is John Blue </h1> -->
    <h1 style= {{ color:"blue", backgroundColor:"red" }} > My name is John Blue </h1>
    ```
3. and u can put inline styling into variable
    ``` js
    function App() {
   
        const coloringName = {
            color: "#FF8C00", 
            backgroundColor: "#FF2D00"
        }
        
        return (
            <h1 style={coloringName}>My name is John Blue</h1>
        )
    }
    ```
---
# Part 3 Props

## 17 Props Concept 
- props is like property in html, u can fill it and pass it into React Components
- example :
1. in App.js
    ```html
    import React from "react"
    import Bio from "./Bio"

    const App = () => {
        return(
            <React.Fragment>
                <!-- Jenis2nya : -->
                <!-- props are included in javascript objects  -->
                <Bio biodata= {{name:"hammam", lastname:"islami"}} />
                <!-- 2 props -->
                <Bio name="andi" lastname="aja" />
                <!-- 1 props -->
                <Bio name="hai" />
                <!-- no props -->
                <Bio />
            </React.Fragment>
        )
    }

    export default App
    ```
2. in Bio.js
    ```js
    import React from "react"

    const Bio = (props) => {
        console.log(props)
        )
    }

    export default Bio
    ```
3. Result of console.log
    ```js
    >{biodata: {name: "hammam", lastname: "islami"}}
    >{name: "andi", lastname: "aja"}
    >{name: "hai"}
    >{}
    ```
4. How to call props ?
1. in Bio.js
    - for props are included in javascript objects :
        ```html
        import React from "react"

        const Bio = (props) => {
            return(
                <React.Fragment>
                    <!-- look at this line -->
                    <h1>Hai {props.biodata.name} your lastname is {props.biodata.lastname} </h1>
                </React.Fragment>
            )
        }

        export default Bio
        ```
    - for 2 props :
        ```html
        <h1>Hai {props.name} your lastname is {props.lastname} </h1>
        ```
## 20 Doesnt Display Props if Props was Empty Using CSS
1. in App.js
    ```html
    <Bio biodata= {{name:"hammam", lastname:"islami"}} />
    <Bio biodata= {{name:"hammam"}} />
    ```
2. in Bio.js
    ```html
    const Bio = (props) => {
        return(
            <React.Fragment>
                <h1>Hai {props.biodata.name}</h1> 
                <!-- look at this line -->
                <h2 style={{display: props.biodata.lastname ? "block" : "none"}}>your lastname is {props.biodata.lastname} </h2>
                <!-- it will make line above doesnt appear if lastname didnt exist -->
            </React.Fragment>
        )
    }
    ```

# Part 4 Mapping Component
## 21 Mapping Components in React
- In real world, most the data you will be displaying on a page, will actually becoming some kind of http call to an api. Where database serve will return json to you.
- So, how we map this ? in this example we use products in vschoolProducts.js to be displayed
1. in vschoolProducts.js
    ```js
    const products = [
        {
            id: "1",
            name: "Pencil",
            price: 1,
            description: "Perfect for those who can't remember things! 5/5 Highly recommend."
        },
        {
            id: "2",
            name: "Housing",
            price: 0,
            description: "Housing provided for out-of-state students or those who can't commute"
        }
    ]

    export default products
    ``` 
2. in Product.js make a template for `<Product />` which will be displayed in App.js
    ```html
    import React from "react"

    function Product(props) {
        return (
            <React.Fragment>
                <h2>{props.name}</h2>
                <h3>{props.price}</h3>
                <h3>{props.description}</h3>
                ================================
            </React.Fragment>
        )
    }

    export default Product
    ```
3. in App.js
    ```html
    import React from "react"
    import Product from "./Product"
    import productsData from "./vschoolProducts"

    <!-- map is kinda like foreach in php, it will map array productsData from vschoolProducts.js-->
    <!-- u must add key for every item of array -->
    const Data = productsData.map(product => {
        return (
            <Product 
            key={product.id} 
            name={product.name}
            price={product.price}
            description={product.description}
            />
        )
        
    })  
    function App() {
        
    return (
        <div>
            {Data}    
        </div>
    )
    }

    export default App
    ```
4. Cara lebih singkat untuk ngoper `product` :
    1. di App.js di bagian mapping :
    ```html
    const Data = productsData.map(product => {
        return (
            <Product 
            key={product.id} 
            product={product}
            />
            <!-- look at line above, all included inside product property -->
        )     
    })  
    ```
    2. in Product.js
    ```html
    function Product(props) {
        return (
            <React.Fragment>
                <h2>{props.product.name}</h2>
                <h3>{props.product.price}</h3>
                <h3>{props.product.description}</h3>
                ================================
            </React.Fragment>
            <!-- look at line above -->
        )
    }
    ```

- references for learning :
``` js
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find
// https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findindex
```
---
# Part 5 Class-based Components

## 24 Class-based Components
- Different between function components: 
    1. Class-based Components can has state and lifecycle method
    2. syntax:jsx must be in render() method, props automatically recognized
- example in index.js : using function:
    ```html
    import React from "react"
    import ReactDOM from "react-dom"

    // #1
    function App() {
        return (
            <div>
                <Header username="Hammam" />
            </div>
        )
    }

    // #2
    function Header(props) {
        return (
            <header>
                <p>Welcome, {props.username}!</p>
            </header>
        )
    }

    ReactDOM.render(<App />, document.getElementById("root"))
    ```
- using class :
    ```html
    import React from "react"
    import ReactDOM from "react-dom"

    // #1
    <!-- class must extends React.Components -->
    class App extends React.Component {
        <!-- has render method -->
        render(){
            return (
                <div>
                    <Header username="Hammam" />
                </div>
            )
        }
    }

    // #2
    class Header extends React.Component {
        render(){
            return (
                <header>
                    <!-- props automatically recognized, add this. before props-->
                    <p>Welcome, {this.props.username}!</p>
                </header>
            )
        }
    }

    ReactDOM.render(<App />, document.getElementById("root"))
    ```
- simplifying syntax : u can just call `extends Components`, just add {Component} in import React, `import React, {Component} from "react"`

---
# Part 5.1 Class-based Components (State)

## 26 React State
- state : the data that the component maintain, which means can change its value
- when the state is updated, the component will be rerendered
- example:
    ```html
    <!-- dont g=forget to import Component for class -->
    import React,{Component} from "react"

    class App extends Component {
        <!-- put super() and state inside constructor -->
        constructor(){
            super()
            <!-- use this.state syntax, state must be an object -->
            this.state={
                name : "Hammam",
                age : 24            
            }
        }
        
        render(){    
            return (
                <div>
                    <!-- how to simply use state -->
                    <h1>{this.state.name}</h1>
                    <h3>{this.state.age} years old</h3>
                </div>
            )    
        }
    }

    export default App
    ```
- result : <br/>
    ![](img_react_scrimba/state1.png?raw=true)
## 27 State Practice 2 (Simple Conditional State (hardcoding the state) )
``` html
import React, {Component} from "react"

class App extends Component {
    constructor(){
        super()
        this.state={
            isLoggedIn : true
        }
    }
    
    render(){    
        return (
            <div>
                <!-- look at this line in here we use ternary operator-->
                <h1>You are currently logged {this.state.isLoggedIn == true ? 'in' : 'out' } </h1>
            </div>
        )
    }
}

export default App
```
- result : <br/>
    ![](img_react_scrimba/state2.png?raw=true)

## 27.1 State Practice 3
- (connenction from product) In case u want to put data from 'vschoolProducts.js' into state, u can do by this way:
    ```html
    import React,{Component} from "react"
    import Product from "./Product"
    import productsData from "./vschoolProducts"

    class App extends Component { 
        
        constructor()
        {
            super()
            <!-- look at this line, u put productsData from vschoolProduct.js into state with name products -->
            this.state = {
                products:productsData
            }
        }
        
        render()
        { 
            <!-- calling products state -->
            const Data = this.state.products.map(product => 
            {
                return (
                    <Product 
                    key={product.id} 
                    product={product}
                    />
                )   
            })    
            return (
                <div>
                    {Data}    
                </div>
            )
        }  
    }

    export default App
    ```

## 30 Handling Events
- in react, handling event syntax is camelCase, example: `onclick` become `onClick`
- example :
    ``` js
    import React from "react"

    function handleClick() {
        console.log("I was clicked")
    }

    // https://reactjs.org/docs/events.html#supported-events

    function App() {
        return (
            <div>
                <img src="https://www.fillmurray.com/200/100"/>
                <br />
                <br />
                <!-- u dont need to add () in this handleClick function -->
                <button onClick={handleClick}>Click me</button>
            </div>
        )
    }

    export default App
    ```
## 32 React setState : Changing the State
```html
import React from "react"

class App extends React.Component {
    constructor() {
        super()
        this.state = {
            count: 0
        }
        <!-- u must bind the function if u use setState() inside it -->
        this.handleClick = this.handleClick.bind(this)
    }
    
    handleClick() {
        <!-- 1. u cant change the state directly,instead u must use setState() method from Component class -->
        <!-- 2. to see previous state value, u must call a function and pass variable there,this variable will include previous state value, in this example look at prevSate -->
        <!-- 3. look at prevState, it is just variable name to get previous state value in object form, u can change the name -->
        <!-- 4. if u console.log(prevState), it will display {count:0} -->
        this.setState((prevState) => {
            return {
                count: prevState.count + 1
            }
        })
    }
    
    render() {
        return (
            <div>
                <h1>{this.state.count}</h1>
                <!-- add this. for function in class -->
                <button onClick={this.handleClick}>Change!</button>
            </div>
        )
    }
}

export default App
```
## 33. React To Do App Phase 6.
- `component` itu ibarat mock up, km cuma mengkode apa2 yang boleh ditaruh (coding template), semua isi fungsi, apa yang akan dilakukan, itu ada di `App.js`
- 
- contoh 1: disini dijelaskan cara ngoper fungsi dari `App.js` (cukup oper nama),isi parameter akan di jelaskan di component(`TodoItem.js`):
1. di App.js
    ```html
    import React from "react"
    import TodoItem from "./TodoItem"
    import todosData from "./todosData"

    class App extends React.Component {
        constructor() {
            super()
            this.state = {
                todos: todosData
            }
            this.handleChange = this.handleChange.bind(this)
        }
        
        handleChange(id) {
            console.log(id)
        }
        
        render() {
            const todoItems = this.state.todos.map(
                item => 
                <TodoItem 
                key={item.id} 
                item={item}
                handleChange={this.handleChange}
                />)
                <!-- look at line above, disini fungsi dioper ke component -->
            
            return (
                <div className="todo-list">
                    {todoItems}
                </div>
            )    
        }
    }

    export default App
    ```
2. di TodoItem.js
    ```html
    import React from "react"

    function TodoItem(props) {
        return (
            <div className="todo-item">
                <input 
                    type="checkbox" 
                    checked={props.item.completed} 
                    onChange={() => props.handleChange(props.item.id)} 
                    <!-- look at this line, disini id diisi -->
                />
                <p>{props.item.text}</p>
            </div>
        )
    }

    export default TodoItem
    ```
    - hasil ketika checkbox di klik, di console akan muncul idnya

- contoh 2: value `completed` akan berubah jika di klik
1. di App.js
    ```html
    import React from "react"
    import TodoItem from "./TodoItem"
    import todosData from "./todosData"

    class App extends React.Component {
        constructor() {
            super()
            this.state = {
                todos: todosData
            }
            this.handleChange = this.handleChange.bind(this)
        }
        
        handleChange(id) {
            this.setState(prevState =>{
                <!-- updatedTodos itu variable tempat mapping sama naro yang berubah -->
                const updatedTodos = prevState.todos.map(
                    item => {
                        if(item.id == id){
                            item.completed = !item.completed
                        }
                        <!-- nyimpen item yang baru ke variable updatedTodos -->
                        return item
                    })
                <!-- ngubah state sesuai dengan isi updatedTodos     -->
                return{
                    todos:updatedTodos
                }
            })
        }
        
        render() {
            const todoItems = this.state.todos.map(
                item => <TodoItem key={item.id} item={item} handleChange={this.handleChange} />)
            

            return (
                <div className="todo-list">
                    {todoItems}
                </div>
            )    
        }
    }

    export default App
    ```
2. di TodoItem.js --> masih sama kaya contoh 1

---
# Part 5.2 Lifecycle Methods

## 34 and 35. Lifecycle Methods Part 1 and Part 2
- only class that have lifecycle,  but now there is hook
- Lifecycle method : method yang akan dijalankan ketika suatu component melalui fase tertentu dalam jalur hidupnya
- Every changes in react will trigger render
- fase2nya bisa dilihat di pdf sem 4 kemaren
- Beberapa methods :
    1. componentDidMount() 
        - very first time component shows up, react will run componentDidMount method,
        - this method will only run once while the component showing up on the screen
        - yang berarti re-render component yang merubah tampilan tidak akan re-run componentDidMount
        - thus because the method doesnt actually unmount and remount
        - exp : for API call 
    2. ShouldComponentUpdate()
        - making a decision wether this need to change or not
    3. componentWIllUnmount()    
- Deprecated methods:
    1. componentWillReceiveProps()
    2. componentWillMount()
    3. componentWillUpdate()
- new methods:
    1. getDerivedStatefromProps()
    2. getSnapshotBeforeUpdate()
---

# Part 5.3 Conditional Rendering

## 36 React Conditional Render
- react will show up component based on conditional render
- example 1 (notification will appear if messange available) :
    ```html
    import React, {Component} from "react"
    import Conditional from "./Conditional"

    class App extends Component {

        constructor() {
            super()
            this.state = {
                unreadMessages: [
                    "Call your mom!",
                    "New spam email available. All links are definitely safe to click."
                ]
            }
        }

        render() {
            return (
                <div>
                    <!-- example 1 using ternary operator -->
                    {this.state.unreadMessages.length > 0 ? <h2>You have {this.state.unreadMessages.length} unread messages!</h2> : null }
                    <!-- example 2 using boolean -->
                    {this.state.unreadMessages.length > 0 && <h2>You have {this.state.unreadMessages.length} unread messages!</h2>}
                </div>
            )
        }
    }

    export default App
    ```

## 38 React Conditional Render Practice
```js
import React from "react"

class App extends React.Component {
    constructor() {
        super()
        this.state = {
            isLoggedIn: false
        }
        this.handleClick = this.handleClick.bind(this)
    }
    
    handleClick() {
        this.setState(prevState => {
            return {
                isLoggedIn: !prevState.isLoggedIn
            }
        })
    }
    
    render() {   
        //bisa taro di variable dulu biar nggk numpuk dalam satu baris
        let buttonText = this.state.isLoggedIn ? "LOG OUT" : "LOG IN"
        let displayText = this.state.isLoggedIn ? "Logged in" : "Logged out"
        return (
            <div>
                <h1>{displayText}</h1>
                <button onClick={this.handleClick}>{buttonText}</button>
            </div>
        )
    }
}

export default App
```
- result : <br/>
    ![](img_react_scrimba/conditionalRender1.png?raw=true)

## 39 Todo App Phase 7 (Final)
1. in App.js
    ```js
    import React from "react"
    import TodoItem from "./TodoItem"
    import todosData from "./todosData"

    class App extends React.Component {
        constructor() {
            super()
            this.state = {
                todos: todosData
            }
            this.handleChange = this.handleChange.bind(this)
        }
        
        handleChange(id) {
            this.setState(prevState => {
                const updatedTodos = prevState.todos.map(todo => {
                    if (todo.id === id) {
                        todo.completed = !todo.completed
                    }
                    return todo
                })
                return {
                    todos: updatedTodos
                }
            })
        }
        
        render() {
            const todoItems = this.state.todos.map(item => <TodoItem key={item.id} item={item} handleChange={this.handleChange}/>)
            
            return (
                <div className="todo-list">
                    {todoItems}
                </div>
            )    
        }
    }

    export default App
    ```
2. in TodoItem.js
    ```js
    import React from "react"

    function TodoItem(props) {

        const completedStyle = {
            fontStyle: "italic",
            color: "#cdcdcd",
            textDecoration: "line-through"
        }

        return (
            <div className="todo-item">
                <input 
                    type="checkbox" 
                    checked={props.item.completed} 
                    onChange={() => props.handleChange(props.item.id)}
                />
                <p style={props.item.completed == true ? completedStyle : null} >{props.item.text}</p>
            </div>
        )
    }

    export default TodoItem
    ```
- result : <br/>
    ![](img_react_scrimba/todoFinal.png?raw=true)
---
## 40. Fetching Data from an API with React
```js
import React, {Component} from "react"

class App extends Component {
    constructor() {
        super()
        this.state = {
            loading: false,
            character: {}
        }
    }
    
    componentDidMount() {
        this.setState({loading: true})
        //feth API
        fetch("https://swapi.co/api/people/1")
            //change response to json format
            .then(response => response.json())
            //getting data and put into state
            .then(data => {
                this.setState({
                    loading: false,
                    character: data
                })
            })
    }
    
    render() {
        const text = this.state.loading ? "loading..." : this.state.character.name
        return (
            <div>
                <p>{text}</p>
            </div>
        )
    }
}

export default App
```

---
# 6. Form
## 41. React Forms Part 1
- In general submit form, all data is gathered together pretty much at the last second, but best practice in react form is constantly keep track of all the information in state, which means on every key stroke, state is updated, so that can always have the most updated version of what user typing into the form.
- principle :
    1. onChange -> for updating every change state
    2. event.target -> for getting the value
    3. [event.target.name] -> for getting the name (u must wrap it inside square bracket [] )
    4. value -> for single source of truth
```html
import React, {Component} from "react"

class App extends Component {
    constructor() {
        super()
        this.state = {
            firstName: "",
            lastName: ""
        }
        this.handleChange = this.handleChange.bind(this)
    }
    
    handleChange(event) {
        this.setState({
            <!-- getting isi and name of input -->
            [event.target.name]: event.target.value
        })
    }
    
    render() {
        return (
            <form>
                <!-- principle : value, name, onChange -->
                <input 
                    type="text" 
                    value={this.state.firstName} 
                    name="firstName" 
                    placeholder="First Name" 
                    onChange={this.handleChange} 
                />
                <br/>
                <h1>{this.state.firstName}</h1>
            </form>
        )
    }
}

export default App
```
- result : <br/>
    ![](img_react_scrimba/form1.png?raw=true)

## 41. Form in React Part 2
- libaray : formik
- Principle : 
    1. `textarea` ->  will use value like text
    2. `checkbox` -> will use checked (checked/unchecked where button is clicked) 
    3. `radio button` -> radio is combination of input textbox and checkbox, which means they will use both value and checked (checked/unchecked based on value)
    4. `select` -> will use value, value is gotten from `<option>`
```js
import React, {Component} from "react"

class App extends Component {
    constructor() {
        super()
        this.state = {
            firstName: "",
            lastName: "",
            isFriendly: false,
            gender: "",
            favColor: "blue"
        }
        this.handleChange = this.handleChange.bind(this)
    }
    
    handleChange(event) {
        const {name, value, type, checked} = event.target
        type === "checkbox" ? this.setState({ [name]: checked }) : this.setState({ [name]: value })
    }
    
    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <input 
                    type="text" 
                    value={this.state.firstName} 
                    name="firstName" 
                    placeholder="First Name" 
                    onChange={this.handleChange} 
                />
                <br />
                <input 
                    type="text" 
                    value={this.state.lastName} 
                    name="lastName" 
                    placeholder="Last Name" 
                    onChange={this.handleChange} 
                />
                
                <textarea 
                    value={"Some default value"}
                    name="textArea"
                    onChange={this.handleChange}
                />
                
                <br />
                
                <label>
                    <input 
                        type="checkbox" 
                        name="isFriendly"
                        checked={this.state.isFriendly}
                        onChange={this.handleChange}
                    /> Is friendly?
                </label>
                <br />
                <label>
                    <input 
                        type="radio" 
                        name="gender"
                        value="male"
                        checked={this.state.gender === "male"}
                        onChange={this.handleChange}
                    /> Male
                </label>
                <br />
                <label>
                    <input 
                        type="radio" 
                        name="gender"
                        value="female"
                        checked={this.state.gender === "female"}
                        onChange={this.handleChange}
                    /> Female
                </label>
                {/* Formik */}
                <br />
                
                <label>Favorite Color:</label>
                <select 
                    value={this.state.favColor}
                    onChange={this.handleChange}
                    name="favColor"
                >
                    <option value="blue">Blue</option>
                    <option value="green">Green</option>
                    <option value="red">Red</option>
                    <option value="orange">Orange</option>
                    <option value="yellow">Yellow</option>
                </select>
                
                <h1>{this.state.firstName} {this.state.lastName}</h1>
                <h2>You are a {this.state.gender}</h2>
                <h2>Your favorite color is {this.state.favColor}</h2>
                <button>Submit</button>
            </form>
        )
    }
}

export default App

```
## 44. React Meme Generator
- disini manggil api nya response.data.memes soalnya ada return success nya dan data yg diperluin itu di data.memes 
- passed

---
# 7. React Container & Component Architecture
- passed

---
# React Hook

## 51 Hook Intro
- Uses:
    1. "Hook into" state or lifecycle methods of components without using classes
    2. Only use functional components across the board
    3. what we learn:
        - useState()
        - useEffect()

## 52 and 53. UseState()
- different between using class and hook :
### 1. Using Class:
```js
import React from "react"

class App extends React.Component {
    //look at this
    constructor() {
        super()
        this.state = {
            answer: "Yes"
        }
    }
    
    render() {
        return (
            <div>
                <h1>Is state important to know? {this.state.answer}</h1>
            </div>
        )
    }
}

export default App
```
### 2. Using Hook :
```js
import React, {useState} from "react"

function App() {
    // ini state-state nya
    const [count, setCount] = useState(0)
    const [answer, setAnswer] = useState("Yes")
    
    // buat ngubah fungsinya, setternya ditaruh di fungsi biar gampang, trs dalam setter bikin fungsi lagi buat ngatur perubahan statenya
    // kenapa harus ada prevCount kenapa nggk langsung count? karena react itu asyncrous dan lebih aman ada prevStatenya biar tw yg diupdate itu state yang sekarang dipake
    // ingat event listener tu ada eventnya, jadi jika pake inline function harus ada () => pada onClick()
    function increment() {
        setCount(prevCount => prevCount + 1)
    }
    
    function decrement() {
        setCount(prevCount => prevCount - 1)
    }
    
    return (
        <div>
            <h1>{count}</h1>
            <button onClick={increment}>Increment</button>
            <button onClick={decrement}>Decrement</button>
            <h3>{answer}</h3>
        </div>
    )
}

export default App
```
- Search about array destructuring

## 54. useEffect() P.1
- Easiest way, useEffect is considered as a replacement for 3 lifecycle method:
    1. componentDidMount
    2. componentDidUpdate
    3. componentWillUnmount
- useEffect is a hook for encapsulating code that has ‘side effects,’ and is like a combination of componentDidMount, componentDidUpdate, and componentWillUnmount. 
- What is side effect?
    - A "side effect" is anything that affects something outside the scope of the function being executed. These can be, say, a network request, which has your code communicating with a third party (and thus making the request, causing logs to be recorded, caches to be saved or updated, all sorts of effects that are outside the function.
    - There are more subtle side effects, too. Changing the value of a closure-scoped variable is a side effect. Pushing a new item onto an array that was passed in as an argument is a side effect. Functions that execute without side effects are called "pure" functions: they take in arguments, and they return values. Nothing else happens upon executing the function. This makes the easy to test, simple to reason about, and functions that meet this description have all sorts of useful properties when it comes to optimization or refactoring.
1. example 1 componentDidUpdate:
    ```js
    import React, {useState, useEffect} from "react"
    import randomcolor from "randomcolor"

    function App() {
        const [count, setCount] = useState(0)
        const [color, setColor] = useState("")
        
        function increment() {
            setCount(prevCount => prevCount + 1)
        }
        
        function decrement() {
            setCount(prevCount => prevCount - 1)
        }
        
        // it will make infinite loop, it kinda like componentDidUpdate,what useEffect is doing, every single time are functions returned on another word every time this component renders, is calling useEffect() and useEffect() is setting the state which in turn causing the component to rerender
        useEffect(() => {
            setColor(randomcolor())
        })
        
        return (
            <div>
                <h1 style={{color: color}}>{count}</h1>
                <button onClick={increment}>Increment</button>
                <button onClick={decrement}>Decrement</button>
            </div>
        )
    }

    export default App
    ```
2. example 2.1 componentDidMount:
    ```js
    import React, {useState, useEffect} from "react"
    import randomcolor from "randomcolor"

    function App() {
        const [count, setCount] = useState(0)
        const [color, setColor] = useState("")
        
        function increment() {
            setCount(prevCount => prevCount + 1)
        }
        
        function decrement() {
            setCount(prevCount => prevCount - 1)
        }
        
        //this make randomcolor() only is triggered once when program run
        useEffect(() => {
            setColor(randomcolor())
        }, [])
        
        return (
            <div>
                <h1 style={{color: color}}>{count}</h1>
                <button onClick={increment}>Increment</button>
                <button onClick={decrement}>Decrement</button>
            </div>
        )
    }

    export default App
    ```
3. example 2.2 componentDidMount :
    ```js
    import React, {useState, useEffect} from "react"
    import randomcolor from "randomcolor"

    function App() {
        const [count, setCount] = useState(0)
        const [color, setColor] = useState("")
        
        function increment() {
            setCount(prevCount => prevCount + 1)
        }
        
        function decrement() {
            setCount(prevCount => prevCount - 1)
        }
        
        // this make randomcolor() is triggered every time count change
        useEffect(() => {
            setColor(randomcolor())
        }, [count])
        
        return (
            <div>
                <h1 style={{color: color}}>{count}</h1>
                <button onClick={increment}>Increment</button>
                <button onClick={decrement}>Decrement</button>
            </div>
        )
    }

    export default App
    ```
## 55. useEffect() P.2
- trivia use of componentWillUnmount : destroying any side effects set up in componentDidMount
- example of use componentDidMount : when using document.addEventListener(...) or subscription
4. example 3 componentWillUnmount :
    ```js
    import React, {useState, useEffect} from "react"
    import randomcolor from "randomcolor"

    function App() {
        const [count, setCount] = useState(0)
        const [color, setColor] = useState("")
        
        useEffect(() => {
            const intervalId = setInterval(() => {
                setCount(prevCount => prevCount + 1)
            }, 1000)
            // use return with function to make clean up function
            return () => clearInterval(intervalId)
            // its needed to add [] because kan useEffect diatas ke trigger ketika count berubah, nah itu bakal bikin setInterval() baru jadi bakal ada 2 setInterval() yang bersamaan dalam 1 waktu
        }, [])
        
        useEffect(() => {
            setColor(randomcolor())
        }, [count])
        
        return (
            <div>
                <h1 style={{color: color}}>{count}</h1>
            </div>
        )
    }

    export default App
    ```
---
# Adds

## 1. useContext()
- install : npx create-react-app state-management
- Steps :
    1. bikin file context buat jadi provider ama buat ngelempar contextnya
        - ada createContext() --> buat ngelempar contextnya
        - ada Provider --> buat isi contextnya
        - ada return buat wrap mana aja component yang mau pake context
    2. di App.js,  wrap component yang pengen dikasih lempar melalui Provider
    3. component nangkep pake useContext()
- All file :
1. MovieContext.js:
    ```js
    import React, {useState, createContext} from 'react';

    // make content here
    export const MovieContext = createContext();

    // content of the movie context
    export const MovieProvider = (props) => {

        const [movies, setMovies] = useState([
            {
                id : 1,
                name : 'HarryPotter',
                price : '$10',
            },
            {
                id : 2,
                name : 'Game of Thrones',
                price : '$12',
            },
            {
                id : 3,
                name : 'The Witcher',
                price : '$9',
            },
        ]);


        return (
            // ngelemparnya harus pake value
            <MovieContext.Provider value = {[movies, setMovies]}>
                {props.children}
            </MovieContext.Provider>
        );
    }
    ```
2. App.js
    ```js
    import React from 'react';
    import './App.css';
    import MovieList from './MovieList'
    import Nav from './Nav'
    import { MovieProvider } from './MovieContext';
    import AddMovie from './AddMovie';

    function App() {
    
    return (
        <div className="App">
        {/* disini component yang pengen dimasukin movie, di rangkup dalam MovieProvider  */}
        <MovieProvider>
            <Nav />
            <AddMovie />
            <MovieList />
        </MovieProvider>
        </div>
    );
    }

    export default App;
    ```
3. MovieList.js :
    ```js
    import React, {useContext} from 'react';
    import Movie from './Movie';
    import { MovieContext } from './MovieContext';

    const MovieList = () => {
        // di tempat component yang pengen dimasukin, panggil context nya
        const [movies, setMovies] = useContext(MovieContext);

        return(
            <div>
                {movies.map(movie =>(
                    <Movie  key={movie.id} movie={movie} />
                ))}
            </div>
        );
    }

    export default MovieList;
    ```
4. Nav.js :
    ```js
    import React, {useContext} from 'react';
    import { MovieContext } from './MovieContext';

    const Nav = () => {
        
        const navbar = {
            height: "70px",
            backgroundColor: "#333",
            color: "whitesmoke",
            marginBottom: "15px",
            textAlign: "center",
            fontSize: "30px",
            display: "flex",
            justifyContent: "center",
            alignItems: "center"
        }

        const [movies, setMovies] = useContext(MovieContext)

        return(
            <React.Fragment>
                <div style={navbar}>
                    <p>List of Movie : {movies.length} </p>
                </div>
            </React.Fragment>
        );
    }

    export default Nav;
    ```
5. AddMovie.js :
    ```js
    import React, {useState, useContext} from 'react';
    import { MovieContext } from './MovieContext';

    const AddMovie = () => {
        
        const [movies, setMovies] = useContext(MovieContext);
        const [name, setName] = useState('');
        const [price, setPrice] = useState('');

        const updateName = (e) => {
            setName(e.target.value);
        }
        const updatePrice = (e) => {
            setPrice(e.target.value);
        }
        const addMovie = (e) => {
            e.preventDefault();
            const key = movies.length + 1 ;
            setMovies( prevMovies => [...prevMovies, {name:name, price:price, id:key} ])
        }


        return(
            <React.Fragment>
                <form onSubmit={addMovie}>
                    <input type="text" name="name" value={name} onChange={updateName} />
                    <input type="text" name="price" value={price} onChange={updatePrice}  />
                    <button>Submit</button>
                </form>
            </React.Fragment>
        );
    }

    export default AddMovie;
    ```
- screenshot :
    
    ![](img_react_scrimba/movies.png?raw=true)


## 2. React Router Doom
- install : npm install react-router-doom
- Steps : 
    1. Make Home and About component, import all component to App
    2. in App : `import {BrowserRouter as Router, Switch, Route} from 'react-router-dom';`
        - `Router` 
            -  to add the ability to handle routing. 
            -  everything between this Router will have the ability to use routing
        - `Route` 
            - renders out the component based on url
            - cara kerja adalah check url,misal url '/' component yg urlnya itu akan dirender, trs lanjut jika urlnya ada lanjutan,misal '/about', component yag mengandung itu akan dirender juga, nah disini butuh Switch
        - `Switch `
            - Stop the whole process of going trough all of these routes as soon as it goes to one and it matches the url, its gonna stop and its gonna only render out that component
            - exact --> bagian dari `Switch` agar ngerender dengan url yang sama plek
    4. di navbar kasih Link
- All File : kecuali Home, ABout, MovieList
1. App.js
    ```js
    import React from 'react';
    import './App.css';
    import MovieList from './MovieList'
    import Nav from './Nav'
    import { MovieProvider } from './MovieContext';
    import Home from './Home';
    import About from './About';
    import {BrowserRouter as Router, Switch, Route} from 'react-router-dom'; 

    function App() {
    
    return (
        //to wrap all that u want to have the ability to route
        <Router>  
        <div className="App">
            <MovieProvider>
            <Nav />
            {/* to stop searching component based on url if one has found */}
            <Switch>
                {/* routing */}
                {/* exact is used to inform react that the url must be exact to get rendered */}
                <Route path="/" exact component={Home} />
                <Route path="/about" component={About} />
                <Route path="/list" component={MovieList} />
            </Switch>
            </MovieProvider>
        </div>
        </Router>
    );
    }

    export default App;
    ```
2. Nav.js
    ```js
    import React, {useContext} from 'react';
    import { MovieContext } from './MovieContext';
    import {Link} from 'react-router-dom';

    const Nav = () => {
        
        const navbar = {
            height: "70px",
            backgroundColor: "#333",
            color: "whitesmoke",
            marginBottom: "15px",
            textAlign: "center",
            fontSize: "30px",
            display: "flex",
            justifyContent: "center",
            alignItems: "center"
        }

        const [movies, setMovies] = useContext(MovieContext)

        return(
            <React.Fragment>
                <div style={navbar}>
                    <Link style={{color:"white", marginRight:"40px"}} to="/">
                        <p> Home</p>
                    </Link>
                    <Link style={{color:"white", marginRight:"40px"}} to="/about">
                        <p>About</p>
                    </Link>
                    <Link style={{color:"white", marginRight:"40px"}} to="/list">
                        <p>List</p>
                    </Link>
                    <p>List of Movie : {movies.length} </p>
                </div>
            </React.Fragment>
        );
    }

    export default Nav;
    ```
    