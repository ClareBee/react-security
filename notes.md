## React Security Tips

### JWT

- JWR = payload, header, signature thru hashing algorithm -> if verified, shows it hasn't been tampered with

#### Don't

- don't keep in localStorage -> easily scriptable! XXS vulnerabilities...
- don't keep your secret keys used to sign the tokens in the browser => should only ever be kept on the back-end -> browsers are public clients!
- don't decode the token in the client. if you need user info, get it from an endpoint, not from decoding it on the client

#### Do

- do keep long, strong, unguessable secrets :)
- do keep your token payload small (they need to travel in an auth header or in a cookie => larger means larger request/larger bandwidth => lower performance)
- do make sure you're using https (to stop someone intercepting traffic, getting yr token...)

### Sign up/sign in

- never store passwords in the db 'in the clear' -> only ever hashed e.g. via bcrypt to generate hash/salt
- https://github.com/kelektiv/node.bcrypt.js
- see `orbit-api/util.js` for helper methods using bcrypt to sign/verify
