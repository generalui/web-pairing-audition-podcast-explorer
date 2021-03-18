# GraphQL

This guide will demonstrate the following GraphQL examples:

- Queries
  - Search the iTunes Podcast Directory
  - Display details for the currently authenticated user
  - Display all subscribed podcasts for the currently authenticated user
- Mutations
  - Create a new user account
  - Log in with a user account
  - Subscribe to a podcast for the currently authenticated user
  - Unsubscribe to a podcast for the currently authenticated user

## Queries

### Search the iTunes Podcast Directory

```gql
# Example query in GraphIQL
query searchiTunesPodcasts($searchTerm: String!) {
  podcastSearch(searchTerm: $searchTerm) {
    title
    releaseDate
    feedURL
    iTunesId
    categories
  }
}
```

Query Variables (located underneath the query window in GraphIQL)

```json
{
  "searchTerm": "Graph database"
}
```

![../app/__screenshots__/graphiql-example-query-search-itunes-podcasts-01.png](../app/__screenshots__/graphiql-example-query-search-itunes-podcasts-01.png)

This should result in a response like:

```json
{
  "data": {
    "podcastSearch": [
      {
        "title": "Graphistania: Podcast for Neo4j Graph Database community",
        "releaseDate": "2021-01-11T00:00:00.000Z",
        "feedURL": "http://feeds.soundcloud.com/users/soundcloud:users:141739624/sounds.rss",
        "iTunesId": 975377379,
        "categories": ["Technology", "Podcasts"]
      }
    ]
  }
}
```

![../app/__screenshots__/graphiql-example-query-search-itunes-podcasts-02.png](../app/__screenshots__/graphiql-example-query-search-itunes-podcasts-02.png)

### Display details for the currently authenticated user

This query uses the JWT from the currently authenticated user to retrieve user details.

```gql
query currentUser() {
  me {
    id
    username
    password
  }
}
```

HTTP Headers (located underneath the query window in GraphIQL)

```json
{
  "Authorization": "Bearer <JWT_TOKEN>"
}
```

This should result in a response like:

```json
{
  "data": {
    "me": {
      "id": "4ed433aa-7572-4afa-afb7-52d704a10913",
      "username": "rob",
      "password": "$2b$10$X8ndq9WIZrN38ZxMhb/E5OPBllNgFdNfuSef1FSXbqNdOezaxSmCS"
    }
  }
}
```

### Display all subscribed podcasts for the currently authenticated user

This query uses the JWT from the currently authenticated user to retrieve podcasts that the user has subscribed to.

```gql
query myPodcasts() {
  subscribedPodcasts{
    title
    iTunesId
    releaseDate
    feedURL
    categories
    createdBy
    updatedBy
  }
}
```

HTTP Headers (located underneath the query window in GraphIQL)

```json
{
  "Authorization": "Bearer <JWT_TOKEN>"
}
```

This should result in a response like:

```json
{
  "data": {
    "subscribedPodcasts": [
      {
        "title": "Accidental Tech Podcast",
        "iTunesId": "617416468",
        "releaseDate": "2021-02-04T16:24:00.000Z",
        "feedURL": "https://atp.fm/episodes?format=rss",
        "categories": ["Technology", "Podcasts"],
        "createdBy": "4ed433aa-7572-4afa-afb7-52d704a10913",
        "updatedBy": null
      },
      {
        "title": "Graphistania: Podcast for Neo4j Graph Database community",
        "iTunesId": "975377379",
        "releaseDate": "2021-01-11T00:00:00.000Z",
        "feedURL": "http://feeds.soundcloud.com/users/soundcloud:users:141739624/sounds.rss",
        "categories": ["Technology", "Podcasts"],
        "createdBy": "4ed433aa-7572-4afa-afb7-52d704a10913",
        "updatedBy": null
      }
    ]
  }
}
```

## Mutations

### Create a new user account

```gql
mutation($username: String!, $password: String!) {
  signup(username: $username, password: $password) {
    token
  }
}
```

Query Variables (located underneath the query window in GraphIQL)

```json
{
  "username": "justauser",
  "password": "letmein"
}
```

This should result in a response like:

```json
{
  "data": {
    "signup": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjliN2I3MDBhLWE5MTktNDI5Ni05YzhlLTJmMzRmNjE5N2Q1MyIsInVzZXIiOiJqdXN0YXVzZXIiLCJpYXQiOjE2MTI3MjYwMTEsImV4cCI6MTYxMjczMzIxMX0.WR4oCqEFtmDU9LWaHlQVCaJQwFpcJxKdgSzgJqzMl4Q"
    }
  }
}
```

### Log in with a user account

```gql
mutation($username: String!, $password: String!) {
  login(username: $username, password: $password) {
    token
  }
}
```

Query Variables (located underneath the query window in GraphIQL)

```json
{
  "username": "justauser",
  "password": "letmein"
}
```

This should result in a response like:

```json
{
  "data": {
    "login": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjliN2I3MDBhLWE5MTktNDI5Ni05YzhlLTJmMzRmNjE5N2Q1MyIsInVzZXJuYW1lIjoianVzdGF1c2VyIiwiaWF0IjoxNjEyNzI2MjgwLCJleHAiOjE2MTI3MzM0ODB9.1ywQtpweNP3zBjkbPzRwRuJrj6JA5K_mhMURwveZ4wk"
    }
  }
}
```

### Subscribe to a podcast for the currently authenticated user

This mutation uses the JWT from the currently authenticated user to subscribe to a podcast in our database.

```gql
mutation($iTunesId: String!) {
  subscribeToPodcast(iTunesId: $iTunesId) {
    iTunesId
    title
    releaseDate
    feedURL
    categories
  }
}
```

Query Variables (located underneath the query window in GraphIQL)

```json
{
  "iTunesId": "617416468"
}
```

HTTP Headers (located underneath the query window in GraphIQL)

```json
{
  "Authorization": "Bearer <JWT_TOKEN>"
}
```

This should result in a response like:

```json
{
  "data": {
    "subscribeToPodcast": {
      "iTunesId": "975377379",
      "title": "Graphistania: Podcast for Neo4j Graph Database community",
      "releaseDate": "2021-01-11T00:00:00.000Z",
      "feedURL": "http://feeds.soundcloud.com/users/soundcloud:users:141739624/sounds.rss",
      "categories": ["Technology", "Podcasts"]
    }
  }
}
```

### Unsubscribe to a podcast for the currently authenticated user

This mutation uses the JWT from the currently authenticated user to unsubscribe to a podcast in our database.

```gql
mutation($iTunesId: String!) {
  unsubscribeToPodcast(iTunesId: $iTunesId)
}
```

Query Variables (located underneath the query window in GraphIQL)

```json
{
  "iTunesId": "617416468"
}
```

HTTP Headers (located underneath the query window in GraphIQL)

```json
{
  "Authorization": "Bearer <JWT_TOKEN>"
}
```

This should result in a response like:

```json
{
  "data": {
    "unsubscribeToPodcast": "SUCCESS: You are no longer subscribed to Accidental Tech Podcast"
  }
}
```
