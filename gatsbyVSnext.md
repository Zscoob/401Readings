## 401 readings
getsby vs next.js

## Static site generator vs. server side rendered pages.
A static site generator generates static HTML on build time. It doesn’t use a server.

With server-side rendering, it dynamically generates the HTML every time a new request comes in. It uses a server for this.

Gatsby is a static site generator tool.

Next is mainly a tool for server-side rendered pages (although it also supports static exports)

Of course, both can call APIs client side. The difference is Next requires a server to be able to run. Gatsby can function without any server at all.

Gatsby just generates pure HTML/CSS/JS. This means you can host it at S3, Netlify or other static hosting services. This makes it extremely scalable to high traffic loads.

Next creates HTML on runtime. So each time a new request comes in, it creates a new HTML page from the server.

## Gatsby handles your data
Another big difference between the two toolkits is how they handle data.

Gatsby dictates how you should handle data in your app.

Next does not dictate how you should handle your data. How you do it is entirely up to you.

So how does Gatsby handle data? All data for your whole apps go into GraphQL. Let’s say you fetch data from an API. Then you first have to tell Gatsby to import the data into a GraphQL database.

Your React components fetches the data from the GraphQL endpoint.

Gatsby also has lots of plugins for various data sources which (in theory) makes it easy to integrate against many data sources. Some examples of data sources plugins are contentful, Wordpress, MongoDB and GraphQL

When building for production, GraphQL is no longer used, but the data is persisted into JSON files instead.

With Next on the other hand, how you manage your data is up to you. You have to decide on your own architecture how to manage data. You have to make a decision if you want to use GraphQL, Redux, pure React.