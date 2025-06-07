# Benjie's TSConfig

This is a bunch of TSConfig options I've grouped together to stop me having to
create massive TSConfig files all over the place.

**THIS MODULE DOES NOT FOLLOW SEMVER** (see below).

## Usage

Add the package to your `"devDependencies"`:

```sh
npm install --save-dev @benjie/tsconfig
yarn add --dev @benjie/tsconfig
```

Add to your `tsconfig.json` depending on your needs:

### @benjie/tsconfig/quick.json

Add it to your tsconfig.json:

```json
"extends": "@benjie/tsconfig/quick.json"
```

These are my generally preferred options for just bringing up a **modern**
project quickly, typically used when hacking out a proof of concept or a test
case.

**IMPORTANT**: ensure your `package.json` contains `"type": "module"`, or
otherwise write your files as `.mts` extension files.

Outline:

- Everything in `@tsconfig/node22`
- Support for `node --experimental-strip-types` as per [the Node.js docs](https://nodejs.org/docs/latest/api/typescript.html#type-stripping)
- Disables "lint-y" checks, deferring these to ESLint
- Enables and forces type checking of JavaScript
- My general preferences

<details>
  <summary>The full configuration</summary>

```jsonc
{
  "$schema": "https://json.schemastore.org/tsconfig",
  "_version": "22.0.0",
  "compilerOptions": {
    // From @tsconfig/node22:
    "lib": ["es2024", "ESNext.Array", "ESNext.Collection", "ESNext.Iterator"],
    "module": "nodenext",
    "target": "es2022",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "moduleResolution": "node16",

    // From https://nodejs.org/docs/latest/api/typescript.html#type-stripping
    "target": "esnext",
    "rewriteRelativeImportExtensions": true,
    "erasableSyntaxOnly": true,
    "verbatimModuleSyntax": true,

    // Related:
    "allowImportingTsExtensions": true,

    // From Benjie's preferences

    // I generally keep `tsc` running in parallel with other processes and
    // don't want it clearing the screen unless I opt into that.
    "preserveWatchOutput": true,

    // This has runtime performance overhead! Specifically it means a lot of
    // time is spent in 'instance_members_initializer' in V8. Disable.
    "useDefineForClassFields": false,

    // Use ESLint for these "lint"-y checks
    "noFallthroughCasesInSwitch": false,
    "noUnusedParameters": false,
    "noUnusedLocals": false,

    // Generally good stuff for authoring modules
    "pretty": true,
    "sourceMap": true,
    "importHelpers": true,

    // Generally good stuff for hacking out a project quickly
    "allowJs": true,
    "checkJs": true,
  },
}
```

</details>

### Library

The options I commonly use for authoring libraries

**TODO**

## No semver

**This package does not follow semver.** The major version number follows the
Node.js version that it targets (ref: `@tsconfig/node22` for example), but
actual updates follow Benjie's ever-changing preferences. Since TypeScript is a
development-only concern this should only ever cause build-time failures (even
with production type stripping, all types are simply ignored).

## Contributing

**These are my personal preferences** and although input is welcome, do not
expect your PR to be merged if I don't think it will benefit my projects or is
simply not to my taste.

If you notice that I've added a flag which is implied by another flag, please do
send a PR to remove it. I do not want redundancy in these configs.
