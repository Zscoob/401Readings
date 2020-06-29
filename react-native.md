# reacings 401
react native

## basics
The first style that we find is flex: 1, the flex prop will define how your items are going to "fill" over the available space along your main axis. Since we only have one container, it will take all the available space of the parent component. In this case, it is the only component, so it will take all the available screen space.

The following style is justifyContent: "center". This align children of a container in the center of the container's main axis and finally we have alignItems: "center", which align children of a container in the center of the container's cross axis.

Some of the things in here might not look like JavaScript to you. Don't panic. This is the future.

First of all, ES2015 (also known as ES6) is a set of improvements to JavaScript that is now part of the official standard, but not yet supported by all browsers, so often it isn't used yet in web development. React Native ships with ES2015 support, so you can use this stuff without worrying about compatibility. import, from, class, and extends in the example above are all ES2015 features. If you aren't familiar with ES2015, you can probably pick it up by reading through sample code like this tutorial has. If you want, this page has a good overview of ES2015 features.

The other unusual thing in this code example is <View><Text>Hello world!</Text></View>. This is JSX - a syntax for embedding XML within JavaScript. Many frameworks use a specialized templating language which lets you embed code inside markup language. In React, this is reversed. JSX lets you write your markup language inside code. It looks like HTML on the web, except instead of web things like <div> or <span>, you use React components. In this case, <Text> is a Core Component that displays some text and View is like the <div> or <span>.

## state
Unlike props that are read-only and should not be modified, the state allows React components to change their output over time in response to user actions, network responses and anything else.

In a React component, the props are the variables that we pass from a parent component to a child component. Similarly, the state are also variables, with the difference that they are not passed as parameters, but rather that the component initializes and manages them internally.