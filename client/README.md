# TLDRMailer dashboard

React + Vite dashboard for creating, regenerating, and sending newsletters and managing the mailing list. Users sign in with Auth0 (settings in `src/config/auth0-config.json`), and the dashboard calls the API at `http://localhost:3001/api`.

```bash
npm install
npm start       # Vite dev server on http://localhost:3000
npm run build   # production build in dist/
```

See the [root README](../README.md) for the architecture and full local setup.
