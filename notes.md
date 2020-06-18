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
- using Redirect component from react-router (https://reacttraining.com/react-router/web/example/auth-workflow)

### Auth State

- React Context Vs prop-drilling: https://reactjs.org/docs/context.html
- persisting auth state on page refresh via browser's localStorage (JSON.stringify for objects)
- you should NOT store jwt in localStorage due to vulnerability to cross-site scripting attacks (could steal token & use it!) but ok here for experimenting :) (better to use cookies)
- check if current user is authenticated. Different in SPA from 'round-trip' applications as decided on the client-side => risky. BUT data is behind protected API endpoints & token has expiresAt to force re-login
- useHistory hook from react-router: https://reacttraining.com/react-router/web/api/Hooks
- qq of roles/scopes => keep things fine-grained/extensible (OAuth2: https://oauth.net/2/) => pass through isAuthenticated/isAdmin on the auth context

### Client-side Routing

### Authenticated HTTP Requests

### Protecting API Endpoints

### Hardening the App

### Switching to Cookies
