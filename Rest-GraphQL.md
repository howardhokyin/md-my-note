- [Rest](#rest)
  - [Problem](#problem)
- [GraphQL](#graphql)
  - [What is GraphQL](#what-is-graphql)
  - [GraphQL client request](#graphql-client-request)
  - [Schema](#schema)
  - [Resolver](#resolver)
  - [Query Variable](#query-variable)
  - [Related Data](#related-data-like-how-to-connect-different-table)
  - [Mutation](#mutation-add-delete-edit)
  - [GraphQL servers](#graphql-servers)
    - [Apollo](#apollo)
    - [Yoga](#yoga)
  - [Flow](#flow)
- [Compare with Rest](#compare-with-rest)
- [Reference](#reference)

# Rest

## Problem

- Lead to `over-fetching` or `under-fetching` of data.

# GraphQL

## What is `GraphQL`

- `Query Language`
- Server-side runtime
- Client can receive `exactly` the data it needs
- Just have `one end-point`

## GraphQL client request

A client sends a GraphQL query or mutation request to the GraphQL Yoga server.

- **Query**: Used to request data.
- **Mutation**: Used to modify data.

## Schema

- Acts as the `contract` between the `client` and `server` / `frontend` and `backend`
- Define all operation that API can do, i.e what field will be returned
- Every single schema must have query
- Doesn't use URL to specify resources (❌GET/books/123)
- Define relationship on schema

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

- Resolvers define `how to fetch the data` for each field in your schema.

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

- Applying data modification to resources such as add, delete, update
- Any kind of action we want to do for the data.

- Here is example for `add`, `delete` and `update`

1. Add `type Mutation` on `schema`

```gql
type Mutation {
  deleteGame(id: ID!): [Game]
  addGame(game: AddGameInput!): Game # see below caution: group args
  updateGame(id: ID!, edits: EditGameInput!): Game
}
```

> **Caution**: To add a group of argument like `addGame(id,title,platform)`. We can use `input AddGameInput{}` example below:
>
> > ```gql
> > input AddGameInput {
> >   title: String!
> >   platform: [String!]!
> > }
> > ```
>
> ---

> **Caution**: To edit a group of argument like `editGame(id,title,platform)`. We can use `input EditGameInput{}` example below:
>
> > ```gql
> > # not using AddGameInput since it will have to update two data
> > input EditGameInput {
> >   title: String
> >   platform: [String!]!
> > }
> > ```
>
> ---

2. Add `mutation` on `resolver`

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
  # this part added >>>>>>>>>
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
    updateGame(_,args){
      # mapping to create a new array of games
      db.games = db.games.map((g)=>{
        if (g.id === args.id){
          return {...g, ...args.edits}
        }
        return g
      })

      return db.games.find((g)=> g.id === args.id)
    }
  }

  # this part added <<<<<<
}
```

## Subscription

- `Client side` receive notification on data modification

## Connection for each file

1.  `Client` sends query or mutation to `server`
2.  -> type (schema) -> query (schema) -> resolver

## GraphQL servers

### Apollo

### Yoga

## Flow

# Compare with Rest

- Same
  | Feature | REST | GraphQL |
  | ------------------ | :---------------: | ---------------: |
  | HTTP Usage | Yes | Yes |
  | Request Format | URL-based | URL-based |
  | Response Format | JSON | JSON |

- Different
  | Feature | REST | GraphQL |
  | :------------------- | :----------------: | :--------------: |
  | Data Fetching | Multiple Endpoints | Single Endpoint |
  | Data Over-fetching | Yes | No |
  | Data Under-fetching | Yes | No |
  | Versioning | Required | Not Needed |
  | Schema | None | Defined |
  | Caching | Built-in | Requires Custom |
  | | ||

- Comparison
  | | REST | GraphQL |
  | ------------------- | :----------------: | --------------: |
  |✅ | | Single req|
  |✅ |Cache | |
  |❌ |If 1+n table, multiple req from client side | |
  |❌ |Over-fetching and Under-fetching | |

# Reference

- [YouTube: GraphQL Course for Beginners [
  freeCodeCamp.org]](https://www.youtube.com/watch?v=5199E50O7SI)
- [Youtube: What Is GraphQL? REST vs. GraphQL [ByteByteGo]
  ](https://www.youtube.com/watch?v=yWzKJPw_VzM)
- [Article: What’s the Difference Between GraphQL and REST?](https://aws.amazon.com/compare/the-difference-between-graphql-and-rest/)
