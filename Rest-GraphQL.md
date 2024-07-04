- [Rest](#rest)
  - [Problem](#problem)
- [GraphQL](#graphql)
  - [What is GraphQL](#what-is-graphql)
  - [GraphQL client request](#graphql-client-request)
  - [Schema](#schema)
  - [Resolver](#resolver)
  - [Query Variable](#query-variable)
  - [Related Data](#related-data-like-how-to-connect-different-table)
  - [GraphQL servers](#graphql-servers)
    - [Apollo](#apollo)
    - [Yoga](#yoga)
  - [Flow](#flow)
- [Compare with Rest](#compare-with-rest)

# Rest

## Problem

- Receiving the data than it needs

# GraphQL

## What is `GraphQL`

- `Query Language`
- Server-side runtime
- Client can receive `exactly` the data it needs
- Just have `one end-point`

## GraphQL client request

A client sends a GraphQL query or mutation request to the GraphQL Yoga server.

## Schema

- Acts as the `contract` between the `client` and `server` / `frontend` and `backend`
- Define all operation that API can do, i.e what field will be returned
- Every single schema must have query

---

1. Define `type`

```gql
# define type first
export const typeDefs = '#graphql
  type Game{
    id: ID!, # ! = not allow to be 'null'
    title: String!,
    # [] = array | String! = string can be 'null' | []! = array can be 'null'
    platform: [String!]!
  }

  type Review{
    id: ID!
    rating: Int!
    content: String!
  }

  type Author{
    id: ID!
    name: String!
    verified: Boolean!
  }
```

2. Define Query (the entry point), what data should be return for them

```gql
# define query
type Query {
  reviews: [Review]
  games: [Game]
  authors: [Author]
}
```

## Resolver

- How we response to the graph

```gql
const resolvers={
  # from schema Query
  Query:{
    # game is  a function
    games(){
      # return some data
      return
    }
  }
}
```

## Query variable

- In order to get one return instead of whole object.

1. In `Type Query`, set it to return one result only

```gql
type Query {
  reviews: [Review]
  # id = data field (pass in 'id') , ID = data type ID
  review(id: ID!): Review # To get one single 'review'
  games: [Game]
  authors: [Author]
}
```

2. In `resolver`, set `review()` and pass into 3 argus - `(parent, args, context)`

- parent:
- args:
- context:

> **Caution:** If you don't need one of them, you can use `_` to skip it. `(_, args, context)`

```gql
# for finding one review only by passing id.
# db = database , reviews = the object (whole table of reviews) , find = if equal to args, then return.
review(_,args){
  return db.reviews.find((review)=> review.id === args.id)
}
```

## Related Data (like how to connect different table)

- relationship between table
- To connect table (define relationship)
- Apollo can know how to create a nested Json

1. update `type` on schema

```gql
export const typeDefs = '#graphql
  type Game{
    id: ID!,
    title: String!,
    platform: [String!]!
    review: [Review!]! # relation with `review`(1 to many)
  }

  type Review{
    id: ID!
    rating: Int!
    content: String!
    game: Game! # relation with `game` (1 to 1)
    author: Author! # relation with `author` (1 to 1)
  }

  type Author{
    id: ID!
    name: String!
    verified: Boolean!
    review: [Review!]!
  }
```

2. update `resolver`

```gql
const resolvers={
  # from schema Query
  Query:{
    # game() is a function
    games(){
      # return some data
      return db.games
    }
    game(_,args){
      return db.games.find((game)=>game.id===args.id)
    }
  },
  # to create nested, create a object of the table
  # Apollo will get the single game from game(_,args) first
  # parent = game object ie. game.id
  # = game(_,args){
  #    return db.games.find((game)=>game.id===args.id)
  #  }
  Game: {
    reviews(parent){
      return db.review.filter((r) => r.game.id === parent.id)

    }
  }
  Review: {
    author(parent){
      return db.authors.find((a)=> a.id === parent.author_id)
    }
    game(parent){
      return db.games.find((g)=> g.id === parent.game_id)
    }
  }
}
```

## Mutation (add, delete, edit)

- Any kind of action we want to do for the data.

1. Add `type Mutation` on `schema`

```gql
type Mutation {
  deleteGame(id: ID!): [Game]
  addGame(game: AddGameInput!): Game # see below caution: group args
}
```

2. add `mutation` on `resolver`

```gql
const resolvers={

  Query:{

    games(){
      return db.games
    }
    game(_,args){
      return db.games.find((game)=>game.id===args.id)
    }
  },

  Game: {
    reviews(parent){
      return db.review.filter((r) => r.game.id === parent.id)
    }
  }
  Review: {
    author(parent){
      return db.authors.find((a)=> a.id === parent.author_id)
    }
    game(parent){
      return db.games.find((g)=> g.id === parent.game_id)
    }
  }
  # this part added
  Mutation:{
    deleteGame(_,args){
      return db.games.filter((g)=>g.id!==args.id)
    }
    addGame(_,args){
      let game={
        ...args.game,
        # don't use the same method to get random id
        id: Math.floor(Math.random()*10000).toString()
      }
      db.games.push(game)
      return game
    }
  }
}
```

> **Caution**: To add a group of argument like `addGame(id,name,title)`. We can use `input AddGameInput{}` example below:
>
> > ```gql
> > input AddGameInput {
> >   title: String!
> >   platform: [String!]!
> > }
> > ```
>
> ---

## Connection for each file

- type (schema) -> query (schema) -> resolver

## GraphQL servers

### Apollo

### Yoga

## Flow

# Compare with Rest

✅ Solved over-fetching or under-fetching
