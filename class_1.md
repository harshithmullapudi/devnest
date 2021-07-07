# Class 1

1. Class based components
2. Life cycle methods
3. Webpacks: Transpiler


## Class Based Components

```
class MyComponent extends Component {
render() {
    return(
        <h1> Hello world </h1>
        )
    }
}
```

### Props

In a React component, props are variables passed to it by its parent component.

### State

State on the other hand is still variables, but directly initialized and managed by the component.

```
class MyComponent extends Component {
  constructor(props) {
    super(props);
    this.state= {
      property: 'value'
    }
  }
  render() {
  return(
  <h1> {this.state.property} </h1>  
  )
  }
}
```

#### setState

There is a designated way to change states. To change state, we use this.setState() method. Let’s change the property property to 'hussain'

## Life cycle methods

![all life cycle methods](https://miro.medium.com/max/700/1*G-OoSxZ3NQ7M3Qndscrwpw.jpeg)


### constructor()

constructor() method is called when the component is initiated and it’s the best place to initialize our state. The constructor method takes props as an argument and starts by calling super(props) before anything else.

```
import React, { Component } from 'react'

export default class App extends Component {
  constructor(props){
    super(props)
    this.state = {
      name: 'Constructor Method'
    }
  }
  render() {
    return (
      <div>
       <p> This is a {this.state.name}</p>
      </div>
    )
  }
}
```

### getDerivedStateFromProps()

The getDerivedStateFromProps method is called right before rendering the element in our DOM. It takes props and state as an argument and returns an object with changes to the state.


```
import React, { Component } from 'react'

export class ChildComponent extends Component {
    constructor(props){
        super(props)
        this.state = {
          name: 'Constructor Method'
        }
      }

    static getDerivedStateFromProps(props, state) {
        return {name: props.nameFromParent} 
      }
    render() {
        return (
            <div>
                This is a {this.state.name}
            </div>
        )
    }
}


export default class getDerivedStateFromPropsMethod extends Component {
   
    render() {
        return (
            <div>
                <ChildComponent nameFromParent="getDerivedStateFromProps Method"/>
            </div>
        )
    }
}

```


### render()

This is the only compulsory method required by the React. This method is responsible to render our JSX to DOM

```
import React, { Component } from 'react'

export default class renderMethod extends Component {
    render() {
        return (
            <>
                 <p>This is a render method</p>
            </>
        )
    }
}

```

### componentDidMount()

The most common and widely used lifecycle method is componentDidMount. This method is called after the component is rendered. You can also use this method to call external data from the API.

```
import React, { Component } from 'react'

export default class componentDidMountMethod extends Component {
  constructor(props){
    super(props)
    this.state = {
      name: 'This name will change in 5 sec'
    }
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({name: "This is a componentDidMount Method"})
    }, 5000)

  }
  render() {
    return (
      <div>
       <p>{this.state.name}</p>
      </div>
    )
  }
}

```

## #Updating

### shouldComponentUpdate()

This lifecycle method is used when you want your state or props to update or not. This method returns a boolean value that specifies whether rendering should be done or not. The default value is true.

```
import React, { Component } from 'react'

export default class shouldComponentUpdateMethod extends Component {
  constructor(props){
    super(props)
    this.state = {
      name: 'shouldComponentUpdate Method'
    }
  }
  shouldComponentUpdate() {
    return false; //Change to true for state to update
  }

  componentDidMount(){
    setTimeout(() => {
      this.setState({name: "componentDidMount Method"})
    }, 5000)
  }
  render() {
    return (
      <div>
       <p>This is a {this.state.name}</p>
      </div>
    )
  }
}
```

### getSnapshotBeforeUpdate()

This method is called right before updating the DOM. It has access to props and state before the update. Here you can check what was the value of your props or state before its update. So let see how it works.

Note: componentDidUpdate() should be included otherwise you will get an error.

```
import React, { Component } from 'react'

export default class getSnapshotBeforeUpdateMethod extends Component {
    constructor(props){
        super(props)
        this.state = {
          name: 'constructor Method'
        }
      }

      componentDidMount(){
        setTimeout(() => {
          this.setState({name: "componentDidMount Method"})
        }, 5000)
      }
      getSnapshotBeforeUpdate(prevProps, prevState) {
        document.getElementById('previous-state').innerHTML = "The previous state was " + prevState.name
      }
      componentDidUpdate() {
        document.getElementById('current-state').innerHTML = "The current state is " + this.state.name
      }
    render() {
        return (
            <>
               <h5>This is a {this.state.name}</h5>
                <p id="current-state"></p>
                <p id="previous-state"></p>
            </>
        )
    }
}
```

### componentDidUpdate()

The componentDidUpdate method is called after the component is updated in the DOM. This is the best place in updating the DOM in response to the change of props and state.

```
import React, { Component } from 'react'

export default class componentDidUpdateMethod extends Component {
    constructor(props){
        super(props)
        this.state = {
            name: 'from previous state'
        }
    }
    componentDidMount(){
        setTimeout(() => {
            this.setState({name: "to current state"})
          }, 5000)
    }
    componentDidUpdate(prevState){
        if(prevState.name !== this.state.name){
            document.getElementById('statechange').innerHTML = "Yes the state is changed"
        }
    }
    render() {
        return (
            <div>
                State was changed {this.state.name}
                <p id="statechange"></p>
            </div>
        )
    }
}
```
## #Unmounting

### componentWillUnmount()

If there are any cleanup actions like canceling API calls or clearing any caches in storage you can perform that in the componentWillUnmount method. You cannot use setState inside this method as the component will never be re-rendered.

```
import React, { Component } from 'react'

export default class componentWillUnmount extends Component {
    constructor(props){
        super(props)
            this.state = {
                show: true,
            } 
    }
    render() {
        return (
            <>
              <p>{this.state.show ? <Child/> : null}</p>
               <button onClick={() => {this.setState({show: !this.state.show})}}>Click me to toggle</button>
            </>
        )
    }
}

export class Child extends Component{
    componentWillUnmount(){
        alert('This will unmount')
    }
    render(){
        return(
            <>
            I am a child component
            </>
        )
    }
}
```
