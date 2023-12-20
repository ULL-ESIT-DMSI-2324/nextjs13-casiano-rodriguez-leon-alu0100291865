# getSession()

The `getSession()` function is used to retrieve a session or authentication session that binds a verified user identity to a web browser. 
A **session** is usually long-lived and can be terminated by the user logging out. An access and refresh token pair represent a session in the browser, and they are stored in local storage or as cookies.

```js
const { data, error } = await supabase.auth.getSession()
```

Returns the [session](https://supabase.com/docs/guides/auth/sessions), refreshing it if necessary.

The session returned can be `null` if the session is not detected which can happen in the event a user is not signed-in or has logged out.

This method retrieves the current local session (i.e local storage).

If the session has an expired access token, this method will use the refresh token to get a new session.

