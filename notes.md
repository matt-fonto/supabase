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

```js
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

### 7. E-mails templates

- To edit them, go to /authentication, on the sidebar, configuration (section)/emails/templates
- Templates: confirm signup, invite user, magic link, change email address, reset password
- We can config the subject heading and the message body
- Some cool templates: see `email-templates.md`

### 8. URL Configuration

- Starts with localhost:3000, but once we're in prod, it should be the base URL of the website

### 9. Read, insert or delete

```js
// read: select
const { data, error } = await supabase
  .from("courses")
  .select("*") // what will return from the query (like a serializer)
  .eq("user_id", user.id);

// insert
const { error } = await supabase
  .from("courses")
  .insert({ title: "Example", user_id: user.id });

// update
const { error } = await supabase
  .from("courses")
  .update({ title: "Example 2" })
  .eq("id", courseId)
  .eq("user_id", user.id);

// delete
const { error } = await supabase
  .from("courses")
  .delete()
  .eq("id", courseId)
  .eq("user_id", user.id);
```

- Also, check the API Docs/tables and views to get ready-made supabase queries based on the table and CRUD need

### 9.1 Filtering

```js
const { data, error } = await supabase
  .from("courses")
  .select("*")

  // filters
  .eq("column", "equal to")
  .gt("column", "greater than");
  .lt("column", "less than")
  .gte("column", "greater than or equal to")
  .lte("column", "less than or equal to")
  .like("column", "case sensitive")
  .ilike("column", "case insensitive")
  .is("column", null)
  .in("column", ["Array", "Values"])
  .neq("column", "not equal to")
```

### 10. Subscriptions

1. Enable realtime (must be enabled per table and event (insert, update, delete))

- Dashboard -> database -> replication -> enable realtime -> add table

2. Create subscription: `.channel()` on `.on()`:

```js
const channel = supabase
  .channel("realtime:courses")
  .on(
    "postgres_changes", //
    {
      event: "*",
      schema: "public",
      table: "courses",
      // we could add a filter here
    },
    (payload) => {
      // do something with the updated data
    }
  )
  .subscribe();

// Event shape
{
  eventType: 'INSERT' | 'UPDATE' | 'DELETE',
  new: {},    // new row (insert/update)
  old: {},    // old row (update/delete)
  table: 'courses'
}
```

### 11. Relations

[Supabase video](https://www.youtube.com/watch?v=WOX9g1s43-g)

1. Foreign Keys: defines relationships between tables

```sql
-- column_name <type> REFERENCES referenced_table(referenced_column)
user_id UUID REFERENCES users(id)
```

2. Types of relations

- One-to-one: user has one profile
- Both tables use the same primary key
- Foreign key is also PRIMARY KEY or UNIQUE

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT UNIQUE
);

CREATE TABLE profiles (
  -- one to one
  id UUID PRIMARY KEY REFERENCES users(id) ON DELETE CASCADE,
  username TEXT,
  avatar_url TEXT
)
```

- One-to-many: user has many posts
- Foreign key is not unique, allows many entries with same reference

```sql
CREATE TABLE posts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  -- not unique: one to many
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  title TEXT
);

```

- Many-to-many: posts have many comments
- A join table with two foreign keys and composite primary key

```sql
-- convention when creating a join table is table1name_table2name ()
CREATE TABLE user_courses (
  user_id UUID REFERENCES users(id),
  course_id UUID REFERENCES courses(id),
  -- composite primary key
  PRIMARY_KEY (user_id, course_id)
);
```

- Reading foreign tables

```js
const { data, error } = await supabase.from("orders").select(`
    some_column,
    other_table (
      foreign_key
    )
  `);
```

### 12. Functions

- Functions run when some change is done to the database
- Reusable block of SQL/logic
- We could, for instance, increase the count of "views" everytime a fetch request is done to "view" the order

```sql
CREATE FUNCTION add(a INT, b INT)
RETURN INT AS $$
BEGIN
  RETURN a + b;
END;

$$ LANGUAGE plpgsql;
```

- Increment function

```sql
CREATE FUNCTION increment(row_id INT)
RETURNS void as
$$
  UPDATE table_name
  SET field_name = field_name + 1
  WHERE id = row_id;
$$
```

- We can then call the function from within our app

```js
// supabase.rpc("function_name", {args})
const { data } = await supabase.rpc("increment", { row_id: "123" });
```

### 13. Triggers

- We can call our functions with triggers
- They call our functions on a given specific moment, e.g., `BEFORE INSERT`, `BEFORE UPDATE`, etc
- It can be "BEFORE" or "AFTER" a specific event

name | table | function | events

- One interesting example is, everytime we create a user with supabase auth.user, we could have a trigger invokes a function to create the user on our custom user table as well

```sql
CREATE FUNCTION handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO public.users (id, email)
  VALUES (NEW.id, NEW.email);
  RETURN NEW;
END;

CREATE TRIGGER on_auth_user_created
AFTER INSERT ON auth.users
FOR EACH ROW
EXECUTE FUNCTION handle_new_user();
```

### 14. Schemas

- Postgres uses schemas, which is as name container for database
- A namespace which contains one or more objects
- Helps to separate different parts of the applications

```sql
CREATE SCHEMA sales;

-- customers is a table inside the sales schema
CREATE TABLE sales.customers (
  id uuid not null primary key,
  name text,
  email text
);
```

### 15. Storage

- Where docs and images are stored
- Buckets:

  - top-level container for files
  - each file lives under a path in a specific bucket
  - can be public or private

- Objects:

  - files stored inside buckets

- Storage is also a schema

```js
// Upload
await supabase.storage.from("avatars").upload("user1/avatar.png", file);

// Download URL
const { data } = supabase.storage
  .from("avatars")
  .getPublicUrl("user1/avatar.png");

// Delete
await supabase.storage.from("avatars").remove(["user1/avatar.png"]);
```

- Creating RLS policies

```sql
-- Enable row-level security

ALTER TABLE storage.objects ENABLE ROW LEVEL SECURITY;

-- Policy for SELECT operations
CREATE POLICY objects_select_policy ON storage.objects FOR SELECT
  USING (auth.role() = "authenticated");

-- Policy for INSERT operations WITCH CHECK !
CREATE POLICY objects_insert_policy ON storage.objects FOR INSERT
  WITH CHECK (auth.role() = "authenticated");

-- Policy for UPDATE
CREATE POLICY objects_update_policy ON storage.objects FOR UPDATE
  USING (auth.role() = "authenticated");

-- Policy for DELETE
CREATE POLICY objects_delete_policy ON storage.objects FOR DELETE
  USING (auth.role() = "authenticated");
```

### Logs

### Extensions

### Realtime

### Edge-functions

### Webhooks

```

```
