+++
title = "Using Events"
weight = 2
+++

## Explaining Events

Events are not the same as subscriptions. A subscription is a request to Glimesh to be notified of a specific change or action. An event is how the library notifies you of the subscription. There are events for all subscriptions and several other events.

> Order of Operations: Subscription -> Glimesh -> time passes... -> Something happened on Glimesh -> Library Event -> Your code

Events are built on the event emitters from [Event Emitter 3](https://www.npmjs.com/package/eventemitter3). You can listen for the events that you want to handle. The following events are supported:

Websocket events
- Close
- Connected
- Error
- Message
- Open

Glimesh heartbeat
- Heartbeat

Unknown events are any event that the library can't identify. Great for new features (*if we had any*)
- Unknown

Use this for your own events!
- Custom

Glimesh has acknowledged your subscription and will notify you when the subscribed topic happens
- ChatReady
- ChannelReady
- FollowReady

The topic happened, here is your data!
- ChatData
- ChannelData
- FollowData

A mutation has completed
- BanData
- ChatData
- DeleteData
- FollowData
- LongTimeoutData
- ShortTimeoutData
- UnbanData
- UnFollowData
- UpdateStreamInfoData

You can add any amount of listeners for any amount of events. You can also remove listeners if you no longer need the notification.

Adding a listener is easy. First, select the event that you want. Then subscribe to it. The below code will run every time the event happens. 

```js
a.getEvents().on("ChatData", (data) => {
    console.log("A chat message is below!");
    console.log(data);
});
```

> Don't forget that you must first *subscribe* before the event can detect the change!

We can also specify that we only want to run the code the first time the event happens. We can do that with the `once` method.

```js
a.getEvents().once("Open", () => {
    console.log("The connection is open");
});
```

Once the open event happens the listener will no longer run. It will be removed from memory. If you want a listener to run more than once, but less than *forever*, you can remove the listener manually.

```js
a.getEvents().removeListener("ChatData", (arg) => {
    console.log("Will no longer listen for chat data");
});
```

Note that this will remove all listeners for the specified event, not just 1.

You can also emit custom events. This is great for building on top of this library. However, the corresponding listener needs to have the same amount of params in the callback to be able to access all of the data.

```js
a.getEvents().emit("Custom", "some", {custom: "data"}, 123, ["wow", "so many options!"]);
```

If you need more functionality related to events be sure to look at the Event Emitter [documentation](https://www.npmjs.com/package/eventemitter3)!