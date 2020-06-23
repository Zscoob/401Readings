# 401 readings
class 37 custom hooks

## Custom Hooks
Custom Hooks are JavaScript functions whose names are prefixed with the word use. A custom Hook is a normal function but we hold them to a different standard. By adding the word use to the beginning, it lets us know that this function follows the rules of Hooks.

we may want to do on several pages or inside numerous different functional components in our app. When information changes we want to update the document title with some type of string. Additionally, we don't want to repeat this logic inside every functional component. We will start by extracting this code into a Hook locally on the same page, and then see how the same Hook can be imported into many components and co-located. 

So we know that a Hook can call a Hook. And if that is true then our custom Hook can also call one of the React Core Basic Hooks, like useEffect. Let's review a functional component that updates the document title one more time.

