### React learning notes

---

* install node

* install create-react-app:

  ```
  npm install -g create-react-app
  create-react-app appName
  OR,
  npx create-react-app appName
  ```

* why we use create-react-app: lots of packages are added. (for example, Webpack, Bable, Dev Server...) We don't have to add these useful packages manually.
* src, public, package.json, package-lock.json, README.md, node_modules, .gitignore
* JS module systems: import vs require

#### JSX

* If you want to know how Babel transfer the es6(or higher) and JSX to es5 code, you can try it on <https://babeljs.io/>

* not Html!!

* Browsers don't understand JSX code.

* Why JSX even if it is not required for React? 

  * much easier than writing codes using React.createElement(xx,xx,xx)
  * HTML format is easy to know what is going to display and maintain.

* If there are multiple lines of JSX codes, we need return them wrapped by parenthesis.

* double quotes for JSX properties and single quotes everywhere else

* JSX vs HTML

  1. Custom styling syntax is different. 

     ```react
     <button style={{backgroundColor: 'blue', color: 'white'}}>Submit</button>
     // first pair of {} means I want to reference a javascrip variable inside JSX
     // second pair of {} means It is a Javascript object
     // background-color --> backgroundColor
     ```

  2. adding a class is different.  class --> className (keyword class is occupied)
  3. JSX can reference a javascript variable. (also seen on point1)
     
     * embed it with a set of curly braces
  4. value JSX cannot show? **Objects** (cannot be react child)
  5. React will find the forbidden property names and log the warning in the console window. 

#### props

* <https://semantic-ui.com/>   cdn

* Faker.js to fake some data: faker.image.avatar()

* create reusable components if needed

  * Descriptive name: start with uppercase and use camel convention
  * file name equals to component name
  * make component configurable by React **props** system
  * import & export
  * component nesting: show another component inside one component(parent components and child components)

* Props system

  * passing data from a parent component to a child component
  * Goal: customize or configure a child component
  * Steps: pass down from parent(like an attribute); children consume the props(from props parameter/this.props)
  * we can pass multiple props definitely.
  * we can also pass custom children(props.children) --> wrap the child component in parent

  ```react
  <ApprovalCard>
  	<CommentDetails />
  </ApprovalCard>
  // then in ApprovalCard, we can reach/render the CommentDetails component by calling props.children
  ```

#### class component (vs functional component)

* Before Hooks system, class component can use **lifecycle** methods to execute some codes at specific time and can use **state** system to update the content but functional component cannot.
* After Hooks system, they are similar.
* Geolocation api --> built in api in modern browsers [link to MDN](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API)
* If the result of asynchronous operations will be used to provide contents, then pure functional component might not handle it. So we need class componnet and state system.
* Rules:
  1. must be a js class
  2. must extends React.Component(multiple built in functions)
  3. Must have render method that return some jsx codes

#### state system

* Rules:

  1. only usable in class component(except for functional component with hooks)

  2. state is a js object that contains some data relevant to a component

  3. updating state will cause a component to be (almost) instantly rerendered

  4. must be initialized when a component is created(in constructor or in class scope) --> 2 ways

     ```react
     class App extends React.Component {
       constructor(props) {
         super(props); // must do!! it is to call the parent class's constructor function
         
         this.state = { // initialize the state object
           lat: null
         };
       }
     }
     
     // abbrevitaed syntax
     class App extends React.Component {
       state = { lat : null};
     } // Babel will help to define the constructor and initialize the state as above(they are totally doing the same thing)
     ```

  5. only can be updated by calling `setState`

     Two way to call setState. Object/callback function --> [asynchronous ](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous)

     We are not required to update all the properties in state object.

* conditionally render content

#### Lifecycle Methods

constructor

render

componentDidMount(after render get called)

componentDidUpdate(after render get called)

componentWillUnmount

* Why?
  * constructor --> do one-time setup (also can do data loading but not recommended)
  * render --> only for returning jsx
  * componentDidMount --> do data loading
  * componentDidUpdate --> do more data loading when state/props changes
  * componentWillUnmount --> cleanup

* There are some more lifecycle methods which are not used frequently
  * shouldComponentUpdate
  * getDerivedStateFromProps
  * getSnapshotBeforeUpdate

* javascript ternary:  condition ? option1: option2;

add style files    `import 'path/file.css' `

default props(defaultProps)  `Spinner.defaultProps = {xxx: xxx}`

avoid conditionals in render(hard to maintain or change) --> avoid multiple returns in render, put them into a helper function

#### handle user input with forms and events

onChange(callback)

onClick

onSubmit -> form element

event.target.value

Naming convention of callback function:  on/handle + element name + event name

如果是单行code，也可以inline而不用单独定义一个method

* Uncontrolled vs controlled element
  * event handler -> update state -> state decide the input value
  * Why prefer **Controlled element**? For Uncontrolled element, the only way to get the value of an input is reaching to the DOM node. But for controlled element, we can easily fetch this information from state.
  * Storing the data in the component rather than in DOM node itself. Let React drive and store all the data. 

event.preventDefault()  --> prevent the default actions for HTML element. For example, pressing enter key will trigger the default action for `form` tag, which will refresh the page and submit the form data to server.

**"this"** in javascript:

```react
class SearchBar extends React.Component {
  onFormSubmit(event) {
    event.preventDefault();
    console.log(this.state.xxx); // cannot read the state of undefined
  }
  
  render() {
    return (
      <div>
        <form onSubmit={this.onFormSubmit}>
          xxxxx
        </form>
      </div>
    );
  }
}

// Reason why cannot read state?
// we assign the function onFormSubmit to form's onSubmit. But when the onSubmit is called, we don't know what is "this". At least, we should call onSubmit.bind(this)(). So we need to also assign the "this" context.
```

* what is this?

  * reference to the instance

* how is this determined?

  * don't look at the function itself, should look at where the function is called.

  * ```react
    class Car {
      setDriveSound(sound) {
        this.sound = sound;
      }
      drive() {
        return this.sound;
      }
    }
    
    const car = new Car();
    car.setDriveSound('vroom');
    car.drive(); // in this case, this -> car, so this.sound -> car.sound = 'vroom'
    
    const truck = {
      sound: 'putputput',
      driveMyTruck: car.drive // here we assign the function from car instace but haven't decide what is this
    }
    truck.driveMyTruck(); // driveMyTruck = car.drive ==> this -> truck, so this.sound -> truck.sound
    
    // This is the rule how we determine what this is.
    
    // Consider one case
    const drive = car.drive;
    drive(); // there is no dot here, so this = undefined.
    ```

* how to fix? (multiple approaches) --> every time we want to use this in our function!

  * ```react
    class SearchBar extends React.Component {
      onFormSubmit = event => {
        event.preventDefault();
        console.log(this.state.xxx);
      };
      
      render() {
        return (
          <div>
            <form onSubmit={this.onFormSubmit}>
              xxxxx
            </form>
          </div>
        );
      }
    }
    ```

  * ```react
    class SearchBar extends React.Component {
      constructor() {
        this.onFormSubmit = this.onFormSubmit.bind(this);
      }
      
      onFormSubmit(event) {
        event.preventDefault();
        console.log(this.state.xxx);
      };
      
      render() {
        return (
          <div>
            <form onSubmit={this.onFormSubmit}>
              xxxxx
            </form>
          </div>
        );
      }
    }
    ```

  * ```react
    class SearchBar extends React.Component {
      onFormSubmit(event) {
        event.preventDefault();
        console.log(this.state.xxx);
      };
      
      render() {
        return (
          <div>
            <form onSubmit={e => this.onFormSubmit(e)}>
              xxxxx
            </form>
          </div>
        );
      }
    }
    ```

communication children to parent ==> pass down call back function from parent to children, so children could notify and pass data back to parent.

#### fetch data from outside API or server

Unsplash API

axios(highly recommend) vs fetch

promise.then() vs async + await

```js
function a() {
  axios.get(url, {params: {xx}}).then(response => {xxx})
}

async function b() {
  const response = await axios.get(url, {params: {xx}});
  
  console.log(response);
}

class xxx extends React.Component {
  onSubmit = async xx => {
    xxx
  };
}
```

create custom clients (we can create seperate functions, and we can also do like below:)

```react
import axios from 'axios';

export default axios.create({
  baseURL: 'baseurl',
  headers: {
    xxx: 'xxx'
  }
});
________________________

import api from 'path';

class xxx extends React.Component {
  onSubmit = async term => {
    api.get('/search/photos', {
      params: {
        query: term
      }
    });
  };
}

// axios.create: create a new instance of axios with a custom config
// https://github.com/axios/axios#creating-an-instance
```

#### render a list of records

arrays.map

The purpose of 'key' for list

* performace consideration: help React to render lists more precise, more performant.
* Usually use unique ids
* Only add keys on root element in a list

**Grid system**(css): [grid_guide](https://css-tricks.com/snippets/css/complete-guide-grid/)

Ref system: (instead of using document.querySelector())

* give you the **direct access to a single DOM element** rendered by a component

* We create refs in constructor, assign them to instace variable and pass to a particular JSX element as props

* ```react
  class ImageCard extends React.Component {
    constructor(props) {
      super(props);
      
      this.imageRef = React.createRef();
    }
    
    render() {
      return (
        <img ref={this.imageRef} src={xxx} alt={xxx} />
      )
    }
  }
  ```

* something about **console**

  * extremly funcy to print out some information

  * Sometimes feel confused with the console log, but that is the **order of operations thing.**

  * ```react
    componentDidMount() {
      console.log(this.imageRef); // reference to the img tag
      console.log(this.imageRef.current.clientHeight); // 0 
    }
    ```

  * Why?? Console does not yet know exactly what data is inside of that image or that Dom node of image. Only when you expand the dom node, at this instant time, browser goes to that DOM node and pulls out all of these properties. So when you tried to log `this.imageRef.current.clientHeight` in componetDidMount(img tag is here but need time to download from outside services via src), the image was not loaded yet, then it logged as 0. When you tried to expand the DOM node reference, the image was already loaded, so you can see the clientHeight. 

* css code to render list in lecture: the idea is to allocate spans to each image according to the height

  ```css
  .img-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    grid-gap: 0 10px; // row gap 0, column gap 10px
    grid-auto-rows: 10px; // the height of each allocated rows
  }
  
  .img-list img {
    grid-row-end: span {Math.ceil(clientHeight / 10)} // this is pseudocode to describe the idea, depending on specific images
  }
  ```

#### Practice

#### Hooks

#### Deployment(if using create-react-app)

Vercel(very easy to use), Netlify(closely integrated with github)







#### Redux

* what is redux?

  * state management library(the same state we learned in class componnet)
  * in charge of handling data inside of our application
  * make creating complex app easier
  * not required for react app
  * not explicitly designed for react app

* **Redux cycle**

  * Action creator

    * a function to return a plain javascript object(Action)

  * Action

    * type
      * ALL_UPPERCASE, And delimitted by _ instead of space
    * payload

  * Dispatch

    * copy the action and pass to each reducer
    * a part of Redux library itself

  * **Reducers**

    * take in action and existing data, and process that action and make some changes to data and return it to centralized position

    * a function takes in a slice of old state and action

    * **Important**: every time we need to change the state, we need to return the **brand new array or object**. Avoid modifying existing data structure and return it. (pure function)

    * Some array built in functions could be used, like filter. (will not change the old state but return a new state); object spread

    * No `Object.assign(state, { otherFilters: action.payload.data })`, but we can use `Object.assign({}, state, { otherFilters: action.payload.data })`

    * At the first time the reducer is called, the state is undefined. So we need to set default value for state.

    * ```javascript
      const claimsHistory = (oldList = [], action) => {
        if (action.type === 'xxx') {
          return [...oldList, action.payload];
        }
        
        return oldList;
      }
      ```

    * Reason: 通过源代码，我们发现，`var nextStateForKey = reducer(previousStateForKey, action)`, **nextStateForKey**就是通过 reducer 执行后返回的结果(state)，然后通过`hasChanged = hasChanged || nextStateForKey !== previousStateForKey`来比较新旧两个对象是否一致，***此比较法，比较的是两个对象的存储位置***，也就是浅比较法，所以，当我们 reducer 直接返回旧的 state 对象时，Redux 认为没有任何改变，从而导致页面没有更新。(为什么redux这样设计？？？因为深比较代价较大，非常耗性能，需要比较很多次)

    * What is pure function:

      * 相同的输入永远返回相同的输出
      * 不修改函数的输入值
      * 不依赖外部环境状态
      * 无任何副作用 

  * State

    * central repository of all information created by reducer

* Redux.combineReducers, Redux.createStore

  ```javascript
  const {combineReducer, createStore} = Redux;
  const ourReducers = combineReducer({
    reducer1,
    reducer2,
    reducer3
  }); // put together all reducers(key names will show as state property name in store)
  const store = createStore(ourReducers); // store represent the entire Redux application
  
  store.dispatch(action);
  store.getState();
  ```

* Cannot manully change the store data, can only go through the redux cycle to modify the store. (reduce complexity on larger apps and self-documenting)

#### React with Redux

* React, Redux(not only for React), React-Redux
  * React-Redux is a third library to provide a bunch of helper functions to get Redux to work nicely with React.
    * **Provider**(redux store) --> App --> **Connect**(a special componnet, communicates with Provider) --> SongList

* Named & default exports

  * ```javascript
    export const xxx = xxx;
    const x = x;
    export default x;
    export default expression;
    export default function(x) {xx};
    
    import x, {xxx} from path; 
    ```

* Add action creators

* Build up reducers

  * ```javascript
     import {combineReducers} from 'redux';
      
    const reducer1 = () => {xxx}; 
    const reducer2 = (previousState, action) => {xx};
    
    export default combineReducer({
      songs: reducer1,
      selectedSong: reducer2
    });
    ```

* Write up the Provider

  * also get a reference to the redux store

  * ```react
    import React from 'react';
    import ReactDOM from 'react-dom';
    import {Provider} from 'react-redux';
    import {createStore} from 'redux';
    import reducers from 'path_to_reducers';
    import App from './App'; // top component in our app
    
    ReactDOM.render(
    	<Provider store={createStore(reducers)}>
      	<App />
      </Provider>,
      document.querySelector('#root')
    );
    ```

    In a redux application, we rarely interactive directly with redux store. But there could be some advanced cases.

* Connect function

  * ```react
    import {connect} from 'react-redux';
    
    class List extends React.Component {
      
    }
    
    export default connect()(List); // first argument is configuration
    ```

  * mapStateToProps

    * It is a convention to call it mapStateToProps, could be arrow function or function variable.

    * The first argument is always state, and the return type is always object.

    * ```javascript
      const mapStateToProps = state => {
        return {
          songs: state.songs
        };
      }; 
      export default connect(mapStateToProps)(List);
      // except the songs, we can also see the dispatch function in this.props, which is the same we used to update the store state(this happens only when You do not provide mapDispatchToProps or Your customized mapDispatchToProps function return specifically contains dispatch)
      ```

    * can access to songs in this.props, then use it to render the song lists

  * mapDispatchToProps(two ways)

    * objects

      * ```react
        import {selectSong} from './action_creators';
        
        export default connect(mapStateToProps, {selectSong})(List);
        ```

    * function

      * ```react
        import {selectSong} from './action_creators';
        
        const mapDispatchToProps = dispatch => ({
          selectSong: song => dispatch(selectSong(song))
        });
        
        export default connect(mapStateToProps, mapDispatchToProps)(List);
        ```

  * functional component with connect

    * the same as what we did with class componnet

#### Async actions with redux thunk

* absolutely understand how reducer works

* understand how to make API request with Redux

  * General data loding with Redux

    * component rendered onto the screen
    * componentDidMount get called
    * we call action creator from componentDidMount(if components are generally responsible to fetch data)
    * Action creators run the codes to make an API request
    * API responds with data
    * Action creator returns an action with fetched data as payload (where react_redux comes into play)
    * reducers see the action and update state
    * new state --> component gets rerendered(mapStateToProps)

  * Api part, we can create an axios instance so we don't have to repeat the baseURL. 

    ```javascript
    const api = axios.create({
      baseURL: 'some base url here'
    });
    
    // Then we can use api.get(relative path) to make a GET request
    ```

  * Understanding **async action creators**

    * ```javascript
      const fetchPosts = async () => {
        const response = await api.get('/posts');
        
        return {
          type: 'xx',
          payload: response
        };
      };
      
      store.dispatch(fetchPosts());
      ```

      * **Why above codes didn't work?? error message??**
        * Action creators must return **plain object** with type property!! (becuase of async/await, it is not returnning plain object... you can see it on babel.io)
        * If removing async/await?? By the time our action gets to reducers, we won't have fetched our data.(asynchronous call)

    * dispatch --> **middleware** --> reducers

* understand the purpose of **redux-thunk**(middleware)

  * Middleware: functions to slightly change the behavior of redux store, which add in new capabilities or features. Here, help us to make network requests from redux side. (Redux-thunk or something very similar)
    * function that get called with every action we dispatch
    * has the ability to stop, modify or otherwise mess around the actions
    * most popular use is for dealing with async action creators
  * Redux-thunk is not only for async action creators, also allows us to do many other things.
  * With redux-thunk, action creator can **return action objects or return functions**.

  * ![Screen Shot 2020-11-19 at 12.03.10 AM](/Users/hualinfeng/Library/Application Support/typora-user-images/Screen Shot 2020-11-19 at 12.03.10 AM.png)

  * If returning a function, that function will receive dispatch and getState(unlimited power on redux store) as arguments and need to dispatch a new action(basically an object) in this function.

    * with dispatch, we can change redux data
    * with getState, we can read redux data

  * [Link to redux-thunk](https://github.com/reduxjs/redux-thunk)

  * Wire up the middleware to our redux store

    * ```react
      import React from 'react';
      import ReactDOM from 'react-dom';
      import {Provider} from 'react-redux';
      import {createStore, applyMiddleware} from 'redux';
      import reducers from 'path_to_reducers';
      import thunk from 'redux-thunk';
      import App from './App'; // top component in our app
      
      ReactDOM.render(
      	<Provider store={createStore(reducers, applyMiddleware(thunk))}>
        	<App />
        </Provider>,
        document.querySelector('#root')
      );
      ```

  * Rewrite the action creator with redux-thunk

    * ```react
      const fetchPosts = () => async (dispatch, getState) => {
        const response = await api.get('/posts');
        
        dispatch({type: 'xx',payload: response}); // need to dispatch action manually
      };
      
      store.dispatch(fetchPosts); // action creator could return function
      ```

#### Redux store design

* When you first start up the redux application, each reducer is going to automatically be called one time. (chance to set up default value)

* Rules of reducers

  * Must return any value(cannot be undefined or do not have return statement)

  * Produce state/data to be used inside of your app using only previous state and the action.

  * Must not return 'out of itself' to decide what value to return (**reducers are pure**)

    * cannot reach out to outside function/disks/DOM node/API request.....
    * only some calculation depending on previous state and action type + payload.

  * Must not mutate its input 'state' argument. (don't have to worry about integers/strings)

    * Mutation: add/delete/change(array or object)

    * need to see **source code** to know the truth. Actually you can mutate state every time you want. But How redux decide whether a state has changed???

      * ```javascript
        let hasChanged = false;
        const nextState = {};
        // go through all reducers
        for (let i = 0; i < finalReducerKeys.length; i++) {
          const key = finalReducerKeys[i]
          const reducer = finalReducers[key]
          const previousStateForKey = state[key]
          const nextStateForKey = reducer(previousStateForKey, action)
          if (typeof nextStateForKey === 'undefined') {
            const errorMessage = getUndefinedStateErrorMessage(key, action)
            throw new Error(errorMessage)
          } // rule 1
          nextState[key] = nextStateForKey
          hasChanged = hasChanged || nextStateForKey !== previousStateForKey // this is shallow comparison(so if you only mutate on an array or object rather than return a new one, redux will regard it as not changed...)
        }
        	hasChanged = hasChanged || finalReducerKeys.length !== Object.keys(state).length
        	return hasChanged ? nextState : state
        ```

* Safe state updates

  * Array
    * Remove: `state.filter(ele => ele !== 'hi')`
    * Add: `[...state, 'hi']`
    * Replace: `state.map(ele => ele === 'hi' ? 'hii' : ele)`
  * Object
    * Update:  `{...state, name: 'Sam'}`
    * Add: `{...state, age: 30}`
    * Remove: `{...state, age: undefined}`(not recommented) or `_.omit(state, 'age')`(loadash library) --> when you open console on loadash doc page, they already import as _

* Use switch statement in reducers to make sure dealing with each action.

* We can do some pre-calculations in mapStateToProps instead of directly passing store state.

* mapStateToProps, mapDispatchToProps and connect function may be defined in a separate file with the components.

* mapStateToProps will be called with not only `state` but also `ownProps`.

* Overfetch some data?

  * memorize function from lodash library

    * _.memorize(function) is to memorize a function(actually the returned value from first call). So if you call this function with the same parameters for more than one time, it will return you with the cached returned value(will not execute the function) from the first-time call. (you may return a function)

    * one solution with less codes but may not be the best.

      * ```javascript
        // Old version
        export const fetchUser = id => async dispatch => {
          const response = await api.get(`/user/${id}`);
          
          dispatch({
            type: 'xxx',
            payload: response.data
          });
        };
        
        // We cannot simply apply the _.memorize() here
        // Correct one
        export const fetchUser = id => async dispatch => _fetchUser(id, dispatch);
        const _fetchUser = _.memorize(async (id, dispatch) => {
          	const response = await api.get(`/user/${id}`);
          
            dispatch({
              type: 'xxx',
              payload: response.data
            });
        });
        ```

  * Another more universal solution:

    * Action creators in Action creator

    * ```javascript
      import _ from 'lodash';
      
      export const fetchPostsAndUsers = () => async (dispatch, getState) => {
      	await dispatch(fetchPosts());
        
        const userIds = _.uniq(_.map(getState().posts, 'userId'));//_.uniq will pick out the unique items from array; _.map will pick out the property userId from an array of objects.
        
        userIds.forEach(id => dispatch(fetchUser(id)));
        // if we need to wait for this to be finished. We may use 
        // await Promise.all(userIds.map(id => dispatch(fetchUser(id))))
      };
      
      export const fetchPosts = () => async dispatch => {
        const response = await api.get('/posts');
        
        dispatch({type: 'xx',payload: response}); // need to dispatch action manually
      };
      
      export const fetchUser = id => async dispatch => {
        const response = await api.get(`/user/${id}`);
        
        dispatch({
          type: 'xxx',
          payload: response.data
        });
      };
      ```

    * Chain from lodash

      * ```javascript
        const userIds = _.uniq(_.map(getState().posts, 'userId'));
        
        userIds.forEach(id => dispatch(fetchUser(id)));
        
        // with chain
        _.chain(getState().posts)
        	.map('userId')
        	.uniq()
        	.forEach(id => dispatch(fetchUser(id)))
        	.value(); // to execute steps
        ```

### Streaming application

OBS(open broadcasting software) --> RTMP server(real time messaging protocol) --> web server  --> browser

​																							<-- browser get video feed

Challenges:

1. need to be able to navigate around to seperate pages in our app
2. need to allow users to log in/out(Google OAuth)
3. need to handle forms in Redux
4. need to **master** CRUD(create, read, update, destroy) operations in React/Redux
5. need good error handling

#### React Router(a library)

* some related libraries:
  * react-router: core navigation lib(some lower logics) --> we don't install it manually
  * React-router-dom: navigation for dom-based apps(what we want!)
  * React-router-native: for react-native apps
  * React-router-redux: not recommended by react/redux officials

* `import {BrowserRouter, Route} from 'react-router-dom';`

* only care about the characters after domain and port definition(not full url)

* histroy object in browser  -- url in address bar --> BrowserRouter, and Route decide whether to <u>show or hide</u> according to path matching. (Route component is visible depending on the url)

* How to get matched?
  *  rule: check whether the extracted url **contains** the path string
  * `exact` keyword --> override the rule to check `extracted url === path string`

* how to navigate?

  * In traditional HTML, we can use anchor link to the relative path to navigate.

    * ```html
      <a href="/pagetwo">Navigate to page two</a>
      ```

    * BAD!!

    * browser will re-request the index.html from server and dump old HTML file(**all the React/Redux data will be lost**), and then download and execute the js files.

  * We use **`Link`** component from `react-router-dom` to replace all the anchor tag.

    * ```react
      <Link to="/pagetwo">Navigate to page two</Link>
      ```

    * Actually it is still an **anchor** tag. But react-router detects the click event and <u>prevents it from the traditional(default) way</u> of anchor tag. (Will not refetch and reload the page). It is a single page app.

* Router Types**(relevant to deployment)**

  * BrowserRouter(discussed before)
    * localhost:3000/pagetwo
    * traditional server: if it didn't find the specific route, let's say '/pagetwo', it will return 404 error.
    * react dev server: <u>check dev resources --> check public dir --> serve up the index.html</u> (In this case, we can use BrowserRouter. Because our react app is in js files)
    * <u>need to make sure the server is configured in the identical fasion with react dev server.</u> 
  * HashRouter
    * Use everything after a '#' as the path
    * localhost:3000/#/pagetwo
    * Why?
      * <u>we can always make requests to the domain+port url(for example, localhost:3000) and we can configure the localhost:3000 to always return the index.html</u>. And our server should not look the thing after hash #, the hash is only for use by client or the browser.
      * And when the application loads up, our app can look at whatever after the hash and then determine what content to show on the screen.
      * more flexible because it does not require special configuration by the backend server. You can just have a single html file and a single route.
    * a good example: doing a deployment to get-help pages.(does not allow you to do any type of special logic, expects you to make a request to some defined resouce)
  * MemoryRouter
    * doesn't use url to track navigation
    * localhost:3000/

* For always visible components, we don't include them in BrowserRouter.

* If any conponent is not a child of react router, then you cannot use react router related components. So use links inside router!

### Handle Authentication

In general,

* Email/password authentication(eaiser and more traditional)

* OAuth authentication([OAuth](https://oauth.net/) 2.0)

  * User authenticates with outside service provider(like Google)
  * User authrizes our app to access their information
  * Outside provider tells us about the user
  * We are trusting the outside provider to correctly handle identification of a user
  * **OAuth can be used for**
    * user identification in our app
    * our app making actions on behalf of user(let our app <u>have access to users' data</u> on outside service providers)
      * How?? 
      * List of `scopes`: a permission that you are granting to an app(scope is a string value)
      * For example: [Google's scope list](https://developers.google.com/identity/protocols/oauth2/scopes#google-sign-in)

* Server vs browser

  * both get identifying information of users from service providers, also a `token` that server/browser app can use to make request on behalf of users
  * Differences:
    * needs to access users' data when they are not logged in   vs    only needs to access data when they are logged in
    * difficult to setup because need to store lots of info about the user   vs   very easy due to google's js lib to automate flow

* Flow on Browser app(easy to setup)

  * click on button
  * we use google's js lib to initiate OAuth process
  * Google's js lib makes auth request to google(on google side)
  * Google display a confirmation screen to user in a popup window
  * user accepts
  * Popup window closes
  * Google's js lib invokes a callback in our react/redux app (back to our side)
  * the callback provides with authorization token and profile info for user

  When user logged out our app or logged out google service, we will get another callback invoked. we should also take this callback into account.

* Set up OAuth
  * https://console.developers.google.com/
  * 598056569582-fimtald34aeo2jh9u7nr42fe97m18r83.apps.googleusercontent.com
  * client id is for browser app, client secret is for servers.
  * Google is trying to make the gapi small so only have a `load` method for you to load the specific portion of gapi(or the whole js client lib).
  * [load the client lib + client setup(init) --> then we can make api requests](https://developers.google.com/identity/sign-in/web/reference)
  * After we initialize the lib, we can get the auth object(`gapi.auth2.getAuthInstance()`) then we can do some operations on authentication.
* Integrate with Redux store
  * actions
  * reducers
  * connect to component

### Redux DevTools

how to use?

debug session: path?debug_session=(some string), redux devtools will persist the actions and data during refreshes.

### React Form

A [library](https://redux-form.com/8.3.0/) to help us update/get data for forms *automatically* in redux application. Help us to create action creaters, build reducers and get state for us automatically.

* add redux form reduer to redux store

  `import {reducer} from 'redux-form';`

* "connect" our component to this library so we can use **tons of helper function from this.props** to update/get form state from store.

  ```react
  import {Field, reduxForm} from 'redux-form;
  // reduxForm returns a function, which looks like "connect" in react-redux --> make sure we can call some action creators and get form data into our component.(automatically)
  // Field（react component) is essentially an input element
  
  class StreamCreate extends React.Component {
      renderInput(formProps) {
          return <input {...formProps.input} />;
      }
      render() {
          return (
              <form>
                  <Field name="title" component={this.renderInput} />
                  <Field name="description" component={this.renderInput} />
              </form>
          );
      }
  }
  
  // receive a single object for config
  export default reduxForm({
    form: 'streamCreate' // the name for the form, will be stored as the key of subreducer in the form reducer.
  })(StreamCreate);
  ```

* **Field component**

  * The Field component itself does not know how to show a text/checkbox... input element --> set component props
  * **name**: the property this field is going to manage(show in sub-reducer)
  * **component**: received a function which returns a custimized input component. The function passed in will get a parameter including value and event handler like onChange.

* Our job is to take the handler and take value from props.

* Customization

  * The additional props added to Field component will be passed to the function passed to component property

  * ```react
    renderInput({input, label}) {
      return (
        <div className="field">
          <label>{label}</label>
          <input {...input} />
        </div>
      );
    }
    render() {
      return (
        <form className="ui form">
          <Field
            name="title"
            component={this.renderInput}
            label="Enter title:" // passed to this.renderInput
            />
          <Field
            name="description"
            component={this.renderInput}
            label="Enter Description:"  // passed to this.renderInput
            />
        </form>
      );
    }
    ```

* **Submit**

  * <u>handleSubmit</u> is provided by redux-form, which already takes event object and calls event.preventDefault(). It will be passed in our own callback function which will have form values as parameter and invoked after the form is submitted.

    ```react
    onSubmit(formValues) {
    	console.log(formValues); // is an object
    }
    render() {
      return (
        <form
          className="ui form"
          onSubmit={this.props.handleSubmit(this.onSubmit)} // pay attention to here
        >
          <Field
            name="title"
            component={this.renderInput}
            label="Enter title:"
          />
          <Field
            name="description"
            component={this.renderInput}
            label="Enter Description:"
          />
          <button className="ui button primary">Submit</button>
        </form>
      );
    }
    ```

  * **Validation**

    * redux-form will automatically call a method `validate(formValues)`. We have to define this function. --> initial render and every single time when user interact with form.

    * If the validate method return empty object, it means no errors; if non-empty object, there are some errors. We need to define error messages corresponding tp field name. And the error for each field will be passed to the component props(which is a function) located in meta. We can access it via `meta.error`

    * Customize when to show errors. (When clicking on submit?? Or when user blur from a field)

      * there is an `autoComplete: "off"` property on input tag
      * there is a property `touched` inside meta, which represent which user has interacted with this form field.
      * we can determine to show error message based on `meta.error` and `meta.touched`
      * Noted: semantic UI does not show error message(display:none) by default so we need to add one more className "error"

    * Reference code:

      ```react
      import React from 'react';
      import {Field, reduxForm} from 'redux-form';
      
      class StreamCreate extends React.Component {
          renderInput({input, label, meta: {error, touched}}) {
              const showErrorMsg = error && touched;
              return (
                  <div className={`field${showErrorMsg ? ' error' : ''}`}>
                      <label>{label}</label>
                      <input {...input} />
                      {showErrorMsg && (
                          <div className="ui error message">{error}</div>
                      )}
                  </div>
              );
          }
      
          onSubmit(formValues) {
              console.log(formValues);
          }
      
          render() {
              return (
                  <form
                      className="ui form error"
                      onSubmit={this.props.handleSubmit(this.onSubmit)}
                  >
                      <Field
                          name="title"
                          component={this.renderInput}
                          label="Enter title:"
                      />
                      <Field
                          name="description"
                          component={this.renderInput}
                          label="Enter Description:"
                      />
                      <button className="ui button primary">Submit</button>
                  </form>
              );
          }
      }
      
      const validate = (formValues) => {
          const errors = {};
      
          if (!formValues.title) {
              errors.title = 'Please enter the title';
          }
      
          if (!formValues.description) {
              errors.description = 'Please enter the description';
          }
      
          return errors;
      };
      
      export default reduxForm({
          form: 'streamCreate',
          validate
      })(StreamCreate);
      
      ```

      

* 

---





