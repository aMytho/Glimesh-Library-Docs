+++
title = "Basic Subscriptions"
weight = 2
+++

## Waiting for data...

Subscriptions are used to ask Glimesh for a notification when something happens. At the moment, we have subscriptions for new followers, chatMessages, and channel changes. More will be added as the platform grows.

Before we can make a subscription, we need to add an event listener. This must be done **first**. if you subscribe without a listener you will not recieve the data in our system. However, if you are writing your own system it may work.

```js
a.getEvents().once("ChannelReady", (rdy) => {
    console.log("Glimesh is ready to notify us about channel changes!");
    console.log(rdy);
});

a.getEvents().on("ChannelData", (data) => {
    console.log("Something happened!");
    console.log(data);
});
```

In this case I added 2 listeners. The first is optional, the second is required. The first listener will only run once time. It will tell us that Glimesh received our subscription. There is no data associated with it. The second will run whenever the specified channel has a change. This will continue to run until the connection is closed.

> Notice the naming convention of topicEvent. This is followed throughout the library.

Now we need to ask Glimesh for the notifications. We do this via a subscription.

```js
a.subToEvent("Channel", [{channelId: 1}]);
```

We ask to be notified whenever the channel with an ID of 1 is changed. You should use your own channel for testing. As with all GraphQL requests you can modify the return value. This specific subscription allows us to pass in a null paramater. Glimesh allows some subscriptions to be global where all events on all channels are sent through. You should only use this feature if you truly need that data. 