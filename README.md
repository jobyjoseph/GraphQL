# GraphQL
GraphQL is a new API standard which is more efficient, powerful and flexible alternative to REST. GraphQL enables __declarative data fetching__. Instead of fixed endpoints for API, GraphQL enables single endpoint and returns just the data client asks for.

## Why GraphQL over REST?
- Increased mobile usage creates need for efficient data loading
- When different frameworks and applications access a single GraphQL endpoint, it can fetch data based on requirement
- Fast development by avoiding the API versioning step

## Making GraphQL request
In case of GraphQL request, a POST request is send from client to GraphQL server. In the GraphQL response, the root key of response object will be `data`

## Core Concepts
### Schema Definition Language(SDL)
GraphQL has its own SDL that is used to define schema of API. 

#### Simple SDL Type
Here is an example how we can use the SDL to define a simple type called `Person`:
```
type Person {
  name: String!
  age: Int!
}
```
This type has two fields, they’re called `name` and `age` and are respectively of type `String` and `Int`. The `!` following the type means that this field is required.

#### Relationship between types
It’s also possible to express relationships between types.
```
type Post {
  title: String!
  author: Person!
}
```

### Mutations
_Mutations_ is used for making changes to the data that is currently stored in database. There generally are three kinds of mutations:
- creating new data
- updating existing data
- deleting existing data
Mutations follow the same syntactical structure as queries, but they always need to start with the `mutation` keyword. Here’s an example for how we might create a new `Person`:
```
mutation {
  createPerson(name: "Joby", age: 39) {
    name
    age
  }
}
```

### Subscriptions
Subscriptions are used to establish realtime connection to the server in order to get immediately informed about important events. When a client subscribes to an event, it will initiate and hold a steady connection to the server. Whenever that particular event then actually happens, the server pushes the corresponding data to the client. Unlike queries and mutations that follow a typical _request-response-cycle_, subscriptions represent a stream of data sent over to the client.

Example:
```
subscription {
  newPerson {
    name
    age
  }
}
```


## GraphQL Query Samples
### Basic Query
Let’s take a look at an example query that a client could send to a server:
```
{
  allPersons {
    name
  }
}
```
The `allPersons` field in this query is called the _root field_ of the query. Everything that follows the root field, is called the _payload_ of the query. The only field that’s specified in this query’s payload is `name`.
### Combine two tables
Here we take `name` and `age` from `Person` table and corresponding posts from `Post` table.
```
{
  allPersons {
    name
    age
    posts {
      title
    }
  }
}
```
### Get last items using attributes
Get last 2 names from `Person` table:
```
{
  allPersons(last: 2) {
    name
  }
}
```

Here is a list of SDLs we have discussed so far:
```
type Query {
  allPersons(last: Int): [Person!]!
}

type Mutation {
  createPerson(name: String!, age: Int!): Person!
}

type Subscription {
  newPerson: Person!
}

type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}

type Post {
  title: String!
  author: Person!
}
```

## Resolver Functions
Resolver functions fetch the data for its field.
