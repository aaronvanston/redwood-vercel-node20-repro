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

This repository is connected to Vercel here:

- Project: `aaron-vanston/redwood-vercel-node20-repro`
- Production URL: https://redwood-vercel-node20-repro.vercel.app
- Deployment URL: https://redwood-vercel-node20-repro-3bovt8k3w-aaron-vanston.vercel.app
- Deployment ID: `dpl_75AwH8yyMezNGmoPiPTAbzeNgvZP`
- Runtime for `api/ping`: `nodejs20.x`

Observed result on Vercel:

```text
curl: (28) Operation timed out after 40003 milliseconds with 0 bytes received
```

Runtime logs show the same Vercel Node bridge failure seen in the Cedar repro:

```text
Unhandled Rejection: SyntaxError: Unexpected end of JSON input
    at JSON.parse (<anonymous>)
    at IncomingMessage.<anonymous> (/opt/rust/nodejs.js:2:14238)
```

The generated `/.redwood/functions/ping` path returns the web fallback HTML; the actual deployed Vercel lambda path is `api/ping`.
