## References

- <https://nextjs.org/docs/app/building-your-application/routing/route-handlers>

## Route handlers

Route Handlers allow you to create custom request handlers for a given route using the Web Request and Response APIs.

![](https://nextjs.org/_next/image?url=%2Fdocs%2Flight%2Froute-special-file.png&w=1920&q=75&dpl=dpl_G8PQHiE8wn1mU6ZGgW4V6pQKJWbh)

Route Handlers are only available inside the `app` directory. They are the equivalent of API Routes inside the `pages` directory meaning you **do not need to use API Routes and Route Handlers together**.

Route Handlers are defined in a `route.js|ts` file inside the `app` directory:

```js
export const dynamic = 'force-dynamic' // defaults to auto
export async function GET(request) {}
```

There cannot be a `route.js` file at the same route segment level as `page.js`.

If an unsupported method is called, Next.js will return a `405 Method Not Allowed response`.

The `dynamic` property is set to `force-dynamic`. This tells Next.js to always generate a dynamic page for this route, regardless of whether it matches a statically generated route. This is useful for routes that depend on dynamic data, such as those that fetch data from a backend API.

The default behavior for dynamic routes in Next.js is to use **auto-dynamic** mode. In this mode, Next.js will first attempt to find a statically generated page that matches the route. If no matching statically generated page is found, then Next.js will generate a dynamic page on demand.

Using `force-dynamic` mode can be helpful for routes that always need to be dynamic, even if there is a matching statically generated page. This can be useful for routes that depend on frequently updated data, such as live chat or news feeds.

Here is a table summarizing the difference between `auto-dynamic` and `force-dynamic` modes:

| Mode            | Description |
|---              |    ---      |
| `auto-dynamic`  | Next.js will generate a dynamic page only if there is no matching statically generated page. |
| `force-dynamic` | Next.js will always generate a dynamic page for this route, regardless of whether there is a matching statically generated page. |

Overall, using `force-dynamic` mode can be useful for routes that require dynamic data and that should always be generated dynamically. This can be helpful for ensuring that users always see the most up-to-date data.