# supabase.auth.getSession

The `supabase.auth.getSession` method is used to retrieve the current authentication session for the user. The session object contains information about the current user, such as 
- their ID, 
- name, and 
- email address. 
- It also contains the user's access **token**, which is used to authenticate subsequent requests to Supabase.

Here's a breakdown of how `supabase.auth.getSession` works:

1. **Checks for access token:** The function first checks if there is an access token present in the cookies. If there is no access token, then the function returns `null`.

2. **Verifies access token:** If an access token is found, the function verifies its validity by sending a request to Supabase's authentication API. The request includes the access token and the Supabase URL.

3. **Returns session object:** If the access token is valid, the function retrieves the corresponding session object from Supabase. The session object is then returned.

The `supabase.auth.getSession` method is useful for checking whether the user is currently authenticated and for retrieving information about the current user. It can be used to implement features such as user authentication, user management, and authorization.

Here's an example of how to use the `supabase.auth.getSession` method in a Next.js application:

```jsx
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import { NextResponse } from 'next/server'

export async function GET(request) {
  const url = new URL(request.url)
  const code = url.searchParams.get('code')
  const supabase = createRouteHandlerClient({ cookies })
  const session = await supabase.auth.getSession()
  if (session) {
    console.log('Current user:', session.user)
  }

  return NextResponse.redirect(url.origin)
}
```

## Example of session object

Here we have tricked the `app/(dashboard)/layout.jsx` component to show the session object in the console. 

```jsx
import { createServerComponentClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import util from 'util'

// components
import Navbar from '@/app/components/Navbar'

export default async function DashboardLayout({ children }) {
  const supabase = createServerComponentClient({ cookies })
  // learning
  let session = await supabase.auth.getSession();
  console.log(util.inspect(session, { depth: null }));

  const { data } = await supabase.auth.getSession()

  return (
    <>
      <Navbar user={data?.session?.user} />
      {children}
    </>
  )
}
```

This is the session object that is returned by `supabase.auth.getSession`:
```js
{
  data: {
    session: {
      expires_at: 1703086315,
      expires_in: 3598,
      token_type: 'bearer',
      access_token: 'eyJhbGc...cEX2DAbVQ',
      refresh_token: 'Na3egR-v2s4cigWbZsJj2Q',
      provider_token: null,
      provider_refresh_token: null,
      user: {
        id: '8297301b-2893-414a-b572-74dd70999f50',
        factors: null,
        aud: 'authenticated',
        iat: 1703082715,
        iss: 'https://bdtgljrnoacxfkctszfz.supabase.co/auth/v1',
        email: 'alu0100291865@ull.edu.es',
        phone: '',
        app_metadata: { provider: 'email', providers: [ 'email' ] },
        user_metadata: {},
        role: 'authenticated',
        aal: 'aal1',
        amr: [ { method: 'email/signup', timestamp: 1703082715 } ],
        session_id: '1a3c50c1-0289-4705-b6a2-7418c88d0bea'
      }
    }
  },
  error: null
}
```