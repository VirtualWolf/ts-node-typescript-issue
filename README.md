# Problem

With the configuration in this repository (Node.js 20.9.0, Typescript 5.3.2, ts-node 10.9.1), trying to run the Typescript file with `ts-node` fails:

```
[virtualwolf@shade:~/Downloads/tsconfig-test]$ npm run start:dev

> tsconfig-test@1.0.0 start:dev
> ts-node src/index.ts

/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/index.ts:859
    return new TSError(diagnosticText, diagnosticCodes, diagnostics);
           ^
TSError: ⨯ Unable to compile TypeScript:
error TS6053: File '@tsconfig/node20/tsconfig.json' not found.

    at createTSError (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/index.ts:859:12)
    at reportTSError (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/index.ts:863:19)
    at createFromPreloadedConfig (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/index.ts:874:36)
    at phase4 (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/bin.ts:543:44)
    at bootstrap (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/bin.ts:95:10)
    at main (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/bin.ts:55:10)
    at Object.<anonymous> (/Users/virtualwolf/Downloads/tsconfig-test/node_modules/ts-node/src/bin.ts:800:3)
    at Module._compile (node:internal/modules/cjs/loader:1241:14)
    at Object.Module._extensions..js (node:internal/modules/cjs/loader:1295:10)
    at Module.load (node:internal/modules/cjs/loader:1091:32) {
  diagnosticCodes: [ 6053 ]
}
```

# Fix 1: Change `tsconfig.json`'s `extends` path

```diff
{
-   "extends": "@tsconfig/node20/tsconfig.json",
+   "extends": "./node_modules/@tsconfig/node20/tsconfig.json",
    "compilerOptions": {
        "outDir": "./dist"
    }
}
```

```
[virtualwolf@shade:~/Downloads/tsconfig-test]$ npm run start:dev

> tsconfig-test@1.0.0 start:dev
> ts-node src/index.ts

Hello, world!
```

# Fix 2: Downgrade to Typescript 5.2.2:

```
[virtualwolf@shade:~/Downloads/tsconfig-test]$ npm uninstall typescript && npm install -D --save-exact typescript@5.2.2

up to date, audited 22 packages in 536ms

found 0 vulnerabilities

changed 1 package, and audited 22 packages in 688ms

found 0 vulnerabilities
[virtualwolf@shade:~/Downloads/tsconfig-test]$ npm run start:dev

> tsconfig-test@1.0.0 start:dev
> ts-node src/index.ts

Hello, world!
```
