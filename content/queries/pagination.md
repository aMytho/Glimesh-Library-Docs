+++
title = "Pagination"
weight = 3
+++

## Requesting Lots of Data

Some queries return a large amount of information. To avoid long load times and application freezes, Glimesh limits the amount of data you receive in some requests. Pagination allows us to move through all of the data that Glimesh returns. While we can't get it all in one query, we can still access the data is an efficient way.

> I strongly suggest you read the Glimesh [Pagination](https://glimesh.github.io/api-docs/docs/api/pagination/) document. It has more info thatn this article. 

The first thing you need to know is whic queries return paginated data. Generally speaking, any query that returns more than 1 item from a single query is a paginated one. For example, the `users` query is paginated while the `user` query is not. There are some exceptions. However, the best way to know is to use the documentation. [Voyager](https://glimesh.github.io/api-docs/docs/api/voyager/) will shows you all the info that you need. Scroll to the bottom of the page and uncheck the "Skip Relay" box. You will then see the paginated queries.

Paginated queries have access to 4 special paramaters which are divided into 2 groups. I call the first group Pagination Order and the second Pagination Location. The order group has the paramaters `first` and `last`. Both of them require a number to represent how many items you want returned. The second group has the paramaters `before` and `after`. This specifies the location from which to search. If that doesn't make sense, *please* read the documentation linked above on pagination! My docs are not meant to replace the Glimesh docs. I would know, seeing as I wrote both :)

```js
a.createQuery("Users", [{last: 100}, null])
```

While those paramaters are needed for large queries, they can be omitted if you want a large amount of data. It will have a limit but you don't always need to specify the amount or location. You can only have one paramter from each group. **Ex: You can't have both a first and last paramater.**

> In the library, the pagination paramaters are always after any other paramaters. If a paramater has an optional param outside of the pagianted ones a default value must be placed in the first paramater. As always, see the intellisense for help.

The second thing is accessing the data in the query. You will need to specify the retVal if you want custom data. The default library is set up to access paginated data but if you want different data returned you need to use the retVal. The default convention is `edges {node {DATA HERE}}`. For example, if you were moving through a user query you may use `edges {node {id, username}}` to return the user ID and the username. Paginated queries are returned as arrays.

As before, if the above retVal doesn't make sense, look at the documentation linked above!