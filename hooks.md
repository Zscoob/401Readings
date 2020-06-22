# 401 readings
class 36 hooks

## effects and hooks
### using classes
In React class components, the render method itself shouldn’t cause side effects. It would be too early — we typically want to perform our effects after React has updated the DOM.

This is why in React classes, we put side effects into componentDidMount and componentDidUpdate. Coming back to our example, here is a React counter class component that updates the document title right after React makes changes to the DOM:
___________________________________________________________________________________________
          class Example extends React.Component {
            constructor(props) {
              super(props);
              this.state = {
                count: 0
              };
            }

            componentDidMount() {
              document.title = `You clicked ${this.state.count} times`;
            }
            componentDidUpdate() {
              document.title = `You clicked ${this.state.count} times`;
            }

            render() {
              return (
                <div>
                  <p>You clicked {this.state.count} times</p>
                  <button onClick={() => this.setState({ count: this.state.count + 1 })}>
                    Click me
                  </button>
                </div>
              );
            }
          }
___________________________________________________________________________________________
 we have to duplicate the code between these two lifecycle methods in class.

This is because in many cases we want to perform the same side effect regardless of whether the component just mounted, or if it has been updated. Conceptually, we want it to happen after every render — but React class components don’t have a method like this. We could extract a separate method but we would still have to call it in two places.

## useEffect
 By using this Hook, you tell React that your component needs to do something after render. React will remember the function you passed (we’ll refer to it as our “effect”), and call it later after performing the DOM updates. In this effect, we set the document title, but we could also perform data fetching or call some other imperative API.

 Placing useEffect inside the component lets us access the count state variable (or any props) right from the effect. We don’t need a special API to read it — it’s already in the function scope. Hooks embrace JavaScript closures and avoid introducing React-specific APIs where JavaScript already provides a solution.

 By default, it runs both after the first render and after every update. (We will later talk about how to customize this.) Instead of thinking in terms of “mounting” and “updating”, you might find it easier to think that effects happen “after render”. React guarantees the DOM has been updated by the time it runs the effects.

