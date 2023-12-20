# createRouteHandlerClient

This is the `app/api/auth/callback/route.js` route handler in charge of exchanging the code for a session token:

```js
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import { NextResponse } from 'next/server'

export async function GET(request) {
  const url = new URL(request.url)
  const code = url.searchParams.get('code')

  if (code) {
    const supabase = createRouteHandlerClient({ cookies })
    await supabase.auth.exchangeCodeForSession(code)
  }

  return NextResponse.redirect(url.origin)
}
```

The `createRouteHandlerClient` function is a helper function provided by the Supabase JS SDK that simplifies the process of creating a Supabase client for use in a server-side Next.js application. This function is specifically designed for routing-based authentication scenarios, **where the Supabase client is created using cookies that are passed from the client-side to the server-side**.

Here's a breakdown of how `createRouteHandlerClient` works:

1. **Parses cookies:** The function first parses the `cookies` object provided to it. This object typically contains cookies set by the client-side application using the `next/headers` API.

2. **Extracts access token:** The function extracts the access token from the parsed cookies. The access token is a unique identifier that is used to authenticate users with Supabase.

3. **Creates Supabase client:** Using the extracted access token, the function creates a Supabase client instance. This `supabase` client will be used to interact with Supabase on behalf of the authenticated user.

The `createRouteHandlerClient` function simplifies the process of creating a Supabase client for server-side authentication in Next.js, as it handles 
- the parsing of cookies and 
- extraction of the access token. 

This makes it easier to focus on the core authentication logic without having to manage the underlying cookie handling.


This asynchronous call 

```js
    await supabase.auth.exchangeCodeForSession(code)
```

exchanges the `code` for a session token with Supabase. This is part of the OAuth flow where the temporary code is exchanged for a **token** that can be used to authenticate the user.

After the code is exchanged for a **session token**, the user is redirected back to the  dashboard:

```js
  return NextResponse.redirect(url.origin)
```


