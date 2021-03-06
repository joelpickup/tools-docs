---
title: Adding a GraphQL endpoint
order: 202
description: How to set up Apollo Server
---

Apollo Server exports the `apolloExpress`, `apolloConnect`, `ApolloHAPI` and `apolloKoa` functions which can be used to add a GraphQL HTTP endpoint to your Express, Connect, HAPI or Koa 2 server.

<h2 id="apolloOptions">Apollo Server options</h2>

Apollo Server accepts an ApolloOptions object as its single argument. An ApolloOptions object has the following properties:

```js
// options object
const ApolloOptions = {
  schema: GraphQLSchema,

  // values to be used as context and rootValue in resolvers
  context?: any,
  rootValue?: any,

  // function used to format errors before returning them to clients
  formatError?: Function,

  // additional validation rules to be applied to client-specified queries
  validationRules?: Array<ValidationRule>,

  // function applied for each query in a batch to format parameters before passing them to `runQuery`
  formatParams?: Function,

  // function applied to each response before returning data to clients
  formatResponse?: Function,
})
```

Alternatively, Apollo Server can accept a function which takes the request as input and returns an ApolloOptions object or a promise that resolves to one:

```js
// example options function (for express)
apolloExpress(request => ({
  schema: typeDefinitionArray,
  context: { user: request.session.user }
}))
```

<h2 id="apolloExpress">Using with Express</h2>

The following code snippet shows how to use Apollo Server with Express:

```js
import express from 'express';
import bodyParser from 'body-parser';
import { apolloExpress } from 'apollo-server';

const PORT = 3000;

var app = express();

app.use('/graphql', bodyParser.json(), apolloExpress({ schema: myGraphQLSchema }));

app.listen(PORT);
```

<h2 id="apolloConnect">Using with Connect</h2>

The following code snippet shows how to use Apollo Server with Connect:

```js
import connect from 'express';
import bodyParser from 'body-parser';
import { apolloConnect } from 'apollo-server';

const PORT = 3000;

var app = connect();

app.use('/graphql', bodyParser.json(), apolloConnect({ schema: myGraphQLSchema }));

app.listen(PORT);
```

The `options` passed to `apolloConnect` are the same as those passed to `apolloExpress`.


<h2 id="apolloHAPI">Using with HAPI</h2>

The following code snippet shows how to use Apollo Server with HAPI:

```js
import Hapi from 'hapi';
import { ApolloHAPI } from 'apollo-server';

const server = new Hapi.Server();

const HOST = 'localhost';
const PORT = 3000;

server.connection({
    host: HOST,
    port: PORT,
});

server.register({
    register: new ApolloHAPI(),
    options: { schema: myGraphQLSchema },
    // or options: request => ({ schema, ... })
    routes: { prefix: '/graphql' },
});
```


<h2 id="apolloKoa">Using with Koa 2</h2>

The following code snippet shows how to use Apollo Server with Koa:

```js
import koa from 'koa';
import koaRouter from 'koa-router';
import koaBody from 'koa-bodyparser';
import { apolloKoa } from 'apollo-server';

const app = new koa();
const router = new koaRouter();
const PORT = 3000;

app.use(koaBody());

router.post('/graphql', apolloKoa({ schema: myGraphQLSchema }));
app.use(router.routes());
app.use(router.allowedMethods());
app.listen(PORT);
```
