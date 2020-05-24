# React.js Essential Training

Create react elements using JSX at `/src/index.js`
```js
import React from 'react'           //import the react library
import ReactDOM from 'react-dom'    //needed to render React components

var style = { backgroundColor: 'orange', color: 'white', fontFamily: 'Arial' }

ReactDOM.render(
    <div style={style}>
        <h1 id="heading-element">Hello World!</h1>
        <p>We're glad you're here!</p>
    </div>,
    document.getElementById('root')     //root div at /public/index.html
)     
```

## Refactor React Components

* Create a class component with a custom method. See below example like SkiDayCounter:
```js
import React, { Component } from 'react'    //replace React.Component by Component
import { render } from 'react-dom'          //replace ReactDOM.render by render

let skiData = {	total: 50, powder: 20, backcountry: 10,	goal: 100 }

class SkiDayCounter extends Component {     //using Component instead of React.Component
    calcGoalProgress = (total, goal) => {   //custom method for this component
		return total/goal*100 + '%'
	}
	render() {
		const {total, powder, backcountry, goal} = this.props
		return (
			<section>
				<div><p>Total Days: {total}</p></div>
				<div><p>Powder Days: {powder}</p></div>
				<div><p>Backcountry Days: {backcountry}</p></div>
				<div><p>Goal Progress: {this.calcGoalProgress(total, goal)}</p></div>
			</section>
		)
	}
}
render(     //using render instead of ReactDOM.render
	<SkiDayCounter 
		total={skiData.total}
		powder={skiData.powder}
		backcountry={skiData.backcountry}
		goal={skiData.goal}/>, 
	document.getElementById('root')
)
```

* Create a function component with custom functions
```js
const calcGoalProgress = (total, goal) => {
	return total/goal*100 + '%'
}
const SkiDayCounter = ({total, powder, backcountry, goal}) => {
	return (
		<section>
				<div>
					<p>Total Days: {total}</p>
				</div>
				<div>
					<p>Powder Days: {powder}</p>
				</div>
				<div>
					<p>Backcountry Days: {backcountry}</p>
				</div>
				<div>
					<p>Goal Progress: {calcGoalProgress(total, goal)}</p>
				</div>
		</section>
	)
}
```

## Props and State
Compose components sample:

```js
import React from 'react'
import { render } from 'react-dom'

const Book = ({title, author, pages}) => {
	return (
		<section>
			<h2>{title}</h2>
			<p>by: {author}</p>
			<p>Pages: {pages} pages</p>
		</section>
	)
}

const Library = () => {
	return (
		<div>
			<Book title="The Sun Also Rises" author="Ernest Hemingway" pages={260}/>
			<Book title="White Teeth" author="Zadie Smith" pages={480}/>
			<Book title="Cat's Cradle" author="Kurt Vonnegut" pages={304}/>
		</div>
	)
}

render(
	<Library />, 
	document.getElementById('root')
)
```

Refactor components sample

```js
let bookList = [
	{"title": "Hunger", "author": "Roxane Gay", "pages": 320},
	{"title": "The Sun Also Rises", "author": "Ernest Hemingway", "pages": 260},
	{"title": "White Teeth", "author": "Zadie Smith", "pages": 480},
	{"title": "Cat's Cradle", "author": "Kurt Vonnegut", "pages": 304}
]

const Book = ({title, author, pages}) => {
	return (
		<section>
			<h2>{title}</h2>
			<p>by: {author}</p>
			<p>Pages: {pages} pages</p>
		</section>
	)
}

const Library = ({books}) => {
	return (
		<div>
			{books.map(
				(book, i) => 
					<Book key={i} title={book.title} author={book.author} pages={book.pages}/>
			)}
		</div>
	)
}

render(
	<Library books={bookList}/>, 
	document.getElementById('root')
)
```