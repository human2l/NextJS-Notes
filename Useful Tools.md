## Glassmorphism

https://glassmorphism.com

## Classnames (npm)

https://www.npmjs.com/package/classnames

A simple JavaScript utility for conditionally joining classNames together.

```react
var classNames = require('classnames');

class Button extends React.Component {
  // ...
  render () {
    var btnClass = classNames({
      btn: true,
      'btn-pressed': this.state.isPressed,
      'btn-over': !this.state.isPressed && this.state.isHovered
    });
    return <button className={btnClass}>{this.props.label}</button>;
  }
}
```

## Next.js Code Elimination

https://next-code-elimination.vercel.app/

<img src="Useful Tools.assets/Screen Shot 2022-03-19 at 12.10.43 PM.png" alt="Screen Shot 2022-03-19 at 12.10.43 PM" style="zoom:50%;" />

## Icons

https://fonts.google.com/icons (material icons)

## Foursquare Place API

<img src="Useful Tools.assets/Screen Shot 2022-03-20 at 1.44.20 PM.png" alt="Screen Shot 2022-03-20 at 1.44.20 PM" style="zoom:30%;" />

https://foursquare.com/products/places-api/

Search places: https://developer.foursquare.com/reference/place-search

## Unsplash (free photos api)

https://unsplash.com/developers

Unsplash JS SDK: https://github.com/unsplash/unsplash-js

## Airtable

Works like firebase + google sheet

Bases -> HELP -> API document

<img src="Useful Tools.assets/Screen Shot 2022-03-23 at 11.36.00 AM.png" alt="Screen Shot 2022-03-23 at 11.36.00 AM" style="zoom:50%;" />

<img src="Useful Tools.assets/Screen Shot 2022-03-23 at 11.38.11 AM.png" alt="Screen Shot 2022-03-23 at 11.38.11 AM" style="zoom:50%;" />

https://github.com/Airtable/airtable.js

## Framer

animation

https://www.framer.com/docs/

## Youtube API

https://developers.google.com/youtube/v3/docs/search/

Go setup a new API key before using Youtube API:

<img src="Useful Tools.assets/Screen Shot 2022-03-27 at 7.33.08 PM.png" alt="Screen Shot 2022-03-27 at 7.33.08 PM" style="zoom:50%;" />

Then enable YouTube Data API

## Have I been pwned

https://haveibeenpwned.com/

Check if people's security data leaked

## Magic

Passwordless authentication service provider

## react-modal(npm)

https://www.npmjs.com/package/react-modal

## jsonwebtoken(npm)

https://www.npmjs.com/package/jsonwebtoken

```js
import jwt from "jsonwebtoken";

const token = jwt.sign(
        {
          ...metadata,
          iat: Math.floor(Date.now() / 1000),
          exp: Math.floor(Date.now() / 1000 + 7 * 24 * 60 * 60),
          "https://hasura.io/jwt/claims": {
            "x-hasura-allowed-roles": ["user", "admin"],
            "x-hasura-default-role": "user",
            "x-hasura-user-id": `${metadata.issuer}`,
          },
        },
        process.env.JWT_SECRET_KEY
      );
```

## cookie

https://www.npmjs.com/package/cookie

```js
import cookie from "cookie";
const MAX_AGE = 7 * 24 * 60 * 60;
export const setTokenCookie = (token, res) => {
  const setCookie = cookie.serialize("token", token, {
    maxAge: MAX_AGE,
    expires: new Date(Date.now() + MAX_AGE * 1000),
    secure: process.env.NODE_ENV === "production",
    path: "/",
  });
  res.setHeader("Set-Cookie", setCookie);
};
```

