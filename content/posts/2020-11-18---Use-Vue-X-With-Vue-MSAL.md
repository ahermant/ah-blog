---
template: post
title: Use Vue MSAL in a VueX store
slug: vue-msal-vuex-store
socialImage: /media/vue.js_logo_2.svg
draft: false
date: 2020-11-18T19:45:12.351Z
description: How to use the methods and authentication data of vue-msal in a VueX store
category: VueJS
tags:
  - MSAL
  - VueJS
  - VueX
  - VueMSAL
  - authentication
---

[vue-msal](https://www.npmjs.com/package/vue-msal) allows an user to call some MSAL methods and to get the authentication context from the Vue components. Here is a solution to access to these methods and context from a [VueX Store](https://vuex.vuejs.org/guide/). 

## Pre-requisites

You need a running Vue application with a store and an [Azure Application configured to receive some SPA requests](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-spa-app-registration).

Then we install [vue-msal](https://www.npmjs.com/package/vue-msal) 

`npm install vue-msal`

## Plugin call

In main.js, we import the plugin and initialize our MSAL context

``` javascript

import msal from "vue-msal"

Vue.use(msal, {
  auth: {
    clientId: "<your-client-id>",
    authority: "<your-tenant-url>",
    redirectUri: "<your-redirect-url>",
    scopes: ["<your-scopes-separated-with-a-comma>"]
  },
  cache: {
    cacheLocation: "localStorage"
  },
  mode: "redirect"
});

```

Since the msal context is defined in the plugin as a prototype, we should already be able to call `this.$msal` from our view components to get the MSAL context.

## Export the context

If we want our store to be able to get the context, we need to export it from `main.js`

``` javascript
const msalContext = Vue.prototype.$msal;

if (!msalContext.isAuthentified)
  msalContext.signIn();

export { msalContext }
```

## Retrieve the context from the store

Now we can retrieve our context from the store including our access token and our user info

``` javascript
import Vue from 'vue'
import Vuex from 'vuex'
import { msalContext } from '../main'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
  },
  mutations: {
  },
  actions: {
    getContext() {
      return msalContext;
    }
  },
  modules: {
  }
})

```


