# Learning React.js
React is a JS library built at Facebook.

## Getting Started

Setup React Developer Tools using chrome: [React Developer Tools extension](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=es). Edit extension setting with "Allow access to file URLs" enabled.

## React Element and JSX
Create a react element using **React.createElement**:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>React Project</title>
        <script src="https://unpkg.com/react@16.7.0/umd/react.development.js"></script>
        <script src="https://unpkg.com/react-dom@16.7.0/umd/react-dom.development.js"></script>
    </head>
    <body>
        <div id="root"></div>
        <script type="text/javascript">
            ReactDOM.render(
                React.createElement(
                    "div", 
                    null, 
                    React.createElement(
                        "h1",
                        null,
                        "Oh hello!"
                    )
                ),
                document.getElementById("root")
            );
        </script>
    </body>
</html>    
```

Using JSX (JS as XML). Require **script type="text/babel"** and **className** instead of class:  
```html 
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">  
    let city = "Madrid";            
    ReactDOM.render(
        <h1 className="heading">Hello {city}</h1>,
        document.getElementById("root")
    );
</script>
```

## React Components

* Functions as **const**: Use Pascal Case for functions names:
```html
<script type="text/babel">  
    const Hello = (props) => {
        return (
            <div className="heading">
                <h1>Welcome to {props.library}</h1>
                <p>{props.message}</p>
            </div>
        )
    };    
    ReactDOM.render(
        <Hello library="React" message="Enjoy!"/>,
        document.getElementById("root")
    );
</script>
```

* Properties: see below exaple with name and country properties
```html
<script type="text/babel">            
    const Lake = ({name, country}) => {
        return (
            <div className="heading">
                <h1>{name}</h1>
                <p>{country}</p>
            </div>
        )
    }; 
    const App = () => (
        <div>
            <Lake name="Lake Tahoe" country="USA" />
            <Lake name="Angora Lake" country="Angola"/>
            <Lake name="Shirley Lake" country="Australia"/>
        </div>
    );
    ReactDOM.render(
        <App />,
        document.getElementById("root")
    );
</script>
```

* Classes: always include the **render** method
```js           
const Lake = ({name}) => <h1>{name}</h1>;
class App extends React.Component {
    render() {
        return (
            <div>
                <Lake name="Lake Tahoe" />
                <Lake name="Angora Lake" />
                <Lake name="Shirley Lake" />
            </div>
        )
    }
}    
```

* State: When a component's State data changes, the render function will be called again to re-render the state change.
```js
class App extends React.Component {
    state = {
        logged: false
    }
    render() {
        return (
            <div>
                The user is {this.state.logged ? "logged in" : "logged out"}.
            </div>
        )
    }
}
```

## Events and enhance rendering