What even is ‘children’?
The React docs say that you can use props.children on components that represent ‘generic boxes’ and that ‘don’t know their children ahead of time’. For me, that didn’t really clear things up. I’m sure for some, that definition makes perfect sense but it didn’t for me.
My simple explanation of what this.props.children does is that it is used to display whatever you include between the opening and closing tags when invoking a component.
A simple example
Here’s an example of a stateless function that is used to create a component. Again, since this is a stateless function, there is no 'this' keyword so just use props.children
const Picture = (props) => {
  return (
    <div>
      <img src={props.src}/>
      {props.children}
    </div>
  )
}
This component contains an <img> that is receiving some props and then it is displaying {props.children}.
Whenever this component is invoked {props.children} will also be displayed and this is just a reference to what is between the opening and closing tags of the component.
//App.js
render () {
  return (
    <div className='container'>
      <Picture key={picture.id} src={picture.src}>
          //what is placed here is passed as props.children  
      </Picture>
    </div>
  )
}
Instead of invoking the component with a self-closing tag <Picture /> if you invoke it will full opening and closing tags <Picture> </Picture> you can then place more code between it.
This de-couples the <Picture> component from its content and makes it more reusable.
A more complex example
Here I am taking the above components and using them in a simple app that keeps track of the ID of a picture when a button is clicked.
//App.js
import React, { Component } from 'react';
import Picture from './Picture';
import Button from './Button';
class App extends Component {
  constructor(props) {
    super(props);
this.state = {
      pictures: [
        {id: 1, src: 'http://via.placeholder.com/200x100'},
        {id: 2, src: 'http://via.placeholder.com/400x200'},
        {id: 3, src: 'http://via.placeholder.com/200x100'}
      ],
      currentPic: null
    };
this.setCurrentPic = this.setCurrentPic.bind(this);
  }
setCurrentPic(id) {
    this.setState({currentPic: id});
  }
render () {
    return (
      <div>
        <div className='squares'>
          {this.state.pictures.map((picture) => {
            return (
              <Picture key={picture.id} src={picture.src}>
                <Button
                  pictureSrc={picture.src}
                  setCurrentPic={this.setCurrentPic}
                  id={picture.id}
                />
              </Picture>
            )
          })}
        </div>
        <div>
          <p>Current selected picture ID is {this.state.currentPic}</p>
        </div>
      </div>
    )
  }
}
export default App;
//Picture.js
import React, { Component } from 'react';
const Picture = (props) => {
  return (
    <div className='picture'>
      <img src={props.src} className='picture'/>
      {props.children}
    </div>
  )
}
export default Picture;
//Button.js
import React, { Component } from 'react';
class Button extends Component {
  constructor(props) {
    super(props);
    this.state = {
      pictureId: null,
      label: null
    };
    this.buttonLabel = this.buttonLabel.bind(this);
  }
buttonLabel(src) {
    src.includes('200x100')
    ? this.setState({pictureId: this.props.id, label: 'Small'})
    : this.setState({pictureId: this.props.id, label: 'Large'})
  }
componentDidMount() {
    this.buttonLabel(this.props.pictureSrc)
  }
render() {
    return (
      <div>
        <button
          onClick={() => this.props.setCurrentPic(this.props.id)}
        >
          {this.state.label}
        </button>
      </div>
    )
  }
}
export default Button;
When invoking the <Picture> component I am passing a <Button> component as it’s children and this is then rendered due to using {props.children} in the definition of the <Picture> component.

But the real power of props.children is that it can be anything!
What if I changed the render() method of App.js to the following:
render () {
    return (
      <div>
        <div className='squares'>
            <Picture src={this.state.pictures[0].src}>
              <p>Hey, I'm some text!</p>
            </Picture>
            <Picture src={this.state.pictures[1].src}>
              <Button pictureSrc={this.state.pictures[1].src}></Button>
            </Picture>
            <Picture src={this.state.pictures[2].src}>
              <Picture src={this.state.pictures[2].src} />
            </Picture>
        </div>
        <div>
          <p>Current selected picture ID is {this.state.currentPic}</p>
        </div>
      </div>
    )
  }
Here, instead of using map() I am invoking <Picture> three times and passing it different content each time. The first is just some text, the second is the <Button> component and the third is another <Picture> component.

dont have to change the <Picture> component at all. Since it knows to just render props.children it will do just that and I can customize the content under each picture to whatever I need. I can keep reusing the same component.