# Redwood Vercel Function Repro

This is a fresh RedwoodJS app created with:

```sh
npx create-redwood-app@8.9.0 redwood-vercel-node20-repro --yes --typescript --no-git-init --telemetry false
```

The scaffold was run under Node `v20.20.2`, because Redwood 8.9.0 supports `node >=20.0.0 <21.0.0` and generated:

```json
{
  "engines": {
    "node": "=20.x"
  }
}
```

The only intentional app changes are:

- `api/src/functions/ping.ts`: a minimal custom function using the documented Redwood handler shape.
- `vercel.json`: sets the Vercel build command to `yarn rw deploy vercel`.

## Repro

Deploy to Vercel with the RedwoodJS framework preset and Node.js 20.x, then call:

```sh
vercel curl /api/ping --deployment <deployment-url> -- --max-time 40 -i
```
