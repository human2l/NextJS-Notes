# Password

<img src="Password vs Passwordless.assets/Screen Shot 2022-03-28 at 12.35.29 PM.png" alt="Screen Shot 2022-03-28 at 12.35.29 PM" style="zoom:50%;" />

# Magic - Passwordless

## Docs: 

https://magic.link/docs/login-methods/email/integration/web

<img src="Password vs Passwordless.assets/passwordless-login.gif" alt="passwordless-login" style="zoom:50%;" />

<img src="Password vs Passwordless.assets/Screen Shot 2022-03-28 at 1.26.26 PM.png" alt="Screen Shot 2022-03-28 at 1.26.26 PM" style="zoom:50%;" />

## DID token

Decentralized ID (DID) tokens are used as cryptographically-generated proofs that are used to manage user access to your application's resource server.

### [What is a DID Token?](https://magic.link/docs/introduction/decentralized-id#what-is-a-did-token)

By adapting W3C's [Decentralized Identifiers](https://w3c-ccg.github.io/did-primer/) (DID) protocol, the **DID token** created by the Magic client-side SDK (see [`getIdToken`](https://magic.link/docs/api-reference/client-side-sdks/web#getidtoken)) leverages the [Ethereum](https://ethereum.org/) blockchain and [elliptic curve cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography) to generate verifiable proofs of identity and authorization. These proofs are encoded in a lightweight, digital signature that can be shared between client and server to manage permissions; protect routes and resources, or authenticate users.

The DID token is encoded as a Base64 JSON string tuple representing `**[proof, claim]**`:

- `proof`: A digital signature that proves the validity of the given `claim`.
- `claim`: Unsigned data the user asserts. This should equal the `proof` after Elliptic Curve recovery.

Copy

```js
const claim = JSON.stringify({ ... }); // Data representing the user's access
const proof = sign(claim); // Sign data with Ethereum's `personal_sign` method
const DIDToken = btoa(JSON.stringify([proof, claim]));
```

## Implementing Magic

`npm install --save magic-sdk`

#### lib/magic-client.js

```js
import { Magic } from "magic-sdk";

export const createMagic = () => {
  return (
    typeof window !== "undefined" &&
    new Magic(process.env.NEXT_PUBLIC_MAGIC_PUBLISHABLE_API_KEY)
  );
};
```

##### [`loginWithMagicLink`](https://magic.link/docs/api-reference/client-side-sdks/web#loginwithmagiclink)

#### pages/login.js

```react
import { createMagic } from "../lib/magic-client";
//------------------
const handleLoginWithEmail = async (e) => {
    e.preventDefault();
    if (!email) {
      setUserMsg("Please enter a valid email");
      return;
    }
    // log in a user by their email
    try {
      const magic = createMagic();
      const didToken = await magic.auth.loginWithMagicLink({
        email,
      });
      //If login/signup successfully, Magic will send back the didToken
      if (didToken) {
        router.push("/");
      }
    } catch (error) {
      console.error("Something went wrong logging in", error);
    }
  };
```

Magic will create an IndexedDB in browser to store keys:

<img src="Password vs Passwordless.assets/Screen Shot 2022-03-28 at 2.34.36 PM.png" alt="Screen Shot 2022-03-28 at 2.34.36 PM" style="zoom:50%;" />

##### [`getMetadata`](https://magic.link/docs/api-reference/client-side-sdks/web#getmetadata)

```react
useEffect(() => {
    (async () => {
      // Assumes a user is already logged in
      try {
        const { email } = await magic.user.getMetadata();
        if (!email) {
          router.push("/login");
          return;
        }
        setUsername(email);
      } catch (err) {
        console.error("Error retrieving email", err);
      }
    })()
  }, [router]);
```

