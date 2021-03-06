[toc]

# Basics

## Start a new project

`npx create-next-app` or `yarn create-next-app`

`npx` is a node package runner to run node executable

## Upgrade Next.js version

1. `npm install react@latest react-dom@latest`

2. `npm audit`

3. Check the high vulnerabilities, then fix it

4. `npm audit fix`

5. keep doing step 2-4 till all of vulnerabilities fixed

Resource: https://nextjs.org/docs/upgrading

## Next.js project folder structure

<img src="Next.js.assets/Screen Shot 2022-03-18 at 12.55.44 PM.png" alt="Screen Shot 2022-03-18 at 12.55.44 PM" style="zoom:50%;" />

### Pages:

Next.js will create a router for files in `/pages` base on file name

`_app.js` is the entry of project, is a wrapper of our app, we may add global features here, i.e. footer

## CSS Module

`*.module.css`

Any file with surfix ".module.css" is a css module. each module have a scope base on file name. i.e. "Home.module.css" is scope to "home" component.

css module will not affect pages outside its scope. i.e. `.container` in one module will **not overwrite** `.container` in another one. This because each class will be given a unique name.

<img src="Next.js.assets/Screen Shot 2022-03-17 at 11.08.59 AM.png" alt="Screen Shot 2022-03-17 at 11.08.59 AM" style="zoom:50%;" />

all styles in `module.css` will be export as object. and be inported to the page.

```css
.container {
	color: red;
}
```

```react
import styles from "../styles/hello.module.css"
const hello = () => {
	return <div className={style.container}>Hello World</div>;
}
export default Hello
```

## Mobile-First Strategy

Whenever create any website and app, we should start sketching, prototyping and coding the smallest screen first

Reference: https://css-tricks.com/how-to-develop-and-test-a-mobile-first-design-in-2021/

## Device Width

```css
sm: min-width: 640px; //small device
md: min-width: 768px; // medium device
lg: min-width: 1024px; // large device
xl: min-width: 1280px; // extra large device
2xl: min-width: 1536px; // 2 x extra large device
```

## Head Component

`import Head from "next/head";`

```jsx
<Head>
  <title>Create Next App</title>
  <meta name="description" content="Generated by create next app" />
  <link rel="icon" href="/favicon.ico" />
</Head>
```

# Routing

<img src="Next.js.assets/Screen Shot 2022-03-18 at 12.58.57 PM.png" alt="Screen Shot 2022-03-18 at 12.58.57 PM" style="zoom:50%;" />

## Index Routes

`index.js`in folder `pages` will automatically become the default route `/`

i.e. pages/index.js -> `/`	pages/products/index.js -> `/products`

<img src="Next.js.assets/Screen Shot 2022-03-17 at 6.12.36 PM.png" alt="Screen Shot 2022-03-17 at 6.12.36 PM" style="zoom:50%;" />

## Nested Routes

Any nested files inside the `Pages` folder are essentially nested route

i.e. pages/products/phones.js -> `/products/phones`

## Dynamic Routes

To enable dynamic Routes, use brackets in the file name `[]`, whatever in the brackets becomes to a variable can be read by `{ useRouter }` from  `next/router`  through `query.id` attribute of router object created by `useRouter()`

i.e. pages/products/[id].js -> `/products/101`,`/products/102`......

```js
import { useRouter } from "next/router";

const Products = () => {
  const router = useRouter();
  return <div>Products id: {router.query.id}</div>;
};

export default Products;
```

Above code will show "Products id: 123" on page  when user access url `/products/123`

## Catch All Routes

Catch all routes under a specific url

Use `...` to forward all suitable routes to the file.

`localhost:3000/api/coffee/hello/world/blahblah` =>  `/api/coffee/[...whateveritis].js`

Note: router will first try figure out if a url is meet index/next/dynamic routes, if not, then try "catch all routes" rule

## Add route using Link component

<img src="Next.js.assets/Screen Shot 2022-03-18 at 12.59.55 PM.png" alt="Screen Shot 2022-03-18 at 12.59.55 PM" style="zoom:50%;" />

`<a>`will refresh the page, `<Link>`won't

```jsx
import Link from 'next/link';

export default () => {
  return (
    <div>
      <Link href='/'><a>Back to home</a></Link> 
    </div>
  );
}
```

Note: Remember to wrap the `<a>`, don't use `<Link>` only.

### Link props

`Link` accepts the following props:

- `href` - The path or URL to navigate to. This is the only required prop
- `as` - Optional decorator for the path that will be shown in the browser URL bar. Before Next.js 9.5.3 this was used for dynamic routes, check our [previous docs](https://nextjs.org/docs/tag/v9.5.2/api-reference/next/link#dynamic-routes) to see how it worked. Note: when this path differs from the one provided in `href` the previous `href`/`as` behavior is used as shown in the [previous docs](https://nextjs.org/docs/tag/v9.5.2/api-reference/next/link#dynamic-routes).
- [`passHref`](https://nextjs.org/docs/api-reference/next/link#if-the-child-is-a-custom-component-that-wraps-an-a-tag) - Forces `Link` to send the `href` property to its child. Defaults to `false`
- `prefetch` - Prefetch the page in the background. Defaults to `true`. Any `<Link />` that is in the viewport (initially or through scroll) will be preloaded. Prefetch can be disabled by passing `prefetch={false}`. When `prefetch` is set to `false`, prefetching will still occur on hover. Pages using [Static Generation](https://nextjs.org/docs/basic-features/data-fetching/get-static-props) will preload `JSON` files with the data for faster page transitions. Prefetching is only enabled in production.
- [`replace`](https://nextjs.org/docs/api-reference/next/link#replace-the-url-instead-of-push) - Replace the current `history` state instead of adding a new url into the stack. Defaults to `false`
- [`scroll`](https://nextjs.org/docs/api-reference/next/link#disable-scrolling-to-the-top-of-the-page) - Scroll to the top of the page after a navigation. Defaults to `true`
- [`shallow`](https://nextjs.org/docs/routing/shallow-routing) - Update the path of the current page without rerunning [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/get-static-props), [`getServerSideProps`](https://nextjs.org/docs/basic-features/data-fetching/get-server-side-props) or [`getInitialProps`](https://nextjs.org/docs/api-reference/data-fetching/get-initial-props). Defaults to `false`
- `locale` - The active locale is automatically prepended. `locale` allows for providing a different locale. When `false` `href` has to include the locale as the default behavior is disabled.

## router.events

You can listen to different events happening inside the Next.js Router. Here's a list of supported events:

- `routeChangeStart(url, { shallow })` - Fires when a route starts to change

- `routeChangeComplete(url, { shallow })` - Fires when a route changed completely

- `routeChangeError(err, url, { shallow })` - Fires when there's an error when changing routes, or a route load is cancelled

- `err.cancelled` - Indicates if the navigation was cancelled

- `beforeHistoryChange(url, { shallow })` - Fires before changing the browser's history

- `hashChangeStart(url, { shallow })` - Fires when the hash will change but not the page

- `hashChangeComplete(url, { shallow })` - Fires when the hash has changed but not the page

> **Note:** Here `url` is the URL shown in the browser, including the [`basePath`](https://nextjs.org/docs/api-reference/next.config.js/basepath).

example: setIsLoading to false when page has been redirected(logged in). (or user could keep click the login button)

```react
useEffect(() => {
    const handleComplete = () => {
      setIsLoading(false);
    };
    router.events.on("routeChangeComplete", handleComplete);
    router.events.on("routeChangeError", handleComplete);
    return () => {
      router.events.off("routeChangeComplete", handleComplete);
      router.events.off("routeChangeError", handleComplete);
    };
  }, [router.events]);
```

# Image Component

Next.js by default lazy load all of the images on the page. As user scroll down, Next.js will download the image that about to show.

# Document in Next.js

`_app.js` is only responsible for `<body>` . To handle whatever outside the body, like `<html>` and  `<head>`, we use `Document`

Create `_document.js` in `/pages`

Create a class `MyDocument extends Document` from 'next/document'

```js
import Document, { Html, Head, Main, NextScript } from "next/document";

class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const initialProps = await Document.getInitialProps(ctx);
    return { ...initialProps };
  }

  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

Note: `<Head>` Component here will be applied to all the pages in our app

`<Main>`Component responsible for the outest `div` in our app, add id on it: `<div id="__next">`

`<NextScript>`Component responsible for adding all of the scripts in our app

Develop Note: Each time when we changes `_document.js` we need to restart the server

## Apply fonts in Document

There are several ways you can setup fonts in Next.js.

1. You can choose to download those fonts, store them in your project and then apply those in your code.
2. You can copy links directly from Google Fonts, set them inside _document.js without having to store them locally in your project for example,[ check the docs](https://nextjs.org/docs/basic-features/font-optimization)

### For the first way:

Copy the font files into `/public/fonts/`, then we need to add `<link>` in `_document.js`:

#### _document.js

```jsx
<Head>
  <link
    rel="preload"
    href="/fonts/IBMPlexSans-Bold.ttf"
    as="font"
    crossOrigin="anonymous"
  />
  <link
    rel="preload"
    href="/fonts/IBMPlexSans-Regular.ttf"
    as="font"
    crossOrigin="anonymous"
  />
  <link
    rel="preload"
    href="/fonts/IBMPlexSans-SemiBold.ttf"
    as="font"
    crossOrigin="anonymous"
  />
 </Head>
```

<img src="Next.js.assets/Screen Shot 2022-03-18 at 1.00.57 PM.png" alt="Screen Shot 2022-03-18 at 1.00.57 PM" style="zoom:50%;" />

#### globals.css

```css
body {
	font-family: IMBPlexSans, //other fonts
}
@font-face {
  font-family: 'IBMPlexSans';
  font-style: normal;
  font-weight: 500;
  src: url(/fonts/IBMPlexSans-Regular.ttf) format('truetype');
}

@font-face {
  font-family: 'IBMPlexSans';
  font-style: normal;
  font-weight: 600;
  src: url(/fonts/IBMPlexSans-SemiBold.ttf) format('truetype');
}

@font-face {
  font-family: 'IBMPlexSans';
  font-style: normal;
  font-weight: 700;
  src: url(/fonts/IBMPlexSans-Bold.ttf) format('truetype');
}
```

### For the second way:

Copy the link from google fonts:

<img src="Next.js.assets/Screen Shot 2022-03-26 at 2.29.55 PM.png" alt="Screen Shot 2022-03-26 at 2.29.55 PM" style="zoom:50%;" />

#### _document.js

```react
import Document, { Html, Head, Main, NextScript } from "next/document";

class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const initialProps = await Document.getInitialProps(ctx);
    return { ...initialProps };
  }

  render() {
    return (
      <Html>
        <Head>
          <link rel="preconnect" href="https://fonts.googleapis.com" />
          <link
            rel="preconnect"
            href="https://fonts.gstatic.com"
            crossOrigin="true"
          />
          <link
            href="https://fonts.googleapis.com/css2?family=Roboto+Slab&display=swap"
            rel="stylesheet"
          />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

And apply the css rules in `globals.css`:

<img src="Next.js.assets/Screen Shot 2022-03-26 at 2.33.10 PM.png" alt="Screen Shot 2022-03-26 at 2.33.10 PM" style="zoom:50%;" />

# SEO and Rendering Techniques

<img src="Next.js.assets/Screen Shot 2022-03-18 at 11.59.35 AM.png" alt="Screen Shot 2022-03-18 at 11.59.35 AM" style="zoom:50%;" />

### Indexing

<img src="Next.js.assets/Screen Shot 2022-03-18 at 12.08.21 PM.png" alt="Screen Shot 2022-03-18 at 12.08.21 PM" style="zoom:50%;" />

### Analysis

<img src="Next.js.assets/Screen Shot 2022-03-18 at 1.01.55 PM.png" alt="Screen Shot 2022-03-18 at 1.01.55 PM" style="zoom:50%;" />

## Next.js Pre-rendering

<img src="Next.js.assets/Screen Shot 2022-03-18 at 12.18.59 PM.png" alt="Screen Shot 2022-03-18 at 12.18.59 PM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-18 at 1.02.36 PM.png" alt="Screen Shot 2022-03-18 at 1.02.36 PM" style="zoom:50%;" />

The key difference is, Next.js app can show user the content  before JS loads. 

That means bots can read the content of the page much quicker.

# Rendering Techniques in Next.js

<img src="Next.js.assets/Screen Shot 2022-03-18 at 2.20.31 PM.png" alt="Screen Shot 2022-03-18 at 2.20.31 PM" style="zoom:50%;" />

## Static Generation (SSG)

<img src="Next.js.assets/Screen Shot 2022-03-18 at 1.19.01 PM.png" alt="Screen Shot 2022-03-18 at 1.19.01 PM" style="zoom:50%;" />

When user access our website, no new file generated.

All pages without external data will be defaultly pre-rendered.

Pages with external data will fetch the data first(inside `getStaticProps()` function) then pre-rendered

### getStaticProps

<img src="Next.js.assets/Screen Shot 2022-03-19 at 11.56.36 AM.png" alt="Screen Shot 2022-03-19 at 11.56.36 AM" style="zoom:50%;" />

Everything inside `getStaticProps()` will be server side code. i.e. `console.log()` will be shown in server terminal

<img src="Next.js.assets/Screen Shot 2022-03-19 at 11.58.04 AM.png" alt="Screen Shot 2022-03-19 at 11.58.04 AM" style="zoom:40%;" />

```react
export async function getStaticProps(context) {
  //fetch data here
  const data = fetch('blahblah')
  return {
    props: {
      data
    }, // will be passed to the page component as props
  };
}

export default function Home(props) {
	console.log(props.data)
  //-----------
}
```

### getStaticPahts

<img src="Next.js.assets/Screen Shot 2022-03-19 at 12.57.55 PM.png" alt="Screen Shot 2022-03-19 at 12.57.55 PM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-19 at 12.58.58 PM.png" alt="Screen Shot 2022-03-19 at 12.58.58 PM" style="zoom:40%;" />

```react
export const getStaticProps = async (staticProps) => {
  const params = staticProps.params;
  return {
    props: {
      coffeeStore: coffeeStoresData.find((coffeeStore) => {
        return coffeeStore.id.toString() === params.id;
      }),
    },
  };
};

export const getStaticPaths = async () => {
  return {
    paths: [{ params: { id: "0" } }, { params: { id: "1" } }],
    fallback: false,
  };
};

const CoffeeStore = (props) => {
  const router = useRouter();
  return (
    <div>
      CoffeeStore {router.query.id}
      <Link href="/">
        <a>Back to home</a>
      </Link>
      <p>{props.coffeeStore.id}</p>
      <p>{props.coffeeStore.name}</p>
    </div>
  );
};
```

### fallback: false

<img src="Next.js.assets/Screen Shot 2022-03-19 at 1.28.26 PM.png" alt="Screen Shot 2022-03-19 at 1.28.26 PM" style="zoom:60%;" />

Note: `getStaticPaths()` tells server the "static path" to be pre-rendered

`fallback: false` will redirect any routes doesn't exist in `getStaticPaths()` to 404 page.

### fallback: true

<img src="Next.js.assets/Screen Shot 2022-03-19 at 1.41.36 PM.png" alt="Screen Shot 2022-03-19 at 1.41.36 PM" style="zoom:50%;" />

When user access a route does not in `getStaticPaths()`,  server will treat this route not "static pre-rendered" and try to download the page to the client. 

In case above, server give error because it needs time to access the data, however meanwhile the render is already begin rendering the page and failed to get `props.coffeeStore.id`

To prevent error above, we need to add a loading function by`{ useRouter }` from  `next/router`

<img src="Next.js.assets/Screen Shot 2022-03-19 at 1.51.20 PM.png" alt="Screen Shot 2022-03-19 at 1.51.20 PM" style="zoom:50%;" />

```react
const CoffeeStore = (props) => {
  const router = useRouter();

  //router.isFallback tells us if the newly download data is ready or not
  if (router.isFallback) {
    return <h1>Loading...</h1>;
  }

  return (
    <div>
      CoffeeStore {router.query.id}
      <Link href="/">
        <a>Back to home</a>
      </Link>
      <p>{props.coffeeStore.id}</p>
      <p>{props.coffeeStore.name}</p>
    </div>
  );
};
```

## Incremental Static Regeration (ISR)

<img src="Next.js.assets/Screen Shot 2022-03-18 at 2.21.45 PM.png" alt="Screen Shot 2022-03-18 at 2.21.45 PM" style="zoom:50%;" />

We set an interval. i.e. 60s. When user first request to our page, we only serve the cached one(v1) no matter how many times user send new requests. Untill 60s passed, server will generate the latest page when user request.

<img src="Next.js.assets/Screen Shot 2022-03-30 at 7.24.15 PM.png" alt="Screen Shot 2022-03-30 at 7.24.15 PM" style="zoom:50%;" />

### Fallback

<img src="Next.js.assets/Screen Shot 2022-03-30 at 7.30.43 PM.png" alt="Screen Shot 2022-03-30 at 7.30.43 PM" style="zoom:50%;" />

If the data fetching of our server is very quick, we can use `blocking` as fallback. If it is slow, we use `true` as fallback to show user a loading state.

## Server-side rendering (SSR)

Applying this when we need to provide user the latest data timely. i.e. news(we want user see the latest news every time). We won't be able to cache data on CDN. We also need to generate page for each request. These make the process slower on server side, but makes client side faster than CSR

### Server-side rendering (SSR) vs Static Generation(SSG)

<img src="Next.js.assets/Screen Shot 2022-03-27 at 4.57.29 PM.png" alt="Screen Shot 2022-03-27 at 4.57.29 PM" style="zoom:50%;" />

### [getServerSideProps](https://nextjs.org/docs/api-reference/data-fetching/get-server-side-props)

```js
export async function getServerSideProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```

### Context parameter

The `context` parameter is an object containing the following keys:

- `params`: If this page uses a [dynamic route](https://nextjs.org/docs/routing/dynamic-routes), `params` contains the route parameters. If the page name is `[id].js` , then `params` will look like `{ id: ... }`.
- `req`: [The `HTTP` IncomingMessage object](https://nodejs.org/api/http.html#http_class_http_incomingmessage).
- `res`: [The `HTTP` response object](https://nodejs.org/api/http.html#http_class_http_serverresponse).
- `query`: An object representing the query string.
- `preview`: `preview` is `true` if the page is in the [Preview Mode](https://nextjs.org/docs/advanced-features/preview-mode) and `false` otherwise.
- `previewData`: The [preview](https://nextjs.org/docs/advanced-features/preview-mode) data set by `setPreviewData`.
- `resolvedUrl`: A normalized version of the request `URL` that strips the `_next/data` prefix for client transitions and includes original query values.
- `locale` contains the active locale (if enabled).
- `locales` contains all supported locales (if enabled).
- `defaultLocale` contains the configured default locale (if enabled).

Note: cookies can be get from `req`

[getServerSideProps return values](https://nextjs.org/docs/api-reference/data-fetching/get-server-side-props#getserversideprops-return-values):

1. notFound
2. redirect
3. props

#### /pages/example.js

```react
export const getServerSideProps = async (context) => {
  const disneyVideos = await fetchVideos();
  const data = "some data"
  const userId = "someone"
  
  //return notFound
  if (!data) {
    return {
      notFound: true,
    }
  }
  
  //return redirect
  //The permanent flag as true would be a 301 permanent redirect whereas the false option would be a 302 temporary redirect.
  if(!userId) {
    return {
      props: {},
      redirect: {
        destination: '/login',
        permanent: false
      }
    }
  }

  //return props
  return {
    props: {
      disneyVideos,
    },
  };
};

export default function Example(props) {
	console.log(props.disneyVideos)
}
```

## Client-side rendering (CSR)

<img src="Next.js.assets/Screen Shot 2022-03-18 at 2.39.19 PM.png" alt="Screen Shot 2022-03-18 at 2.39.19 PM" style="zoom:50%;" />

i.e. personal dashboard. administration software

Using React `useEffect` hooks

`SWR` is a React hook built by Next.js. Very useful for client side fetching

# Comparison of CSR, SSR, SSG and ISR

https://dev.to/pahanperera/visual-explanation-and-comparison-of-csr-ssr-ssg-and-isr-34ea

# SWR

**stale-while-revalidate**

`npm install swr`

If your page contains frequently updating data, and you don???t need to pre-render the data, SWR is a perfect fit and no special setup needed: just import `useSWR` and use the hook inside any components that use the data.

Here???s how it works:

- First, immediately show the page without data (or cached data with SWR). You can show loading states for missing data.
- Then, fetch the data on the client side and display it when ready.

This approach works well for user dashboard pages, for example. Because a dashboard is a private, user-specific page, SEO is not relevant and the page doesn???t need to be pre-rendered. The data is frequently updated, which requires request-time data fetching.

```react
const fetcher = (url) => axios.get(url).then((res) => res.data);
// or use default fetch:
// const fetcher = (url) => fetch(url).then((res) => res.json());
  const { data, error } = useSWR(`/api/getCoffeeStoreById?id=${id}`, fetcher);//we can add a third param as initial data
  useEffect(() => {
    if (data && data.length > 0) {
      console.log("data from SWR", data);
      setCoffeeStore(data[0]);
    }
  }, [data]);
  if (error) <div>Something went wrong retrieving coffee store page: {error}</div>;
```

SWR will polling request to the server after a few seconds

# Environment Variables in Next.js

## Client side environment variables

To add environment variables to the JavaScript bundle, open `next.config.js` and add the `env` config:

```js
module.exports = {
  env: {
    customKey: 'my-value',
  },
}
```

Now you can access `process.env.customKey` in your code. For example:

```jsx
function Page() {
  return <h1>The value of customKey is: {process.env.customKey}</h1>
}

export default Page
```

Next.js will replace `process.env.customKey` with `'my-value'` at build time. Trying to destructure `process.env` variables won't work due to the nature of webpack [DefinePlugin](https://webpack.js.org/plugins/define-plugin/).

For example, the following line:

```jsx
return <h1>The value of customKey is: {process.env.customKey}</h1>
```

Will end up being:

```jsx
return <h1>The value of customKey is: {'my-value'}</h1>
```

## Server side environment variables

Next.js has built-in support for loading environment variables from `.env.local` into `process.env`.

An example `.env.local`:

```bash
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

This loads `process.env.DB_HOST`, `process.env.DB_USER`, and `process.env.DB_PASS` into the Node.js environment automatically allowing you to use them in [Next.js data fetching methods](https://nextjs.org/docs/basic-features/data-fetching/overview) and [API routes](https://nextjs.org/docs/api-routes/introduction).

For example, using [`getStaticProps`](https://nextjs.org/docs/basic-features/data-fetching/get-static-props):

```js
// pages/index.js
export async function getStaticProps() {
  const db = await myDB.connect({
    host: process.env.DB_HOST,
    username: process.env.DB_USER,
    password: process.env.DB_PASS,
  })
  // ...
}
```

### Exposing Environment Variables to the Browser

By default environment variables are only available in the Node.js environment, meaning they won't be exposed to the browser.

In order to expose a variable to the browser you have to prefix the variable with `NEXT_PUBLIC_`. For example:

```bash
NEXT_PUBLIC_ANALYTICS_ID=abcdefghijk
```

This loads `process.env.NEXT_PUBLIC_ANALYTICS_ID` into the Node.js environment automatically, allowing you to use it anywhere in your code. The value will be inlined into JavaScript sent to the browser because of the `NEXT_PUBLIC_` prefix. This inlining occurs at build time, so your various `NEXT_PUBLIC_` envs need to be set when the project is built.

```js
// pages/index.js
import setupAnalyticsService from '../lib/my-analytics-service'

// NEXT_PUBLIC_ANALYTICS_ID can be used here as it's prefixed by NEXT_PUBLIC_
setupAnalyticsService(process.env.NEXT_PUBLIC_ANALYTICS_ID)

function HomePage() {
  return <h1>Hello World</h1>
}

export default HomePage
```

# React useContext with useReducer

React Context provide a scope that states can be shared by all components.

`useReducer` Helps when we need to handle multiple state.

#### _app.js

```react
import { useReducer, createContext } from "react";
import "../styles/globals.css";

export const StoreContext = createContext();

export const ACTION_TYPES = {
  SET_LAT_LONG: "SET_LAT_LONG",
  SET_COFFEE_STORES: "SET_COFFEE_STORES",
};

const storeReducer = (state, action) => {
  switch (action.type) {
    case ACTION_TYPES.SET_LAT_LONG: {
      return { ...state, latLong: action.payload.latLong };
    }
    case ACTION_TYPES.SET_COFFEE_STORES: {
      return { ...state, coffeeStores: action.payload.coffeeStores };
    }
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
};

const StoreProvider = ({ children }) => {
  const initialState = {
    latLong: "",
    coffeeStores: [],
  };

  const [state, dispatch] = useReducer(storeReducer, initialState);

  return (
    <StoreContext.Provider value={{ state, dispatch }}>
      {children}
    </StoreContext.Provider>
  );
};

function MyApp({ Component, pageProps }) {
  return (
    <StoreProvider>
      <Component {...pageProps} />;
    </StoreProvider>
  );
}

export default MyApp;
```

To update state:

```js
import { useContext } from "react";
import { ACTION_TYPES, StoreContext } from "../pages/_app";
const { dispatch } = useContext(StoreContext);
//-----------
dispatch({
  type: ACTION_TYPES.SET_LAT_LONG,
  payload: {
    latLong: `${latitude},${longitude}`,
  },
});
```

To get state:

```js
import { useContext } from "react";
import { ACTION_TYPES, StoreContext } from "./_app";
//destructure state
//const { state } = useContext(StoreContext);
//const { coffeeStores, latLong } = state;
//or
//destructure state(nested way)
const {
  state: {
    coffeeStores,
    latLong
  }
} = useContext(StoreContext)
```

# Serverless Function (API)

<img src="Next.js.assets/Screen Shot 2022-03-22 at 9.48.55 AM.png" alt="Screen Shot 2022-03-22 at 9.48.55 AM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-22 at 9.49.19 AM.png" alt="Screen Shot 2022-03-22 at 9.49.19 AM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-22 at 9.49.50 AM.png" alt="Screen Shot 2022-03-22 at 9.49.50 AM" style="zoom:50%;" />

Serverless: no need to maintain server infrastructure

<img src="Next.js.assets/Screen Shot 2022-03-22 at 9.55.33 AM.png" alt="Screen Shot 2022-03-22 at 9.55.33 AM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-22 at 10.19.56 AM.png" alt="Screen Shot 2022-03-22 at 10.19.56 AM" style="zoom:50%;" />

API routes provide a solution to build your **API** with Next.js.

Any file inside the folder `pages/api` is mapped to `/api/*` and will be treated as an API endpoint instead of a `page`. They are server-side only bundles and won't increase your client-side bundle size.

For example, the following API route `pages/api/user.js` returns a `json` response with a status code of `200`:

```js
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' })
}
```

## Request

API routes provide built in middlewares which parse the incoming request (`req`). Those middlewares are:

- `req.cookies` - An object containing the cookies sent by the request. Defaults to `{}`
- `req.query` - An object containing the [query string](https://en.wikipedia.org/wiki/Query_string). Defaults to `{}`
- `req.body` - An object containing the body parsed by `content-type`, or `null` if no body was sent

## Response

The [Server Response object](https://nodejs.org/api/http.html#http_class_http_serverresponse), (often abbreviated as `res`) includes a set of Express.js-like helper methods to improve the developer experience and increase the speed of creating new API endpoints.

The included helpers are:

- `res.status(code)` - A function to set the status code. `code` must be a valid [HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
- `res.json(body)` - Sends a JSON response. `body` must be a [serializable object](https://developer.mozilla.org/en-US/docs/Glossary/Serialization)
- `res.send(body)` - Sends the HTTP response. `body` can be a `string`, an `object` or a `Buffer`
- `res.redirect([status,] path)` - Redirects to a specified path or URL. `status` must be a valid [HTTP status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). If not specified, `status` defaults to "307" "Temporary redirect".
- `res.unstable_revalidate(urlPath)` - [Revalidate a page on demand](https://nextjs.org/docs/basic-features/data-fetching/incremental-static-regeneration#on-demand-revalidation-beta) using `getStaticProps`. `urlPath` must be a `string`.

#### /pages/api/getCoffeeStoresByLocation

```js
import fetchCoffeeStores from "../../lib/coffee-store";

const getCoffeeStoresByLocation = async (req, res) => {
  try {
    const { latLong, limit } = req.query;
    const response = await fetchCoffeeStores(latLong, 30);
    res.status(200);
    res.json(response);
  } catch (error) {
    console.error("There is an error:", error);
    res.status(500);
    res.json({ message: "Something went wrong", error });
  }
};

export default getCoffeeStoresByLocation;
```

Then the api can be called by `/pages`code:

#### /pages/index.js

```react
const fetchedCoffeeStores = await fetch(
    `/api/getCoffeeStoresByLocation?latLong=${latLong}&limit=30`
);
```

Note: fetch API route from `getStaticProps` is **NOT recommanded**, we should directly write server-side code in `getStaticProps`

# Middleware

Note: Middleware is a new feature of v12

<img src="Next.js.assets/Screen Shot 2022-03-28 at 10.04.44 PM.png" alt="Screen Shot 2022-03-28 at 10.04.44 PM" style="zoom:50%;" />

## [Execution Order](https://nextjs.org/docs/middleware#execution-order)

If your Middleware is created in `/pages/_middleware.ts`, it will run on all routes within the `/pages` directory. The below example assumes you have `about.tsx` and `teams.tsx` routes.

```bash
- package.json
- /pages
    _middleware.ts # Will run on all routes under /pages
    index.tsx
    about.tsx
    teams.tsx
```

If you *do* have sub-directories with nested routes, Middleware will run from the top down. For example, if you have `/pages/about/_middleware.ts` and `/pages/about/team/_middleware.ts`, `/about` will run first and then `/about/team`. The below example shows how this works with a nested routing structure.

```bash
- package.json
- /pages
    index.tsx
    - /about
      _middleware.ts # Will run first
      about.tsx
      - /teams
        _middleware.ts # Will run second
        teams.tsx
```

Middleware runs directly after `redirects` and `headers`, before the first filesystem lookup. This excludes `/_next` files.

#### pages/_middleware.js

```js
import { NextResponse } from "next/server";
import { verifyToken } from "../lib/utils";

export async function middleware(req, ev) {
  const token = req.cookies.token;
  const userId = await verifyToken(token);
  if (
    // if auth or static/login path
    userId ||
    req.nextUrl.pathname.includes("/login") ||
    req.nextUrl.pathname.includes("/static")
  ) {
    return NextResponse.next();
  } else {
    // if unauth, redirect to login
    return NextResponse.redirect(`${req.nextUrl.origin}/login`);
  }
}
```



# Lint

#### package.json

```json
"scripts": {
  "lint": "next lint"
}
```

Next.js will automatically install `eslint` and `eslint-config-next` as development dependencies in your application and create an `.eslintrc.json` file in the root of your project that includes your selected configuration.

`npm run lint`

More eslint plugin : https://nextjs.org/docs/basic-features/eslint

# Deployment

<img src="Next.js.assets/Screen Shot 2022-03-25 at 11.47.17 AM.png" alt="Screen Shot 2022-03-25 at 11.47.17 AM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-22 at 9.55.33 AM.png" alt="Screen Shot 2022-03-22 at 9.55.33 AM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-25 at 12.00.30 PM.png" alt="Screen Shot 2022-03-25 at 12.00.30 PM" style="zoom:50%;" />

`npm run build`

<img src="Next.js.assets/Screen Shot 2022-03-25 at 3.03.12 PM.png" alt="Screen Shot 2022-03-25 at 3.03.12 PM" style="zoom:50%;" />

`npm run start` to start server

## Cloud Ready Application

<img src="Next.js.assets/Screen Shot 2022-03-25 at 7.52.10 PM.png" alt="Screen Shot 2022-03-25 at 7.52.10 PM" style="zoom:50%;" />

### Deploy on Vercel

Vercel created the next.js framework, so it compated very will with vercel deployment

<img src="Next.js.assets/Screen Shot 2022-03-25 at 8.25.22 PM.png" alt="Screen Shot 2022-03-25 at 8.25.22 PM" style="zoom:50%;" />

Add environment variables from `.env.local`file:

<img src="Next.js.assets/Screen Shot 2022-03-25 at 8.30.46 PM.png" alt="Screen Shot 2022-03-25 at 8.30.46 PM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-25 at 8.33.47 PM.png" alt="Screen Shot 2022-03-25 at 8.33.47 PM" style="zoom:50%;" />

<img src="Next.js.assets/Screen Shot 2022-03-25 at 8.35.23 PM.png" alt="Screen Shot 2022-03-25 at 8.35.23 PM" style="zoom:50%;" />

All of the Serverless functions logs can be checked here:

<img src="Next.js.assets/Screen Shot 2022-03-25 at 8.54.03 PM.png" alt="Screen Shot 2022-03-25 at 8.54.03 PM" style="zoom:50%;" />

Note: The Realtime Logs cannot persist, or we have to use an external service. (`set up a Log Drain` in the image above)

Vercel uses Vercel's Edge Network as CDN. Vercel will automatically push app to all of the Global Edge Network.

Serverless Functions by default runs in iad1 server.

| iad1 | Washington, D.C., USA | AWS us-east-1 |
| ---- | --------------------- | ------------- |
|      |                       |               |

To change Serverless Functions server, need to go Pro plan

<img src="Next.js.assets/Screen Shot 2022-03-25 at 9.14.08 PM.png" alt="Screen Shot 2022-03-25 at 9.14.08 PM" style="zoom:50%;" />

### Deploy on Netlify

<img src="Next.js.assets/Screen Shot 2022-03-25 at 10.09.26 PM.png" alt="Screen Shot 2022-03-25 at 10.09.26 PM" style="zoom:50%;" />

Netlify use next.js plugin to makes it work.

## Lighthouse Performance

Use Chrome lighhouse tool or https://web.dev to get a lighthouse report

<img src="Next.js.assets/Screen Shot 2022-03-25 at 10.36.58 PM.png" alt="Screen Shot 2022-03-25 at 10.36.58 PM" style="zoom:50%;" />

try to fix it one by one till all green









# TODOs

TODO: add code for middleware
