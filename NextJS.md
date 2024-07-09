# NextJs

## What is Nextjs

- A `Framework` for building fast & search-engine friendly applications
- lib + tools + conventions
- file-system
- SSR (Server-side Rendering )
  - ✅Smaller bundles
  - ✅Resource efficient
  - ✅SEO
  - ✅More secure
  - ✅Every components will be `Server-side` when it created

## Routing

- File-system

## Fetching Data

❌ `useState()` + `useEffect()`

## Data Caching

```typescript
// using 'cache: 'no-store'' to not save any caching in local
const res = await fetch('<API Link>', { cache: 'no-store' });
```

```typescript
// using 'next: { revalidate: 10 }' to fetch the data every 10 secs.
const res = await fetch('<API Link>', { next: { revalidate: 10 } });
```

## Rendering

Two types: 1. Client-side 2. Server-side

- Server-side will have 2 types:

### Static

- `Render at build time`
- If pages or components are needed, they will not re-render again

### Dynamic

-`Render at request time`

## Authentication

- Using `session` to store user credentials if auth
  > **`Session Management`**
  >
  > 1. Cookie-based
  > 2. Database
- `next-auth`

## Middleware

- Control who can access different parts of website
  - specially for 3 parts:
    > 1. require to protect route and role navigation
    > 2. pubic route
    > 3. unauthenticated user
- Logic to authorize user access `by checking token`
- Use **`Matcher`** to specify which route do not require authorization

### files set up

1. auth.ts
2. .env.local

```typescript
const options = {
...
callbacks: {
async signIn({ account, profile }) {
if (account.provider === "google") {
return profile.email_verified && profile.email.endsWith("@example.com")
}
return true // Do different verification for other providers that don't have `email_verified`
},
}
...
}
```

For production: https://{YOUR_DOMAIN}/api/auth/callback/google
For development: http://localhost:3000/api/auth/callback/google
