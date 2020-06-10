# 401 Readings
class 28 readings for 401d1 ASPdotNET/React

## Using JWT to authenticate and authorize requests in Postman
### What is JWT?
JSON Web Token (JWT) is an open standard for securely transmitting information between parties as a JSON object.
 It’s pronounced jot

 JWT is commonly used for authorization. JWTs can be signed using a secret or a public/private key pair. Once a user is logged in, each subsequent request will require the JWT, allowing the user to access routes, services, and resources that are permitted with that token.

### Set up an API with JWT authentication
Node.js API from Auth0 or use of another API
npm packages
nosde.server.js
run in postman

### Save the JWT as a variable
You could copy the access token from the response to use in your next request, but it’s tedious to do it for every request you want to authorize.

save the JWT as a variable so that we can reuse the token over and over again in future requests. Create a new environment. Under the Tests tab, save the access token as an environment variable with pm.environment.set()

Under the Quick Look icon, we can see that our JWT is saved as an environment variable. Now we can use our token in subsequent requests.

### Add JWT to headers in Postman
There are 2 ways to send your JWT to authorize your requests in Postman: adding a header or using an authorization helper.

#### Option 1: add an authorization header
The first option is to add a header. Under the Headers tab, add a key called Authorization with the value Bearer <your-jwt-token>. Use the double curly brace syntax to swap in your token’s variable value.

If your authorization accepts a custom syntax, you can manually tweak the prefix here (e.g. Token <your-access-token> instead of Bearer <your-access-token).

#### Option 2: use an authorization helper
The second option is to use an authorization helper. Under the Authorization tab, select the Bearer Token authorization type. Use the double curly brace syntax to swap in your token’s variable value.

Click the orange Preview Request button to see a temporary header has been added under the Headers tab. This temporary header is not saved with your request or collection.

### Difference of approach
#### Option 1: add an authorization header
  - User can tweak the prefix (e.g. Token <your-access-token> instead of Bearer <your-access-token>).
  - Authorization header is displayed explicitly in the API documentation.
  - With both of these options, you can share the request and collection with your teammates. Header is saved with the request and collection under the header property.

#### Option 2: use an authorization helper
  - Can set authorization at the collection-, folder-, or request-level. Easy to set up the same authorization method for every request inside the collection or folder.
  - With both of these options, you can share the request and collection with your teammates. Authorization is saved under the auth property.

### Scripts to check token expiration
JWT tokens don’t live forever. After a specified period of time, they expire and you will need to retrieve a fresh one.

there are 2 approaches for checking the expiration of your JWT. The approach you use choose will depend on your specific circumstances.

#### Option 1: Separate request at the beginning of the collection
This option is ideal if you’re working with a small collection that runs quickly, or you have a long-lived token that is not likely to expire by the end of the collection run. In this case, create an initial request at the beginning of the collection to retrieve and store the token. You can use the same token value throughout the remainder of your collection run.
#### Option 2: Pre-request script to run before each request
This option is good if you’re working with a large collection that might take a while to run, or you have a short-lived token that could expire soon. In this case, add some logic in a pre-request script to check if the current token is expired. If the token is expired, get a fresh one (e.g. using pm.sendRequest()) and then reset your new token’s time to live. With this approach, remember that you can use a collection- or folder-level script to run this check prior to every request in the collection or folder.

### Sessions to keep stuff private
Sessions are an additional layer within the Postman app that stores variable values locally. By default, sessions do not sync with Postman servers. Changes captured in the individual session remain local to your Postman instance, unless you explicitly sync to the cloud.

Go to your Settings, and toggle off “Automatically persist variable values”.

Now when you send a request and set a variable, the CURRENT VALUE is populated. You can think of this as a value that’s stored in a local session.

If you want to share this value with your teammates or sync it to the Postman servers, this requires another step to to explicitly sync to the cloud. To sync all of your Current Values to the Initial Values, click Persist All. To sync only a single Current Value to the Initial Value, copy and paste the value from the 3rd column to the second column.

Session variables allow you to reuse data and keep it secure while working in a collaborative environment. They allow you more granular control over syncing to the server or sharing information with your teammates



