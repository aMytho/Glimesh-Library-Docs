+++
title = "Mutations"
weight = 2
+++

## Using Mutations to modify data

The library supports all public mutations. We can make a mutation using the `createMutation` function. The function has 3 params. The first is the mutation. The below mutations are availble.

- BanUser
- CreateChatMessage
- DeleteChatMessage
- Follow
- LongTimeout
- ShortTimeout
- UnbanUser
- Unfollow
- UpdateStreamInfo

The second is the mutation params. These vary based on the mutation. Generally, all mutations have a target (channel, user, some form of ID) and an action. For example, updateStreamInfo requires a channelId and the updated title.

The last is the retVal. This is the same as all other return values in this library. A few examples are listed below.

Sending a message to a channel
```js
a.createMutation("CreateChatMessage", [{channelId: 6}, {message: "Hello World!"}])
```

Time a user out
```js
a.createMutation("ShortTimeout", [{channelId: 6}, {userId: 777}]);
```

Updating a title
```js
a.createMutation("UpdateStreamInfo", [{channelId: 6}, {title: "Doing something cool, watch me :)"}]);
```