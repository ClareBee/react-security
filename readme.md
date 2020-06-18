# ReactSecurity - Orbit

- by Ryan Chenkie on https://courses.reactsecurity.io/

## To run

```bash
cd orbit-app
npm install
cd ../orbit-api
npm install
```

- backend needs .env file with `ATLAS_URL` for a MongoDB Atlas Cluster & a `JWT_SECRET`
- on terminal (launches on `http://localhost:3000`)

```bash
cd orbit-app
npm start
```

- in new terminal (launches on `http://localhost:3001`):

```bash
cd orbit-api
npm run dev
```

### Features:

- see notes.md for more details!
- JWTs & localStorage (using ), Auth State in React Apps, Client-Side routes, API protected endpoints, roles/authentication, Code-Splitting, XXS protection via DOMPurify, Cookies, CSRF protection via Csurf

#### Security packages

Express middleware:

- https://github.com/auth0/express-jwt
- https://github.com/expressjs/cookie-parser
- https://github.com/expressjs/csurf

Helpful packages:

- https://github.com/auth0/jwt-decode
- https://github.com/auth0/node-jsonwebtoken (signing jwt)
- https://github.com/kelektiv/node.bcrypt.js (password hashing/salting)
- https://github.com/cure53/DOMPurify (sanitize innerHTML)

## License of original Orbit Code

MIT
