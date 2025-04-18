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

### Schema

- It's a namespace inside your PostgreSQL that orgniazes db objects -- tables, views, functions
- Default schema: `public`, all tables go here unless specified otherwise
- Useful for modularization: separate user-facing data, admin tables, or external integrations
- Each schema has its own permissions and RLS policies

## Course

### 1. Tables

- A table has rows (records/entries) and columns (fields/properties)
- The model of what we want to create
- Add the name in snake_case
- Table structure:
  - primary keys: usually `id UUID`
  - foreigns keys: relationships to other tables
  - constraints: e.g., `NOT NULL`, `UNIQUE`
  - default values: e.g.`now()`, `gen_random_uuid()`

### 2. RLS (Row Level Security)

- By adding RLS to the table, the table won't be accessible until a policy is written to open it up
- We, as admin, need to specify who can read/write to that table
- To create a RLS policy, go to authentication > policy > select table > new policy > get started quickly | full customization

```sql
-- basic policy structure
CREATE POLICY "policy_name"
ON schema.table
-- the actions
-- FOR: {action}
FOR { SELECT | INSERT | UPDATE | DELETE }
-- TO: {role}: authenticated, anon, etc
TO {role}
-- USING: for SELECT, UPDATE, DELETE
USING (
  condition
)
-- WITH CHECK: only for INSERT, UPDATE
WITH CHECK (
  condition
);
```

```sql
-- enable read access to everyone

CREATE POLICY "policy_name"
-- on schema.table (schema:public, table:clients)
ON public.clients
-- SELECT = READ
FOR SELECT USING (
    true
);
```

```sql
-- Enable insert access for auth users only

CREATE POLICY "policy_name"
ON public.clients
FOR INSERT
-- to <role>
TO authenticated
WITH CHECK (true);
```

#### Most Common RLS policies

- Authenticated users read their own data

```sql
CREATE POLICY "Users can view their own data"
ON public.schema
FOR SELECT
-- TO: role
TO authenticated
-- auth.uid: the authenticated user's id (supabase auth)
-- user_id: column in table that stores which user "owns" that row
USING (auth.uid() = user_id);
```

- Authenticated users can update their own data

```sql
CREATE POLICY "Update own profile"
ON public.profiles
FOR UPDATE
TO authenticated
-- USING (authenticated users'id = the column of the user)
USING (auth.uid() = id)
WITH CHECK (auth.uid() = id);
```

- Users can insert data tied to their UID

```sql
CREATE POLICY "Insert own data"
ON public.messages
FOR INSERT
TO authenticated
USING (auth.uid() = user_id);
```

- Users can delete their own data

```sql
CREATE POLICY "Delete own messages"
ON public.messages
FOR DELETE
TO authenticated
USING (auth.uid() = user_id)
```

### 3. Authentication

- For adding the providers, you need to have the OAuth `CLIENT_ID` and `CLIENT_SECRET`
- You will need to add to the `createServerClient`, the public URL and the anon public key
  - You can find these on settings/API

### 4. API Docs

- One other very important path is **API Docs**
- There in you find:
  - how to connect to your project
  - the different api keys
  - how to manage a user
  - the different supabase commands to CRUD your different tables

### 5. User management

- Supabase manages a built-in `auth.users` table
- Each user has: id, email, created_at, user_metadata, app_metadata
- It's common to create a separate `public.profiles` table for app-specific user info. Then, you can extend the base user. Link it to `auth.users` using the same `id`

```sql
CREATE TABLE profiles (
  id UUID PRIMARY KEY REFERENCES auth.users(id),
  username TEXT,
  avatar_url TEXT
);
```

### 6. Recover password

````js
// forgot-password-form
try {
    // The url which will be included in the email. This URL needs to be configured in your redirect URLs in the Supabase dashboard at https://supabase.com/dashboard/project/_/auth/url-configuration
    const { error } = await supabase.auth.resetPasswordForEmail(email, {
      redirectTo: `${window.location.origin}/auth/update-password`,
    })
    if (error) throw error
    setSuccess(true)
  } catch (error: unknown) {
    setError(error instanceof Error ? error.message : 'An error occurred')
  } finally {
    setIsLoading(false)
}
```

### E-mails templates

- To edit them, go to /authentication, on the sidebar, configuration (section)/emails/templates

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
````
