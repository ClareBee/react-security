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

- protect routes based on auth
- protect routes based on role
- react-router-dom's `<Route />` has a render prop where you can check if user is authenticated and redirect if not

```javascript
const AuthenticatedRoute = ({ children, ...rest }) => {
  const authContext = useContext(AuthContext);

  return (
    <Route
      {...rest}
      render={() =>
        authContext.isAuthenticated() ? (
          <AppShell>{children}</AppShell>
        ) : (
          <Redirect to="/" />
        )
      }
    />
  );
};
```

### Authenticated HTTP Requests

- add JWT to axios request
- add HTTP interceptor to axios
  e.g.

```javascript
axios.interceptors.request.use(
  (config) => {
    config.headers.authorization = `Bearer ${accessToken}`;
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);
```

- But this sends accessToken to servers we don't know enough about.
- Best option - set up an instance of axios to keep it specific:

```javascript
const authAxios = axios.create({
  baseUrl: apiUrl,
});
```

and then add the interceptors onto that

- putting the special version of axios onto context - see `orbit-app/src/context/FetchContext.js`

### Protecting API Endpoints

- use middleware to protect endpoints on the backend
  e.g.

```javascript
app.use((req, res, next) => {
  // code
  console.log(req.headers);
  next();
});
```

- best to use library rather than rolling your own
- `npm i --save express-jwt jwt-decode`

```javascript
  secret: process.env.JWT_SECRET,
  issuer: "api.orbit",
  audience: "api.orbit",
});

app.get("/api/dashboard-data", checkJwt, (req, res) => res.json(dashboardData));
```

- attach user onto the request object w custom middleware
- 'sub' claim => user id aka subject: stored on token

```javascript
const attachUser = (req, res, next) => {
  const token = req.headers.authorization;
  if (!token) {
    return res.status(401).json({ message: "Authentication invalid" });
  }
  const decodedToken = jwtDecode(token.slice(7)); // get rid of Bearer scheme
  if (!decodedToken) {
    return res
      .status(401)
      .json({ message: "There was a problem authorising the request" });
  } else {
    req.user = decodedToken;
    next();
  }
};

app.use(attachUser); // applies to all below here
```

### Hardening the App

### Switching to Cookies
