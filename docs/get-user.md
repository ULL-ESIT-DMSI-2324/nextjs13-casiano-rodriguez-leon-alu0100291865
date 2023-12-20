# The User Object

The user object stores all the information related to a user in your application. The user object can be retrieved using one of these methods:

[supabase.auth.getUser()](https://supabase.com/docs/reference/javascript/auth-getuser)

```ts
const { data: { user } } = await supabase.auth.getUser()
```

Retrieve a user object as an admin using [supabase.auth.admin.getUserById()](https://supabase.com/docs/reference/javascript/auth-admin-getuserbyid)

```ts
const { data, error } = await supabase.auth.admin.getUserById(1)
```

A user can sign in with one of the following methods:

- Password-based method (with email or phone)
- Passwordless method (with email or phone)
- OAuth
- SAML SSO

An identity describes the authentication method that a user can use to sign in. A user can have multiple identities. These are the types of identities supported:

- Email
- Phone
- OAuth
- SAML

> A user with an email or phone identity will be able to sign in with either a password or passwordless method (e.g. use a one-time password (**OTP**) or **magiclink**). By default, a user with an unverified email or phone number will not be able to sign in.

The user object contains the following attributes:

<table>
<thead><tr><th>Attributes</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td><code class="short-inline-codeblock">string</code></td><td>The unique id of the identity of the user.</td></tr><tr><td>aud</td><td><code class="short-inline-codeblock">string</code></td><td>The audience claim.</td></tr><tr><td>role</td><td><code class="short-inline-codeblock">string</code></td><td>The role claim used by Postgres to perform Role Level Security (RLS) checks.</td></tr><tr><td>email</td><td><code class="short-inline-codeblock">string</code></td><td>The user's email address.</td></tr><tr><td>email_confirmed_at</td><td><code class="short-inline-codeblock">string</code></td><td>The timestamp that the user's email was confirmed. If null, it means that the user's email is not confirmed.</td></tr><tr><td>phone</td><td><code class="short-inline-codeblock">string</code></td><td>The user's phone number.</td></tr><tr><td>phone_confirmed_at</td><td><code class="short-inline-codeblock">string</code></td><td>The timestamp that the user's phone was confirmed. If null, it means that the user's phone is not confirmed.</td></tr><tr><td>confirmed_at</td><td><code class="short-inline-codeblock">string</code></td><td>The timestamp that either the user's email or phone was confirmed. If null, it means that the user does not have a confirmed email address and phone number.</td></tr><tr><td>last_sign_in_at</td><td><code class="short-inline-codeblock">string</code></td><td>The timestamp that the user last signed in.</td></tr><tr><td>app_metadata</td><td><code class="short-inline-codeblock">object</code></td><td>The <code class="short-inline-codeblock">provider</code> attribute indicates the first provider that the user used to sign up with. The <code class="short-inline-codeblock">providers</code> attribute indicates the list of providers that the user can use to login with.</td></tr><tr><td>user_metadata</td><td><code class="short-inline-codeblock">object</code></td><td>Defaults to the first provider's identity data but can contain additional custom user metadata if specified. Refer to <a href="/docs/guides/auth/auth-identity-linking#the-user-identity"><strong>User Identity</strong></a> for more information about the identity object.</td></tr><tr><td>identities</td><td><code class="short-inline-codeblock">UserIdentity[]</code></td><td>Contains an object array of identities linked to the user.</td></tr><tr><td>created_at</td><td><code class="short-inline-codeblock">string</code></td><td>The timestamp that the user was created.</td></tr><tr><td>updated_at</td><td><code class="short-inline-codeblock">string</code></td><td>The timestamp that the user was last updated.</td></tr></tbody>
</table>



## Configuration

Supabase Auth provides these configuration options to control user access to your application:

- **Allow new users to sign up**: Users will be able to sign up. If this config is disabled, only existing users can sign in.

- **Allow unverified email sign in**: Users will not need to have a verified email address to sign in.

  If enabled, you can structure your RLS policies to provide different access controls to a user with an unverified email and a user with a verified email. For example,

    ```sql
    -- Allows a user to read all posts regardless of whether they have a verified email address
    create policy "policy_name"
    ON public.posts
    for select to authenticated using (
        true
    );

    -- Only allow users with a verified email address to insert posts
    create policy "policy_name"
    ON public.posts
    for insert to authenticated with check (
        auth.jwt()->>'email_verified' is true
    );
    ```
