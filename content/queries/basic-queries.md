+++
title = "Basic Queries"
weight = 2
+++

## Making a Query

Our library supports all queries. You can make a query with the `createQuery` function. The most basic queries don't require any paramaters. They also don't require a retVal. We will make one now.

```js
connection.createQuery("Categories", []).then(data => {
    console.log(data)
})
```

This query requests all categories that are on Glimesh. This query doesn't require any paramaters. But what if you need to use a paramater? That is where the second argument comes in!

All paramaters are passed in as an array of objects. Each object has a single key and a single value. For example, if I was making a user query I would want to specify the user I am requesting. Using the documentation, I can see that I can use a userId or a username. I will opt for the latter since that is easy to remember.

```js
connection.createQuery("User", [{username: "Mytho"}]).then(data => {
    console.log(data)
})
```

This will return data about myself. Cool! But how do you know which paramaters to add? Or what about which queries require paramaters? Intellisense can help with some of that. As you type, you should see suggestions. Based on the query selected the intellisense knows which paramaters should exist. It will display them. However, they don't use the exact format that we used. Instead of `{username: "mytho"}`, you should see `ID or UserbyUsername`. This is because intellisense can't display exact values. However, we can use this documentation to look up what the values are supposed to be. We can also look at the types included in the npm modules. By doing so, we can see the structure required.

```ts
/**
 * Select a user by their username
 */
export interface UserByUsername {
    username: number;
    userId?: never;
}
```
>Your result may slightly vary as this part is still being developed. However, the documentation will always have up to date code.

We can see that it has a username property and it will never have a userId. This is simply to prevent someone from trying to enter both a username and a id. You only have to pay attention to the required property.

So now you know how to add paramaters. But what about knowing which queries require them? While intellisense will help here, the best action is to simply use the Glimesh documentation (specifically [Voyager](https://glimesh.github.io/api-docs/docs/api/voyager/)). Voyager is a great way to view all the data you can request and know what it needs to aquire said data.

There is one more thing to cover. Currently, we are only getting specific data from our query. In a real environment we would be manually entering the data we want. Our library supports this in the form of `retVal`.

Simply add the data you want in valid GraphQL format as a string. Note that you are **only** entering the query part. You don't need external brackets (and adding them would cause the query to fail!).

```js
connection.createQuery("User", [{username: "Mytho"}], "id, avatarUrl, socialDiscord, channel {status}").then(data => {
    console.log(data);
});
```
This query requests the user Mytho. It returns the userId, avatarUrl, Discord URL, and status of the channel (is the user streaming?). You can request any data using the retVal. 