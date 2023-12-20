## Reference

See <https://supabase.com/docs/reference/javascript/auth-signup>

## supabase.auth.signUp

Creates a new user.

- By default, the user needs to verify their email address before logging in. To turn this off, **disable Confirm email** in [your project](https://supabase.com/dashboard/project/_/auth/providers).
- **Confirm email** determines if users need to confirm their email address after signing up.
  - If **Confirm email is enabled**, a `user` is returned but session is `null`.
  - If **Confirm email is disabled**, both a `user` and a `session` are returned.
- When the user confirms their email address, they are redirected to the `[SITE_URL](https://supabase.com/docs/reference/auth/config#site_url)` by default. You can modify your `SITE_URL` or [add additional redirect URLs in your project](https://supabase.com/dashboard/project/_/auth/url-configuration).
- If `signUp()` is called for an **existing confirmed user**:
  - If **Confirm email is enabled** in your project, an obfuscated/fake user object is returned.
  - If **Confirm email is disabled**, the error message, `User already registered` is returned.
To fetch the currently logged-in user, refer to [`getUser()`](https://supabase.com/docs/reference/javascript/auth-getuser).

```js

const { data, error } = await supabase.auth.signUp({
  email: 'example@email.com',
  password: 'example-password',
})
```