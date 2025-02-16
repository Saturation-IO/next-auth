<p align="center">
   <br/>
   <a href="https://authjs.dev" target="_blank"><img height="64px" src="https://authjs.dev/img/logo/logo-sm.png" /></a>&nbsp;&nbsp;&nbsp;&nbsp;<img height="64px" src="https://raw.githubusercontent.com/nextauthjs/adapters/main/packages/pouchdb/logo.svg" />
   <h3 align="center"><b>PouchDB Adapter</b> - NextAuth.js</h3>
   <p align="center">
   Open Source. Full Stack. Own Your Data.
   </p>
   <p align="center" style="align: center;">
      <img src="https://github.com/nextauthjs/next-auth/actions/workflows/release.yml/badge.svg?branch=main" alt="CI Test" />
      <img src="https://img.shields.io/bundlephobia/minzip/@next-auth/pouchdb-adapter" alt="Bundle Size"/>
      <img src="https://img.shields.io/npm/v/@next-auth/pouchdb-adapter" alt="@next-auth/pouchdb-adapter Version" />
   </p>
</p>

## Overview

This is the PouchDB Adapter for [`auth.js`](https://authjs.dev). This package can only be used in conjunction with the primary `auth.js` package. It is not a standalone package.

Depending on your architecture you can use PouchDB's http adapter to reach any database compliant with the CouchDB protocol (CouchDB, Cloudant, ...) or use any other PouchDB compatible adapter (leveldb, in-memory, ...)

## Getting Started

1. Install `next-auth` and `@next-auth/pouchdb-adapter`, as well as `pouchdb`.

> **Prerequisite**: Your PouchDB instance MUST provide the `pouchdb-find` plugin since it is used internally by the adapter to build and manage indexes

```js
npm install next-auth @next-auth/pouchdb-adapter pouchdb
```

2. Add this adapter to your `pages/api/[...nextauth].js` next-auth configuration object.

```js
import NextAuth from "next-auth"
import Providers from "next-auth/providers"
import { PouchDBAdapter } from "@next-auth/pouchdb-adapter"
import PouchDB from "pouchdb"

// Setup your PouchDB instance and database
PouchDB.plugin(require("pouchdb-adapter-leveldb")) // Or any other PouchDB-compliant adapter
  .plugin(require("pouchdb-find")) // Don't forget the `pouchdb-find` plugin

const pouchdb = new PouchDB("auth_db", { adapter: "leveldb" })

// For more information on each option (and a full list of options) go to
// https://authjs.dev/reference/configuration/auth-options
export default NextAuth({
  // https://authjs.dev/reference/providers/oauth-builtin
  providers: [
    Providers.Google({
      clientId: process.env.GOOGLE_ID,
      clientSecret: process.env.GOOGLE_SECRET,
    }),
  ],
  adapter: PouchDBAdapter(pouchdb),
  // ...
})
```

## Advanced

### Memory-First Caching Strategy

If you need to boost your authentication layer performance, you may use PouchDB's powerful sync features and various adapters, to build a memory-first caching strategy.

Use an in-memory PouchDB as your main authentication database, and synchronize it with any other persisted PouchDB. You may do a one way, one-off replication at startup from the persisted PouchDB into the in-memory PouchDB, then two-way, continuous, retriable sync.

This will probably not improve performance much in a serverless environment for various reasons such as concurrency, function startup time increases, etc.

For more details, please see https://pouchdb.com/api.html#sync

## Contributing

We're open to all community contributions! If you'd like to contribute in any way, please read our [Contributing Guide](https://github.com/nextauthjs/.github/blob/main/CONTRIBUTING.md).

## License

ISC
