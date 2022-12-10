
# Glimesh Library Documentation

This is the documentation for the client library.

## Feature Support

This library supports EVERYTHING that Glimesh can do. All queries, mutations, and subscriptions have intellisense built in. Typescript types will guide you to the correct syntax. We also allow you to communicate over the raw websocket connection. This means there are no limits.

The libarary is built on top of [Event Emitter 3](https://www.npmjs.com/package/eventemitter3). As a result we use emitters for all subscriptions. You subscribe to the Glimesh event with the API. Then you add a listener to be notified when that event happens. We have events for both Glimesh events (new follower, chat message, etc) and internal library events (connection status, custom events, heartbeats, etc).

Want event emitters without using our system? Don't worry, we support custom events in addition to the regular events. It is very easy to build with.

## Installation

```sh
npm i glimesh-browser-api
```

This will install all neccassary files.

## Getting Started

First, import the project. It supports ESM (recomended) or UMD.

>ESM
```js
import {GlimeshConnection} from "glimesh-browser-api"
```

>UMD
```html
    <script src="/path/to/node_modules/glimesh-browser-api/dist/main.umd.js"></script>
```

>> The rest of the documentation will assume you are using ESM. If you are not, simply use the UMD variables in place of ESM. They work the same.

Now we need to authenticate with Glimesh. You can use a client-id or an access token. For most uses a client-id is the best. For more info [aaa](https://google.com).

```js
// Create a connection with a client ID
let connection = new GlimeshConnection({
    clientId: "",
});

// Opens the connection. If true it will connect with an access token
connection.connectToGlimesh(false);

/// - - - - - - - - 

// Create a connection with a client ID
let connection = new GlimeshConnection({
    accessToken: "",
});

// Opens the connection. If true it will connect with an access token
connection.connectToGlimesh(true);
```

Now let's interact with the API! We will listen for any incoming chat messages. Then, we will query some additional data and play around with the event emitter.

We first need to add the listener. 

```js
connection.getEvents().on("ChatData", (data) => {
    console.log("Chat data detected!");
    console.log(data)
});
```

If you send a message you may be surprised to see that nothing happens. That's because this only subscribes to the listener, not the Glimesh event. We need to tell Glimesh that we want to be notified when someone talks in chat. We can do that with the `subToEvent` function. Replace `6` with your channel Id.

```js
connection.subToEvent("Chat", [{
    channelId: 6
}]);
```
>This must be after the above code!

Now send a message. You should see it in the console. By default, it will be nested in several layers of objects. This is the data returned from the GraphQL subscription. You can adjust the data returned by modifying the `retVal`. More on that in later documentation.

Now that we have a subscription, let's create a query. We will see who is on the homepage. Add the following code to your project.

```js
connection.createQuery("HomepageChannels", []).then(data => {
    console.log("Query Complete!");
    console.log(data);
    });
```

An empty array might seem a little strange. However, most queries require paramaters to be passed to them. If this query had a paramater it would need to be in the brackets with it's value. However, this is a simple query so that is not needed. Unlike subscriptions, queries return a promise with the value contained in them. No events are required. However, you could still add a listener for them if you wish.

The data will be logged in the console.

## What Next

Try making more queries and subscriptions. Learn to use the intellisense. It helps!

You can add custom paramaters to most funcions. You can modify the return value to request custom data instead of the default request.

```js
connection.createQuery("Channels", [{status: "LIVE"}, {first: 2}], "edges {node {status, id}}");
```
Feel free to ignore our system and write your own. It will output all events.

## FAQ

### Typescript Support

The types are included in the library. Simply import them. Most editors allow JS to show some typings. However, structure will not be enforced.

### Difference between ESM and UMD

The UMD package is an older method for importing packages. Unless you cannot use ESM, you should avoid this import. UMD modules are imported into the global scope. This means that your other code (and any viewer of your site) can use the library. This is usually not positive.

The ESM package is the new system. It uses the import/export syntax. As a result, it works great with frameworks such as Angular. However, any script tag with a type of module can use it. It is not included on the global scope.