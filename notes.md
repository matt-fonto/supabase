# Supabase

## Main concepts

### PostgresSQL Core

- Built on PostgresSQL
- We can use SQL directly via Supabase dashboard or programmatically

### Auth

- User authentication via email, password, OAuth (Google, Github, etc)
- Supports row-level-security (RLS), so access is controlled via policies
- Built-in session management with JWT

### Database + RLS Policies

- Tables, views, functions: all standard PostgresSQL
- RLS lets you define who can read/write each row

```sql
CREATE POLICY "Users can view their own data"
ON profiles FOR SELECT
USING (auth.uid() = id);
```

### Edge functions (Deno)

- Serverless functions deployed close to the user
- Built with Deno, they can handle custom business logic or external API calls

### Storage

- Store and serve files (image, videos, etc)
- Files are stored in buckets and access is controlled via policies

### Realtime

- Subscribe to changes in tables (INSERT, UPDATE, DELETE) using websockets
- Useful for building collaborative or dynamic apps (chat, dashboard, etc)

### Client SDK

- Auto-generated TypeScript client for accessing database, auth, storage, and more
- Work with supabase like a JS/TS API:

```js
const { data, error } = await supabase
  .from("messages")
  .select("*")
  .eq("user_id", user.id);
```

### Auto-generated APIs

- Supabase creates REST and GraphQL APIs automatically for database tables

## Course

### Tables

- A table has rows and columns
- The model of what we want to create
- Add the name in snake_case
- By adding RLS, the table won't be accessible until a policy is written to open it up

### RLS (Row Level Security)

### Authentication

### User management

### Recover password

### E-mails templates

### URL Configuration

### Read, insert or delete

### Subscriptions

### Relations

### Functions

### Triggers

### Schemas

### Storage

### Logs

### Extensions

### Realtime

### Edge-functions

### Webhooks
